# Context Compaction 机制深度解析

> 来源：Claude Code 工程之道学习笔记 | 整理日期：2026-06-14
> 分析方法：从 Claude Code 2.1.168 二进制 + CatPaw extension.js 源码提取

---

## 1. 为什么需要 Context Compaction？

### 1.1 根本原因：LLM 的无状态 API

LLM 是**有状态无记忆**的矛盾体——能处理长上下文，但每次调用必须把**完整历史**全部传入。

随着 Agentic Loop 不断循环，上下文线性增长：

```
第1轮:  [System 8K] + [Turn1: 2K]               = 10K
第5轮:  [System 8K] + [Turn1~5: 30K + 工具50K]  = 88K
第10轮: [System 8K] + [累计历史: 150K+]          ← 接近 200K 上限
```

### 1.2 不处理的三个后果

| 后果 | 说明 |
|------|------|
| **API 报错** | 触发 `context_length_exceeded`，任务强制中断 |
| **内容丢失** | 即使没报错，离上限越近，新内容越容易被截断（模型"忘记"前面内容） |
| **费用飙升** | 每轮 Token 数线性增长，每次都要传完整历史 |

### 1.3 最典型的噪声场景

```
任务：运行测试套件，分析失败原因

测试输出（噪声）: 10,000 Token
若不用子智能体隔离 → 这 10K 每轮都要重传
后续 5 轮 = 重复传送 10K × 5 = 50,000 Token 白白浪费
```

---

## 2. Context Compaction 的触发机制

### 2.1 触发条件（源码级）

```javascript
// 从 CatPaw extension.js 提取的核心逻辑
async function shouldCompact(compactionControl, usage) {
    // 1. 统计当前总 Token 数（包含缓存部分）
    let totalTokens = usage.input_tokens
                    + (usage.cache_creation_input_tokens ?? 0)
                    + (usage.cache_read_input_tokens ?? 0)
                    + usage.output_tokens;

    // 2. 对比阈值
    let threshold = compactionControl.contextTokenThreshold ?? DEFAULT_THRESHOLD;

    // 3. 超过阈值就触发
    if (totalTokens < threshold) return false;
    return true;
}
```

**阈值规则（来自 Claude Code 二进制文本）**：
```
Auto-compact summarizes the conversation when context usage approaches this limit.
The actual threshold is the minimum of this setting and your model's maximum
context window.
```

即：`触发阈值 = min(用户设置的阈值, 模型最大上下文窗口)`，通常在 **80-95%** 时触发。

### 2.2 配置项

```
autoCompactEnabled: true/false  ← 是否开启自动压缩（/config 中配置）

手动触发：/compact              ← 随时手动触发压缩
```

---

## 3. Compaction 的执行过程（三步）

### 步骤一：发起专用 API 调用

```javascript
// 用原始完整 messages + summaryPrompt，发一次新 API 请求
let compactionResult = await client.beta.messages.create({
    model: model,
    messages: [
        ...originalMessages,        // 完整历史全部传入（这一次是值得的）
        { role: "user", content: summaryPrompt }  // 追加压缩指令
    ],
    headers: { "x-stainless-helper": "compaction" }  // 标记为压缩请求
});
```

### 步骤二：summaryPrompt 内容（源码中的真实 Prompt）

```
You have been working on the task described above but have not yet completed it.
Write a continuation summary that will allow you (or another instance of yourself)
to resume work efficiently in a future context window where the conversation history
will be replaced with this summary. Your summary should be structured, concise, and
actionable. Include:

1. Task Overview
   The user's core request and success criteria
   Any clarifications or constraints they specified

2. Current State
   What has been completed so far
   Files created, modified, or analyzed (with paths if relevant)
   Key outputs or artifacts produced

3. Important Discoveries
   Technical constraints or requirements uncovered
   Decisions made and their rationale
   Errors encountered and how they were resolved
   What approaches were tried that didn't work (and why)

4. Next Steps
   Specific actions needed to complete the task
   Any blockers or open questions to resolve
   Priority order if multiple steps remain

5. Context to Preserve
   User preferences or style requirements
   Domain-specific details that aren't obvious
   Any promises made to the user

Wrap your summary in <summary></summary> tags.
```

### 步骤三：用摘要替换历史

```javascript
// 把完整 messages 历史，替换为单条摘要消息
params.messages = [{ role: "user", content: compactionResult.content }];
// 后续 Agentic Loop 继续执行，但历史已被压缩
```

---

## 4. 压缩效果对比

```
压缩前：
messages = [
  {role:user,      content: "帮我重构这个模块..."},        // 2K
  {role:assistant, content: "<工具调用: Read 50个文件>"}, // 80K
  {role:user,      content: "<50个文件内容>"},              // 100K
  {role:assistant, content: "分析结果..."},                 // 10K
  ...
]
总计: ~192K Token

压缩后：
messages = [
  {role:user, content: `<summary>
    1. Task: 重构 auth 模块，要求保持 TypeScript strict mode
    2. Current: 已完成 utils/ 下的 3 个文件，正在处理 service/
    3. Discoveries: 发现循环依赖在 auth.ts:L42，已通过...
    4. Next: 需要处理 middleware/session.ts
    5. Context: 用户要求所有函数必须有 JSDoc 注释
  </summary>`}
]
总计: ~4K Token  ← 压缩率约 98%
```

**Agentic Loop 无缝继续，不中断，不需要用户介入。**

---

## 5. Compaction 相关的 Hook

```json
// .claude/settings.json 中配置
{
  "hooks": {
    "PreCompact": [{
      "hooks": [{
        "type": "command",
        "command": "echo '即将压缩，保存重要信息到 memory/'"
      }]
    }],
    "PostCompact": [{
      "hooks": [{
        "type": "command",
        "command": "echo '压缩完成，继续任务'"
      }]
    }]
  }
}
```

**PreCompact**：可以在压缩前把重要内容保存到 Memory 文件
**PostCompact**：压缩完成后的通知或清理操作

---

## 6. CatPaw 的 Compaction 机制

**结论：CatPaw 完全继承 Claude Code 的 Compaction 机制，没有自己实现。**

- CatPaw extension.js 里的 `compactionControl`、`contextTokenThreshold`、`summaryPrompt` 完全来自 `@anthropic-ai/claude-agent-sdk`（npm 包引入）
- `autoCompactEnabled` 配置项直接透传给 Claude Code 内核
- 触发逻辑、压缩 Prompt、替换逻辑全部与官方一致

---

## 7. 子智能体的 Compaction 优化作用

使用子智能体可以从根源上减少 Compaction 的压力：

```
不用子智能体：
  10,000 Token 测试噪声进入主对话
  → 每轮都要重传这 10K
  → 5 轮 = 50K 浪费，10 轮 = 100K 浪费
  → 更频繁触发 Compaction

用子智能体：
  子智能体内部处理测试（独立上下文，一次性花 41K）
  主对话只收到 100 Token 摘要
  → 主对话上下文增长极慢
  → Compaction 触发频率大幅降低
  → 对话轮数越多，优势越明显（书中数据：超过 50% 节省）
```

---

*参考：CatPaw extension.js 中 `summaryPrompt`（变量 `IV`）的完整提取 + Claude Code 二进制 compact 相关文本*

