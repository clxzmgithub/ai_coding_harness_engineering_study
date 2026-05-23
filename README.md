# 🤖 AI Coding & Harness Engineering Study

> **个人学习型项目** · 非生产工程
>
> 系统学习 AI 编程、Harness 工程理论、AI Coding 工具链、大模型应用及 Agent 开发的知识沉淀仓库。

---

## 🗺️ 项目定位

在 AI 重塑软件工程的时代背景下，本仓库作为个人学习档案，聚焦以下七个核心方向：

| # | 方向 | 核心关注点 |
|---|------|-----------|
| 1 | **AI 编程（AI Coding）** | 方法论、Prompt Engineering、人机协作工作流 |
| 2 | **Harness 工程理论** | 测试线束、工程质量保障、CI/CD 与 AI 融合 |
| 3 | **AI Coding 工具** | 主流工具横向对比、使用技巧与最佳实践 |
| 4 | **国内外 AI 动态** | 前沿论文、公司动态、评测榜单、政策观察 |
| 5 | **主流大模型** | GPT / Claude / Gemini / DeepSeek 等能力深度对比 |
| 6 | **AI 工具生态** | 工具分类选型、Tool Chain 组合策略 |
| 7 | **Agent 开发** | ReAct / Multi-Agent 架构、主流框架、实战案例 |

---

## 🎯 学习目标

- [ ] 建立 AI 辅助编程的完整认知框架
- [ ] 掌握主流 AI Coding 工具的使用技巧与应用场景
- [ ] 理解 Harness 工程理论在 AI 时代的实践意义
- [ ] 形成国内外前沿 AI 动态的持续追踪机制
- [ ] 具备独立设计与开发 AI Agent 的能力
- [ ] 积累可复用的学习笔记、代码示例与最佳实践

---

## 📚 学习内容概览

<details>
<summary><b>一、AI 编程（AI Coding）</b></summary>

- AI 辅助编程的核心概念与方法论
- Prompt Engineering 在编程场景中的应用
- AI 与人类协作开发的工作流设计
- AI 编程的质量保障与 Code Review 策略
- Vibe Coding vs. 传统开发模式的边界与取舍

</details>

<details>
<summary><b>二、Harness 工程理论</b></summary>

- Harness 测试线束（Test Harness）基本概念
- 工程测试体系的构建与管理
- AI 生成代码的质量评估与验证体系
- AI 时代的工程质量保障新思路
- 持续集成 / 持续交付（CI/CD）与 AI 的结合

</details>

<details>
<summary><b>三、AI Coding 工具学习</b></summary>

- **IDE 插件类**：GitHub Copilot、Cursor、CatPaw、Windsurf 等
- **对话式编程类**：ChatGPT、Claude、Gemini 等
- **代码生成 / 补全**：Codeium、Tabnine 等
- **自主编程代理**：Devin、SWE-agent、OpenHands 等
- **终端 / CLI 工具**：Claude Code、Gemini CLI 等

</details>

<details>
<summary><b>四、国内外 AI 动态</b></summary>

- 前沿论文与技术博客追踪
- 国内外主流 AI 公司动态（OpenAI、Anthropic、Google、字节、百度、阿里、智谱等）
- 大模型能力评测与排行榜解读（LMSYS、SWE-bench 等）
- AI 政策法规与行业生态观察

</details>

<details>
<summary><b>五、主流大模型学习</b></summary>

- **OpenAI 系列**：GPT-4o、o1、o3、o4 等
- **Anthropic 系列**：Claude 3.x / 4.x
- **Google 系列**：Gemini 1.5 / 2.0 / 2.5
- **国内模型**：DeepSeek、Qwen（通义千问）、豆包、文心等
- **开源模型**：LLaMA 3、Mistral、Phi 等
- 多维度对比：推理能力、代码能力、多模态、上下文长度、价格

</details>

<details>
<summary><b>六、AI 编程工具生态</b></summary>

- 工具全景图与分类选型指南
- 各工具核心能力与使用技巧
- 工具组合使用策略（Tool Chain 设计）
- 企业级 AI Coding 落地实践

</details>

<details>
<summary><b>七、Agent 开发</b></summary>

- Agent 基础理论：ReAct、Plan-and-Execute、Reflection、Multi-Agent
- **主流框架**：LangChain、LlamaIndex、AutoGen、CrewAI、LangGraph
- Agent 工具调用（Function Calling / Tool Use / MCP）
- Agent 记忆管理与状态维护
- Agent 评估与可观测性
- 实战案例与代码示例

</details>

---

## 🗂️ 目录结构

```
ai_coding_harness_engineering_study/
├── README.md                     # 项目说明（本文件）
│
├── 01-ai-coding/                 # AI 编程方法论与实践
├── 02-harness/                   # Harness 工程理论
├── 03-tools/                     # AI Coding 工具学习
│   ├── cursor/
│   ├── copilot/
│   ├── catpaw/
│   └── ...
├── 04-ai-news/                   # 国内外 AI 动态追踪
├── 05-models/                    # 主流大模型学习笔记
│   ├── openai/
│   ├── anthropic/
│   ├── google/
│   ├── deepseek/
│   └── ...
├── 06-tools-ecosystem/           # AI 工具生态与选型
└── 07-agent/                     # Agent 开发实践
    ├── theory/
    ├── langchain/
    ├── autogen/
    └── projects/
```

> 📝 目录结构会随学习进展持续迭代，当前为初步规划。

---

## 🔧 工具与资源速查

### AI Coding 工具

| 工具 | 类型 | 特点 |
|------|------|------|
| [GitHub Copilot](https://github.com/features/copilot) | IDE 插件 | GitHub 官方，深度集成 VSCode / JetBrains |
| [Cursor](https://cursor.com) | AI-first IDE | 原生多文件编辑，Agent 模式强 |
| [Windsurf](https://windsurf.com) | AI-first IDE | Codeium 出品，Cascade 流式编辑 |
| [CatPaw](https://catpaw.sankuai.com) | IDE 插件 | 美团内部 AI 编程助手 |
| [Claude Code](https://claude.ai/code) | CLI | Anthropic 官方终端编程代理 |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli) | CLI | Google 官方终端 AI 工具 |
| [Devin](https://devin.ai) | 自主代理 | 首个商业化 AI 软件工程师 |

### 主流大模型

| 模型 | 厂商 | 代码能力 |
|------|------|---------|
| GPT-4o / o3 / o4 | OpenAI | ⭐⭐⭐⭐⭐ |
| Claude Sonnet / Opus | Anthropic | ⭐⭐⭐⭐⭐ |
| Gemini 2.5 Pro | Google | ⭐⭐⭐⭐⭐ |
| DeepSeek V3 / R1 | DeepSeek | ⭐⭐⭐⭐⭐ |
| Qwen3 | Alibaba | ⭐⭐⭐⭐ |

### Agent 开发框架

| 框架 | 维护方 | 特点 |
|------|--------|------|
| [LangChain](https://langchain.com) | LangChain Inc. | 生态最丰富，工具链完整 |
| [LangGraph](https://langchain-ai.github.io/langgraph) | LangChain Inc. | 图结构编排，适合复杂流程 |
| [AutoGen](https://microsoft.github.io/autogen) | Microsoft | 多 Agent 对话，企业级场景 |
| [CrewAI](https://crewai.com) | CrewAI | 角色扮演式多 Agent 协作 |
| [LlamaIndex](https://llamaindex.ai) | LlamaIndex | RAG 与知识库场景见长 |

---

## 📅 更新记录

| 日期 | 版本 | 内容 |
|------|------|------|
| 2026-05-23 | v0.1.0 | 项目初始化，创建 README |

---

> 🚀 *AI 时代的工程师，既要懂得驾驭 AI 工具，也要保持对工程本质的深刻理解。*

