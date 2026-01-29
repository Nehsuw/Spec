# Specify 命令

## 目的

定义功能规范,描述"做什么"和"为什么",不涉及技术实现。

## 使用场景

- 开始新功能开发时
- 需要明确功能需求时
- 准备与团队沟通需求时

## 输出文件

`specs/[###-feature-name]/spec.md`

例如: `specs/001-user-login/spec.md`

## 核心原则

### ✅ 应该包含
- 用户故事和用户场景
- 验收标准(Given-When-Then格式)
- 功能需求列表
- 关键实体(不涉及实现细节)
- 可量化的成功标准

### ❌ 不应该包含
- 具体技术栈(React、FastAPI等)
- 数据库设计(表结构、字段类型)
- 架构决策(微服务、分层等)
- API设计细节
- 代码实现方式

## 执行步骤

### 1. 收集用户需求

询问用户:
- 要开发什么功能?
- 目标用户是谁?
- 为什么需要这个功能?
- 期望达到什么效果?

### 2. 生成Feature ID

使用3位数字编号,如:
- 001-user-login
- 002-product-catalog
- 003-shopping-cart

### 3. 创建Spec文档

使用以下模板生成 `spec.md`:

```markdown
# Feature Specification: [FEATURE NAME]

**Feature Branch**: `[###-feature-name]`
**Created**: [DATE]
**Status**: Draft
**Input**: User description: "$ARGUMENTS"

## User Scenarios & Testing *(mandatory)*

### User Story 1 - [Brief Title] (Priority: P1)

[Describe this user journey in plain language]

**Why this priority**: [Explain value and why it has this priority level]

**Independent Test**: [Describe how this can be tested independently]

**Acceptance Scenarios**:

1. **Given** [initial state], **When** [action], **Then** [expected outcome]

2. **Given** [initial state], **When** [action], **Then** [expected outcome]

3. **Given** [initial state], **When** [action], **Then** [expected outcome]

### User Story 2 - [Brief Title] (Priority: P2)

[Describe this user journey in plain language]

**Why this priority**: [Explain the value and why it has this priority level]

**Independent Test**: [Describe how this can be tested independently]

**Acceptance Scenarios**:

1. **Given** [initial state], **When** [action], **Then** [expected outcome]

### Edge Cases

- What happens when [boundary condition]?
- How does system handle [error scenario]?

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: System MUST [specific capability]
- **FR-002**: System MUST [specific capability]
- **FR-003**: Users MUST be able to [key interaction]

### Key Entities *(include if feature involves data)*

- **[Entity 1]**: [What it represents, key attributes without implementation]
- **[Entity 2]**: [What it represents, relationships to other entities]

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: [Measurable metric, e.g., "Users can complete account creation in under 2 minutes"]
- **SC-002**: [Measurable metric, e.g., "System handles 1000 concurrent users without degradation"]
- **SC-003**: [User satisfaction metric, e.g., "90% of users successfully complete primary task on first attempt"]
```

### 4. 验证Spec

检查清单:
- [ ] 每个用户故事是否有明确的优先级?
- [ ] 验收标准是否使用Given-When-Then格式?
- [ ] 功能需求是否可量化?
- [ ] 成功标准是否可测量?
- [ ] 是否避免了技术实现细节?
- [ ] 边界情况是否考虑?

### 5. 识别需要澄清的点

列出可能需要澄清的模糊点,例如:
- 某个功能的具体行为不明确
- 边界条件需要确认
- 多种实现方案需要选择

### 6. 创建文件

创建目录和文件:
```bash
specs/[###-feature-name]/
└── spec.md
```

### 7. 下一步引导

完成后,告知用户:

```
✅ Spec 已创建!

检查点:
- [ ] 所有用户故事是否清晰?
- [ ] 验收标准是否完整?
- [ ] 是否有需要澄清的地方?

下一步:
- 如果有模糊点,使用 `/clarify` 命令澄清需求
- 如果需求明确,使用 `/plan` 命令制定技术方案
```

## 示例

### 示例1: 用户登录功能

```markdown
# Feature Specification: User Login

**Feature Branch**: `001-user-login`
**Created**: 2026-01-29
**Status**: Draft
**Input**: User description: "实现用户登录功能,支持手机号验证码登录"

## User Scenarios & Testing

### User Story 1 - 手机号验证码登录 (Priority: P1)

作为用户,我希望使用手机号和验证码登录,以便快速安全地访问系统。

**Why this priority**: 核心功能,用户访问系统的唯一入口

**Independent Test**: 输入手机号 → 发送验证码 → 输入验证码 → 登录成功

**Acceptance Scenarios**:

1. **Given** 用户在登录页面, **When** 输入有效手机号并点击"发送验证码", **Then** 系统发送6位数字验证码到该手机号

2. **Given** 用户收到验证码, **When** 在60秒内输入正确验证码并点击"登录", **Then** 系统验证通过,跳转到首页

3. **Given** 用户输入错误验证码, **When** 点击"登录", **Then** 显示"验证码错误"提示,允许重试

4. **Given** 验证码超过60秒, **When** 用户尝试登录, **Then** 显示"验证码已过期"提示,需要重新发送

### User Story 2 - 自动登录 (Priority: P2)

作为用户,我希望登录后下次自动登录,以便提升使用体验。

**Why this priority**: 重要功能,提升用户体验

**Independent Test**: 登录成功 → 关闭应用 → 重新打开 → 自动登录

**Acceptance Scenarios**:

1. **Given** 用户登录成功, **When** 用户勾选"记住我", **Then** 系统保存登录状态

2. **Given** 用户已保存登录状态, **When** 7天内再次打开应用, **Then** 自动进入首页,无需重新登录

3. **Given** 用户超过7天未使用, **When** 打开应用, **Then** 登录状态失效,需要重新登录

### Edge Cases

- 用户输入错误手机号格式时如何提示?
- 同一手机号1分钟内多次发送验证码如何限制?
- 用户连续输入错误验证码5次如何处理?
- 如果短信服务不可用如何降级?

## Requirements

### Functional Requirements

- **FR-001**: 系统必须支持中国大陆手机号(11位数字,1开头)
- **FR-002**: 验证码必须是6位随机数字
- **FR-003**: 验证码有效期60秒
- **FR-004**: 同一手机号1分钟内只能发送1次验证码
- **FR-005**: 登录状态默认保持7天
- **FR-006**: 用户连续输入错误验证码5次后锁定10分钟

### Key Entities

- **User**: 用户实体,包含用户ID、手机号、注册时间、最后登录时间
- **VerificationCode**: 验证码实体,包含手机号、验证码、创建时间、过期时间、是否使用

## Success Criteria

### Measurable Outcomes

- **SC-001**: 用户从输入手机号到登录成功平均时长 < 30秒
- **SC-002**: 验证码发送成功率 > 99%
- **SC-003**: 验证码验证准确率 100%
- **SC-004**: 90%的用户在首次尝试时输入正确验证码
```

### 示例2: 商品搜索功能

```markdown
# Feature Specification: Product Search

**Feature Branch**: `002-product-search`
**Created**: 2026-01-29
**Status**: Draft
**Input**: User description: "实现商品搜索功能,支持关键词搜索和筛选"

## User Scenarios & Testing

### User Story 1 - 关键词搜索 (Priority: P1)

作为用户,我希望通过关键词搜索商品,以便快速找到我需要的商品。

**Why this priority**: 核心功能,电商平台的主要入口

**Independent Test**: 输入关键词 → 显示搜索结果 → 点击商品查看详情

**Acceptance Scenarios**:

1. **Given** 用户在搜索框, **When** 输入关键词"手机"并点击搜索, **Then** 显示标题或描述包含"手机"的商品列表

2. **Given** 搜索结果显示, **When** 商品数量>20, **Then** 使用分页显示,每页20个商品

3. **Given** 用户输入不存在的关键词, **When** 点击搜索, **Then** 显示"未找到相关商品"提示

### User Story 2 - 筛选和排序 (Priority: P1)

作为用户,我希望对搜索结果进行筛选和排序,以便更精确地找到目标商品。

**Why this priority**: 核心功能,提升搜索体验

**Independent Test**: 搜索商品 → 选择价格范围 → 选择排序方式 → 查看筛选后结果

**Acceptance Scenarios**:

1. **Given** 用户在搜索结果页, **When** 选择价格范围"100-500元", **Then** 只显示价格在此范围内的商品

2. **Given** 用户在搜索结果页, **When** 选择"价格从低到高"排序, **Then** 商品按价格升序显示

3. **Given** 用户选择多个筛选条件, **When** 应用筛选, **Then** 商品满足所有条件(AND逻辑)

### Edge Cases

- 用户输入特殊字符如何处理?
- 搜索结果为0时如何引导用户?
- 用户快速连续搜索如何优化?

## Requirements

### Functional Requirements

- **FR-001**: 搜索必须支持中文、英文、数字
- **FR-002**: 搜索必须匹配商品标题、描述、品牌
- **FR-003**: 支持价格范围筛选
- **FR-004**: 支持品牌筛选
- **FR-005**: 支持排序(综合、价格、销量、新品)
- **FR-006**: 搜索结果分页,每页20个商品

### Key Entities

- **Product**: 商品实体,包含ID、标题、描述、价格、品牌、销量、上架时间
- **SearchQuery**: 搜索查询实体,包含关键词、筛选条件、排序方式、分页信息

## Success Criteria

### Measurable Outcomes

- **SC-001**: 搜索响应时间 < 500ms
- **SC-002**: 搜索结果准确率 > 95%
- **SC-003**: 用户在前3页找到目标商品的比例 > 80%
```

## 常见问题

### Q: Spec应该多详细?

A: 足够详细以让团队理解需求,但不要包含技术实现细节。关注"做什么"而不是"怎么做"。

### Q: 如何确定用户故事的优先级?

A: 
- P1: 核心功能,MVP必须包含
- P2: 重要功能,提升体验
- P3: 锦上添花,可后续迭代

### Q: 如果需求不明确怎么办?

A: 在Spec中标记模糊点,然后使用 `/clarify` 命令澄清。

### Q: Spec可以修改吗?

A: 可以,但修改后需要同步更新Plan和Tasks,并使用 `/analyze` 验证一致性。

## 注意事项

⚠️ **重要**:
- Spec描述的是"做什么",不是"怎么做"
- 避免提及具体技术栈和实现方式
- 验收标准必须可测试
- 成功标准必须可量化
- 每个用户故事都应该是独立可测试的
