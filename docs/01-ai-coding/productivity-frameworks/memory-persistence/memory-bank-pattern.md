# Memory Bank：结构化 AI 编程上下文与跨会话记忆持久化

> **所属板块**：一、AI 编程方法论 → AI 编程提效框架 → AI 编程上下文与记忆持久化
> **学习目标**：掌握 Memory Bank 方法论，解决 AI 跨会话「失忆」问题
> **参考工具**：claude-mem（82k ⭐）、CCSwitch（100k ⭐）、meridian（177 ⭐）

---

## 一、AI 编程「失忆」问题的本质

### 问题描述

```
每次新建 Claude Code 会话：
  ❌ AI 不记得「这个项目用 Spring Boot 3.x，不用 2.x」
  ❌ AI 不记得「认证模块已完成，不要再重复实现」
  ❌ AI 不记得「我们约定用 Result<T> 包装返回值，不用 ResponseEntity」
  ❌ AI 不记得「上次讨论决定用 CQRS 模式，原因是...」

用户的应对方式：
  → 每次重新解释项目背景（浪费 10-20 分钟）
  → 解释不完整时 AI 给出不一致的代码风格
  → 严重时 AI 推翻已有架构决策，引入冲突
```

### 根本原因

LLM 的无状态性：每次会话从零开始，CLAUDE.md 提供静态配置，
但不能动态记录「上次会话发生了什么」、「哪些决策已确定」。

---

## 二、Memory Bank 方法论

### 核心理念

> **不要让 AI 记住，而是让外部文件记住，每次启动时注入**

```
Memory Bank 模式：
  
  [项目目录]
  └── .memory-bank/
      ├── projectbrief.md       # 项目核心概述（不变）
      ├── productContext.md     # 产品背景与目标
      ├── systemPatterns.md     # 架构模式与技术决策（会更新）
      ├── techContext.md        # 技术栈详情
      ├── activeContext.md      # 当前工作重点（每次会话更新）
      └── progress.md           # 已完成/进行中/待做事项
  
  工作方式：
  → 会话开始：AI 读取 .memory-bank/ 所有文件，恢复项目认知
  → 会话进行：AI 正常工作
  → 会话结束：AI 更新 activeContext.md 和 progress.md
```

---

## 三、Memory Bank 文件详解

### 3.1 projectbrief.md —— 项目宪法

```markdown
# 项目简介

## 项目名称
用户管理平台（UMP）

## 核心目标
为公司内部提供统一的用户身份管理服务，支持 10 万+ 用户规模

## 技术约束
- 语言：Java 17 / Spring Boot 3.2
- 数据库：MySQL 8.0（主从架构）+ Redis 7.0（缓存）
- 部署：Kubernetes 1.28（阿里云 ACK）
- **禁止使用的技术**：Lombok（团队规范）、MyBatis-Plus 3.x 以下版本

## 不可更改的架构决策
1. 所有 API 返回 Result<T> 包装，禁用 ResponseEntity
2. 异常处理统一通过 GlobalExceptionHandler，禁止在 Controller 中 try-catch
3. 分页查询统一使用 PageRequest/PageResponse 封装
```

### 3.2 systemPatterns.md —— 架构模式

```markdown
# 架构模式与技术决策记录

## 当前架构
- 模块化单体架构（非微服务），按 DDD 分层
- 包结构：controller → service → domain → infrastructure

## 已确定的设计模式
| 场景 | 模式 | 决策原因 |
|------|------|---------|
| 用户创建 | 建造者模式 | 参数多，防止构造函数爆炸 |
| 权限检查 | 责任链模式 | 多个检查器顺序执行，易扩展 |
| 事件通知 | 观察者模式 | 用户状态变更触发多个下游 |

## ADR（架构决策记录）
### ADR-001: 选择 JWT 而非 Session
- 日期：2026-01-15
- 状态：已确定
- 原因：多实例部署下 Session 共享成本高，JWT 无状态更简单
- 影响：所有认证相关代码基于此，不再变更
```

### 3.3 activeContext.md —— 当前焦点（每次会话更新）

```markdown
# 当前工作上下文

## 当前 Sprint 目标（Sprint 7，2026-06-10 ~ 2026-06-24）
实现「组织架构管理」模块，包括部门 CRUD + 用户归属管理

## 进行中的任务
- [x] DepartmentEntity 和 Repository 层
- [x] DepartmentService 基础 CRUD
- [ ] **当前工作**：DepartmentController（正在开发中，50% 完成）
- [ ] DepartmentServiceTest 单元测试
- [ ] 与 UserService 的关联（用户归属部门）

## 重要决策（本次会话做出的）
- 部门支持多级嵌套（树形结构），用 parentId 字段实现，最大深度 5 层
- 删除部门时采用软删除，标记 deleted_at，子部门不级联删除（需手动迁移）

## 待下次会话继续的上下文
- DepartmentController 的 /api/v1/departments/tree 接口还未实现
- 需要讨论：部门树的缓存策略（Redis 缓存 or 内存缓存？）
```

### 3.4 progress.md —— 里程碑追踪

```markdown
# 项目进度

## 已完成模块
- [x] 用户认证（JWT + Refresh Token）
- [x] 用户 CRUD（含头像上传）
- [x] 角色权限基础框架（RBAC）
- [x] 操作日志记录

## 进行中
- [~] 组织架构管理（70%）

## 待开始
- [ ] 单点登录（SSO）集成
- [ ] 数据导入导出
- [ ] 审计报表
```

---

## 四、工具支持

### 4.1 claude-mem（82k ⭐）

```bash
# 安装
npm install -g claude-mem

# 配置（会话结束时自动捕获并压缩）
claude-mem init  # 初始化 .memory-bank/ 目录

# 手动同步当前会话记忆
claude-mem sync

# 查看当前记忆摘要
claude-mem show
```

**工作原理：**
```
会话结束后：
  1. 捕获会话历史（Claude Code 的 conversation log）
  2. 调用 AI 压缩为结构化摘要
  3. 更新 .memory-bank/activeContext.md 和 progress.md

下次会话开始：
  CLAUDE.md 中配置自动注入：
  "始终先读取 .memory-bank/ 目录下所有文件以恢复项目上下文"
```

### 4.2 meridian（177 ⭐）：零配置 Claude Code 增强

```bash
# 安装（零依赖，单命令）
curl -sL https://raw.githubusercontent.com/markmdev/meridian/main/install.sh | bash
```

meridian 提供三个核心能力：
1. **强制任务脚手架**：每次开始任务都自动创建结构化任务文档
2. **结构化记忆**：内置 .memory-bank 初始化和维护工作流
3. **compaction 后持久上下文**：Claude Code 触发上下文压缩后，
   自动将重要信息持久化，防止关键决策随 compaction 丢失

### 4.3 CCSwitch（100k ⭐）：多工具一体化桌面客户端

CCSwitch 是统一管理多款 AI 编程工具的桌面客户端，Memory 功能是其核心特性之一：

```
统一管理：
  Claude Code / Codex / OpenClaw / Gemini CLI / Hermes Agent

Memory 同步：
  → 一份 .memory-bank/ 在所有工具间共享
  → 切换工具时，Memory 自动跟随
  → 解决「在 Claude Code 里有记忆，切换到 Cursor 又失忆」的问题
```

---

## 五、CLAUDE.md 集成 Memory Bank 的标准模板

```markdown
<!-- CLAUDE.md（项目级）-->

# [项目名称] AI 编程配置

## 🧠 记忆恢复（每次会话必做）
**在做任何事之前，先读取以下文件恢复项目上下文：**
1. `.memory-bank/projectbrief.md` → 项目基础信息
2. `.memory-bank/systemPatterns.md` → 架构决策（绝不推翻）
3. `.memory-bank/techContext.md` → 技术栈详情
4. `.memory-bank/activeContext.md` → 当前工作焦点
5. `.memory-bank/progress.md` → 完成情况

读取完毕后，简单回复：「已恢复项目上下文，当前工作：[activeContext 中的当前任务]」

## 📝 会话结束时
每次会话结束前，更新：
- `activeContext.md`：记录本次完成的内容、未完成的内容、下次继续的上下文
- `progress.md`：更新任务完成状态
- `systemPatterns.md`（如有新的架构决策）

## 技术规范
[项目具体规范...]
```

---

## 六、最佳实践

### 6.1 Memory Bank 文件的维护原则

| 文件 | 更新频率 | 更新时机 | 更新者 |
|------|---------|---------|--------|
| `projectbrief.md` | 极少 | 项目大版本变更 | 人工 |
| `systemPatterns.md` | 偶尔 | 架构决策更改 | AI + 人工确认 |
| `techContext.md` | 偶尔 | 技术栈升级 | 人工 |
| `activeContext.md` | 每次会话 | 会话结束时 | AI 自动更新 |
| `progress.md` | 每次会话 | 完成任务时 | AI 自动更新 |

### 6.2 防止 Memory Bank 腐烂

```
Memory 腐烂问题：
  activeContext.md 两周没更新 → 内容过期 → AI 基于过期信息给出错误建议

解决方案：
  1. 在 CLAUDE.md 中强制要求 AI 验证 activeContext 的时效性
  2. 定期（每 Sprint）人工审查 systemPatterns.md 的准确性
  3. 功能完成后立即更新 progress.md，避免累积
```

### 6.3 上下文分级管理

```
第 1 级：项目永久上下文（projectbrief + systemPatterns）
  → 写入 CLAUDE.md，每次自动加载
  → 内容稳定，不频繁变动

第 2 级：会话动态上下文（activeContext + progress）
  → 写入 .memory-bank/，按需读取
  → 每次会话更新

第 3 级：任务临时上下文（当前任务的详细讨论）
  → 在会话中维护，会话结束后压缩精华进入第 2 级
```

---

## 七、延伸学习

- [ ] 为当前项目初始化一套完整的 Memory Bank 文件结构
- [ ] 安装 claude-mem，体验自动记忆捕获和压缩流程
- [ ] 尝试 meridian 的零配置一键增强
- [ ] 探索 CCSwitch 的多工具统一记忆管理

> 🔗 **关联笔记**：
> - [OpenSpec + GStack + Superpowers 综合实战](../combined-practice/openspec-gstack-superpowers-workflow.md)
> - [OpenSpec 核心工作流与最佳实践](../sdd-spec-driven/openspec-best-practices.md)
