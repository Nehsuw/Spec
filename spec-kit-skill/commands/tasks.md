# Tasks 命令

## 目的

将技术方案拆解为可执行的原子任务,标记依赖关系和并行执行机会。

## 使用场景

- Plan已经完成,准备开始编码时
- 需要明确开发顺序和依赖时
- 团队协作分工时

## 输出文件

`specs/[###-feature-name]/tasks.md`

## 任务格式

`[ID] [P?] [Story] Description`

- **[ID]**: 任务编号,如T001, T002
- **[P]**: 可并行执行标记(optional)
- **[Story]**: 所属用户故事,如[US1], [US2](optional)
- **Description**: 任务描述,必须包含精确文件路径

## 执行步骤

### 1. 理解Plan

仔细阅读Plan,理解:
- 技术栈和依赖
- 架构设计
- API接口
- 数据模型
- 项目结构

### 2. 识别阶段

将任务分为以下阶段:

**Phase 1: Setup** (项目初始化)
- 创建项目结构
- 安装依赖
- 配置工具

**Phase 2: Foundational** (CRITICAL - 阻塞性前置任务)
- 数据库schema
- 基础模型
- 认证框架
- 核心基础设施

**Phase 3+: User Stories** (按优先级P1, P2, P3)
- 按用户故事分组
- 每个用户故事独立实现
- 包含测试任务(可选)

**Phase N: Polish** (优化和完善)
- 文档
- 性能优化
- 代码清理
- 额外测试

### 3. 拆分任务

**任务粒度**: 每个任务应在1-2小时内可完成

**任务原则**:
- 原子化: 一个任务只做一件事
- 可测试: 任务完成后可独立验证
- 明确路径: 包含具体文件路径
- 清晰依赖: 明确前置任务

### 4. 标记依赖和并行

**串行任务**: 有依赖关系,必须按顺序执行
**并行任务**: 标记[P],可同时执行

例如:
```markdown
- T001 创建项目结构
- T002 安装依赖(依赖T001)
- T003 [P] 配置ESLint(依赖T002,可与T004并行)
- T004 [P] 配置Prettier(依赖T002,可与T003并行)
```

### 5. 按用户故事组织

每个用户故事应该:
- 有清晰的目标
- 可独立测试
- 有完成检查点

```markdown
## Phase 3: User Story 1 - [标题] (Priority: P1) 🎯 MVP

**Goal**: [简要目标]

**Independent Test**: [如何独立测试]

### Implementation for User Story 1

- T010 [P] [US1] 创建模型 src/models/entity.py
- T011 [US1] 实现服务 src/services/service.py
- T012 [US1] 创建API src/api/endpoint.py

**Checkpoint** ✅ US1 完成
```

### 6. 生成Tasks文档

使用完整模板:

```markdown
# Tasks: [FEATURE NAME]

**Input**: Design documents from `/specs/[###-feature-name]/`
**Prerequisites**: plan.md, spec.md

**Format**: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel
- **[Story]**: Which user story (e.g., US1, US2)
- Include exact file paths

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and basic structure

- T001 创建项目结构per implementation plan
- T002 初始化[language]项目with[framework]dependencies
- T003 [P] 配置linting和formatting工具

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core infrastructure that MUST be complete before ANY user story

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- T004 Setup数据库schema and migrations framework
- T005 [P] 实现authentication/authorization framework
- T006 [P] Setup API routing and middleware structure
- T007 创建base models/entities that all stories depend on

**Checkpoint** ✅ Foundation ready

## Phase 3: User Story 1 - [Title] (Priority: P1) 🎯 MVP

**Goal**: [Brief description]

**Independent Test**: [How to verify]

### Implementation for User Story 1

- T008 [P] [US1] 创建[Entity1] model in src/models/[entity1].py
- T009 [US1] 实现[Service] in src/services/[service].py
- T010 [US1] 实现[endpoint] in src/api/[file].py

**Checkpoint** ✅ US1 complete

## Phase 4: User Story 2 - [Title] (Priority: P2)

[Similar structure]

## Phase N: Polish & Cross-Cutting Concerns

**Purpose**: Improvements affecting multiple user stories

- T050 [P] Documentation updates in docs/
- T051 Code cleanup and refactoring
- T052 Performance optimization
- T053 [P] Additional unit tests

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies
- **Foundational (Phase 2)**: Depends on Setup - BLOCKS all user stories
- **User Stories (Phase 3+)**: All depend on Foundational
- **Polish (Final Phase)**: Depends on all desired user stories

### Within Each User Story

- Models before services
- Services before endpoints
- Core implementation before integration

### Parallel Opportunities

- All Setup tasks marked [P]
- All Foundational tasks marked [P] (within Phase 2)
- Once Foundational completes, all user stories can start in parallel
```

### 7. 验证Tasks

检查清单:
- [ ] 任务是否按用户故事分组?
- [ ] Foundational阶段是否明确标记为CRITICAL?
- [ ] 每个任务是否包含具体文件路径?
- [ ] 依赖关系是否清晰?
- [ ] 并行机会是否被标记?
- [ ] 每个任务是否可在1-2小时内完成?
- [ ] 每个用户故事是否有独立测试方法?
- [ ] 检查点是否设置?

### 8. 下一步引导

完成后,告知用户:

```
✅ Tasks 已创建!

任务统计:
- 总任务数: [数量]
- Setup: [数量]
- Foundational: [数量]
- User Stories: [数量]
- Polish: [数量]

并行机会:
- [P]标记的任务可以并行执行

下一步:
- 检查任务拆分是否合理
- 使用 `/implement` 命令开始代码实现
```

## 示例

```markdown
# Tasks: User Login

**Input**: plan.md, spec.md
**Prerequisites**: Constitution, Spec, Plan

**Format**: `[ID] [P?] [Story] Description`

## Phase 1: Setup

**Purpose**: Project initialization

- T001 创建项目结构(src/, tests/, docs/)
- T002 初始化Python项目(pyproject.toml, requirements.txt)
- T003 [P] 配置pytest测试框架
- T004 [P] 配置black + flake8代码格式化

## Phase 2: Foundational (CRITICAL)

**Purpose**: Core infrastructure MUST be complete before user stories

**⚠️ CRITICAL**: No user story implementation until this phase completes

- T005 创建数据库schema(users, login_attempts表)
- T006 配置PostgreSQL连接(src/database.py)
- T007 配置Redis连接(src/utils/redis.py)
- T008 创建User模型(src/models/user.py)
- T009 创建LoginAttempt模型(src/models/login_attempt.py)
- T010 [P] 实现JWT工具(src/utils/jwt.py)
- T011 [P] 实现手机号验证器(src/utils/validators.py)
- T012 配置FastAPI应用(src/main.py)

**Checkpoint** ✅ Foundation ready - user story implementation can begin

## Phase 3: User Story 1 - 手机号验证码登录 (Priority: P1) 🎯 MVP

**Goal**: 用户可以通过手机号和验证码登录

**Independent Test**: 
1. 调用发送验证码API
2. 检查Redis中验证码
3. 调用登录API,使用验证码
4. 验证返回的JWT Token

### Implementation for User Story 1

- T013 [US1] 实现短信服务(src/services/sms_service.py)
  - 集成阿里云SDK
  - 实现发送验证码方法
  - 实现备用服务商切换逻辑

- T014 [US1] 实现认证服务(src/services/auth_service.py)
  - 生成验证码方法
  - 验证验证码方法
  - 生成JWT Token方法
  - 验证Token方法

- T015 [US1] 实现用户服务(src/services/user_service.py)
  - 创建/获取用户方法
  - 更新最后登录时间方法

- T016 [US1] 创建发送验证码API(src/api/auth.py POST /auth/send-code)
  - 验证手机号格式
  - 检查发送频率限制
  - 生成验证码
  - 保存到Redis
  - 调用短信服务发送

- T017 [US1] 创建登录API(src/api/auth.py POST /auth/login)
  - 验证手机号格式
  - 验证验证码
  - 检查账号锁定状态
  - 记录登录尝试
  - 生成JWT Token
  - 返回用户信息

- T018 [US1] 添加错误处理和日志记录
  - 统一错误格式
  - 记录关键操作日志
  - 脱敏敏感信息

**Checkpoint** ✅ US1 complete - users can login with phone + verification code

## Phase 4: User Story 2 - 自动登录 (Priority: P2)

**Goal**: 用户登录后可以保持登录状态7天

**Independent Test**:
1. 登录获取Token
2. 使用Token调用需要认证的API
3. 7天内Token有效
4. 7天后Token失效

### Implementation for User Story 2

- T019 [US2] 创建Token刷新API(src/api/auth.py POST /auth/refresh)
  - 验证旧Token
  - 生成新Token
  - 返回新Token

- T020 [US2] 实现认证依赖(src/api/deps.py)
  - 从Header提取Token
  - 验证Token有效性
  - 获取当前用户

- T021 [US2] 创建获取当前用户API(src/api/auth.py GET /auth/me)
  - 需要Token认证
  - 返回当前用户信息

- T022 [US2] 添加Token过期处理
  - 过期自动刷新
  - 刷新失败重新登录

**Checkpoint** ✅ US2 complete - auto-login works

## Phase 5: Security & Polish

**Purpose**: Enhance security and code quality

- T023 [P] 实现防暴力破解(src/services/auth_service.py)
  - 连续5次错误锁定10分钟
  - 记录错误计数到Redis
  - 清除成功后的计数

- T024 [P] 实现IP限流(src/utils/rate_limit.py)
  - 同一IP 1分钟最多3次验证码请求
  - 使用Redis计数

- T025 [P] 添加单元测试(tests/unit/)
  - test_auth_service.py
  - test_sms_service.py
  - test_validators.py
  - test_jwt.py

- T026 [P] 添加集成测试(tests/integration/)
  - test_auth_api.py (测试完整登录流程)

- T027 性能优化
  - 优化数据库查询(添加索引)
  - 优化Redis连接池
  - 异步短信发送

- T028 代码清理和文档
  - 添加docstring
  - 清理未使用代码
  - 更新README

- T029 配置文件管理
  - 从环境变量读取配置
  - 创建.env.example

- T030 Docker容器化
  - 创建Dockerfile
  - 创建docker-compose.yml

## Dependencies & Execution Order

### Phase Dependencies

```
Setup (T001-T004)
    ↓
Foundational (T005-T012) ⚠️ CRITICAL GATE
    ↓
US1 (T013-T018) 🎯 MVP
    ↓
US2 (T019-T022)
    ↓
Polish (T023-T030)
```

### Detailed Dependencies

```
T001 → T002 → [T003, T004]
T002 → T005 → T006 → T007 → T008 → T009 → [T010, T011] → T012
T012 → T013 → T014 → T015 → T016 → T017 → T018
T018 → T019 → T020 → T021 → T022
T022 → [T023, T024, T025, T026] → T027 → T028 → T029 → T030
```

### Parallel Opportunities

**Setup Phase**:
- T003 和 T004 可并行

**Foundational Phase**:
- T010 和 T011 可并行

**Polish Phase**:
- T023, T024, T025, T026 可并行

**User Stories**:
- 完成Foundational后,如果团队资源允许,US1和US2可以由不同人并行开发(但建议按优先级串行)

## Execution Strategy

**推荐执行顺序**:
1. 严格按Setup → Foundational → US1 → US2 → Polish顺序
2. 在每个阶段内,优先执行无[P]标记的任务
3. [P]标记的任务可以多人并行或单人依次执行
4. 每完成一个Checkpoint,进行代码审查和测试

**估算时间**:
- Setup: 2小时
- Foundational: 8小时 ⚠️
- US1: 10小时 🎯
- US2: 6小时
- Polish: 10小时
- **总计**: ~36小时 (约4.5个工作日)
```

## 常见问题

### Q: 任务应该拆分到什么粒度?

A: 每个任务1-2小时可完成。如果超过2小时,继续拆分。

### Q: 如何判断任务可以并行?

A: 如果两个任务:
1. 操作不同的文件
2. 没有依赖关系
3. 不会产生冲突
则可以标记为[P]

### Q: Foundational阶段包含哪些内容?

A: 所有用户故事都依赖的基础设施:
- 数据库schema
- 基础模型
- 认证框架
- 核心工具类

### Q: 每个用户故事都需要测试任务吗?

A: 看项目要求。如果要求高质量,建议每个用户故事都有测试任务。

### Q: Tasks可以在实现过程中调整吗?

A: 可以,但需要:
1. 更新tasks.md
2. 记录调整原因
3. 使用`/analyze`验证一致性

## 注意事项

⚠️ **重要**:
- **Foundational阶段必须完成后才能开始用户故事**
- 任务必须包含具体文件路径
- 每个用户故事必须可独立测试
- 并行任务不能有依赖关系
- 检查点帮助追踪进度
- 任务粒度控制在1-2小时
