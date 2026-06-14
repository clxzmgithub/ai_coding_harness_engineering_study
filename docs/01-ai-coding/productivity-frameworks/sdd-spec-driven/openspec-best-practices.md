# OpenSpec 核心工作流与最佳实践

> **所属板块**：一、AI 编程方法论 → AI 编程提效框架 → SDD 规格驱动开发
> **学习目标**：掌握 Spec-Driven Development 的核心理念，熟练使用 OpenSpec 框架指导 AI 编程
> **参考资源**：[OpenSpec GitHub](https://github.com/Fission-AI/OpenSpec)（54k ⭐）

---

## 一、为什么需要 SDD（规格驱动开发）？

### 核心问题：AI 直接写代码的困境

```
传统 Vibe Coding 流程：
用户：「帮我实现一个用户认证系统」
AI：直接开始写代码 → 写了 300 行之后发现方向完全不对

SDD 流程：
用户：「帮我实现一个用户认证系统」
→ AI 先生成 proposal.md（方案对齐）
→ 用户确认 → AI 生成 design.md（架构设计）
→ 用户确认 → AI 生成 tasks.md（任务分解）
→ 逐步执行，随时可修正
```

### SDD 解决的三大核心问题

| 问题 | 传统方式 | SDD 方式 |
|------|---------|---------|
| **方向偏差** | AI 写了大量错误代码才发现跑偏 | 规格确认后再动手，早期发现偏差 |
| **需求模糊** | AI 自行猜测，用户发现后推倒重来 | 结构化文档迫使需求明确化 |
| **过程不可控** | 一次性交付，修改代价高 | 分阶段确认，每步可介入调整 |

---

## 二、OpenSpec 框架概览

### 核心文件结构

```
project/
├── specs/
│   ├── proposals/          # 方案草案（未确认）
│   │   └── PROPOSAL-001.md
│   ├── designs/            # 架构设计文档（已确认方向）
│   │   └── DESIGN-001.md
│   ├── tasks/              # 任务清单（可执行）
│   │   └── TASKS-001.md
│   └── archive/            # 已完成归档
│       └── ARCH-001.md
└── CLAUDE.md / .cursorrules  # 工具配置
```

### 三类核心文档对比

| 文档 | 触发命令 | 用途 | 示例内容 |
|------|---------|------|---------|
| `proposal.md` | `/opsx:propose` | 方案对齐，确认「做什么」 | 功能描述、边界条件、潜在风险 |
| `design.md` | `/opsx:design` | 架构设计，确认「怎么做」 | 技术选型、模块划分、数据结构 |
| `tasks.md` | `/opsx:tasks` | 任务拆解，确认「干哪些步骤」 | 有序任务列表、依赖关系、验收标准 |

---

## 三、OpenSpec 标准工作流

### 完整 5 步流程

```
第一步：提出需求
  用户 → 输入功能需求（自然语言即可）

第二步：生成方案草案
  → /opsx:propose "用户需求描述"
  → AI 生成 specs/proposals/PROPOSAL-XXX.md
  → 用户审查：接受 / 修改 / 拒绝

第三步：生成架构设计
  → /opsx:design PROPOSAL-XXX
  → AI 生成 specs/designs/DESIGN-XXX.md
  → 用户审查技术方案是否合理

第四步：生成任务清单
  → /opsx:tasks DESIGN-XXX
  → AI 生成 specs/tasks/TASKS-XXX.md
  → 用户审查任务拆分是否合理

第五步：执行并归档
  → /opsx:apply TASKS-XXX（逐步执行任务）
  → /opsx:archive XXX（完成后归档）
```

### 实战示例：实现 JWT 认证模块

**Step 1 - 输入需求：**
```
/opsx:propose 为 Spring Boot 项目添加 JWT 认证，支持 access token（1h）+ refresh token（7d），
需要 /login、/refresh、/logout 三个端点，用户信息存 MySQL
```

**Step 2 - 生成的 PROPOSAL-001.md 示例：**
```markdown
# PROPOSAL-001: JWT 认证模块

## 功能描述
...

## 技术边界
- 框架：Spring Security 6.x + jjwt 0.12.x
- 存储：MySQL（用户表）+ Redis（token 黑名单）
- **不在范围内**：OAuth2、第三方登录

## 潜在风险
- Token 刷新并发问题（需要加锁）
- Redis 不可用时的降级策略

## 决策点（需用户确认）
1. access token 是否存数据库？（推荐：否，纯无状态）
2. 登出是否需要立即失效？（如是，需要 Redis 黑名单）
```

---

## 四、OpenSpec 安装配置

### 方式一：Claude Code 集成

```bash
# 克隆到项目根目录
git clone https://github.com/Fission-AI/OpenSpec.git .openspec

# 或者通过 npm 安装（如果有 CLI 工具）
npm install -g @fission/openspec
```

**配置 CLAUDE.md 加载 OpenSpec 指令：**
```markdown
<!-- CLAUDE.md -->
## 项目规范
本项目使用 OpenSpec SDD 工作流，详见 .openspec/ 目录。
所有功能开发必须先经过 propose → design → tasks 三步规格确认，
禁止直接开始编码而跳过规格文档。
```

### 方式二：Cursor 集成

在 `.cursorrules` 中添加：
```
This project uses OpenSpec SDD workflow. 
Before writing any code for new features:
1. Create specs/proposals/PROPOSAL-XXX.md
2. Wait for user confirmation
3. Create specs/designs/DESIGN-XXX.md
4. Create specs/tasks/TASKS-XXX.md
5. Only then start implementation
```

---

## 五、最佳实践与踩坑总结

### ✅ DO（建议这样做）

1. **Proposal 要聚焦「做什么」而非「怎么做」**
   - 好：描述功能边界、用户故事、验收标准
   - 坏：直接写技术方案（那是 Design 文档的事）

2. **Decision Points 要明确列出**
   - 每个 Proposal 结尾列出 2-5 个关键决策点让用户确认
   - 避免 AI 自行猜测关键技术选型

3. **Tasks 要足够细化（每任务 < 30 分钟）**
   - 细化的任务更容易验收，AI 也更不容易跑偏
   - 每个 task 写明：输入、输出、验收标准

4. **定期归档，保持 specs/ 目录整洁**
   - 已完成功能及时移入 `archive/`
   - 避免 AI 把旧规格误认为当前待实现项

### ❌ DON'T（避免这些错误）

1. **不要跳过 Proposal 直接写 Design**
   - 方向未对齐就设计架构，设计完了发现方向错误，代价极高

2. **不要让 AI 一次性生成三个文档**
   - 每步都需要人工确认，这是 SDD 的核心价值所在

3. **不要把规格文档写得过于冗长**
   - Proposal 1-2 页，Design 2-3 页，Tasks 1-2 页足矣
   - 过长的文档 AI 处理时容易遗漏关键信息

---

## 六、OpenSpec vs 其他 SDD 工具对比

| 工具 | 特点 | 适用场景 |
|------|------|---------|
| **OpenSpec** | 最流行，支持主流工具，社区活跃 | 通用场景，Claude Code / Cursor 用户 |
| **Kiro**（Amazon）| 内置于 IDE，原生 SDD，无需额外配置 | 需要完整 SDD IDE 体验 |
| **spec-kit-zh** | 中文友好，入门门槛低 | 中文团队入门 SDD |
| **spec-coding-mcp** | MCP Server 封装，程序化调用 | 自动化 CI/CD 场景 |

---

## 七、延伸学习

- [ ] 阅读 OpenSpec 官方文档和 examples
- [ ] 在一个真实项目中完整走一遍 SDD 流程
- [ ] 尝试与 GStack 角色套件结合使用（`/reviewer` 审查规格文档）
- [ ] 探索 Kiro 的内置 SDD 工作流与 OpenSpec 的异同

> 🔗 **关联笔记**：
> - [GStack 多角色套件使用指南](../multi-role-team/gstack-setup-guide.md)
> - [OpenSpec + GStack + Superpowers 综合实战](../combined-practice/openspec-gstack-superpowers-workflow.md)
