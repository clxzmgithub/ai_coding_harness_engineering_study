# 🤖 AI Coding & Harness Engineering & 大模型应用 & Agent 应用开发 学习项目

<div align="center">

![Status](https://img.shields.io/badge/状态-持续更新中-brightgreen)
![License](https://img.shields.io/badge/license-MIT-blue)
![AI](https://img.shields.io/badge/AI-Coding%20%26%20Agent-purple)

</div>

---

## 📌 项目定位

本项目是一个面向 **AI 工程提效** 的系统性学习仓库，**不是真实的生产项目**。聚焦于 AI 编程方法论、上下文工程、Harness 工程理论、主流 AI Coding 工具链（Claude Code / Cursor / Codex 等）、大模型能力对比及 Agent 应用开发等前沿方向，以动手实践、工具对比、案例记录为主要学习载体，逐步构建在 AI 时代高效工程的完整知识体系。

**核心学习方向（8 大板块）：**
`AI 编程方法论` · `上下文工程` · `Harness 工程理论` · `AI Coding 工具实践` · `Agent 模式与编排` · `国内外 AI 动态` · `主流大模型` · `Claude Code 源码学习`

> ⚠️ **关联项目分工说明**
>
> 本人维护三个相互补充、各有侧重的学习仓库，共同构成完整的知识体系：
>
> | 项目 | 定位 | 核心聚焦 |
> |------|:----:|---------|
> | [`java_study`](../java_study) | 🔵 基础理论 | Java 语言核心、数据结构与算法、JVM 原理、操作系统、计算机网络——**打地基** |
> | [`java_fullstack_ai_agent_study`](../java_fullstack_ai_agent_study) | 🟠 工程实践 | Spring 生态、分布式架构、大数据、存储中间件、风控爬虫、数据分析、多语言、AI/Agent——**做系统** |
> | **本项目**（`ai_coding_harness_engineering_study`） | 🟣 AI 工程 | AI 编程方法论、上下文工程、Harness 理论、AI Coding 工具链、大模型与 Agent 开发——**用 AI 提效** |

在 AI 重塑软件工程的时代背景下，本仓库聚焦以下核心方向：

| # | 方向 | 核心关注点 |
|---|------|-----------|
| 1 | **AI 编程（AI Coding）** | 方法论、Prompt Engineering、Vibe Coding、人机协作工作流 |
| 2 | **上下文工程** | 上下文窗口、压缩、Tools、MCP、Skill、Command、Hooks、安全 |
| 3 | **Harness 工程理论** | 测试线束、工程质量保障、AI 代码验证体系、CI/CD 与 AI 融合 |
| 4 | **AI Coding 工具实践** | Claude Code、Codex、Cursor、CatPaw 等深度使用与最佳实践 |
| 5 | **Agent 模式与编排** | 各类 Agent 模式、Multi-Agent 编排、主流框架实战 |
| 6 | **国内外 AI 动态** | 前沿论文、公司动态、评测榜单、政策观察 |
| 7 | **主流大模型** | GPT / Claude / Gemini / DeepSeek 等能力深度对比 |
| 8 | **Claude Code 源码学习** | TypeScript 源码阅读、核心模块分析、架构设计、扩展开发 |

---

## 🎯 学习目标

- [ ] 建立 AI 辅助编程的完整认知框架，掌握 Vibe Coding 方法论
- [ ] 深入理解上下文工程：窗口管理、压缩策略、Tools / MCP / Hooks 机制
- [ ] 理解 Harness 工程理论在 AI 时代的实践意义
- [ ] 深度掌握 Claude Code、Codex CLI、Cursor 等主流 AI 编程工具
- [ ] 掌握各类 Agent 模式与 Multi-Agent 编排设计
- [ ] 形成国内外前沿 AI 动态的持续追踪机制
- [ ] 积累可复用的学习笔记、代码示例与最佳实践
- [ ] 深入阅读 Claude Code 源码，理解 AI Coding 工具的内部实现机制

---

## 📚 学习内容概览

<details>
<summary><b>一、AI 编程（AI Coding）</b></summary>

- AI 辅助编程的核心概念与方法论
- **Prompt Engineering** 在编程场景中的应用
  - 系统提示（System Prompt）设计
  - Few-shot / Chain-of-Thought 编程提示技巧
  - 角色扮演式提示（Persona Prompting）
- **Vibe Coding**：氛围编程的理念、边界与最佳实践
- AI 与人类协作开发的工作流设计
- AI 编程的质量保障与 Code Review 策略
- AI 生成代码的可信度评估与风险控制

</details>

<details>
<summary><b>二、上下文工程（Context Engineering）</b></summary>

### 📐 上下文窗口（Context Window）
- 上下文窗口的概念、限制与演进（4K → 1M tokens）
- 不同模型上下文长度对比与实际可用长度分析
- 长上下文的"迷失中间"（Lost in the Middle）问题
- 有效利用上下文窗口的策略与技巧

### 🗜️ 上下文压缩（Context Compression）
- 对话历史裁剪与摘要策略
- RAG（检索增强生成）作为上下文扩展手段
- 滑动窗口 / 重要性排序 / 动态压缩方法
- 工具层面的上下文管理（如 Claude Code 的自动压缩）

### 🔧 Tools（工具调用）
- Function Calling / Tool Use 基本原理
- 工具定义规范（JSON Schema）
- 工具调用的循环机制（Tool Use Loop）
- 并行工具调用 vs. 顺序工具调用
- 工具调用的错误处理与重试策略
- 主流模型工具调用能力对比

### 🔌 MCP（Model Context Protocol）
- MCP 协议概念、设计理念与架构
- MCP Server / Client 工作机制
- MCP 与传统 Function Calling 的对比与优势
- 常用 MCP Server 生态（文件系统、数据库、浏览器、代码执行等）
- 自定义 MCP Server 开发实践
- MCP 在各 AI 编程工具中的集成现状（Claude Code、Cursor、CatPaw 等）

### 🎯 Skill（技能）
- Skill 的概念：可复用的提示 + 工具 + 知识单元
- Skill 设计原则：单一职责、可组合、可测试
- Skill 的描述与触发机制（Trigger 设计）
- Skill 与 Tool 的区别与协作关系
- Skill 注册、管理与版本控制
- CatPaw Skill 体系实践案例

### 💬 Command（命令）
- 自定义命令（Slash Command）的设计与注册
- Command 与 Prompt Template 的关系
- 命令参数化与动态插值
- 在 Claude Code、Cursor、CatPaw 中的 Command 实践

### 🪝 Hooks（钩子）
- Hooks 机制：事件驱动的上下文注入
- Pre-tool / Post-tool Hook 的应用场景
- 会话级别 Hook vs. 全局 Hook
- Hook 用于自动注入上下文、日志记录、结果校验
- Claude Code Hooks 配置实践
- 安全性考量：Hook 的权限控制

### 🔒 安全（Security）
- AI 编程中的安全风险分类
  - Prompt Injection（提示注入）攻击原理与防御
  - 间接注入（Indirect Prompt Injection）
  - 工具调用权限越界风险
  - 数据泄露与敏感信息处理
- AI Agent 的权限最小化原则
- 沙箱隔离与代码执行安全
- Human-in-the-loop 审查机制
- AI 生成代码的安全审查清单

</details>

<details>
<summary><b>三、Harness 工程理论</b></summary>

- **Harness 测试线束**（Test Harness）基本概念与设计思想
- 测试线束的组成：驱动程序（Driver）、桩（Stub）、Mock、Spy
- 工程测试体系的构建：单元测试 → 集成测试 → E2E 测试
- **AI 时代的 Harness 新内涵**
  - AI 生成代码的自动化验证体系
  - 基于 LLM 的测试用例生成
  - AI Coding 工具的质量评估 Harness
  - Benchmark 驱动的 AI 能力评测体系（SWE-bench、HumanEval 等）
- 持续集成 / 持续交付（CI/CD）与 AI 的结合
  - AI 辅助 Code Review 在 CI 中的集成
  - 自动化测试生成流水线

</details>

<details>
<summary><b>四、AI Coding 工具深度实践</b></summary>

### 🖥️ Claude Code（重点）
- 安装、配置与基本工作流
- **CLAUDE.md** 项目上下文文件设计最佳实践
- **Slash Commands** 自定义与管理（`/project:`、`/user:` 命名空间）
- **Hooks** 配置：PreToolUse / PostToolUse / Stop / Notification
- **MCP** 服务集成与自定义 MCP Server
- **Sub-agent** 并行任务模式（Task 工具）
- 权限模型：`--allowedTools` / `--dangerouslySkipPermissions`
- 上下文压缩与长会话管理（`/compact`）
- 与 Git 工作流的深度集成
- 多模型切换（`--model` 参数）
- Headless / CI 模式（`-p` 非交互模式）
- 费用管控与 Token 使用优化

### 💻 OpenAI Codex CLI
- 安装与基本使用
- Approval 模式：`suggest` / `auto-edit` / `full-auto`
- `AGENTS.md` 项目说明文件规范
- Codex 与 Claude Code 的能力对比
- 多模型支持（`--model` 参数）
- 沙箱执行环境配置

### 🎯 Cursor
- Cursor 核心功能：Tab 补全、Chat、Composer、Agent
- **Rules for AI**（`.cursorrules` / `cursor rules`）配置
- **Agent 模式**：自主多步骤代码修改
- **Context 管理**：`@file`、`@folder`、`@web`、`@docs`、`@codebase`
- MCP 服务集成
- Notepads（可复用上下文片段）
- 模型切换与成本控制

### 🌊 Windsurf
- Cascade 流式编辑核心理念
- Flows 工作流与 Agent 任务模式
- 与 Cursor 的差异化对比

### 🐙 GitHub Copilot
- Copilot Chat / Inline / Agent 模式
- 自定义指令（Custom Instructions）
- Copilot Extensions 生态
- 企业版安全合规特性

### 🐾 CatPaw（美团内部）
- CatPaw Skill 体系：技能开发与管理
- MCP 服务集成实践
- 内部工具链集成（大象、学城、Raptor 等）
- 与公司工程规范的结合

### 🛠️ 其他工具
- **Gemini CLI**：Google 官方终端 AI，百万 token 上下文
- **Devin**：自主 AI 软件工程师，全流程代码开发
- **OpenHands**：开源自主编程代理
- **Aider**：终端 Git 感知代码助手
- **Continue**：开源 IDE 插件，支持自托管模型

</details>

<details>
<summary><b>五、Agent 模式与编排</b></summary>

### 🧠 Agent 基础模式
| 模式 | 描述 | 适用场景 |
|------|------|---------|
| **ReAct** | 推理（Reasoning）+ 行动（Acting）交替循环 | 通用任务、工具调用 |
| **Plan-and-Execute** | 先规划完整步骤，再逐步执行 | 复杂多步骤任务 |
| **Reflection** | 自我反思与输出修正 | 代码生成、文档撰写 |
| **Reflexion** | 带记忆的自我反思强化学习 | 持续改进任务 |
| **Self-Ask** | 自我提问分解复杂问题 | 推理密集型任务 |
| **Tree of Thoughts** | 树形搜索探索多条推理路径 | 需要回溯的决策问题 |

### 🤝 Multi-Agent 模式
- **Orchestrator-Worker**：主控 Agent 分发子任务给专用 Worker
- **Supervisor**：监督者模式，负责质量把控与路由
- **Swarm**：去中心化协作，Agent 间直接交接控制权
- **Pipeline**：线性流水线，每个 Agent 处理特定阶段
- **Debate**：多 Agent 辩论求共识，提升回答质量
- **Parallel Fan-out**：并行拆解任务，汇聚结果

### 🗺️ Agent 编排框架
| 框架 | 特点 | 适合场景 |
|------|------|---------|
| **LangGraph** | 图结构状态机，条件边、循环支持 | 复杂有状态工作流 |
| **AutoGen** | 对话式多 Agent，微软出品 | 企业级多角色协作 |
| **CrewAI** | 角色 + 任务 + 工具三元组 | 快速搭建多 Agent 团队 |
| **LangChain AgentExecutor** | 成熟生态，工具链丰富 | 通用 Agent 任务 |
| **Swarm（OpenAI）** | 轻量级 Agent 交接协议 | 简单多 Agent 路由 |
| **Pydantic AI** | 类型安全，结构化输出 | 生产级数据处理 |

### ⚙️ Agent 核心能力
- **记忆管理**：短期记忆（对话历史）/ 长期记忆（向量数据库）/ 实体记忆
- **工具调用**：内置工具 vs. MCP 工具 vs. 自定义工具
- **状态管理**：有状态 vs. 无状态 Agent 设计
- **中断与恢复**：Human-in-the-loop 审批节点
- **可观测性**：LangSmith / LangFuse / OpenTelemetry 集成
- **评估体系**：Agent 行为评测、Trajectory 评估

</details>

<details>
<summary><b>六、国内外 AI 动态</b></summary>

- 前沿论文与技术博客追踪
- 国内外主流 AI 公司动态
  - 海外：OpenAI、Anthropic、Google DeepMind、Meta AI、Mistral
  - 国内：DeepSeek、字节（豆包）、阿里（通义）、百度（文心）、智谱、月之暗面
- 大模型能力评测与排行榜解读
  - LMSYS Chatbot Arena、SWE-bench、HumanEval、MMLU
  - LiveCodeBench、BigCodeBench
- AI 政策法规与行业生态观察
- AI 基础设施动态（GPU 算力、推理优化、量化技术）

</details>

<details>
<summary><b>七、主流大模型学习</b></summary>

- **OpenAI 系列**：GPT-4o、o1、o3、o3-mini、o4-mini 等
- **Anthropic 系列**：Claude 3 Haiku / Sonnet / Opus、Claude 4.x
- **Google 系列**：Gemini 1.5 Pro/Flash、Gemini 2.0、Gemini 2.5 Pro
- **国内模型**：DeepSeek V3 / R1、Qwen3（通义千问）、豆包、文心 4.0 等
- **开源模型**：LLaMA 3.x、Mistral、Phi-4、Gemma 等
- 多维度对比
  - 推理能力、代码能力、指令遵循、多模态
  - 上下文长度、价格、速度、API 稳定性
  - 工具调用能力、结构化输出质量

</details>

<details>
<summary><b>八、Claude Code 源码学习</b></summary>

### 🔍 源码概览
- 仓库结构与技术栈（TypeScript / Node.js）
- 核心模块划分与依赖关系
- 构建、调试与本地运行方式

### 🧩 核心模块分析
- **CLI 入口**：命令解析、参数处理、模式分发
- **对话引擎**：消息循环、流式输出、上下文管理
- **工具系统**：内置工具（Bash / FileEdit / Read 等）注册与执行机制
- **MCP Client**：MCP 协议客户端实现、Server 连接与工具发现
- **Hooks 系统**：Hook 事件定义、触发时机、配置加载
- **权限系统**：allowedTools 过滤、用户审批流程、dangerouslySkipPermissions
- **上下文压缩**：自动摘要触发条件、压缩策略实现
- **Sub-agent（Task 工具）**：并行子任务的创建与结果汇聚

### 🏗️ 架构设计洞察
- REPL 交互模型 vs. Headless 非交互模型的实现差异
- 流式 SSE / Anthropic SDK 的集成方式
- 多模型适配（`--model` 切换机制）
- 配置文件加载优先级（全局 / 项目级 `CLAUDE.md` 合并策略）
- 错误处理与重试机制

### 🔌 扩展开发
- 自定义内置工具的开发与注册
- 自定义 MCP Server 与 Claude Code 的集成
- 基于源码理解优化 CLAUDE.md 与 Hooks 配置

</details>

---

## 🗂️ 目录结构

```
ai_coding_harness_engineering_study/
├── README.md                          # 项目说明（本文件）
│
├── 01-ai-coding/                      # AI 编程方法论与实践
│   ├── prompt-engineering/            #   Prompt 工程技巧
│   ├── vibe-coding/                   #   Vibe Coding 理念与实践
│   └── workflow/                      #   人机协作工作流设计
│
├── 02-context-engineering/            # 上下文工程（重点）
│   ├── context-window/                #   上下文窗口管理
│   ├── context-compression/           #   上下文压缩策略
│   ├── tools/                         #   工具调用（Function Calling）
│   ├── mcp/                           #   MCP 协议与实践
│   ├── skill/                         #   Skill 体系设计
│   ├── command/                       #   自定义命令
│   ├── hooks/                         #   Hooks 机制
│   └── security/                      #   AI 安全（Prompt Injection 等）
│
├── 03-harness/                        # Harness 工程理论
│   ├── test-harness-basics/           #   测试线束基础
│   ├── ai-code-validation/            #   AI 生成代码验证体系
│   └── benchmark/                     #   AI 能力评测 Benchmark
│
├── 04-tools/                          # AI Coding 工具深度实践
│   ├── claude-code/                   #   Claude Code（重点）
│   │   ├── claude-md/                 #     CLAUDE.md 设计实践
│   │   ├── hooks/                     #     Hooks 配置
│   │   ├── mcp/                       #     MCP 集成
│   │   └── commands/                  #     自定义命令
│   ├── codex-cli/                     #   OpenAI Codex CLI
│   ├── cursor/                        #   Cursor 深度使用
│   │   ├── rules/                     #     Rules for AI 配置
│   │   └── agent-mode/                #     Agent 模式实践
│   ├── windsurf/                      #   Windsurf
│   ├── copilot/                       #   GitHub Copilot
│   ├── catpaw/                        #   CatPaw（美团）
│   ├── gemini-cli/                    #   Gemini CLI
│   └── others/                        #   Aider、Continue、OpenHands 等
│
├── 05-agent/                          # Agent 模式与编排
│   ├── patterns/                      #   Agent 设计模式
│   │   ├── react/                     #     ReAct 模式
│   │   ├── plan-execute/              #     Plan-and-Execute
│   │   ├── reflection/                #     Reflection / Reflexion
│   │   └── multi-agent/               #     Multi-Agent 模式
│   ├── orchestration/                 #   Agent 编排
│   │   ├── langgraph/                 #     LangGraph 实践
│   │   ├── autogen/                   #     AutoGen 实践
│   │   ├── crewai/                    #     CrewAI 实践
│   │   └── swarm/                     #     Swarm 模式
│   └── observability/                 #   Agent 可观测性
│
├── 06-ai-news/                        # 国内外 AI 动态追踪
│
├── 07-models/                         # 主流大模型学习笔记
│   ├── openai/                        #   GPT / o 系列
│   ├── anthropic/                     #   Claude 系列
│   ├── google/                        #   Gemini 系列
│   ├── deepseek/                      #   DeepSeek 系列
│   ├── domestic/                      #   国内其他模型
│   └── comparison/                    #   多模型横向对比
│
├── 08-tools-ecosystem/                # AI 工具生态与选型
│   ├── landscape/                     #   工具全景图
│   └── selection-guide/               #   选型指南
│
└── 09-claude-code-source/             # Claude Code 源码学习（重点）
    ├── overview/                      #   仓库结构与技术栈概览
    ├── cli-entry/                     #   CLI 入口与命令解析
    ├── conversation-engine/           #   对话引擎与消息循环
    ├── tool-system/                   #   工具系统（内置工具注册与执行）
    ├── mcp-client/                    #   MCP Client 实现
    ├── hooks-system/                  #   Hooks 系统
    ├── permission-system/             #   权限系统
    ├── context-compression/           #   上下文压缩实现
    ├── sub-agent/                     #   Sub-agent（Task 工具）
    └── architecture-insights/        #   架构设计洞察与扩展开发
```

> 📝 目录结构会随学习进展持续迭代，当前为初步规划。

---

## 🔧 工具与资源速查

### AI Coding 工具全景

| 工具 | 类型 | 底层模型 | 核心特点 |
|------|------|---------|---------|
| [Claude Code](https://docs.anthropic.com/claude-code) | CLI Agent | Claude | Hooks、MCP、Sub-agent、CLAUDE.md |
| [Codex CLI](https://github.com/openai/codex) | CLI Agent | GPT / o 系列 | AGENTS.md、沙箱执行、多审批模式 |
| [Cursor](https://cursor.com) | AI-first IDE | 多模型 | Rules、Agent 模式、@Context 引用 |
| [Windsurf](https://windsurf.com) | AI-first IDE | 多模型 | Cascade 流式编辑、Flows 工作流 |
| [GitHub Copilot](https://github.com/features/copilot) | IDE 插件 | GPT-4o | 深度集成 VSCode / JetBrains，企业级合规 |
| [CatPaw](https://catpaw.sankuai.com) | IDE 插件 | 多模型 | 美团内部，Skill 体系，内网工具集成 |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli) | CLI | Gemini 2.5 | 百万 token 上下文，Google 工具集成 |
| [Aider](https://aider.chat) | CLI | 多模型 | Git 感知，支持本地模型 |
| [Continue](https://continue.dev) | IDE 插件 | 多模型 | 开源，支持自托管模型 |
| [Devin](https://devin.ai) | 自主代理 | 专有模型 | 全流程自主开发，SWE-bench 领先 |
| [OpenHands](https://github.com/All-Hands-AI/OpenHands) | 自主代理 | 多模型 | 开源 Devin 替代，Docker 沙箱 |

### MCP 生态速查

| MCP Server | 功能 | 场景 |
|-----------|------|------|
| `filesystem` | 文件读写操作 | 代码分析、文档管理 |
| `github` | GitHub API 操作 | PR 创建、Issue 管理 |
| `brave-search` | 网页搜索 | 实时信息获取 |
| `puppeteer` | 浏览器自动化 | Web 测试、截图 |
| `sqlite` / `postgres` | 数据库操作 | 数据查询与分析 |
| `memory` | 知识图谱记忆 | 跨会话信息持久化 |
| `sequential-thinking` | 结构化推理 | 复杂问题分解 |

### 主流大模型

| 模型 | 厂商 | 上下文 | 代码能力 |
|------|------|--------|---------|
| GPT-4o / o3 / o4-mini | OpenAI | 128K | ⭐⭐⭐⭐⭐ |
| Claude Sonnet 4 / Opus 4 | Anthropic | 200K | ⭐⭐⭐⭐⭐ |
| Gemini 2.5 Pro | Google | 1M | ⭐⭐⭐⭐⭐ |
| DeepSeek V3 / R1 | DeepSeek | 128K | ⭐⭐⭐⭐⭐ |
| Qwen3-235B | Alibaba | 128K | ⭐⭐⭐⭐ |

### Agent 开发框架

| 框架 | 维护方 | 特点 |
|------|--------|------|
| [LangGraph](https://langchain-ai.github.io/langgraph) | LangChain Inc. | 图结构状态机，复杂流程编排首选 |
| [AutoGen](https://microsoft.github.io/autogen) | Microsoft | 多 Agent 对话，企业级场景 |
| [CrewAI](https://crewai.com) | CrewAI | 角色 + 任务模式，快速上手 |
| [LangChain](https://langchain.com) | LangChain Inc. | 生态最丰富，工具链完整 |
| [LlamaIndex](https://llamaindex.ai) | LlamaIndex | RAG 与知识库场景见长 |
| [Swarm](https://github.com/openai/swarm) | OpenAI | 轻量级 Agent 交接，实验性 |
| [Pydantic AI](https://ai.pydantic.dev) | Pydantic | 类型安全，生产级结构化输出 |

---

## 📊 学习进度

> 各模块学习状态持续更新。状态说明：🔜 待开始 ｜ 🔥 进行中 ｜ ✅ 已完成 ｜ ⏸️ 暂停

| 主模块 | 子模块 / 知识点 | 状态 | 开始时间 | 备注 |
|--------|----------------|:----:|---------|------|
| **▶ 一、AI 编程（AI Coding）** | | | | |
| | AI 辅助编程核心概念与方法论 | 🔜 待开始 | - | |
| | Prompt Engineering（System Prompt / Few-shot / CoT）| 🔜 待开始 | - | |
| | Vibe Coding 理念与边界 | 🔜 待开始 | - | |
| | 人机协作工作流设计 | 🔜 待开始 | - | |
| | AI 编程质量保障与 Code Review 策略 | 🔜 待开始 | - | |
| **▶ 二、上下文工程（Context Engineering）** | | | | |
| | 上下文窗口（Context Window）原理与策略 | 🔜 待开始 | - | |
| | 上下文压缩（裁剪 / 摘要 / RAG / 动态压缩）| 🔜 待开始 | - | |
| | Tools / Function Calling 机制 | 🔜 待开始 | - | |
| | MCP 协议原理与实践 | 🔜 待开始 | - | |
| | Skill 体系设计（概念 / 触发 / 管理）| 🔜 待开始 | - | |
| | Command（Slash Command）设计与实践 | 🔜 待开始 | - | |
| | Hooks 机制（Pre/Post-tool / 事件驱动）| 🔜 待开始 | - | |
| | AI 安全（Prompt Injection / 权限控制 / 沙箱）| 🔜 待开始 | - | |
| **▶ 三、Harness 工程理论** | | | | |
| | Harness 测试线束基本概念（Driver / Stub / Mock）| 🔜 待开始 | - | |
| | AI 生成代码的自动化验证体系 | 🔜 待开始 | - | |
| | 基于 LLM 的测试用例生成 | 🔜 待开始 | - | |
| | Benchmark 评测体系（SWE-bench / HumanEval）| 🔜 待开始 | - | |
| | CI/CD 与 AI 结合（AI Code Review 流水线）| 🔜 待开始 | - | |
| **▶ 四、AI Coding 工具深度实践** | | | | |
| | Claude Code — 基础工作流 & CLAUDE.md | 🔜 待开始 | - | 🔑 重点 |
| | Claude Code — Hooks 配置实践 | 🔜 待开始 | - | 🔑 重点 |
| | Claude Code — MCP 集成 & 自定义 Server | 🔜 待开始 | - | 🔑 重点 |
| | Claude Code — Sub-agent / Headless / CI 模式 | 🔜 待开始 | - | |
| | Claude Code — 权限模型 & Token 优化 | 🔜 待开始 | - | |
| | OpenAI Codex CLI — 安装 & 审批模式 & AGENTS.md | 🔜 待开始 | - | |
| | Cursor — Tab / Chat / Agent 模式 | 🔜 待开始 | - | 🔑 重点 |
| | Cursor — Rules for AI & @Context 引用 | 🔜 待开始 | - | 🔑 重点 |
| | Cursor — MCP 集成 & Notepads | 🔜 待开始 | - | |
| | Windsurf — Cascade & Flows 工作流 | 🔜 待开始 | - | |
| | GitHub Copilot — Chat / Agent / Extensions | 🔜 待开始 | - | |
| | CatPaw — Skill 体系 & 内网工具集成 | 🔜 待开始 | - | |
| | Gemini CLI — 百万 token 上下文实践 | 🔜 待开始 | - | |
| | 其他工具（Aider / Continue / OpenHands）| 🔜 待开始 | - | |
| **▶ 五、Agent 模式与编排** | | | | |
| | Agent 基础模式（ReAct / Plan-Execute / Reflection）| 🔜 待开始 | - | |
| | Tree of Thoughts / Self-Ask / Reflexion | 🔜 待开始 | - | |
| | Multi-Agent 模式（Orchestrator / Supervisor / Swarm）| 🔜 待开始 | - | |
| | Multi-Agent 模式（Pipeline / Debate / Parallel Fan-out）| 🔜 待开始 | - | |
| | LangGraph 图结构编排实践 | 🔜 待开始 | - | |
| | AutoGen 多 Agent 对话实践 | 🔜 待开始 | - | |
| | CrewAI 角色任务编排实践 | 🔜 待开始 | - | |
| | Agent 记忆管理（短期 / 长期 / 实体记忆）| 🔜 待开始 | - | |
| | Agent 可观测性（LangSmith / LangFuse）| 🔜 待开始 | - | |
| | Agent 评估（Trajectory 评估体系）| 🔜 待开始 | - | |
| **▶ 六、国内外 AI 动态** | | | | |
| | 前沿论文 & 技术博客追踪 | 🔜 待开始 | - | 持续更新 |
| | 海外 AI 公司动态（OpenAI / Anthropic / Google / Meta）| 🔜 待开始 | - | 持续更新 |
| | 国内 AI 公司动态（DeepSeek / 字节 / 阿里 / 智谱等）| 🔜 待开始 | - | 持续更新 |
| | 评测榜单解读（LMSYS / SWE-bench / LiveCodeBench）| 🔜 待开始 | - | |
| | AI 政策法规 & 行业生态观察 | 🔜 待开始 | - | |
| **▶ 七、主流大模型学习** | | | | |
| | OpenAI 系列（GPT-4o / o1 / o3 / o4 系列）| 🔜 待开始 | - | |
| | Anthropic Claude 系列（3.x / 4.x）| 🔜 待开始 | - | |
| | Google Gemini 系列（1.5 / 2.0 / 2.5）| 🔜 待开始 | - | |
| | DeepSeek 系列（V3 / R1）| 🔜 待开始 | - | |
| | 国内其他模型（Qwen3 / 豆包 / 文心等）| 🔜 待开始 | - | |
| | 开源模型（LLaMA / Mistral / Phi / Gemma）| 🔜 待开始 | - | |
| | 多模型横向对比（代码 / 推理 / 多模态 / 价格）| 🔜 待开始 | - | |
| **▶ 八、Claude Code 源码学习** | | | | |
| | 仓库结构与技术栈概览（TypeScript / Node.js）| 🔜 待开始 | - | 🔑 重点 |
| | CLI 入口、命令解析、模式分发 | 🔜 待开始 | - | |
| | 对话引擎：消息循环 & 流式输出 & 上下文管理 | 🔜 待开始 | - | 🔑 重点 |
| | 工具系统：内置工具注册与执行机制 | 🔜 待开始 | - | 🔑 重点 |
| | MCP Client 实现与 Server 工具发现 | 🔜 待开始 | - | 🔑 重点 |
| | Hooks 系统：事件定义 & 触发时机 & 配置加载 | 🔜 待开始 | - | 🔑 重点 |
| | 权限系统：allowedTools 过滤 & 审批流程 | 🔜 待开始 | - | |
| | 上下文压缩：触发条件与压缩策略实现 | 🔜 待开始 | - | |
| | Sub-agent（Task 工具）并行子任务机制 | 🔜 待开始 | - | |
| | 架构设计洞察（REPL vs Headless / 多模型适配）| 🔜 待开始 | - | |
| | 扩展开发：自定义工具 & MCP Server 集成 | 🔜 待开始 | - | |

---

## 🔗 关联项目

> 三个仓库相互独立、各有侧重，合并构成从基础理论 → 工程实践 → AI 提效的完整学习闭环。

| 项目 | 定位 | 核心内容 | 关键词 |
|------|:----:|---------|-------|
| [`java_study`](../java_study) | 🔵 基础理论 | Java 语言核心、数据结构与算法、JVM 原理、操作系统、计算机网络 | 打地基 |
| [`java_fullstack_ai_agent_study`](../java_fullstack_ai_agent_study) | 🟠 工程实践 | Spring 生态、分布式架构、大数据生态、存储中间件、风控爬虫、数据分析、多语言、前端、AI/Agent 工程化 | 做系统 |
| **本项目**（`ai_coding_harness_engineering_study`） | 🟣 AI 工程 | AI 编程方法论、上下文工程、Harness 理论、AI Coding 工具链（Claude Code / Cursor / Codex）、大模型对比、Agent 开发 | 用 AI 提效 |

---

## 📅 更新记录

| 日期 | 版本 | 内容 |
|------|------|------|
| 2026-05-23 | v0.1.0 | 项目初始化，创建 README |
| 2026-05-23 | v0.2.0 | 补充上下文工程、Agent 模式、AI Coding 工具深度实践等内容 |
| 2026-05-23 | v0.3.0 | 更新标题、学习进度改为主子分层大表格 |
| 2026-05-23 | v0.4.0 | 优化项目定位、关联项目分工说明、关联项目章节 |
| 2026-05-23 | v0.5.0 | 新增「Claude Code 源码学习」方向（第八大板块） |

---

<div align="center">
  <sub>持续更新中 🚀 · AI 时代的工程师，既要懂得驾驭 AI 工具，也要保持对工程本质的深刻理解。</sub>
</div>

