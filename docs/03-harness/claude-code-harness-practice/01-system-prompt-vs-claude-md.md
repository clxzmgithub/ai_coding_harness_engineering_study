# System Prompt vs CLAUDE.md：上下文窗口中的两类系统配置

> 来源：Claude Code 工程之道学习笔记 | 整理日期：2026-06-14

---

## 1. 核心区别一览

| 对比维度 | System Prompt（内置规则） | CLAUDE.md（项目记忆） |
|---------|--------------------------|----------------------|
| **谁写的** | Anthropic / CatPaw 工程师 | 你 / 你的团队 |
| **能修改吗** | 不能（黑盒） | 完全可以 |
| **内容范围** | Claude 的通用行为规范 | 项目的特定上下文 |
| **生命周期** | 跟随工具版本迭代 | 跟随你的项目 |
| **Token 占用** | 约 3000–8000 Token（估算） | 你写多少就占多少 |
| **类比** | 公司员工手册（Anthropic 发的） | 项目组工作规范（你写的） |

---

## 2. 它们在上下文窗口中的位置

### 2.1 物理存储：同一个 `system` 字段

Claude API 每次请求的消息结构：

```json
{
  "system": "...（一个完整的拼接字符串）",
  "messages": [
    {"role": "user",      "content": "..."},
    {"role": "assistant", "content": "..."}
  ]
}
```

**`system` 字段只有一个，是纯文本字符串。**
Harness 在每次发起 API 调用时，把所有"系统级"内容**按顺序拼接**成这一个字符串：

```
┌──────────────────────────────────────────────────────┐
│           system 字段（一个完整的字符串）              │
│                                                      │
│  [段落1] Anthropic/CatPaw 硬编码规则（约3000-8000T）  │
│          - Claude 的角色定义                          │
│          - 工具使用规范、安全策略、输出格式...          │
│                                                      │
│  [段落2] CLAUDE.md 内容（你写了多少就多少）            │
│          - 项目编码规范、团队约定...                   │
│                                                      │
│  [段落3] 激活的 SKILL.md 内容（按需加载）              │
│          - 当前 Skill 的操作指南...                   │
│                                                      │
│  [段落4] Rules/*.md 内容（Globs 匹配后注入）           │
│          - 条件性规则...                              │
└──────────────────────────────────────────────────────┘
```

**结论：物理上没有分开，逻辑上有顺序。** Claude 看到的是一段完整文本，不知道哪部分是 Anthropic 写的、哪部分是你写的。

### 2.2 Claude Code 官方内置 System Prompt（从二进制提取的真实内容）

```
You are Claude Code, Anthropic's official CLI for Claude.
You are an interactive CLI tool that helps users with software engineering tasks.
In addition to software engineering tasks, you should provide educational insights
about the codebase along the way.
```

---

## 3. CatPaw vs 官方 Claude Code 的 System Prompt 差异

### 3.1 CatPaw 追加的额外 System Prompt

通过分析 CatPaw 扩展源码（`extension.js` 中变量 `Cit`），找到 CatPaw **实际追加的完整内容**：

```
# VSCode Extension Context

You are running inside a VSCode native extension environment.

## Code References in Text
IMPORTANT: When referencing files or code locations, use markdown link syntax
to make them clickable:
- For files: [filename.ts](src/filename.ts)
- For specific lines: [filename.ts:42](src/filename.ts#L42)
- For a range of lines: [filename.ts:42-51](src/filename.ts#L42-L51)
- For folders: [src/utils/](src/utils/)
Unless explicitly asked for by the user, DO NOT USE backticks or HTML tags
like <code> for file references - always use markdown [text](link) format.

## User Selection Context
The user's IDE selection (if any) is included in the conversation context
and marked with ide_selection tags.
```

**这就是 CatPaw 客户端注入的全部额外 System Prompt。** 只有这约200字，告诉 Claude：你在 VSCode 里跑，引用文件用 markdown 链接格式。

### 3.2 注入方式（源码）

```javascript
// CatPaw extension.js 实际代码
systemPrompt: {
    type: "preset",       // 使用官方预设
    preset: "claude_code", // = 官方 Claude Code 的内置规则
    append: Cit           // 追加上面那段 VSCode 上下文
}
```

### 3.3 其他关键差异

```javascript
// CatPaw 还做了这三件事：
var envVars = [
    { name: 'ANTHROPIC_BASE_URL',    value: 'https://mcli.sankuai.com' }, // 路由到美团代理
    { name: 'ANTHROPIC_AUTH_TOKEN',  value: cachedMisId },                // mis 工号认证
    { name: 'ANTHROPIC_CUSTOM_HEADERS', value: customHeaders }            // 注入工作区信息
];

// 自定义 Headers 包含：
// x-working-dir: 当前工作目录
// x-repo-url:    Git 仓库地址
// x-branch:      当前分支
// x-ide-type:    IDE 类型

// 默认模型兜底
const DEFAULT_MODEL = 'kimi-k2.5'; // 用户未配置时默认 Kimi，不是 Claude！
```

---

## 4. CLAUDE.md 的注入时机与生效规则

- **会话初始化时**：Harness 读取所有 CLAUDE.md 文件，拼接到 system 字段
- **不会动态更新**：同一会话中修改 CLAUDE.md 不会生效，需要新开会话
- **原因**：Harness 在会话启动时"冻结"system prompt，后续每轮 API 调用复用同一个 system 字段

### 5层记忆的加载顺序

```
企业配置（最高优先级）
  ↓
用户级 CLAUDE.md（~/.claude/CLAUDE.md）
  ↓
项目级 CLAUDE.md（<project>/CLAUDE.md）
  ↓
Rules/*.md（.claude/rules/，按 Globs 条件匹配）
  ↓
Local CLAUDE.md（.claude/CLAUDE.local.md，不提交 Git）
```

---

## 5. 关键结论

1. **CLAUDE.md 的内容最终成为 system 字段的一部分**，和 Anthropic 内置规则在同一个字符串里，Claude 无法区分来源
2. **CatPaw 的 System Prompt 差异极小**（仅 VSCode 环境说明），真正的差异在 API 代理路由和默认模型上
3. **美团代理层 `mcli.sankuai.com`** 负责：身份鉴权（mis→API Key）、模型路由（支持 Kimi 等）、审计日志、费用管控
4. **修改 CLAUDE.md 后必须重启会话才能生效**

---

*参考：CatPaw extension.js 2.1.163 源码分析 + Claude Code 2.1.168 二进制逆向*

