# Superpowers Skill 包安装与使用指南

> **所属板块**：一、AI 编程方法论 → AI 编程提效框架 → AI 工具 Skill 能力扩展包
> **学习目标**：掌握 Superpowers 跨工具 Skill 生态，通过安装「插件」扩展 AI 工具能力边界
> **参考资源**：[Superpowers GitHub](https://github.com/anthropics/superpowers)（116k+ ⭐）

---

## 一、什么是 Skill 包？为什么需要它？

### AI 工具的默认能力边界

```
Claude Code 默认能做什么？
  ✅ 读写文件、执行 Bash 命令、调用 MCP Server
  ✅ 理解代码结构、生成实现、Code Review
  ❌ 无法自动执行：浏览器截图、数据库可视化分析
  ❌ 无法自动执行：项目依赖图谱生成、架构图绘制
  ❌ 无法自动执行：性能瓶颈可视化、安全漏洞扫描报告

Skill 包做什么？
  = 给 AI 工具「装插件」，扩展这些默认能力边界
  = 一组预设的 Slash Commands + System Prompts + 工具调用配置
```

---

## 二、Superpowers 生态全景

### 覆盖工具（16 款）

| 工具 | 类型 | Superpowers 支持 |
|------|------|-----------------|
| **Claude Code** | CLI Agent | ✅ 完整支持，最多 Skill |
| **Cursor** | IDE | ✅ Rules for AI 集成 |
| **Windsurf** | IDE | ✅ 支持 |
| **Kiro** | IDE（Amazon）| ✅ 原生支持 |
| **Gemini CLI** | CLI Agent | ✅ 支持 |
| **OpenAI Codex** | CLI Agent | ✅ 支持 |
| CatPaw | IDE（美团）| 🔄 部分兼容 |

### Skill 分类总览

```
📁 superpowers/
├── productivity/        # 生产力增强
│   ├── code-review/     #   深度代码审查
│   ├── documentation/   #   文档自动生成
│   └── refactoring/     #   重构助手
├── testing/             # 测试增强
│   ├── unit-test-gen/   #   单元测试生成
│   ├── e2e-testing/     #   E2E 测试（真实浏览器）
│   └── mutation-testing/#   变异测试
├── architecture/        # 架构分析
│   ├── dep-graph/       #   依赖图谱
│   ├── complexity-map/  #   复杂度热力图
│   └── tech-debt/       #   技术债扫描
├── security/            # 安全审查
│   ├── owasp-scan/      #   OWASP Top10 扫描
│   └── dep-vuln/        #   依赖漏洞检测
└── devops/              # DevOps 增强
    ├── dockerfile-opt/  #   Dockerfile 优化
    └── k8s-advisor/     #   K8s 配置建议
```

---

## 三、安装配置

### 方式一：Claude Code（推荐）

```bash
# 1. 克隆 Superpowers 仓库
git clone https://github.com/anthropics/superpowers.git ~/.superpowers

# 2. 在项目 CLAUDE.md 中声明要使用的 Skill
```

**CLAUDE.md 配置示例：**
```markdown
<!-- CLAUDE.md -->

## 已安装的 Superpowers Skill

### 代码审查增强
使用 @~/.superpowers/productivity/code-review/SKILL.md 中定义的 /deep-review 命令
进行超过标准 Code Review 级别的深度审查，包括：
- 性能热点识别
- 内存泄漏风险
- 并发安全分析
- API 设计评估

### 测试生成
使用 /gen-tests 命令为选定代码自动生成：
- JUnit 5 单元测试（含 Mock 框架）
- Spring Boot Test 集成测试模板
- 边界值测试用例

### 架构分析
使用 /arch-map 生成当前模块的依赖关系图
```

### 方式二：通过 MCP Server 使用

```json
// .claude/mcp_servers.json
{
  "superpowers": {
    "command": "npx",
    "args": ["-y", "@superpowers/mcp-server"],
    "env": {
      "SKILLS_PATH": "/Users/yourname/.superpowers"
    }
  }
}
```

### 方式三：中文版（superpowers-zh）

```bash
# 中文用户推荐使用完整中文版，含 6 个国产原创 Skill
git clone https://github.com/jnMetaCode/superpowers-zh.git ~/.superpowers-zh
```

中文版新增 Skill 亮点：
- `/code-explain-zh`：用中文解释复杂代码逻辑
- `/test-gen-java`：专为 Java/Spring 场景优化的测试生成
- `/perf-analysis-zh`：中文性能分析报告

---

## 四、核心 Skill 使用详解

### 4.1 代码知识图谱工具：CodeGraph vs Graphify

> 代码知识图谱工具解决的核心问题：AI 的"看不见全貌"问题
>
> Claude Code 处理大型代码库时，context 窗口有限，无法一次性"看完"整个项目。
> 知识图谱工具将代码预先索引成符号关系图，AI 查询图谱而非扫描文件。

#### ⭐ 首选：CodeGraph（更成熟、开箱即用）

> [CodeGraph GitHub](https://github.com/colbymchenry/codegraph)
> **~25% cheaper · ~62% fewer tool calls · 100% local**

CodeGraph 是一个完整的 MCP Server，通过 tree-sitter 解析代码库、建立本地 SQLite 知识图谱，直接作为 MCP 工具暴露给 Claude Code 等 AI Agent 使用。

**核心优势：**
- **零配置自动同步**：文件变更后 2 秒内自动更新索引
- **MCP 原生集成**：无需额外 Prompt，工具描述自动注入
- **支持 20+ 语言**：TypeScript / Java / Python / Go / Rust / Kotlin / Swift...
- **框架路由识别**：自动识别 Spring / Django / Express 等 14 个框架的 URL 路由

**安装（一键）：**
```bash
curl -fsSL https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.sh | sh
# 安装器自动配置 Claude Code / Cursor / Codex / Gemini CLI 等
```

**初始化项目：**
```bash
cd your-project
codegraph init -i    # 初始化并建立索引（约 30-60 秒）
```

**AI 使用效果（自动，无需手动调用）：**
```
用户："这个项目的请求是怎么到达数据库的？"

有 CodeGraph：
  AI → codegraph_context("请求处理链路") → 直接得到完整调用关系
  AI → codegraph_trace("handleRequest", "executeQuery") → 逐跳追踪
  → 2 次工具调用，0 次文件读取

无 CodeGraph：
  AI → grep → find → Read × 5 → grep × 3 → Read × 3 ...
  → 20+ 次工具调用
```

> 📖 详细安装配置见：[CodeGraph 安装与实战指南](./codegraph-setup-guide.md)

---

#### Graphify（备选，可视化输出）

> [Graphify GitHub](https://github.com/safishamsi/graphify)（66k ⭐）
> 将项目代码 / SQL Schema / 文档转化为**可视化的知识图谱**

**安装：**
```bash
git clone https://github.com/safishamsi/graphify.git ~/.graphify
```

**使用场景：**
```bash
# 场景 1：新人入职，可视化理解大型代码库
/graphify:map src/
# → 输出：模块依赖图（可视化）、核心类关系图、热点代码区域

# 场景 2：理解数据库 Schema 关系
/graphify:schema database.sql
# → 输出：ER 图（可视化）、外键关系、查询路径分析
```

**选型对比：**

| 维度 | CodeGraph | Graphify |
|------|----------|---------|
| 集成方式 | MCP Server（自动） | Slash Command（手动触发） |
| 输出形式 | 结构化 JSON + 源码片段 | 可视化图谱 |
| 自动同步 | ✅ 文件变更自动更新 | ❌ 需手动重新生成 |
| 语言支持 | 20+ 语言 | 主流语言 |
| 维护状态 | 活跃维护 | 较少更新 |
| 使用门槛 | 低（安装后全自动） | 中（需要手动触发命令）|

**建议：优先使用 CodeGraph，Graphify 作为需要可视化展示时的补充。**

### 4.2 beagle：框架感知代码审查

> [beagle GitHub](https://github.com/existential-birds/beagle)（64 ⭐）
> 145 个框架感知代码审查 Skill（Python / Go / React / iOS）

**安装：**
```bash
git clone https://github.com/existential-birds/beagle.git ~/.beagle
```

**使用场景：**
```bash
# Spring Boot 框架感知审查（了解 Spring 最佳实践的视角）
/beagle:review-spring src/main/java/

# React 组件审查（了解 React 性能优化的视角）
/beagle:review-react src/components/

# Go 并发安全审查
/beagle:review-go-concurrency .
```

框架感知 vs 通用审查的差异：
```
通用审查：「这个代码看起来有个 null 检查缺失」
框架感知：「你用了 @Transactional 但方法是 private，
           Spring AOP 无法代理私有方法，事务不会生效」
```

### 4.3 open-agent-hub：Skill 管理器

> [open-agent-hub GitHub](https://github.com/guanyang/open-agent-hub)（899 ⭐）
> 零依赖的轻量 CLI Skill 管理器

```bash
# 安装
npm install -g open-agent-hub

# 搜索可用 Skill
oah search "code review"

# 安装指定 Skill
oah install superpowers-code-review

# 列出已安装的 Skill
oah list

# 查看 Skill 详情
oah info superpowers-code-review
```

---

## 五、最佳实践

### 5.1 Skill 组合使用策略

```
代码开发完成后的标准 Skill 流水线：

1. /graphify:trace <新功能入口>
   → 确认调用链完整，无遗漏的边缘情况

2. /beagle:review-<框架名> <代码路径>
   → 框架感知的深度审查

3. /gen-tests <代码路径>
   → 自动生成测试用例

4. /dep-vuln check
   → 依赖安全扫描
```

### 5.2 避免 Skill 膨胀

- **原则**：只安装实际使用的 Skill，避免 CLAUDE.md 被大量 Skill 描述撑爆
- **建议**：每个项目维护一个 `skills.yml` 声明文件，按需 activate

```yaml
# .claude/skills.yml
active_skills:
  - superpowers/productivity/code-review   # 每次 PR 必用
  - superpowers/testing/unit-test-gen      # 功能完成后
  - graphify                               # 代码库分析
inactive_skills:
  - superpowers/devops/k8s-advisor        # 暂未上 K8s，先禁用
```

### 5.3 自定义 Skill

如果现有 Skill 不满足需求，可以自定义：

```markdown
<!-- .claude/skills/my-custom-review.md -->
# Custom Java Performance Review Skill

## 触发方式
命令：/perf-review

## 行为描述
以 Java 性能专家视角审查代码，重点关注：
1. 不必要的对象创建（GC 压力）
2. 集合使用不当（ArrayList vs LinkedList 误用）
3. 数据库 N+1 查询（在 JPA/MyBatis 场景）
4. 线程池使用不当
5. 缓存缺失的热点读取

## 输出格式
- 🔴 严重（影响线上性能，必须修复）
- 🟡 中等（建议优化，可在下个版本处理）
- 🟢 建议（最佳实践，低优先级）
```

---

## 六、延伸学习

- [ ] 浏览 Superpowers 仓库，找到 3-5 个最适合自己项目的 Skill 并安装
- [ ] 使用 Graphify 对当前项目生成知识图谱，评估有效性
- [ ] 尝试自定义一个 Skill，解决现有工具链中的某个痛点
- [ ] 探索 beagle 对 Java/Spring 场景的 145 个审查规则

> 🔗 **关联笔记**：
> - [OpenSpec + GStack + Superpowers 综合实战](../combined-practice/openspec-gstack-superpowers-workflow.md)
> - [Memory Bank 结构化上下文方法论](../memory-persistence/memory-bank-pattern.md)
