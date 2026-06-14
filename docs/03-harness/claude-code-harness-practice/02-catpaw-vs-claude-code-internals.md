# CatPaw vs Claude Code：内部实现深度对比

> 来源：Claude Code 工程之道学习笔记 | 整理日期：2026-06-14
> 分析方法：CatPaw extension.js 2.1.163 源码 + Claude Code 2.1.168 二进制逆向

---

## 1. 整体架构对比

```
你的请求
   ↓
┌─────────────────────────────────────┐
│         CatPaw Harness              │
│  1. 读取 CLAUDE.md / SKILL.md       │
│  2. 默认模型兜底 kimi-k2.5           │
│  3. 设置 ANTHROPIC_BASE_URL         │
│     = https://mcli.sankuai.com      │
│  4. 注入工作区信息到 Headers         │
└──────────────────┬──────────────────┘
                   ↓ HTTP 请求
┌─────────────────────────────────────┐
│    美团代理层 mcli.sankuai.com       │
│  ① mis 工号 → API Token 转换         │
│  ② 多模型路由（Claude/Kimi/等）       │
│  ③ 审计日志（谁/哪个项目/哪个分支）   │
│  ④ 费用管控（按 mis 工号统计 Token）  │
│  ⑤ 合规内容过滤                      │
└──────────────────┬──────────────────┘
                   ↓ 转发
┌─────────────────────────────────────┐
│  Anthropic Claude API / Kimi API    │
│  （取决于用户选择的模型）              │
└─────────────────────────────────────┘
```

---

## 2. 关键差异：从源码中提取的真实数据

### 2.1 API 入口（最本质的差异）

| | Claude Code（官方） | CatPaw |
|--|--|--|
| **API 入口** | `https://api.anthropic.com` | `https://mcli.sankuai.com` |
| **鉴权方式** | Anthropic API Key | 美团 mis 工号（`cachedMisId`） |
| **自定义 Headers** | 无 | `x-working-dir`、`x-repo-url`、`x-branch`、`x-ide-type` |

```javascript
// CatPaw extension.js 实际代码（第164行附近）
var envVars = [
    {
        name: 'ANTHROPIC_BASE_URL',
        value: 'https://mcli.sankuai.com'   // ← 所有请求走美团代理
    },
    {
        name: 'ANTHROPIC_AUTH_TOKEN',
        value: cachedMisId                   // ← mis 工号作为认证 token
    },
    {
        name: 'ANTHROPIC_CUSTOM_HEADERS',
        value: [
            'customHeaders: ',
            `x-working-dir: ${encodeURIComponent(workspaceInfo.workingDir)}`,
            `x-repo-url: ${encodeURIComponent(workspaceInfo.repoUrl)}`,
            `x-branch: ${encodeURIComponent(workspaceInfo.branch)}`,
            `x-ide-type: ${cachedSource}`
        ].join('\n')
    }
];
```

### 2.2 默认模型兜底

```javascript
// CatPaw extension.js 实际代码
const DEFAULT_MODEL = 'kimi-k2.5';

// 兜底策略：
// - settings.json 不存在 → 创建并写入 { model: 'kimi-k2.5' }
// - settings.json 存在但 model 字段缺失/空串/'default' → 补充 kimi-k2.5
// - settings.json 存在且 model 字段有合法值 → 不动，完全尊重用户配置
```

> ⚠️ **重要**：如果你没有主动在 CatPaw 里配置 Claude 模型，**默认跑的是 Kimi，不是 Claude！**

### 2.3 System Prompt 注入

```javascript
// CatPaw 使用 preset 模式，继承官方内置规则后追加 VSCode 上下文
systemPrompt: {
    type: "preset",
    preset: "claude_code",  // 官方 Claude Code 的完整内置规则
    append: Cit             // CatPaw 追加的 VSCode 环境说明（约200字）
}

// Cit 的完整内容：
var Cit = `
# VSCode Extension Context
You are running inside a VSCode native extension environment.

## Code References in Text
IMPORTANT: When referencing files or code locations, use markdown link syntax...
- For files: [filename.ts](src/filename.ts)
- For specific lines: [filename.ts:42](src/filename.ts#L42)
...
`;
```

### 2.4 版本关系

| | 版本 |
|--|--|
| Claude Code 官方 | 2.1.168 |
| CatPaw 内置 Claude Code | 2.1.163（小版本跟随官方）|

---

## 3. CatPaw 代理层（mcli.sankuai.com）推断做的事

> 服务端黑盒，以下基于架构逻辑推断，不是直接观测结果

| 功能 | 推断依据 |
|------|---------|
| **身份鉴权** | 客户端传 mis 工号，代理层转换为合法 Anthropic Token |
| **模型路由** | 支持 kimi-k2.5、claude 等，代理层做不同厂商 API 适配 |
| **审计日志** | `x-working-dir`、`x-repo-url`、`x-branch` Headers 用于记录"谁在哪问了什么" |
| **费用管控** | 按 mis 工号统计 Token 消耗，做内部计费 |
| **合规过滤** | 对请求/响应做内容扫描，满足美团内部数据合规要求 |
| **不额外注入 SP** | Claude 的 `system` 字段已由客户端 Harness 完整构建，代理层通常只透传 |

---

## 4. 为什么 Claude Code 被认为是最好的编程 Agent

### 4.1 Agentic Loop 成熟度（核心差距）

**工具调用的精确性**：
- Claude Code 的工具设计极度克制，每个工具做一件事（Read/Write/Bash/Grep/Glob）
- 工具调用失败时的 retry 策略、错误处理经过大量打磨
- **不会"幻觉调用"不存在的工具**

**任务完成度判断**：
- Claude Code 的退出条件：`stop_reason === "end_turn"`（Claude 自己说完成了）
- 国内工具常见：超过固定轮数输出"我完成了"（哪怕没完成）

**错误恢复能力**：
- bash 命令报错 → 读错误日志 → 分析原因 → 修复 → 重试
- 这个 observe→think→act 循环深度可达 30-50 轮

### 4.2 Context Compaction（详见专题笔记）

- 自动触发：上下文接近限制时，调用 Claude 对历史做摘要
- 摘要替换历史：200K 压缩到 ~4K，循环无缝继续
- 国内多数工具：直接截断历史或让用户手动清理

### 4.3 精细权限系统

```
四种权限模式（从源码提取）：
- default：遇到危险操作询问用户
- acceptEdits：自动接受文件修改，但 bash 仍需确认
- plan：只读模式，任何修改都拒绝
- bypassPermissions：完全信任（仅限隔离环境）
```

国内工具通常只有"开/关"两档。

### 4.4 开源透明度建立信任

- System Prompt、工具定义、Hooks 格式相对透明（可从二进制逆向）
- 社区形成了大量 CLAUDE.md 模板、Skills 市场
- CatPaw 本身就是在 Claude Code 开放生态上建立的

---

## 5. CatPaw 的定位与价值

**CatPaw 是"接入层 + 皮肤"**，在 Claude Code 基础上做了正确的企业级定制：

| 定制内容 | 价值 |
|---------|------|
| 接入美团 SSO 认证 | 不需要个人 API Key，用公司身份统一管理 |
| 支持内部模型（Kimi 等） | 降低成本，满足内部合规要求 |
| 注入 VSCode 环境上下文 | 文件引用自动变为可点击链接 |
| 接入大象、Raptor 等内部工具 | 通过 MCP/Skills 与公司基础设施打通 |
| 美团代理层审计 | 满足企业合规要求 |

**天花板由 Claude Code 官方 Harness 版本决定**，CatPaw 的核心 Agentic Loop、Compaction、权限系统完全继承自 Claude Code。

---

*参考：CatPaw extension.js 2.1.163 + Claude Code 2.1.168 二进制 strings 提取*

