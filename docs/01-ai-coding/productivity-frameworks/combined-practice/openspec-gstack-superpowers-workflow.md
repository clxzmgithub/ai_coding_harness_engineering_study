# OpenSpec + GStack + Superpowers 三框架协同实战

> **所属板块**：一、AI 编程方法论 → AI 编程提效框架 → 组合最佳实践
> **核心目标**：将 SDD 规格驱动（OpenSpec）+ 多角色团队（GStack）+ Skill 能力扩展（Superpowers）
>             融合为一套完整的 AI 编程最佳实践工作流
> **适用场景**：中等以上复杂度的功能开发（预计 > 1 天的工作量）

---

## 一、三个框架的分工定位

在开始介绍如何组合之前，先明确各框架的职责边界：

```
┌─────────────────────────────────────────────────────────────┐
│                    AI 编程提效全景                            │
├──────────────┬──────────────┬───────────────┬───────────────┤
│  框架/工具   │   核心问题   │    解决方式   │  触发时机      │
├──────────────┼──────────────┼───────────────┼───────────────┤
│  OpenSpec    │ AI 跑偏，    │ 先写规格，    │ 任务启动前    │
│  (SDD)       │ 方向不对齐   │ 再动手实现   │               │
├──────────────┼──────────────┼───────────────┼───────────────┤
│  GStack      │ 单视角盲点，  │ 多专家角色    │ 任务推进中    │
│  (多角色)    │ 漏掉关键问题  │ 轮流审查     │ 阶段完成后    │
├──────────────┼──────────────┼───────────────┼───────────────┤
│  Superpowers │ AI 默认能力   │ 安装「插件」  │ 需要特定能力时 │
│  (Skill包)   │ 边界有限      │ 扩展能力边界  │               │
├──────────────┼──────────────┼───────────────┼───────────────┤
│  Memory Bank │ AI 跨会话     │ 结构化记忆    │ 会话开始/结束 │
│  (记忆持久化) │ 失忆          │ 持久化注入    │               │
└──────────────┴──────────────┴───────────────┴───────────────┘
```

**类比理解：**
- OpenSpec = 施工前的建筑图纸（先规划再动工）
- GStack = 施工现场的质检团队（多角色把关）
- Superpowers = 专业施工设备（扩展工人能力边界）
- Memory Bank = 项目日志（保证施工连续性）

---

## 二、完整工作流：以「支付回调模块」为例

### 需求背景

> 为电商平台新增微信支付回调处理模块：
> 接收微信回调 → 验签 → 更新订单状态 → 发送通知 → 幂等保证

---

### 阶段 0：启动会话，恢复上下文（Memory Bank）

```markdown
# 会话开始
AI 自动读取 .memory-bank/：
  → projectbrief.md：了解电商平台技术栈（Java 17 / Spring Boot 3.2 / MySQL / Redis）
  → systemPatterns.md：确认架构规范（Result<T> 返回包装、异常统一处理）
  → activeContext.md：上次完成了「订单模块基础 CRUD」，本次新增「支付回调」
  → progress.md：已完成模块一览，支付相关待开始

AI 回复：
  「已恢复项目上下文。当前工作：支付回调模块（Sprint 8 新任务）。
   技术栈：Spring Boot 3.2，已有 OrderService，支付相关尚未开始。」
```

---

### 阶段 1：规格驱动（OpenSpec）—— 先对齐再动手

#### Step 1.1：生成方案草案

```
/opsx:propose 实现微信支付回调处理模块

需求：
- 接收微信 HTTPS POST 回调（/api/payment/wx/notify）
- 验证签名（微信 V3 签名算法）
- 更新订单状态（PENDING → PAID / FAILED）
- 触发「支付成功」领域事件（通知库存扣减、积分发放）
- 幂等保证（相同 transaction_id 只处理一次）
- 必须在 5 秒内返回 SUCCESS 给微信（否则微信会重试）
```

**AI 生成的 PROPOSAL-008.md 关键片段：**

```markdown
# PROPOSAL-008: 微信支付回调模块

## 功能描述
...

## 技术边界
- 框架：Spring Boot + WxJava SDK（微信官方 Java SDK）
- 幂等：Redis SETNX（transaction_id 为 key，TTL 24h）
- 事件：Spring ApplicationEventPublisher（内部异步事件）
- 超时：回调处理超时保护 4.5s（留 0.5s 网络余量）

## 不在范围内
- 支付宝回调（独立 ticket）
- 退款回调（另一个模块）

## 决策点（需用户确认）
1. 是否使用消息队列异步处理后续事件？
   建议：否（延迟高风险），用 @Async 本地异步即可
2. 签名验证失败是否需要告警？
   建议：是（写入 Alert 表，监控仪表盘展示）
3. 同一 transaction_id 重复回调的处理策略？
   建议：直接返回 SUCCESS（幂等），不重复更新订单
```

#### Step 1.2：GStack `/em` 审查规格

在提案生成后，立即用架构师视角审查：

```
/em 请审查 specs/proposals/PROPOSAL-008.md 的架构合理性

重点关注：
1. 幂等方案是否有并发问题（Redis SETNX 是否足够？）
2. 5 秒超时限制下，@Async 是否有隐患
3. 与现有 OrderService 的耦合程度
```

**`/em` 反馈示例：**
```
⚠️ 架构风险 1：Redis SETNX 幂等方案
  问题：如果 Redis 不可用，幂等保护失效，订单可能被重复更新
  建议：数据库唯一索引（transaction_id）作为兜底，Redis 作为一级缓存

⚠️ 架构风险 2：@Async 超时风险
  问题：@Async 的线程池默认无界，高并发下可能导致线程堆积
  建议：配置有界线程池 + 拒绝策略（CallerRunsPolicy 或直接同步降级）

✅ 整体架构方向合理，事件驱动解耦设计符合 DDD 原则
```

#### Step 1.3：确认并生成设计文档

用户确认并修正决策点后：

```
/opsx:design PROPOSAL-008
```

`DESIGN-008.md` 生成，包含：UML 序列图（文本版）、类设计、API 定义、数据库变更

#### Step 1.4：生成任务清单

```
/opsx:tasks DESIGN-008
```

**TASKS-008.md 示例：**
```markdown
# TASKS-008: 微信支付回调模块实现任务清单

## Task 1: 基础类和配置
- [ ] WxPayConfig 配置类（从 application.yml 读取 appId / mchId / apiKey）
- [ ] WxPayCallbackController（接收 POST /api/payment/wx/notify）
- 验收：接口可接收请求，返回 200

## Task 2: 签名验证
- [ ] WxPaySignatureValidator 服务（V3 签名算法实现）
- [ ] 集成 WxJava SDK 的签名验证
- 验收：合法请求通过验证，篡改请求返回 400

## Task 3: 幂等保证
- [ ] PaymentIdempotencyService（Redis + 数据库双重幂等）
- 验收：相同 transaction_id 第二次调用直接返回 SUCCESS

## Task 4: 订单状态更新
- [ ] PaymentCallbackService.process() 核心逻辑
- [ ] OrderService.markAsPaid() / markAsFailed() 方法
- 验收：PAID 状态正确更新，领域事件正确发布

## Task 5: 异步通知下游
- [ ] PaymentSuccessEvent 领域事件类
- [ ] StockDeductionListener（库存扣减监听器）
- [ ] PointsGrantListener（积分发放监听器）
- 验收：事件监听器正确触发，不阻塞主流程

## Task 6: 超时保护
- [ ] 4.5s 超时拦截器配置
- 验收：超时时返回 FAIL，触发微信重试（不影响幂等）

## Task 7: 测试
- [ ] WxPayCallbackControllerTest（集成测试）
- [ ] 幂等场景测试（重复调用）
- [ ] 签名验证失败场景测试
```

---

### 阶段 2：并行开发 + 持续 Skill 支持

#### 开发过程中：随时调用 Superpowers Skill

```bash
# 完成 Task 2（签名验证）后，立即进行框架感知审查
/beagle:review-spring src/main/java/com/example/payment/

# 结果示例：
⚠️ WxPaySignatureValidator 使用了 @Autowired 字段注入，
   Spring Boot 官方推荐构造器注入，便于测试和避免循环依赖
⚠️ 签名验证失败直接抛出 RuntimeException，
   建议使用自定义异常类 PaymentSignatureException，
   配合 GlobalExceptionHandler 统一处理
```

```bash
# 完成 Task 3（幂等）后，用 Graphify 分析调用链
/graphify:trace PaymentCallbackController.notify

# 结果：
# notify() → SignatureValidator → IdempotencyService
#          → OrderService.markAsPaid() → OrderRepository
#          → ApplicationEventPublisher → StockDeductionListener
#          → ApplicationEventPublisher → PointsGrantListener
# 发现：StockDeductionListener 里调用了 InventoryService，
#       但 InventoryService 又调用了 OrderService（潜在循环依赖！）
```

---

### 阶段 3：多角色全面审查（GStack）

功能基本完成后，按风险等级依次执行 GStack 角色：

#### 3.1 代码质量审查

```
/reviewer
```

```
Code Review 报告：

🔴 严重：
  - PaymentCallbackService 中事务边界不清晰，
    @Transactional 包含了 @Async 调用，异步方法不在事务内，
    可能导致订单状态更新成功但事件未发布

🟡 中等：
  - WxPayConfig 中 apiKey 字段应该 @Value 注入而非 @ConfigurationProperties
    （当前实现导致密钥出现在 toString() 输出中，有日志泄漏风险）
  - IdempotencyService 的 Redis 操作没有错误处理，Redis 故障时整个回调会报错

🟢 建议：
  - 使用 Java 17 的 Record 类型简化 WxPayCallback DTO
  - 常量提取为枚举类（PAYMENT_STATUS_PAID 等）
```

#### 3.2 安全审查（支付场景必执行）

```
/security
```

```
安全审计报告：

🔴 严重 [OWASP A02 - 加密失败]：
  WxPayConfig.apiKey 出现在 application.yml 明文配置中。
  必须改为环境变量注入或使用配置中心（如 Apollo / Nacos）加密存储。
  
🔴 严重 [签名验证绕过风险]：
  当前实现：签名验证失败时返回 400，但不记录请求详情。
  攻击者可以枚举测试，无速率限制，无 IP 封禁。
  建议：失败请求写入安全日志 + IP 频率限制（Redis 计数器）

🟡 中等 [SSRF 风险]：
  如果后续需要回调微信，确保目标 URL 经过白名单验证，
  防止 SSRF 攻击。

✅ 通过：SQL 查询均使用参数化，无注入风险
✅ 通过：HTTPS 强制，无降级风险
```

#### 3.3 生成单元测试

```
/gen-tests src/main/java/com/example/payment/
```

```java
// 自动生成的测试示例（JUnit 5 + Mockito）

@ExtendWith(MockitoExtension.class)
class PaymentCallbackServiceTest {

    @Mock
    private WxPaySignatureValidator signatureValidator;
    @Mock
    private PaymentIdempotencyService idempotencyService;
    @Mock
    private OrderService orderService;
    @Mock
    private ApplicationEventPublisher eventPublisher;

    @InjectMocks
    private PaymentCallbackService service;

    @Test
    @DisplayName("正常支付回调：签名有效 + 非重复 → 订单状态更新 + 事件发布")
    void process_validCallback_updatesOrderAndPublishesEvent() {
        // given
        WxPayCallback callback = buildValidCallback("wx_tx_001", "SUCCESS");
        given(signatureValidator.validate(any())).willReturn(true);
        given(idempotencyService.isProcessed("wx_tx_001")).willReturn(false);

        // when
        service.process(callback);

        // then
        verify(orderService).markAsPaid("ORDER-001");
        verify(eventPublisher).publishEvent(any(PaymentSuccessEvent.class));
        verify(idempotencyService).markProcessed("wx_tx_001");
    }

    @Test
    @DisplayName("幂等场景：相同 transaction_id 重复回调 → 直接返回 SUCCESS，不重复处理")
    void process_duplicateCallback_returnsSuccessWithoutProcessing() {
        // given
        given(signatureValidator.validate(any())).willReturn(true);
        given(idempotencyService.isProcessed("wx_tx_001")).willReturn(true);

        // when & then
        assertThatNoException().isThrownBy(() -> service.process(buildValidCallback("wx_tx_001", "SUCCESS")));
        verify(orderService, never()).markAsPaid(any());
    }

    @Test
    @DisplayName("签名验证失败 → 抛出 PaymentSignatureException")
    void process_invalidSignature_throwsException() {
        given(signatureValidator.validate(any())).willReturn(false);
        assertThatThrownBy(() -> service.process(buildValidCallback("wx_tx_002", "SUCCESS")))
                .isInstanceOf(PaymentSignatureException.class);
    }
}
```

#### 3.4 可靠性审查（核心链路必执行）

```
/sre
```

```
SRE 可靠性审查：

🔴 严重 [单点故障]：
  Redis 不可用时，幂等服务抛出异常，整个回调失败。
  微信会重试，重试时 Redis 恢复，但订单可能被重复处理。
  建议：Redis 异常时降级为数据库唯一索引保证幂等。

🟡 中等 [无监控]：
  支付回调无成功率指标、无延迟监控。
  建议：接入 Micrometer，记录 payment.callback.success/failure 计数器。

🟡 中等 [线程池配置]：
  当前 @Async 使用默认线程池（SimpleAsyncTaskExecutor），
  无线程上限，高峰期可能导致 OOM。
  建议：配置自定义线程池，最大线程数 = 核心数 * 2 + 1。
```

---

### 阶段 4：发布准备（GStack `/release`）

所有审查通过、问题修复后：

```
/release

任务：
1. 生成本次功能的 PR 描述
2. 生成 CHANGELOG 条目
3. 确认版本号递增策略
```

```markdown
## PR 描述（自动生成）

### feat: 微信支付回调处理模块

**变更内容：**
- 新增 `PaymentCallbackController`：接收微信支付回调
- 新增 `WxPaySignatureValidator`：V3 签名验证（集成 WxJava SDK）
- 新增 `PaymentIdempotencyService`：Redis + DB 双重幂等保证
- 新增 `PaymentCallbackService`：核心回调处理逻辑
- 新增领域事件：`PaymentSuccessEvent` + 两个监听器
- 修复安全问题：apiKey 改为环境变量注入
- 配置有界 @Async 线程池（max=10，queue=50）

**测试覆盖：**
- 单元测试：PaymentCallbackServiceTest（5 个用例，含幂等场景）
- 集成测试：WxPayCallbackControllerTest（E2E 验证）

**注意事项：**
- 部署前需配置环境变量 `WX_PAY_API_KEY`
- Redis 7.0+ 必须可用（幂等服务依赖）
```

---

### 阶段 5：归档 & 更新记忆

```bash
# 归档 OpenSpec 规格文档
/opsx:archive 008

# 更新 Memory Bank
# AI 自动更新 .memory-bank/activeContext.md：
# - 已完成：微信支付回调模块（TASKS-008 全部完成）
# - 关键决策记录：幂等双重保障方案、有界线程池配置
# - 下次会话：支付宝回调模块（PROPOSAL-009 待建立）

# AI 自动更新 .memory-bank/progress.md
# - [x] 微信支付回调模块（2026-06-14 完成）
```

---

## 三、工作流时间分配参考

| 阶段 | 框架 | 占总时间 | 价值 |
|------|------|---------|------|
| 规格对齐（阶段 1） | OpenSpec | ~20% | 防止 80% 的方向性错误 |
| 开发（阶段 2） | Superpowers | 随时 | 实时质量反馈 |
| 全面审查（阶段 3） | GStack | ~15% | 捕获细节 Bug 和风险 |
| 发布准备（阶段 4） | GStack `/release` | ~5% | 标准化交付物 |
| 记忆持久化（阶段 5） | Memory Bank | ~5% | 下次会话效率保障 |

**结论：额外投入 40-45% 的时间在框架流程上，可以节省 3-5x 的返工成本。**

---

## 四、精简版工作流（快速迭代场景）

对于低风险变更（< 半天工作量），可以使用精简版：

```
1. 确认 Memory Bank 上下文（5 分钟）
2. 简单 Proposal（1 页 Markdown，10 分钟）
   → /em 快速确认（不需要完整 Design 和 Tasks）
3. 开发
   → /beagle 快速审查
4. /reviewer 代码审查
   → 涉及安全：/security
5. 更新 Memory Bank（5 分钟）
```

---

## 五、常见误区与解答

**Q：规格文档不是浪费时间吗？直接写代码更快？**

> A：短期来看写规格确实多花 20%，但平均一次方向性返工需要 2-5 倍代价。
> 数据：OpenSpec 社区统计，使用 SDD 后代码返工率下降 65%。
> 结论：「磨刀不误砍柴工」，对复杂功能尤其值得。

**Q：GStack 23 个角色太多，都要用吗？**

> A：不需要。核心必用 3 个：`/reviewer`（代码质量）+ `/security`（安全）+ `/qa`（测试）。
> 其余按场景选用。80% 的问题 3 个核心角色就能发现。

**Q：Superpowers Skill 安装太多，CLAUDE.md 会不会太长？**

> A：按需激活。在 `.claude/skills.yml` 中管理，每个项目只激活 3-5 个最常用的 Skill，
> 避免 context 被 Skill 描述占满。

---

## 六、延伸学习路径

1. **入门**：先只用 Memory Bank，体验跨会话上下文保持的价值
2. **进阶**：引入 OpenSpec，在下个功能开发中完整走一遍 SDD 流程
3. **深化**：安装 GStack，在代码完成后用 `/reviewer` + `/security` 把关
4. **完整**：加入 Superpowers，为常用场景安装 3-5 个 Skill
5. **精通**：根据团队实际情况，定制角色、Skill、Memory 结构

> 🔗 **子笔记**：
> - [OpenSpec 核心工作流与最佳实践](../sdd-spec-driven/openspec-best-practices.md)
> - [GStack 安装配置与角色使用指南](../multi-role-team/gstack-setup-guide.md)
> - [Superpowers Skill 包安装与使用指南](../skill-packs/superpowers-install-guide.md)
> - [Memory Bank 结构化上下文方法论](../memory-persistence/memory-bank-pattern.md)
