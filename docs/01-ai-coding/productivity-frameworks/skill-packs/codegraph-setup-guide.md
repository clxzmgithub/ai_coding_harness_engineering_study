# CodeGraph：代码知识图谱 MCP Server 安装与实战指南

> **所属板块**：一、AI 编程方法论 → AI 编程提效框架 → AI 工具 Skill 能力扩展包
> **学习目标**：掌握 CodeGraph 的安装配置，通过代码知识图谱大幅降低 AI Agent 的 token 消耗和工具调用次数
> **参考资源**：[CodeGraph GitHub](https://github.com/colbymchenry/codegraph)（本地已 clone：`~/Documents/GitHub/https_clone/codegraph`）
> **核心价值**：~25% cheaper · ~62% fewer tool calls · 100% local

---

## 一、CodeGraph 是什么？解决什么问题？

### AI Agent 探索代码库的传统痛点

```
不使用 CodeGraph 时，Claude Code 如何探索代码库：

用户："这个项目的请求是怎么到达数据库的？"

AI 的行为：
  1. ls / find 扫描目录结构（工具调用 #1-3）
  2. grep -r "request" 全文搜索（工具调用 #4-8）
  3. Read Controller.java（工具调用 #9）
  4. Read Service.java（工具调用 #10）
  5. Read Repository.java（工具调用 #11）
  6. grep "method calls" 追踪调用链（工具调用 #12-14）
  ...最终消耗 20+ 工具调用、数百万 token
```

```
使用 CodeGraph 后：

AI 的行为：
  1. codegraph_context("请求到数据库的调用链")（工具调用 #1）
     → 直接返回：Controller → Service → Repository 的完整符号图，
       包括每个方法的源码、调用关系、继承实现关系
  2. codegraph_trace("handleRequest", "executeQuery")（工具调用 #2）
     → 两跳之间的完整路径，每一跳带源码
  → 结束！0 次文件读取，0 次 grep
```

### Benchmark 数据（真实测试）

在 7 个真实开源项目上，使用 Claude Opus 4.8 headless 模式测试：

| 项目 | 语言 | 费用节省 | Token 减少 | 速度提升 | 工具调用减少 |
|------|------|---------|-----------|---------|------------|
| VS Code | TypeScript ~10k 文件 | 33% | 70% | 27% | **80%** |
| Django | Python ~3k 文件 | 23% | 70% | 28% | 77% |
| Tokio | Rust ~790 文件 | 35% | 70% | 37% | 79% |
| OkHttp | Java ~645 文件 | 11% | 48% | 26% | 70% |
| Gin | Go ~110 文件 | 15% | 35% | 9% | 47% |
| **平均** | | **25%** | **57%** | **23%** | **62%** |

> 核心机制：有了知识图谱，AI 直接查询符号关系而不是扫描文件——一次 codegraph 调用顶替了原本 10-20 次 grep/read 调用。

---

## 二、工作原理

```
┌─────────────────────────────────────────────────────────┐
│                      Claude Code                         │
│   "How does a request reach the database?"               │
│       → 直接调用 CodeGraph MCP 工具                       │
└─────────────────────┬───────────────────────────────────┘
                      │ MCP 协议
                      ▼
┌─────────────────────────────────────────────────────────┐
│                  CodeGraph MCP Server                    │
│  context · trace · explore · callers · callees · impact  │
│                      ↓                                   │
│              SQLite 知识图谱                              │
│    symbols · edges · files · FTS5 全文搜索                │
└─────────────────────────────────────────────────────────┘
```

**构建流程：**
1. **提取**：`tree-sitter` 解析源代码 AST → 提取符号（函数、类、方法）和边（调用、继承、导入）
2. **存储**：存入本地 SQLite（`.codegraph/codegraph.db`），FTS5 全文搜索
3. **解析**：解析引用关系：函数调用 → 定义、导入 → 源文件、类继承、框架路由
4. **自动同步**：文件监听器（FSEvents/inotify）监听文件变更，2 秒防抖后自动增量同步

**支持 20+ 语言：** TypeScript / JavaScript / Java / Python / Go / Rust / C# / PHP / Ruby / C / C++ / Swift / Kotlin / Dart / Scala / Svelte / Vue 等

---

## 三、安装配置

### 方式一：一键安装（推荐）

```bash
# macOS / Linux
curl -fsSL https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.sh | sh

# Windows (PowerShell)
irm https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.ps1 | iex
```

安装器会自动：
- 检测已安装的 AI 工具（Claude Code / Cursor / Codex / Gemini CLI / Kiro 等）
- 写入各工具的 MCP Server 配置
- 设置 Claude Code 的 auto-allow 权限（不需要每次确认）
- 初始化当前项目

### 方式二：npm 安装

```bash
# 零安装（临时使用）
npx @colbymchenry/codegraph

# 全局安装（推荐）
npm i -g @colbymchenry/codegraph
```

### 方式三：从本地源码安装（用于源码学习）

```bash
# 已 clone 到本地：~/Documents/GitHub/https_clone/codegraph

cd ~/Documents/GitHub/https_clone/codegraph
npm install
npm run build

# 链接到全局
npm link

# 验证
codegraph --version
```

### 手动配置 Claude Code（如果自动安装未生效）

编辑 `~/.claude.json`（全局配置）或项目的 `.claude/settings.json`：

```json
{
  "mcpServers": {
    "codegraph": {
      "type": "stdio",
      "command": "codegraph",
      "args": ["serve", "--mcp"]
    }
  }
}
```

设置 auto-allow（避免每次工具调用都询问）：

```json
{
  "permissions": {
    "allow": [
      "mcp__codegraph__codegraph_search",
      "mcp__codegraph__codegraph_context",
      "mcp__codegraph__codegraph_trace",
      "mcp__codegraph__codegraph_callers",
      "mcp__codegraph__codegraph_callees",
      "mcp__codegraph__codegraph_impact",
      "mcp__codegraph__codegraph_explore",
      "mcp__codegraph__codegraph_node",
      "mcp__codegraph__codegraph_status",
      "mcp__codegraph__codegraph_files"
    ]
  }
}
```

---

## 四、项目初始化

```bash
# 进入项目目录
cd your-project

# 初始化并立即建立索引（推荐）
codegraph init -i

# 仅初始化（之后手动建立索引）
codegraph init
codegraph index
```

初始化后会在项目根目录创建 `.codegraph/` 目录（加入 `.gitignore`）：

```
project/
└── .codegraph/
    └── codegraph.db     # SQLite 知识图谱索引
```

### 验证安装

```bash
# 检查索引状态
codegraph status

# 示例输出：
# Files: 1,247 indexed
# Nodes: 48,392 (functions: 12,847 · methods: 18,293 · classes: 3,921 ...)
# Edges: 198,471 (calls: 87,231 · contains: 65,442 · imports: 31,208 ...)
# Index health: ✅ OK
# Journal: wal
# Last sync: 2 seconds ago
```

---

## 五、MCP 工具使用详解

CodeGraph 向 AI Agent 暴露 10 个 MCP 工具，**无需在 CLAUDE.md 中额外配置**——工具描述由 MCP Server 在 `initialize` 时自动注入。

### 工具速查表

| 工具 | 用途 | 典型场景 |
|------|------|---------|
| `codegraph_search` | 按名称搜索符号 | 找到某个类/方法的位置 |
| `codegraph_context` | 为任务构建相关代码上下文 | 理解某功能涉及哪些代码 |
| `codegraph_trace` | 追踪两个符号之间的调用路径 | "A 怎么调用到 B" |
| `codegraph_callers` | 找所有调用某函数的地方 | 修改前的影响范围 |
| `codegraph_callees` | 找某函数调用了哪些函数 | 理解函数的依赖 |
| `codegraph_impact` | 修改某符号影响哪些代码 | 重构前的风险评估 |
| `codegraph_explore` | 返回多个相关符号的源码+关系 | 深入理解某个子系统 |
| `codegraph_node` | 获取单个符号的详细信息 | 查看某方法的完整源码 |
| `codegraph_files` | 获取已索引的文件结构 | 替代 ls/find 命令 |
| `codegraph_status` | 检查索引健康状态 | 确认索引是最新的 |

### 工具使用示例

```
// 场景：理解某个功能的完整实现

// Step 1: 搜索入口点
codegraph_search("UserAuthentication")
→ 返回：UserAuthController (class) · UserAuthService (class) · IAuthRepository (interface)

// Step 2: 追踪登录调用链
codegraph_trace("UserAuthController.login", "JwtTokenService.generate")
→ 返回：
  hop 1: UserAuthController.login → UserAuthService.authenticate [calls]
    源码：public Result<UserVO> login(@RequestBody LoginRequest req) { ... }
  hop 2: UserAuthService.authenticate → JwtTokenService.generate [calls]
    源码：public UserVO authenticate(LoginRequest req) { ... }

// Step 3: 修改前评估影响范围
codegraph_impact("UserAuthService.authenticate")
→ 返回：受影响的 23 个符号，分布在 8 个文件
  UserAuthController.login ← 直接调用者
  AdminAuthController.adminLogin ← 直接调用者
  OAuth2Service.linkAccount ← 间接影响
  ...

// Step 4: 深入理解认证子系统
codegraph_explore("UserAuthService JwtTokenService TokenBlacklistService")
→ 返回：这三个类的完整源码 + 它们之间的调用关系图
```

---

## 六、CLI 命令参考

```bash
# 初始化与索引管理
codegraph init [path]           # 初始化项目
codegraph init -i [path]        # 初始化 + 立即建立索引
codegraph index [path]          # 全量重建索引（--force 强制，--quiet 静默）
codegraph sync [path]           # 增量更新
codegraph status [path]         # 查看索引统计和健康状态
codegraph uninit [path]         # 删除 .codegraph/ 目录

# 查询工具（不需要 AI，可直接用）
codegraph query <搜索词>         # 搜索符号（--kind 过滤类型，--json 输出 JSON）
codegraph callers <符号名>       # 查找调用者
codegraph callees <符号名>       # 查找被调用者
codegraph impact <符号名>        # 影响分析（--depth 深度）
codegraph context <任务描述>     # 构建 AI 上下文（--format markdown/json）
codegraph files [path]          # 展示文件结构（比 find 快）

# 测试影响分析（CI 友好）
codegraph affected src/Service.java  # 找受影响的测试文件
git diff --name-only | codegraph affected --stdin  # 从 git diff 管道输入

# MCP Server 管理
codegraph serve --mcp           # 手动启动 MCP Server（通常由 AI 工具自动管理）
codegraph install               # 重新运行安装器
codegraph uninstall             # 从所有 AI 工具移除配置
```

### 实用 CI/CD 集成

```bash
#!/usr/bin/env bash
# pre-push hook：只运行受影响的测试

AFFECTED=$(git diff --name-only HEAD | codegraph affected --stdin --quiet)
if [ -n "$AFFECTED" ]; then
  echo "运行受影响的测试：$AFFECTED"
  npx vitest run $AFFECTED
else
  echo "无受影响的测试文件"
fi
```

---

## 七、框架路由识别

CodeGraph 能识别 14 个 web 框架的路由文件，将 URL 路径链接到 Handler：

| 框架 | 语言 | 识别方式 |
|------|------|---------|
| **Spring** | Java | `@GetMapping`, `@PostMapping`, `@RequestMapping` |
| **Django** | Python | `path()`, `re_path()` in `urls.py` |
| **FastAPI** | Python | `@app.get(...)`, `@router.post(...)` |
| **Flask** | Python | `@app.route(...)` |
| **Express** | Node.js | `app.get(...)`, `router.post(...)` |
| **NestJS** | Node.js | `@Controller` + `@Get/@Post` |
| **Gin** | Go | `r.GET(...)`, `router.HandleFunc(...)` |
| **Laravel** | PHP | `Route::get()`, `Route::resource()` |

**实际效果：**
```
// 询问："/api/users/{id}" 这个接口是哪个方法处理的？

// 没有 CodeGraph：AI grep "users" → 找到几十个文件 → 逐个读...
// 有了 CodeGraph：
codegraph_search("/api/users/{id}", kind="route")
→ 返回：UserController.getUser (method) · 路由节点 /api/users/{id} → handler
```

---

## 八、自动同步机制

CodeGraph 的零配置自动同步是其核心特性之一：

```
编辑 src/UserService.java
  → 文件系统监听器触发（< 100ms）
  → 防抖等待（默认 2 秒）
  → 增量重新索引 UserService.java
  → 下一次 codegraph 查询立即看到最新状态
```

**Staleness Banner（过渡保护）：**
在防抖窗口内，如果查询涉及正在等待同步的文件，MCP 响应会自动添加 `⚠️` 提示，告知 AI 直接读取该文件，确保不会基于过期索引给出错误答案。

---

## 九、与其他工具的对比

| 工具 | 定位 | 优势 | 劣势 |
|------|------|------|------|
| **CodeGraph** | 代码结构知识图谱 MCP Server | 精确的符号/调用关系，支持 20+ 语言，零配置自动同步 | 需要初始化建立索引 |
| **Graphify** | 代码库知识图谱 Skill | 可视化输出，包含 SQL Schema | 需要额外配置，维护较少 |
| **Cody** | 代码库 AI 搜索 | 语义搜索，Cloud 模式 | 需要云端连接，隐私问题 |
| **grep/Bash** | 原始文本搜索 | 零依赖，精确匹配 | 不理解语义，只有文本 |

**选择建议：**
- **首选 CodeGraph**：免费、本地、精准、支持主流语言、自动同步
- Graphify 作为备选：需要可视化图谱输出时

---

## 十、从源码学习 CodeGraph

本地已 clone：`~/Documents/GitHub/https_clone/codegraph`

### 源码架构一览

```
src/
├── index.ts              # 公开 API：CodeGraph 类（init/open/close/indexAll/sync...）
├── extraction/           # 提取层：tree-sitter 解析 → 符号 + 边
│   ├── ExtractionOrchestrator.ts
│   ├── tree-sitter.ts    # 核心提取逻辑（extractMethod/extractCall/extractInheritance）
│   └── languages/        # 每语言一个提取器（java.ts / go.ts / typescript.ts ...）
├── resolution/           # 解析层：名称解析 + 框架路由识别
│   ├── ReferenceResolver.ts
│   ├── import-resolver.ts
│   └── frameworks/       # Express/Django/Spring/Gin... 框架路由识别器
├── graph/                # 图查询层
│   ├── GraphTraverser.ts # BFS/DFS / 影响半径 / 路径查找
│   └── GraphQueryManager.ts
├── context/              # 上下文构建：AI 消费的 markdown/JSON 输出
├── db/                   # SQLite 数据库层（FTS5 全文搜索）
│   └── schema.sql        # 表结构：nodes / edges / files / fts_nodes
├── mcp/                  # MCP Server 实现
│   ├── MCPServer.ts
│   ├── tools.ts          # 10 个 MCP 工具的实现
│   └── server-instructions.ts  # 注入给 AI Agent 的工具使用指南
└── sync/                 # 文件监听 + 增量同步
    └── FileWatcher.ts    # FSEvents/inotify/RDCW 封装
```

### 值得深入研究的设计点

1. **`server-instructions.ts`**：MCP Server 在 `initialize` 时向 AI 注入的使用指南——这是 CodeGraph 控制 AI 行为的唯一入口（不写 CLAUDE.md），很有参考价值
2. **`tools.ts` 的 `getExploreBudget()`**：根据项目文件数量动态调整输出大小，防止大型项目 context 溢出
3. **`callback-synthesizer.ts`**：如何识别动态调用（EventEmitter、React setState → render）并合成"虚拟边"，解决静态解析的固有局限
4. **`__tests__/evaluation/`**：Eval 框架——用真实代码库评估知识图谱质量的方法论，值得借鉴到 Harness 工程实践中

---

## 十一、延伸学习

- [ ] 安装 CodeGraph，在当前 Java 项目上跑一次 `codegraph init -i`，观察索引建立过程
- [ ] 用 `codegraph status` 查看 Java 代码库的符号统计（nodes/edges 分布）
- [ ] 开启 Claude Code，对话中让 AI 回答 "xxx 方法是怎么被调用的"，观察是否使用了 codegraph 工具
- [ ] 阅读 `src/mcp/server-instructions.ts`，理解 CodeGraph 如何"教"AI 使用工具
- [ ] 阅读 `src/extraction/languages/java.ts`，理解 Java 符号提取的实现
- [ ] 对比研究 `docs/benchmarks/` 下的 Benchmark 分析文档

> 🔗 **关联笔记**：
> - [Superpowers Skill 包安装与使用指南](./superpowers-install-guide.md)（含 Graphify 介绍）
> - [OpenSpec + GStack + Superpowers 综合实战](../combined-practice/openspec-gstack-superpowers-workflow.md)
> - [Claude Code 实战 —— Harness 工程之道](../../03-harness/claude-code-harness-practice/claude-code-harness-engineering.md)
