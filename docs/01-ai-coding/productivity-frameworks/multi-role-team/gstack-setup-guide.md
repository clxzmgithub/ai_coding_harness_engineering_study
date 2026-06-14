# GStack 安装配置与多角色 AI 工程团队使用指南

> **所属板块**：一、AI 编程方法论 → AI 编程提效框架 → 虚拟 AI 工程团队
> **学习目标**：掌握 GStack 虚拟工程团队套件，通过多角色视角全方位提升代码质量
> **参考资源**：[GStack GitHub](https://github.com/garrytan/gstack)（109k ⭐）

---

## 一、GStack 是什么？

GStack 是 YC CEO Garry Tan 开源的 AI 编程多角色 Slash Command 套件。
其核心理念是：**给 AI 分配不同的专家角色视角，弥补单一视角的盲点**。

### 实测数据

> 60 天内，使用 GStack + Claude Code 交付了 3 个生产级服务、40+ 功能
> 代码生产率（LOC 速率）号称提升 **810x**

### 为什么多角色比单一视角更有效？

```
单一视角问题：
  你问 AI："帮我写这个功能"
  → AI 只关注「能不能跑」，忽略了安全漏洞
  → AI 只关注「功能正确」，忽略了 UX 流畅度
  → AI 只关注「代码写出来」，忽略了是否写了测试

多角色解法：
  /em        → 架构师视角：「这个设计符合系统边界吗？」
  /security  → 安全审计视角：「这里有没有 SQL 注入？」
  /designer  → UX 视角：「用户会觉得这个流程顺畅吗？」
  /qa        → 测试视角：「边界情况都覆盖了吗？」
```

---

## 二、GStack 核心角色全览（23 个）

### 🏗️ 工程与架构类

| 命令 | 角色 | 核心职责 | 典型使用场景 |
|------|------|---------|------------|
| `/em` | Engineering Manager | 架构审查、技术决策、代码边界 | PR 前架构 Review |
| `/cto` | CTO | 技术战略、技术债评估 | 重大技术选型 |
| `/sre` | SRE 工程师 | 可靠性、监控、故障恢复 | 上线前可靠性审查 |
| `/devops` | DevOps 工程师 | CI/CD、部署、容器化 | 流水线设计 |

### 🛡️ 质量与安全类

| 命令 | 角色 | 核心职责 | 典型使用场景 |
|------|------|------------|------------|
| `/reviewer` | Code Reviewer | 发现 Bug、逻辑漏洞、代码异味 | 每次 PR 必用 |
| `/security` | 安全工程师 | OWASP Top10 + STRIDE 威胁建模 | 涉及认证/授权/用户数据时 |
| `/qa` | QA 工程师 | 真实浏览器 E2E 测试 | 功能完成后验收 |
| `/auditor` | 代码审计专家 | 深度安全合规审计 | 金融/支付模块 |

### 🎨 产品与设计类

| 命令 | 角色 | 核心职责 | 典型使用场景 |
|------|------|---------|------------|
| `/ceo` | CEO/PM | 产品方向、用户价值对齐 | 需求讨论、功能规划 |
| `/designer` | UI/UX 设计师 | 界面可用性、设计规范 | 前端页面评审 |
| `/ux` | UX 研究员 | 用户旅程、交互逻辑 | 新功能设计评审 |

### 📦 交付类

| 命令 | 角色 | 核心职责 | 典型使用场景 |
|------|------|---------|------------|
| `/release` | Release Manager | 发 PR、Changelog、版本管理 | 功能完成后发布 |
| `/tech-writer` | 技术文档工程师 | API 文档、用户手册 | 功能上线前文档 |
| `/pm` | 项目经理 | 进度跟踪、风险管理 | Sprint 规划 |

---

## 三、安装与配置

### Step 1：克隆 GStack

```bash
# 克隆到项目目录下（推荐）
git clone https://github.com/garrytan/gstack.git .gstack

# 或者全局安装（适用于多个项目）
git clone https://github.com/garrytan/gstack.git ~/gstack-global
```

### Step 2：Claude Code 配置

在项目根目录的 `CLAUDE.md` 中引用 GStack：

```markdown
<!-- CLAUDE.md -->

## 虚拟工程团队配置（GStack）
本项目使用 GStack 多角色套件，Slash Commands 位于 .gstack/commands/ 目录。

### 角色使用规范
- 新功能开发完成后，**必须**执行 /reviewer 进行代码 Review
- 涉及用户数据/认证的模块，**必须**执行 /security 进行安全审计
- UI 变更需执行 /designer 进行 UX 评审
- 每个 Sprint 结束执行 /release 生成 Changelog
```

### Step 3：Cursor 配置

将 `.gstack/commands/` 目录中的 `.md` 文件内容整合到 `.cursorrules`，
或者在 Cursor 的 Custom Instructions 中声明角色规范。

### Step 4：验证安装

```bash
# Claude Code 中测试
/reviewer  # 应该能看到 Code Review 模式激活

# 如果命令未识别，检查 .gstack/commands/ 是否在 CLAUDE.md 的 allowed directories 中
```

---

## 四、最佳实践：GStack 在 Git 工作流中的位置

### 推荐的开发循环

```
1. 需求阶段
   /ceo    → 产品方向对齐
   /em     → 架构方案审查

2. 开发阶段
   写代码...
   /reviewer → 随时 Code Review（不必等 PR）

3. 功能完成
   /qa       → 端到端测试用例生成 + 执行
   /security → 安全审计（涉及敏感功能必执行）
   /designer → UI/UX 评审（涉及前端必执行）
   /sre      → 可靠性检查（核心链路必执行）

4. 发布阶段
   /release  → 生成 PR 描述、Changelog、版本标签
```

### 实战案例：用户登录功能 Review

```bash
# 完成登录功能开发后

# Step 1: 代码质量检查
/reviewer
# → 输出：发现 3 处潜在 Bug，2 个代码异味，建议重构点

# Step 2: 安全审计（登录涉及认证，必做）
/security
# → 输出：
#   ⚠️ 密码错误次数未限制（暴力破解风险）
#   ⚠️ JWT 密钥硬编码在配置文件（应从环境变量读取）
#   ✅ SQL 参数化查询，无注入风险
#   ✅ 密码 bcrypt 哈希，强度合规

# Step 3: 生成测试
/qa
# → 输出：E2E 测试用例 + 边界测试（空密码、特殊字符等）

# Step 4: 发布
/release
# → 输出：PR 描述模板 + Changelog 条目
```

---

## 五、GStack 变体项目

| 项目 | 特点 | Stars | 适用场景 |
|------|------|-------|---------|
| **GStack**（原版）| 23 个角色，通用工程团队 | 109k ⭐ | 通用全栈项目 |
| **ostack** | 针对 SaaS 场景定制 | 102 ⭐ | SaaS 产品开发 |
| **nanostack** | 极简版：plan→review→test→ship | 201 ⭐ | 小项目、快速迭代 |
| **claude-forge** | oh-my-zsh 风格，模块化插件 | 747 ⭐ | 需要高度定制化的团队 |

### claude-forge vs GStack 选型参考

```
选 GStack：
  ✓ 需要开箱即用的完整角色套件
  ✓ 团队规模小，直接用 Garry Tan 的最佳实践
  ✓ 快速上手，不需要定制

选 claude-forge：
  ✓ 需要高度定制化（自定义角色、命令）
  ✓ 已有自己的工作流规范，需要模块化组合
  ✓ oh-my-zsh 用户，喜欢插件化管理
```

---

## 六、性能调优建议

### 减少不必要的角色调用

并非每次都需要全套角色审查。按场景选择：

```
低风险变更（如修改文案、样式调整）：
  只需 /reviewer 快速过一遍即可

中风险变更（新功能、业务逻辑修改）：
  /reviewer + /qa

高风险变更（认证、支付、权限、数据库 schema）：
  /reviewer + /security + /qa + /sre
```

### 上下文管理

- 每次调用角色时，提供相关的代码文件路径，减少 AI 扫描全库的开销
- 使用 `@文件名` 语法精确指定上下文范围

---

## 七、延伸学习

- [ ] 阅读 GStack 每个 Slash Command 的提示词实现，学习角色设计方法
- [ ] 尝试基于 GStack 模板自定义团队专属角色（如：`/java-architect`）
- [ ] 探索 claude-forge 的插件机制，实现更细粒度的定制
- [ ] 结合 OpenSpec 使用：用 `/em` 审查规格文档的架构合理性

> 🔗 **关联笔记**：
> - [OpenSpec 核心工作流与最佳实践](../sdd-spec-driven/openspec-best-practices.md)
> - [OpenSpec + GStack + Superpowers 综合实战](../combined-practice/openspec-gstack-superpowers-workflow.md)
