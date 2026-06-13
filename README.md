# 🤖 AI Coding & Harness Engineering 学习项目

<div align="center">

![Status](https://img.shields.io/badge/状态-持续更新中-brightgreen)
![License](https://img.shields.io/badge/license-MIT-blue)
![AI](https://img.shields.io/badge/AI-Coding%20%26%20Harness-purple)

</div>

---

## 📌 项目定位

本项目是一个面向 **AI 工程提效与工具研究** 的系统性学习仓库，**不是真实的生产项目**。聚焦于 AI 编程方法论、AI 工具工程化（MCP / Skill / Hooks）、Harness 工程理论（Eval / LLM-as-Judge）、主流 AI Coding 工具链（Claude Code / Cursor / CatPaw / Codex 等）的深度实践与工具源码研究，以及大模型能力了解等前沿方向，以动手实践、工具对比、案例记录、理论笔记为主要学习载体，逐步构建在 AI 时代高效工程的完整知识体系。

**核心学习方向（8 大板块）：**
`AI 编程方法论` · `AI 工具工程化` · `Harness 工程理论` · `AI Coding 工具实践` · `AI 算法与大模型理论` · `国内外 AI 动态` · `主流大模型` · `Claude Code 源码学习`

> ⚠️ **关联项目分工说明**
>
> 本人维护三个相互补充、各有侧重的学习仓库，共同构成完整的知识体系：
>
> | 项目 | 定位 | 核心聚焦 |
> |------|:----:|---------|
> | [`java_study`](../java_study) | 🔵 基础理论 | Java 语言核心、并发深度、JVM 原理、IO/NIO 体系、数据结构与算法、设计模式、计算机基础理论、信息安全基础、软件工程基础、性能工程、调试排查、前沿技术趋势——**打地基** |
> | [`java_fullstack_ai_agent_study`](../java_fullstack_ai_agent_study) | 🟠 工程实践 | Spring 生态、分布式架构、大数据、存储中间件、风控爬虫、数据分析、多语言、**AI/Agent 应用开发（Spring AI / LangChain4j / LangGraph / AutoGen 等）**、上下文工程（工程化落地）、Agent 模式与编排（工程实践）、测试工程——**做系统** |
> | **本项目**（`ai_coding_harness_engineering_study`） | 🟣 AI 工程提效 | AI 编程方法论、AI 工具工程化（MCP/Skill/Hooks）、Harness 理论、AI Coding 工具链（Claude Code / Cursor / Codex）、大模型理论了解——**用 AI 提效** |

> 📐 **AI/Agent 在两个项目中的分工边界**
>
> | 关注点 | `java_fullstack_ai_agent_study`（做系统）| 本项目（用 AI 提效）|
> |--------|:---------------------------------------:|:------------------:|
> | Spring AI / LangChain4j 接入、模型 API 调用 | ✅ 工程化落地 | — |
> | RAG 系统工程化（向量数据库集成 / 检索服务）| ✅ 系统集成 | — |
> | Function Calling 封装与业务对接 | ✅ 业务接口 AI 化 | — |
> | 上下文工程（工程化落地，Spring AI Memory 接口等）| ✅ 工程落地 | — |
> | Agent 模式与编排框架（LangGraph / AutoGen 等）| ✅ 应用开发 | — |
> | Agent SDK（OpenAI Agents SDK / Google ADK 等）| ✅ 工程实践 | — |
> | MCP 协议原理与扩展开发（AI 工具侧）| — | ✅ 工具工程化 |
> | Harness / Eval 框架（LLM-as-Judge）| — | ✅ 质量保障 |
> | AI Coding 工具使用（Claude Code / Cursor 深度实践）| — | ✅ 工具提效 |
> | Claude Code 源码研究 | — | ✅ 源码学习 |
> | AI 算法原理（Transformer / RLHF / MoE，了解层）| — | ✅ 理论了解 |

在 AI 重塑软件工程的时代背景下，本仓库聚焦以下核心方向：

| # | 方向 | 核心关注点 |
|---|------|-----------|
| 1 | **AI 编程（AI Coding）** | 方法论、Prompt Engineering、Vibe Coding、Plan/Act 模式、规则文件工程化、局限性认知 |
| 2 | **AI 工具工程化** | Rules 文件体系、MCP、Skill、Command、Hooks、AI 安全 |
| 3 | **Harness 工程理论** | 测试线束、AI 代码验证、Eval 框架（LLM-as-Judge / RAGAS）、CI/CD 与 AI 融合 |
| 4 | **AI Coding 工具实践** | Claude Code、Codex、Cursor、Windsurf、CatPaw 等深度使用与最佳实践 |
| 5 | **AI 算法与大模型理论** | 机器学习基础、深度学习、Transformer 架构、大模型原理（RLHF/SFT/MoE/Scaling Law）|
| 6 | **国内外 AI 动态** | 前沿论文、公司动态、评测榜单、政策观察 |
| 7 | **主流大模型** | GPT / Claude / Gemini / DeepSeek 等能力深度对比 |
| 8 | **Claude Code 源码学习** | TypeScript 源码阅读、核心模块分析、架构设计、扩展开发 |

---

## 🎯 学习目标

- [ ] 建立 AI 辅助编程的完整认知框架，掌握 Vibe Coding 方法论，清晰认知 AI 编程的局限性
- [ ] 掌握规则文件体系工程化（CLAUDE.md / .cursorrules / AGENTS.md）的最佳实践
- [ ] 掌握 AI 工具工程化核心能力：MCP、Skill/Command、Hooks 及 AI 安全
- [ ] 建立 AI 输出质量保障体系：Eval 框架、LLM-as-Judge、RAGAS、Braintrust 等工具实践
- [ ] 深度掌握 Claude Code、Codex CLI、Cursor 等主流 AI 编程工具
- [ ] **了解 AI 算法与大模型底层原理**（Transformer/RLHF/SFT/MoE/Scaling Law），为大模型选型、Prompt 策略制定提供理论支撑
- [ ] 形成国内外前沿 AI 动态的持续追踪机制
- [ ] 积累可复用的学习笔记、代码示例与最佳实践
- [ ] 深入阅读 Claude Code 源码，理解 AI Coding 工具的内部实现机制

---

## 🗺️ 学习路径建议

> 8 大板块并非孤立的，它们之间存在依赖关系。以下是建议的学习路径，可根据个人基础灵活调整。

### 📍 推荐入门顺序

```
[基础认知层]
① AI 编程方法论（一）            ← 建立基本认知框架，最先入手
          ↓
[工程实践层]
② AI 工具工程化（二）            ← 学会扩展和定制 AI 工具
③ AI Coding 工具深度实践（四）   ← 掌握日常开发提效的核心工具
          ↓
[进阶方向层]
④ AI 算法与大模型理论（五）       ← 为大模型选型打理论底色（了解层）
⑤ Harness 工程理论（三）         ← 保障 AI 输出质量的工程体系
⑥ 主流大模型（七）               ← 了解各模型能力差异，辅助工具选型
          ↓
[深度研究层]
⑦ Claude Code 源码学习（八）     ← 从源码理解 AI 工具的内部机制
⑧ 国内外 AI 动态（六）           ← 持续追踪前沿，贯穿全程
```

### 🔗 板块间关键依赖关系

| 学习这个 | 需要先了解 | 关系说明 |
|---------|-----------|---------|
| AI 工具工程化（MCP / Hooks） | AI 编程方法论 | 工程化是方法论的工具层落地 |
| Harness / Eval 框架 | AI 编程方法论 | 先理解 AI 编程流程，才能设计验证体系 |
| Claude Code 源码 | AI 工具工程化（MCP / Hooks） | 源码学习需要先理解 MCP / Hooks 的概念 |

> 📌 **注意**：上下文工程（Context Engineering）和 Agent 模式与编排已迁移至 [`java_fullstack_ai_agent_study`](../java_fullstack_ai_agent_study) 项目（板块22/23），在那里以工程落地视角（Spring AI 接入 / 记忆体系构建 / LangGraph 编排 / Agent SDK 应用）进行深度实践。

### ⏰ 时间分配参考

| 优先级 | 板块 | 预估投入 | 说明 |
|--------|------|---------|------|
| 🔴 高 | AI Coding 工具实践（Claude Code / Cursor） | 持续投入 | 直接影响日常开发效率 |
| 🔴 高 | AI 工具工程化（MCP / Hooks） | 1-2 周 | 工程化提效的关键 |
| 🟠 中高 | AI 编程方法论 | 1 周 | 认知框架，先入手 |
| 🟠 中高 | AI 算法与大模型理论 | 1-2 周 | 了解层，为选型提供支撑 |
| 🟡 中 | Harness / Eval 框架 | 1-2 周 | 质量保障，可并行学习 |
| 🟡 中 | Claude Code 源码 | 按兴趣深入 | 深度研究方向 |
| 🟢 低 | AI 动态追踪 | 碎片化持续 | 保持视野，非集中学习 |

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
- **Plan Mode vs. Act Mode**：先规划后执行的工作流理念
  - 任务分解与规划（Planning）阶段的设计
  - 执行（Acting）阶段的验证与迭代策略
- **多轮对话设计**：如何有效地与 AI 持续迭代
  - 会话状态保持与上下文传递策略
  - 增量式开发：小步前进、及时确认
  - 有效反馈格式：描述期望结果而非操作步骤
- **规则文件体系（Rules Files）工程化**
  - `CLAUDE.md` / `.cursorrules` / `AGENTS.md` / `GEMINI.md` 的作用与差异
  - 项目级 vs. 用户级规则文件的管理策略
  - 规则内容设计：代码规范、架构约束、工作流指引
- AI 与人类协作开发的工作流设计
- AI 编程的质量保障与 Code Review 策略
- **AI 编程的局限性认知**
  - 幻觉（Hallucination）：错误代码、虚假 API 引用
  - 错误传播：一步出错、后续雪崩
  - Over-engineering 倾向与技术债积累
  - 长会话退化：上下文污染导致质量下降
  - 何时该放弃 AI、手动接管的判断依据

</details>

<details>
<summary><b>二、AI 工具工程化</b></summary>

> AI 工具工程化关注的是**工具层面的扩展、集成与行为控制**——如何让 AI 工具连接更多能力、更安全地运行、更灵活地定制。

### 📋 Rules / Instructions 文件工程化
> AI 工具的"宪法"：通过项目规则文件持续、稳定地约束 AI 行为，避免每次对话重复说明。

- **各工具规则文件对比**
  - `CLAUDE.md`（Claude Code）：项目上下文、代码约定、工作流指引
  - `.cursorrules` / `.cursor/rules/*.mdc`（Cursor）：AI 行为规则，支持 glob 过滤
  - `AGENTS.md`（Codex CLI / OpenAI）：项目说明与 Agent 行为约束
  - `GEMINI.md`（Gemini CLI）：Google 工具链的项目规则文件
- **规则文件的层级与合并策略**：全局级 → 项目根级 → 子目录级，优先级与覆盖规则
- **规则内容工程化设计**
  - 技术栈说明与依赖约定
  - 代码风格与命名规范
  - 禁止行为清单（不允许修改的文件、目录）
  - 测试要求与 CI 集成说明
  - 常用命令速查（build、test、lint、deploy）
- **规则文件的版本管理与团队协作**：纳入 Git 管理、团队共享最佳实践
- **规则冲突与优先级调试**：如何排查 AI 行为与预期不符的原因

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
- Hooks 机制：工具执行生命周期的回调与行为控制
- Pre-tool / Post-tool Hook 的应用场景
- 会话级别 Hook vs. 全局 Hook
- Hook 用于自动注入上下文、日志记录、结果校验、格式化输出
- Claude Code Hooks 配置实践（PreToolUse / PostToolUse / Stop / Notification）

### 🔒 AI 安全（Security）
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

### 🔩 测试线束基础（Test Harness）
- **Harness 测试线束**基本概念与设计思想
- 测试线束的组成：驱动程序（Driver）、桩（Stub）、Mock、Spy
- 工程测试体系的构建：单元测试 → 集成测试 → E2E 测试
- **确定性测试 vs. 概率性测试**：AI 输出的不确定性对传统测试方法的挑战

### 🧪 AI 时代的 Harness 新内涵
- AI 生成代码的自动化验证体系
- 基于 LLM 的测试用例生成（TestGen）
- AI Coding 工具的质量评估 Harness
- Benchmark 驱动的 AI 能力评测体系（SWE-bench、HumanEval 等）

### 📐 Eval 框架（AI 评估工程）
> 专为 LLM 输出设计的质量评估体系——与传统单元测试逻辑完全不同。

- **Eval 的核心概念**：Golden Set（标准数据集）、评估指标、评估流水线
- **LLM-as-Judge**：用大模型评估大模型输出质量的方法论
  - 评估提示词设计（Pointwise / Pairwise 评分）
  - 评估偏差（Position Bias / Verbosity Bias）的识别与规避
  - 人工标注 vs. LLM 评估的一致性对齐
- **RAG 评估框架 RAGAS**：上下文相关性、答案忠实度、答案相关性三维度
- **代码生成评估**：功能正确性（Pass@k）、语法合法性、测试通过率
- **主流 Eval 框架工具**
  - [Braintrust](https://braintrust.dev)：生产级 LLM 评估与实验追踪
  - [OpenAI Evals](https://github.com/openai/evals)：OpenAI 官方评估框架
  - [RAGAS](https://ragas.io)：RAG 管道专项评估
  - [DeepEval](https://deepeval.com)：多指标 LLM 评测框架
  - [PromptFoo](https://promptfoo.dev)：Prompt 测试与红队评估
- **回归测试**：Prompt 变更后的质量回归检测方案

### 🔄 持续集成 / 持续交付（CI/CD）与 AI 结合
- AI 辅助 Code Review 在 CI 中的集成
- 自动化测试生成流水线
- Eval 流水线的 CI 化：每次 Prompt 变更自动触发评估

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
<summary><b>五、AI 算法与大模型理论（背景知识，了解层）</b></summary>

> 💡 **本章定位**：作为**理论背景知识**了解，目标是帮助工程师从原理层面理解大模型，为大模型选型、Prompt 策略制定提供理论支撑。深度 Agent 应用开发（上下文工程工程化落地 / Agent 框架实践）见 [`java_fullstack_ai_agent_study`](../java_fullstack_ai_agent_study) 项目。

### 🤖 机器学习基础
| 方向 | 学习要点 |
|------|---------|
| 学习范式 | 监督学习 / 无监督学习 / 强化学习的基本区别与适用场景 |
| 特征工程 | 特征选择 / 归一化 / 数据增强的基本原则 |
| 模型评估 | 偏差-方差权衡 / 过拟合与欠拟合 / 交叉验证 |

### 🧠 深度学习核心
| 方向 | 学习要点 |
|------|---------|
| 神经网络基础 | 前向传播 / 反向传播 / 梯度下降 / 激活函数 / 损失函数 |
| 序列模型 | CNN（视觉特征提取）/ RNN / LSTM（序列建模）/ Attention 机制原理 |
| Transformer 架构 | 自注意力（Self-Attention）/ 多头注意力 / Position Encoding / Encoder-Decoder 结构 |
| 大模型架构 | Decoder-only 结构（GPT 范式）/ 涌现能力 / Scaling Law / 参数规模的意义 |

### 🔬 大模型训练体系
| 方向 | 学习要点 |
|------|---------|
| Pre-training | 自监督学习 / 下一个 Token 预测 / 海量语料的作用 |
| SFT（监督微调） | 指令跟随能力的来源 / 微调数据集设计 |
| RLHF | 人类反馈强化学习的基本原理 / 奖励模型 / PPO 优化 |
| DPO | RLHF 的简化替代 / 直接偏好优化 |
| MoE 架构 | 混合专家模型的稀疏激活机制 / 与 Dense 模型对比 |
| 量化与推理优化 | INT8/INT4 量化 / KV Cache / 推测解码（Speculative Decoding）|

### 🌐 大模型生态
| 模型系列 | 关注要点 |
|---------|---------|
| OpenAI GPT 系列 | GPT-4o / o1 / o3 推理模型 / API 能力边界 |
| Anthropic Claude | 宪法 AI（Constitutional AI）/ 长上下文设计 |
| Google Gemini | 多模态原生设计 / 百万 Token 上下文 |
| DeepSeek | 国内开源领军 / MoE 架构 / R1 推理模型 |
| 开源生态 | LLaMA 3.x / Qwen3 / Mistral / Phi / Gemma 的特点 |

> 📎 **与关联项目的分工**：本章重在「理解原理」；Agent 应用开发、上下文工程工程化落地、编排框架实践 → 详见 [`java_fullstack_ai_agent_study`](../java_fullstack_ai_agent_study) 项目板块 17/22/23。

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
├── docs/
│   └── knowledge-map.svg              # 知识体系全景大图
│
├── 01-ai-coding/                      # 一、AI 编程方法论与实践
│   ├── prompt-engineering/            #   Prompt 工程技巧
│   ├── vibe-coding/                   #   Vibe Coding 理念与实践
│   ├── rules-files/                   #   规则文件体系工程化（CLAUDE.md 等）
│   ├── limitations/                   #   AI 编程局限性认知
│   └── workflow/                      #   人机协作工作流设计
│
├── 02-ai-tool-engineering/            # 二、AI 工具工程化
│   ├── rules-instructions/            #   Rules/Instructions 文件工程化
│   ├── mcp/                           #   MCP 协议与实践
│   ├── skill/                         #   Skill 体系设计
│   ├── command/                       #   自定义命令（Slash Command）
│   ├── hooks/                         #   Hooks 机制
│   └── security/                      #   AI 安全（Prompt Injection 等）
│
├── 03-harness/                        # 三、Harness 工程理论
│   ├── test-harness-basics/           #   测试线束基础
│   ├── ai-code-validation/            #   AI 生成代码验证体系
│   ├── eval-framework/                #   Eval 框架（LLM-as-Judge / RAGAS 等）
│   └── benchmark/                     #   AI 能力评测 Benchmark
│
├── 04-tools/                          # 四、AI Coding 工具深度实践
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
├── 05-ai-theory/                      # 五、AI 算法与大模型理论（背景知识，了解层）
│   ├── ml-basics/                     #   机器学习基础
│   ├── deep-learning/                 #   深度学习（神经网络 / Attention）
│   ├── transformer/                   #   Transformer 架构
│   ├── llm-architecture/              #   大模型架构（Decoder-only / Scaling Law）
│   ├── training-system/               #   大模型训练体系（Pre-train / SFT / RLHF / MoE）
│   └── inference-optimization/        #   推理优化（量化 / KV Cache）
│
├── 06-ai-news/                        # 六、国内外 AI 动态追踪
│
├── 07-models/                         # 七、主流大模型学习笔记
│   ├── openai/                        #   GPT / o 系列
│   ├── anthropic/                     #   Claude 系列
│   ├── google/                        #   Gemini 系列
│   ├── deepseek/                      #   DeepSeek 系列
│   ├── domestic/                      #   国内其他模型
│   └── comparison/                    #   多模型横向对比
│
└── 08-claude-code-source/             # 八、Claude Code 源码学习（重点）
    ├── overview/                      #   仓库结构与技术栈概览
    ├── cli-entry/                     #   CLI 入口与命令解析
    ├── conversation-engine/           #   对话引擎与消息循环
    ├── tool-system/                   #   工具系统（内置工具注册与执行）
    ├── mcp-client/                    #   MCP Client 实现
    ├── hooks-system/                  #   Hooks 系统
    ├── permission-system/             #   权限系统
    ├── context-compression/           #   上下文压缩实现
    ├── sub-agent/                     #   Sub-agent（Task 工具）
    └── architecture-insights/         #   架构设计洞察与扩展开发
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

### Eval 评估工具速查

| 工具 | 维护方 | 核心能力 |
|------|--------|---------|
| [Braintrust](https://braintrust.dev) | Braintrust | 生产级 LLM 评估 + 实验追踪 + 数据集管理 |
| [OpenAI Evals](https://github.com/openai/evals) | OpenAI | 官方评估框架，标准化评测套件 |
| [RAGAS](https://ragas.io) | Exploding Gradients | RAG 管道专项评估（忠实度 / 相关性）|
| [DeepEval](https://deepeval.com) | Confident AI | 多指标 LLM 评测，集成 CI/CD |
| [PromptFoo](https://promptfoo.dev) | PromptFoo | Prompt 回归测试 + 红队安全评估 |
| [LangSmith](https://smith.langchain.com) | LangChain | Trace 追踪 + 数据集 + 评估一体化 |

---

## 📊 学习进度

> 各模块学习状态持续更新。状态说明：🔜 待开始 ｜ 🔥 进行中 ｜ ✅ 已完成 ｜ ⏸️ 暂停

| 主模块 | 子模块 / 知识点 | 状态 | 开始时间 | 备注 |
|--------|----------------|:----:|---------|------|
| **▶ 一、AI 编程（AI Coding）** | | | | |
| | AI 辅助编程核心概念与方法论 | 🔜 待开始 | - | |
| | Prompt Engineering（System Prompt / Few-shot / CoT）| 🔜 待开始 | - | |
| | Vibe Coding 理念与边界 | 🔜 待开始 | - | |
| | Plan Mode vs. Act Mode 工作流理念 | 🔜 待开始 | - | |
| | 多轮对话设计（增量开发 / 有效反馈格式）| 🔜 待开始 | - | |
| | 规则文件体系工程化（CLAUDE.md / .cursorrules / AGENTS.md）| 🔜 待开始 | - | 🔑 重点 |
| | 人机协作工作流设计 | 🔜 待开始 | - | |
| | AI 编程质量保障与 Code Review 策略 | 🔜 待开始 | - | |
| | AI 编程局限性认知（幻觉 / 错误传播 / 退化）| 🔜 待开始 | - | |
| **▶ 二、AI 工具工程化** | | | | |
| | Rules / Instructions 文件工程化（各工具规则文件体系）| 🔜 待开始 | - | 🔑 重点 |
| | MCP 协议原理与实践（Server / Client / 自定义）| 🔜 待开始 | - | 🔑 重点 |
| | Skill 体系设计（概念 / 触发 / 管理）| 🔜 待开始 | - | |
| | Command（Slash Command）设计与实践 | 🔜 待开始 | - | |
| | Hooks 机制（Pre/Post-tool / 行为控制）| 🔜 待开始 | - | 🔑 重点 |
| | AI 安全（Prompt Injection / 权限控制 / 沙箱）| 🔜 待开始 | - | |
| **▶ 三、Harness 工程理论** | | | | |
| | Harness 测试线束基本概念（Driver / Stub / Mock）| 🔜 待开始 | - | |
| | 确定性测试 vs. 概率性测试的工程挑战 | 🔜 待开始 | - | |
| | AI 生成代码的自动化验证体系 | 🔜 待开始 | - | |
| | 基于 LLM 的测试用例生成（TestGen）| 🔜 待开始 | - | |
| | Benchmark 评测体系（SWE-bench / HumanEval）| 🔜 待开始 | - | |
| | Eval 框架核心概念（Golden Set / 评估流水线）| 🔜 待开始 | - | 🔑 重点 |
| | LLM-as-Judge（Pointwise / Pairwise / 偏差规避）| 🔜 待开始 | - | 🔑 重点 |
| | RAG 评估框架 RAGAS（相关性 / 忠实度 / 质量）| 🔜 待开始 | - | |
| | Eval 工具链（Braintrust / OpenAI Evals / DeepEval / PromptFoo）| 🔜 待开始 | - | |
| | CI/CD 与 AI 结合（Eval 流水线 / Code Review 流水线）| 🔜 待开始 | - | |
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
| **▶ 五、AI 算法与大模型理论（了解层）** | | | | |
| | 机器学习基础（监督/无监督/强化学习 / 偏差-方差权衡）| 🔜 待开始 | - | |
| | 深度学习核心（神经网络 / 梯度下降 / 激活函数 / 损失函数）| 🔜 待开始 | - | |
| | 序列模型（RNN / LSTM / Attention 机制原理）| 🔜 待开始 | - | |
| | Transformer 架构（Self-Attention / 多头注意力 / Position Encoding）| 🔜 待开始 | - | 🔑 重点 |
| | 大模型架构（Decoder-only / 涌现能力 / Scaling Law）| 🔜 待开始 | - | 🔑 重点 |
| | 大模型训练体系（Pre-training / SFT / RLHF / DPO / MoE）| 🔜 待开始 | - | 🔑 重点 |
| | 推理优化（INT4/INT8 量化 / KV Cache / 推测解码）| 🔜 待开始 | - | |
| | 大模型生态纵览（GPT/Claude/Gemini/DeepSeek/开源生态特点）| 🔜 待开始 | - | |
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
| [`java_study`](../java_study) | 🔵 基础理论 | Java 语言核心（含 SPI/字节码增强/序列化）、JVM 原理、IO/NIO 体系、并发深度（含虚拟线程/响应式）、数据结构与算法、设计模式与设计原则、计算机基础理论（OS/网络/原理/编译）、信息安全基础（密码学/攻防）、软件工程基础（测试/重构/CI）、数学基础、性能工程（JMH/火焰图）、调试与问题排查（Arthas/JFR）、前沿技术趋势 | 打地基 |
| [`java_fullstack_ai_agent_study`](../java_fullstack_ai_agent_study) | 🟠 工程实践 | Spring 生态、分布式架构、大数据生态、存储中间件、风控爬虫、数据分析、多语言、前端、**AI/Agent 应用开发（Spring AI / LangChain4j / LangGraph / AutoGen 等）**、**上下文工程（工程化落地）**、**Agent 模式与编排（工程实践）**、测试工程 | 做系统 |
| **本项目**（`ai_coding_harness_engineering_study`） | 🟣 AI 工程提效 | AI 编程方法论、AI 工具工程化（MCP/Skill/Hooks）、Harness 理论（Eval/LLM-as-Judge）、AI Coding 工具链（Claude Code / Cursor / Codex）、大模型理论了解、Claude Code 源码研究 | 用 AI 提效 |

---

## 📅 更新记录

| 日期 | 版本 | 内容 |
|------|------|------|
| 2026-05-23 | v0.1.0 | 项目初始化，创建 README |
| 2026-05-23 | v0.2.0 | 补充上下文工程、Agent 模式、AI Coding 工具深度实践等内容 |
| 2026-05-23 | v0.3.0 | 更新标题、学习进度改为主子分层大表格 |
| 2026-05-23 | v0.4.0 | 优化项目定位、关联项目分工说明、关联项目章节 |
| 2026-05-23 | v0.5.0 | 新增「Claude Code 源码学习」方向（第八大板块） |
| 2026-05-23 | v0.6.0 | 按方案 B 精准拆分：上下文工程精简为核心五项，新增「AI 工具工程化」独立方向（MCP/Skill/Command/Hooks/安全），全文编号顺延至九大板块 |
| 2026-05-24 | v0.7.0 | 全局地图完整性补全：新增规则文件工程化、Structured Output、Eval 框架（LLM-as-Judge/RAGAS/Braintrust）、Computer Use/Browser Use、Long-running Agent、Agent 安全对齐、Plan/Act 模式、AI 编程局限性认知；新增「学习路径建议」章节（推荐顺序、依赖关系、时间分配）；同步更新学习进度表、目录结构、工具速查 |
| 2026-06-06 | v0.8.0 | 补充 Agent SDK 完整内容：新增「🧰 Agent SDK」章节（OpenAI Agents SDK / Google ADK / AWS Strands / Anthropic Tool Use / Pydantic AI / smolagents 及 A2A 协议） |
| 2026-06-10 | v0.9.0 | **项目定位重构**：承接来自 `java_fullstack_ai_agent_study` 迁移的「AI 算法理论」板块，作为背景知识沉淀；更新板块数量至 10 大板块 |
| 2026-06-10 | v1.0.0 | 同步关联项目描述，与各项目 README 保持一致 |
| 2026-06-13 | v2.0.0 | **重大重构：聚焦 AI 工程提效与工具研究**。将「上下文工程（Context Engineering）」和「Agent 模式与编排」迁移至 `java_fullstack_ai_agent_study`（板块22/23），在那里以工程落地视角深度实践；本项目重新聚焦为 8 大板块（AI Coding 方法论 / AI 工具工程化 / Harness / AI 工具实践 / 大模型理论了解 / AI 动态 / 主流大模型 / Claude Code 源码）；更新项目定位、分工边界表、学习路径、目录结构、学习进度表、关联项目说明 |

---

<div align="center">
  <sub>持续更新中 🚀 · AI 时代的工程师，既要懂得驾驭 AI 工具，也要保持对工程本质的深刻理解。</sub>
</div>
