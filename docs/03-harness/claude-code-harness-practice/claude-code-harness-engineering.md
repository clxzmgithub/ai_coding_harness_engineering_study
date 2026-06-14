# Claude Code 实战 —— Harness 工程之道

> **所属板块**：三、Harness 工程理论 → Claude Code 实战篇
> **核心目标**：将 Harness 工程（测试线束 / Eval / LLM-as-Judge）的理论，
>             通过 Claude Code 这款工具本身的实践载体，形成可落地的工程化方法论
> **定位**：本文是「理论 Harness」与「AI Coding 工具实战」的**交汇点**

---

## 一、为什么需要 AI Coding 的 Harness 工程？

### 传统软件的质量保障 vs AI 生成代码的质量保障

```
传统软件质量保障：
  代码由人类写 → 人类 Code Review → 自动化测试 → CI/CD 门禁 → 上线

AI 生成代码的新挑战：
  代码由 AI 生成 → 人类难以完全 Review（量大速快）→ 
  AI 可能自信地生成看似正确但有细微错误的代码 →
  传统单元测试覆盖不了「AI 行为的非确定性」
  
新增的风险维度：
  1. 语义正确但功能错误（代码能跑，但业务逻辑有误）
  2. 风格漂移（同一项目不同会话生成的代码风格不一致）
  3. 架构决策违背（AI 忘记了架构约束，绕过了规范）
  4. 幻觉引入（AI 伪造了不存在的 API 或库）
```

### Harness 工程在 AI Coding 场景的新含义

```
传统 Harness：为软件提供测试线束（测试框架 + 数据 + 断言）

AI Coding Harness（扩展定义）：
  ┌─────────────────────────────────────────────────┐
  │ Layer 1: 代码质量 Harness（传统）                │
  │   单元测试 / 集成测试 / 代码覆盖率                │
  ├─────────────────────────────────────────────────┤
  │ Layer 2: AI 行为一致性 Harness（新增）            │
  │   验证 AI 是否遵守架构约束和代码规范              │
  ├─────────────────────────────────────────────────┤
  │ Layer 3: AI 输出评测 Harness（Eval）              │
  │   LLM-as-Judge 评估 AI 生成代码的质量             │
  ├─────────────────────────────────────────────────┤
  │ Layer 4: AI 工具效果 Harness                     │
  │   评估 Claude Code 等工具在项目中的实际效果        │
  └─────────────────────────────────────────────────┘
```

---

## 二、Layer 1：代码质量 Harness —— 用 Claude Code 构建

### 2.1 让 Claude Code 生成高质量测试

**核心原则**：不只是让 AI 写代码，同时让 AI 写能「咬住」自己的测试。

```markdown
<!-- 在 CLAUDE.md 中配置测试规范 -->

## 测试生成规范
每次实现新功能时，必须同时生成对应的测试。测试要求：
1. 覆盖率：核心业务逻辑 ≥ 80% 行覆盖率
2. 测试粒度：每个 public 方法至少 3 个测试用例（正常路径 + 边界 + 异常）
3. 测试命名：`方法名_场景描述_预期结果`
4. 禁止 @Disabled 注解（除非有明确注释说明原因）
5. 测试数据：使用 @ParameterizedTest 而非重复复制测试方法
```

**典型提示词模板：**
```
请为 [服务类名] 生成完整的单元测试，要求：
1. 测试框架：JUnit 5 + Mockito 5.x
2. 覆盖以下场景：正常流程、边界值、空值处理、异常分支
3. 特别测试幂等性：相同输入多次调用结果一致
4. 特别测试并发安全性（如果方法有共享状态）
生成后请说明：哪些场景没有被覆盖，以及原因
```

### 2.2 Claude Code Hooks 构建 CI 质量门禁

利用 Claude Code 的 Hooks 机制，在 AI 生成代码后自动运行质量检查：

```json
// .claude/hooks.json
{
  "PostToolUse": [
    {
      "matcher": "Write|Edit|MultiEdit",
      "hooks": [
        {
          "type": "command",
          "command": "bash .claude/scripts/quality-gate.sh $CLAUDE_FILE_PATH",
          "blocking": true,
          "failOnError": true
        }
      ]
    }
  ]
}
```

```bash
#!/bin/bash
# .claude/scripts/quality-gate.sh
# 每次 Claude Code 写入/编辑文件后自动执行的质量门禁

FILE_PATH=$1
echo "🔍 质量门禁检查：$FILE_PATH"

# 1. 编译检查（防止 AI 生成不能编译的代码）
if [[ $FILE_PATH == *.java ]]; then
  javac -cp . "$FILE_PATH" 2>/dev/null || {
    echo "❌ 编译失败，AI 生成了语法错误的代码"
    exit 1
  }
fi

# 2. 代码规范检查（Checkstyle）
checkstyle -c .checkstyle.xml "$FILE_PATH" || {
  echo "❌ 代码规范检查失败"
  exit 1
}

# 3. 架构约束检查（自定义脚本）
bash .claude/scripts/arch-constraint-check.sh "$FILE_PATH" || {
  echo "❌ 违反架构约束"
  exit 1
}

echo "✅ 质量门禁通过"
```

```bash
#!/bin/bash
# .claude/scripts/arch-constraint-check.sh
# 检查 AI 是否违反了架构约束

FILE_PATH=$1

# 约束1：Controller 层不能直接访问 Repository
if [[ $FILE_PATH == *Controller.java ]]; then
  if grep -q "Repository\|Mapper" "$FILE_PATH"; then
    echo "❌ 架构违规：Controller 直接访问 Repository，必须通过 Service 层"
    exit 1
  fi
fi

# 约束2：禁止使用 Lombok（团队规范）
if grep -q "@Data\|@Getter\|@Setter\|@Builder" "$FILE_PATH"; then
  echo "❌ 架构违规：禁止使用 Lombok 注解（团队规范）"
  exit 1
fi

# 约束3：返回类型必须是 Result<T>（禁止直接返回实体）
if [[ $FILE_PATH == *Controller.java ]]; then
  if grep -qP "public\s+(?!Result)" "$FILE_PATH" | grep -q "@GetMapping\|@PostMapping"; then
    echo "⚠️ 警告：Controller 方法返回值建议使用 Result<T> 包装"
  fi
fi

echo "✅ 架构约束检查通过"
```

---

## 三、Layer 2：AI 行为一致性 Harness

### 3.1 规范一致性测试

**目标**：验证 Claude Code 在不同会话中，始终遵守 CLAUDE.md 中定义的规范。

```python
# eval/consistency_test.py
# 使用 Claude Code CLI 批量运行一致性测试

import subprocess
import json
from pathlib import Path

CONSISTENCY_TESTS = [
    {
        "name": "Controller 返回值规范",
        "prompt": "为 UserController 新增一个 /api/users/{id} GET 接口",
        "expected_pattern": r"Result<",
        "forbidden_pattern": r"ResponseEntity",
        "description": "Controller 必须返回 Result<T>，不能用 ResponseEntity"
    },
    {
        "name": "无 Lombok 约束",
        "prompt": "创建 UserDTO 数据传输对象，包含 id、name、email 字段",
        "forbidden_pattern": r"@Data|@Getter|@Builder",
        "description": "禁止使用 Lombok，必须手写 getter/setter"
    },
    {
        "name": "异常处理规范",
        "prompt": "在 UserService 的 findById 方法中处理用户不存在的情况",
        "expected_pattern": r"throw new \w+Exception",
        "forbidden_pattern": r"return null",
        "description": "不能返回 null，必须抛出自定义异常"
    }
]

def run_consistency_test(test_case: dict) -> dict:
    """运行单个一致性测试"""
    result = subprocess.run(
        ["claude", "-p", test_case["prompt"], "--output-format", "json"],
        capture_output=True, text=True, cwd="."
    )
    
    generated_code = result.stdout
    passed = True
    issues = []
    
    import re
    if "expected_pattern" in test_case:
        if not re.search(test_case["expected_pattern"], generated_code):
            passed = False
            issues.append(f"缺少必要模式：{test_case['expected_pattern']}")
    
    if "forbidden_pattern" in test_case:
        if re.search(test_case["forbidden_pattern"], generated_code):
            passed = False
            issues.append(f"包含禁止模式：{test_case['forbidden_pattern']}")
    
    return {
        "test": test_case["name"],
        "passed": passed,
        "issues": issues,
        "description": test_case["description"]
    }

if __name__ == "__main__":
    results = [run_consistency_test(t) for t in CONSISTENCY_TESTS]
    
    passed = sum(1 for r in results if r["passed"])
    total = len(results)
    
    print(f"\n=== AI 行为一致性测试报告 ===")
    print(f"通过率：{passed}/{total} ({100*passed//total}%)\n")
    
    for r in results:
        status = "✅" if r["passed"] else "❌"
        print(f"{status} {r['test']}")
        for issue in r.get("issues", []):
            print(f"   └─ {issue}")
```

### 3.2 架构漂移检测

随着会话增多，AI 可能逐渐引入与初始架构不一致的代码。定期运行漂移检测：

```bash
#!/bin/bash
# .claude/scripts/arch-drift-detect.sh
# 检测 AI 生成代码是否产生了架构漂移

echo "🔍 架构漂移检测报告"
echo "==================="

# 检测1：包结构违规
echo "1. 检查包结构..."
violations=$(find src/main/java -name "*.java" | while read file; do
  # Controller 文件不应该在 service 包下
  if [[ $file == */service/*Controller* ]]; then
    echo "  ⚠️ Controller 出现在 service 包：$file"
  fi
  # Repository 接口不应该在 controller 包下
  if [[ $file == */controller/*Repository* ]]; then
    echo "  ⚠️ Repository 出现在 controller 包：$file"
  fi
done)
echo "$violations" || echo "  ✅ 包结构正常"

# 检测2：循环依赖
echo "2. 检查循环依赖..."
mvn dependency:analyze 2>/dev/null | grep "WARN\|circular" || echo "  ✅ 无循环依赖"

# 检测3：禁用的依赖引入
echo "3. 检查禁止引入的依赖..."
if grep -r "import com.google.common.base.Objects" src/ 2>/dev/null; then
  echo "  ⚠️ 使用了 Guava Objects（应使用 java.util.Objects）"
fi

echo "==================="
echo "检测完成"
```

---

## 四、Layer 3：LLM-as-Judge —— 用 AI 评估 AI 的输出

### 4.1 核心理念

```
传统评测：人工 Review（慢 + 主观 + 不可扩展）
LLM-as-Judge：用另一个 LLM 系统性地评估 Claude Code 的输出质量

适用场景：
  → 批量评测（几十个生成样本，人工 Review 不现实）
  → 标准化评分（固定评分维度，减少主观差异）
  → 持续监测（每次发版前自动运行评测）
```

### 4.2 评测维度设计

| 维度 | 权重 | 评分标准 | 检测方法 |
|------|------|---------|---------|
| **功能正确性** | 40% | 代码逻辑是否正确实现需求 | 运行测试 + LLM 语义审查 |
| **规范一致性** | 25% | 是否遵守项目编码规范 | 静态分析 + Pattern 匹配 |
| **安全性** | 20% | 有无明显安全漏洞 | OWASP 规则 + LLM 安全审查 |
| **可读性** | 10% | 代码可读性和注释质量 | LLM 主观评估 |
| **测试质量** | 5% | 生成测试的有效性 | 变异测试覆盖率 |

### 4.3 LLM-as-Judge 实现

```python
# eval/llm_judge.py
# 使用 Anthropic API 作为 Judge 评估 Claude Code 的代码生成质量

import anthropic
import json

client = anthropic.Anthropic()

JUDGE_SYSTEM_PROMPT = """你是一位资深 Java 工程师，负责评估 AI 生成代码的质量。
评估时严格按照评分维度打分，不带主观偏见。
每个维度满分 10 分，最终给出加权总分和详细改进建议。"""

JUDGE_PROMPT_TEMPLATE = """
请评估以下 AI 生成的 Java 代码：

## 评估背景
- 项目类型：Spring Boot 3.2 后端服务
- 需求描述：{requirement}

## 待评估代码
```java
{generated_code}
```

## 评估维度（按以下格式严格输出 JSON）：
{{
  "scores": {{
    "functional_correctness": {{
      "score": <0-10>,
      "reasoning": "<评分理由>",
      "issues": ["<具体问题1>", "<具体问题2>"]
    }},
    "spec_compliance": {{
      "score": <0-10>,
      "reasoning": "<评分理由>",
      "violations": ["<违规项1>"]
    }},
    "security": {{
      "score": <0-10>,
      "reasoning": "<评分理由>",
      "vulnerabilities": ["<漏洞描述>"]
    }},
    "readability": {{
      "score": <0-10>,
      "reasoning": "<评分理由>"
    }},
    "test_quality": {{
      "score": <0-10>,
      "reasoning": "<评分理由>"
    }}
  }},
  "weighted_score": <加权总分，满分10>,
  "summary": "<总结评语>",
  "top_improvements": ["<最重要的改进建议1>", "<改进建议2>", "<改进建议3>"]
}}
"""

def judge_code(requirement: str, generated_code: str) -> dict:
    """使用 Claude 作为 Judge 评估生成的代码"""
    
    response = client.messages.create(
        model="claude-opus-4-5",
        max_tokens=2048,
        system=JUDGE_SYSTEM_PROMPT,
        messages=[{
            "role": "user",
            "content": JUDGE_PROMPT_TEMPLATE.format(
                requirement=requirement,
                generated_code=generated_code
            )
        }]
    )
    
    raw_text = response.content[0].text
    # 提取 JSON 部分
    import re
    json_match = re.search(r'\{.*\}', raw_text, re.DOTALL)
    if json_match:
        return json.loads(json_match.group())
    return {"error": "解析失败", "raw": raw_text}


def run_eval_suite(test_cases: list) -> dict:
    """批量运行评测套件"""
    results = []
    
    for case in test_cases:
        print(f"评测中：{case['name']}...")
        score_result = judge_code(case["requirement"], case["generated_code"])
        results.append({
            "name": case["name"],
            "result": score_result
        })
    
    # 汇总统计
    all_scores = [r["result"].get("weighted_score", 0) for r in results]
    avg_score = sum(all_scores) / len(all_scores) if all_scores else 0
    
    return {
        "average_score": round(avg_score, 2),
        "total_cases": len(results),
        "details": results
    }


# 示例评测套件
EVAL_SUITE = [
    {
        "name": "用户查询接口",
        "requirement": "实现 GET /api/users/{id} 接口，返回用户信息，用户不存在时返回 404",
        "generated_code": """
@GetMapping("/{id}")
public Result<UserVO> getUser(@PathVariable Long id) {
    UserVO user = userService.findById(id);
    return Result.success(user);
}
"""
    }
    # 更多测试用例...
]

if __name__ == "__main__":
    report = run_eval_suite(EVAL_SUITE)
    print(f"\n=== LLM-as-Judge 评测报告 ===")
    print(f"平均分：{report['average_score']}/10.0")
    print(f"评测用例：{report['total_cases']} 个")
    
    for detail in report["details"]:
        result = detail["result"]
        score = result.get("weighted_score", "N/A")
        summary = result.get("summary", "")
        print(f"\n📊 {detail['name']}")
        print(f"   综合评分：{score}/10")
        print(f"   总结：{summary}")
        for imp in result.get("top_improvements", []):
            print(f"   💡 {imp}")
```

---

## 五、Layer 4：AI 工具效果 Harness —— 评估 Claude Code 的价值

### 5.1 效率指标收集

```bash
#!/bin/bash
# .claude/scripts/productivity-tracker.sh
# 追踪 Claude Code 使用前后的效率指标

# 指标1：代码生产速度（LOC/天）
git log --since="30 days ago" --pretty=format:"%ad" --date=short |
  sort | uniq -c |
  awk '{ total += $1; days++ } END { print "平均 LOC/天：" total/days }'

# 指标2：Bug 修复率（生成代码的 Bug vs 人工代码的 Bug）
echo "=== Bug 密度分析 ==="
git log --since="30 days ago" --grep="fix\|bug" --oneline | wc -l

# 指标3：Code Review 迭代次数（越少越好）
echo "=== PR Review 迭代分析 ==="
gh pr list --state closed --json reviewDecision,comments |
  python3 -c "
import json, sys
prs = json.load(sys.stdin)
avg_comments = sum(len(pr['comments']) for pr in prs) / len(prs) if prs else 0
print(f'平均 Review 评论数：{avg_comments:.1f}')
"
```

### 5.2 Claude Code 使用质量 Dashboard

```markdown
## Claude Code 月度效果报告模板

| 指标 | 本月 | 上月 | 趋势 |
|------|------|------|------|
| AI 生成代码占比 | -% | -% | - |
| 一次性通过 Code Review 率 | -% | -% | - |
| AI 生成代码 Bug 密度（per 1000 LOC）| - | - | - |
| 平均功能交付周期（天）| - | - | - |
| CLAUDE.md 规范遵从率 | -% | -% | - |

### 本月 AI Coding 问题 Top3
1. [问题描述]：出现 N 次，解决方案：[...]
2. [问题描述]：出现 N 次，解决方案：[...]
3. [问题描述]：出现 N 次，解决方案：[...]

### CLAUDE.md 优化建议
基于本月问题，建议新增以下规范：
- [ ] [规范描述]：因为 [问题原因]
```

---

## 六、Harness 工程的完整 Claude Code 配置示例

### CLAUDE.md 的 Harness 专属配置段

```markdown
<!-- CLAUDE.md - Harness 工程配置段 -->

## 🧪 代码质量门禁

### 测试要求（强制）
每次实现功能后，**必须**同时生成：
1. 单元测试（JUnit 5 + Mockito，覆盖正常/边界/异常三类场景）
2. 针对幂等方法的幂等性测试
3. 针对有并发共享状态的线程安全测试

### 验收标准
- 所有新增代码的行覆盖率 ≥ 80%
- 0 个 @Disabled 测试（禁止绕过）
- Checkstyle 0 violations

### 架构约束（不可违反）
| 约束 | 规则 |
|------|------|
| 分层 | Controller → Service → Domain → Repository，不可跨层 |
| 返回值 | 所有 Controller 方法返回 Result<T> |
| 异常 | 不允许返回 null，必须抛出自定义异常 |
| 禁用 | Lombok、直接 new Thread()、System.out.println |

### Hooks 自动化
每次文件写入后自动运行 `.claude/scripts/quality-gate.sh`。
如果质量门禁失败，请说明原因并修复，不得绕过。

## 📊 Eval 记录
每当出现规范违反时，在 `.claude/eval-log.md` 记录：
- 时间、任务描述、违规类型、修复方案
这些记录用于持续优化 CLAUDE.md 规范。
```

---

## 七、实战案例：从 0 搭建 Harness 体系

### 7.1 Week 1：基础层（Layer 1）

```bash
# 目标：建立代码质量基线

# Day 1: 初始化 Hooks
mkdir -p .claude/scripts
cat > .claude/hooks.json << 'EOF'
{"PostToolUse": [{"matcher": "Write|Edit", "hooks": [{"type": "command", "command": "bash .claude/scripts/quality-gate.sh", "blocking": true}]}]}
EOF

# Day 2: 编写架构约束检查脚本（参考 Section 2.2）

# Day 3: 在 CLAUDE.md 中明确测试规范（参考 Section 6）

# Day 4-5: 让 Claude Code 为现有代码补全测试，验证 Hooks 效果
```

### 7.2 Week 2：行为一致性层（Layer 2）

```bash
# 目标：建立规范遵从率基线

# Day 1-2: 编写一致性测试用例（参考 Section 3.1）
# Day 3-4: 运行基线测试，记录初始遵从率
# Day 5: 根据结果优化 CLAUDE.md 中的规范描述
```

### 7.3 Week 3：LLM-as-Judge（Layer 3）

```bash
# 目标：建立代码质量评分基线

# Day 1-2: 搭建 LLM Judge 脚本（参考 Section 4.3）
# Day 3: 选取 10 个历史生成的代码样本，运行评测
# Day 4-5: 分析评测结果，优化提示词和 CLAUDE.md
```

### 7.4 持续运行

```bash
# 建议加入 CI/CD 的 Harness 检查

# .github/workflows/ai-harness.yml
name: AI Coding Harness
on: [push, pull_request]
jobs:
  consistency-test:
    steps:
      - name: AI 行为一致性测试
        run: python eval/consistency_test.py
      - name: 架构漂移检测
        run: bash .claude/scripts/arch-drift-detect.sh
```

---

## 八、常见问题与最佳实践

**Q：Hooks 的 `blocking: true` 会不会让 Claude Code 变慢？**

> A：会增加约 5-15 秒的等待（视检查脚本复杂度）。
> 建议：编译检查必须 blocking，但代码规范检查可以设为 non-blocking（只打印警告）。
> 原则：只有「如果不修复就不该继续」的检查才设 blocking。

**Q：LLM-as-Judge 的评分客观吗？不同模型给分差异大吗？**

> A：LLM-as-Judge 的评分主观性真实存在。缓解方法：
> 1. 固定 Judge 模型版本（不要随着模型升级自动更换 Judge）
> 2. 使用多个 Judge 评分取平均（Claude + GPT-4o，降低单一模型偏见）
> 3. 建立人工校准集（20-30 个有明确好坏判断的样本），定期验证 Judge 准确率

**Q：这套 Harness 体系维护成本高吗？**

> A：初始搭建约需 3-5 天，后续维护主要是：
> - 每次添加新架构约束时，更新约束检查脚本（约 30 分钟/次）
> - 每月分析 Eval 日志，优化 CLAUDE.md（约 2 小时/月）
> - 新增 Hooks 脚本（视需求，不定期）
> 整体维护成本较低，但价值持续累积。

---

## 九、延伸学习路径

- [ ] 阅读「三、Harness 工程理论」板块的 Eval 框架基础（`eval-framework/`）
- [ ] 深入研究 RAGAS 框架，理解其评测维度对 AI Coding 的借鉴意义
- [ ] 探索 PromptFoo 在代码生成评测中的应用
- [ ] 阅读 Claude Code 源码的 Hooks 系统实现（`08-claude-code-source/hooks-system/`）
- [ ] 构建适合自己团队的 Eval Dashboard

> 🔗 **关联笔记**：
> - [Eval 框架基础（LLM-as-Judge / RAGAS）](../eval-framework/)
> - [AI 生成代码验证体系](../ai-code-validation/)
> - [Claude Code Hooks 系统](../../04-tools/claude-code/hooks/)
> - [OpenSpec + GStack + Superpowers 综合实战](../../01-ai-coding/productivity-frameworks/combined-practice/openspec-gstack-superpowers-workflow.md)
