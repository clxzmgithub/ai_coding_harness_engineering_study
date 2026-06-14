# Agentic Loop 深度控制：observe-think-act 循环机制

> 来源：Claude Code 工程之道学习笔记 | 整理日期：2026-06-14
> 分析方法：Claude Code 2.1.168 二进制 + CatPaw extension.js 源码 + 书中 Agentic Loop 图示

---

## 1. 什么是 Agentic Loop

Agentic Loop 是 Claude Code 的**运行时核心引擎**，也叫 Harness 的执行主体。对应三个阶段：

```
┌──────────────────────────────────────────────────────┐
│          Agentic Loop（一个完整循环 = 一个 turn）      │
│                                                      │
│  OBSERVE ──────→ THINK ──────→ ACT                   │
│      ↑                           │                   │
│      └───────────────────────────┘                   │
│                （每轮循环）                           │
└──────────────────────────────────────────────────────┘
```

对应书中的 Agentic Loop 图示（`09-2.5-底层视角`）：

```
① 接收输入（OBSERVE）
   用户 prompt + 系统 prompt + 工具定义 + 对话历史
         │
         ▼
② 模型推理（THINK）
   Claude 分析上下文，生成回复
   回复 = 文本 + 工具调用请求（可选）
         │
    ┌────┴────┐
    │ 有工具   │ 无工具调用
    │ 调用？   │────────→ ⑤ 返回最终结果（end_turn）
    └────┬────┘
         │ 是
         ▼
③ 执行工具（ACT）
   Harness 执行工具，收集结果
   （权限检查 → 执行 → 返回结果）
         │
         ▼
④ 结果回注（OBSERVE 下一轮的输入）
   工具结果追加到对话历史
   回到 ②，继续推理
```

---

## 2. 两个维度的深度控制

### 2.1 主对话的 turns

交互式模式（IDE 中使用）：**无上限**，用户随时可以继续对话
`-p/--print` 模式：可通过 `--max-turns` CLI 参数限制

### 2.2 子智能体（forked agent）的 turns

**源码中的关键变量**（从 `extension.js` 提取）：

```javascript
// Claude Code 核心源码（RZ 函数）
var s6q = 50;  // ← 子智能体默认最大 turns！

async function runForkedAgent({ maxTurns }) {
    let v = maxTurns ?? s6q;  // 未指定时默认 50 轮
    let turnCount = 0;

    for await (let message of runMainLoop({ maxTurns: v, ... })) {
        if (message.type === "assistant") turnCount++;
        // ...处理消息
    }

    // 超过默认 50 轮？记录监控事件
    if (maxTurns === undefined && turnCount >= s6q) {
        recordEvent("tengu_forked_agent_default_turns_exceeded", {
            forkLabel, turnCount
        });
    }
}
```

---

## 3. 一个 turn 的内部执行细节

### 3.1 一轮 = 一次 API 调用 + N 次工具执行

```javascript
// 伪代码：一个 turn 的完整执行
while (true) {
    // OBSERVE: 当前 messages 数组（含历史 + 上轮工具结果）

    // THINK: 发一次 API 请求
    let response = await api.messages.create({
        messages: currentMessages,
        system: systemPrompt,
        tools: availableTools
    });

    // 退出条件判断
    if (response.stop_reason === "end_turn")   break; // ① Claude 说"完成了"
    if (response.stop_reason === "pause_turn") { ...  } // ② 主动暂停等待用户
    // ③ max_turns 耗尽 → break
    // ④ 错误（rate_limit 等）→ break

    // ACT: 执行所有工具调用（可并行）
    let toolResults = await Promise.all(
        response.tool_use_blocks.map(tool => executeTool(tool))
    );

    // OBSERVE（下一轮）: 把工具结果追加到 messages
    currentMessages.push({ role: "user", content: toolResults });

    turnCount++;
}
```

### 3.2 关键：一轮内可以并行执行多个工具

Claude 可以一次发出多个工具调用，Harness 并行执行：

```
第N轮：
THINK → Claude: "我需要同时读 auth.ts、user.ts、token.ts"
ACT  → Harness 并行执行 3 个 Read
OBSERVE → 3 个文件内容同时回注，算一轮

（不是 3 轮，是 1 轮！）
```

这让 Claude Code 能用**较少轮次**完成大量工作。

---

## 4. 四种退出方式（从源码提取）

| stop_reason | 含义 | 处理方式 |
|-------------|------|---------|
| `end_turn` | Claude 主动完成，认为任务完成 | 循环退出，返回结果 |
| `pause_turn` | Claude 主动暂停，等待用户输入 | 等待用户响应后继续循环 |
| `max_turns_reached` | 达到最大 turns 限制 | 循环退出，报告 `error_max_turns` |
| `error_*` | API 错误（rate_limit/billing 等） | 循环退出，报告错误 |

```javascript
// 错误码枚举（从二进制提取）
case "error_max_turns":
    displayError(`Error: Reached max turns (${A.maxTurns})`); break;
case "error_max_budget_usd":
    displayError(`Error: Exceeded USD budget (${A.maxBudgetUsd})`); break;
case "error_max_structured_output_retries":
    displayError("Error: Failed to provide valid structured output"); break;
```

---

## 5. pause_turn：人在环中（Human-in-the-Loop）

这是 Claude Code 独特的中间状态，介于完全自主和完全停止之间：

```
完全自主 ←─────────────────────────────→ 完全停止
  end_turn                pause_turn        abort
  （Claude自己判断完成）   （主动暂停等人）   （强制终止）
```

**pause_turn 的典型场景**：
- 需要用户确认危险操作（如删除大量文件）
- 需要用户提供外部凭证
- 遇到歧义，需要人工决策

决策完成后，Agentic Loop **自动继续**，不需要重新开始。

---

## 6. 深度（轮次）的实际消耗估算

```
任务类型                          典型轮次消耗

修复一个明确的 bug                  3 - 8 轮
重构一个函数                        5 - 15 轮
给一个模块写完整测试                 8 - 20 轮
分析整个项目架构                    10 - 25 轮
从零实现一个新 API 接口              15 - 35 轮
复杂 bug 调试（需多次试错）          20 - 45 轮
完整功能模块实现（含测试）           30 - 50 轮 ← 接近子智能体上限
```

当子智能体超过 50 轮时，触发 `tengu_forked_agent_default_turns_exceeded` 监控事件，Anthropic 用此遥测数据调整后续版本的默认值。

---

## 7. Claude Code vs 国内工具的本质差距

```
Claude Code 的控制逻辑：
  while (true) {
      执行 observe-think-act
      if (stop_reason === "end_turn") break;  // 只有Claude自己说完了才停
  }
  理念：「信任模型对任务完成的判断」
  深度：子智能体默认 50 轮，主对话无上限

典型国内工具的控制逻辑：
  for (let i = 0; i < 5; i++) {  // 固定轮数
      执行一次
  }
  print("我完成了");  // 哪怕没完成
  理念：「防止模型跑太长花太多钱」
  深度：通常 3-10 轮
```

**关键差异**：Claude Code 不会在任务做到一半时主动停下来问"要不要继续"，而是真正跑完为止。这是 Claude Code "更能扛"、"更自主"体验的根本原因。

---

## 8. 三个维度的预算控制

| 维度 | 控制方式 | 默认值 |
|------|---------|--------|
| **turn 数量** | `--max-turns` CLI / `maxTurns` SDK 参数 | 交互式无限制，子智能体 50 |
| **Token 花费** | `--max-budget-usd` CLI 参数 | 无限制 |
| **思考深度** | `--effort` (low/medium/high/xhigh/max) | medium |

三个维度可以组合使用，构建精确的预算控制策略。

---

*参考：Claude Code 二进制 `s6q=50` 变量 + RZ 函数核心循环 + stop_reason 枚举*

