# Spec-Kit 官方模板文件

> 本文档包含 Spec-Kit 的所有官方模板，你可以直接复制使用或作为自定义模板的基础。

---

## 目录

1. [Constitution 模板](#constitution-模板)
2. [Spec Template 模板](#spec-template-模板)
3. [Plan Template 模板](#plan-template-模板)
4. [Tasks Template 模板](#tasks-template-模板)
5. [模板使用指南](#模板使用指南)

---

## Constitution 模板

### 作用

定义项目的"宪法层"，包含非协商原则、技术栈约束、架构规范和编码标准。

### 完整模板

```markdown
# Project Constitution

## Runtime
- Python 3.10
- Conda environment

## Technical Stack
- 后端：Spring Boot 2.7 + MyBatis Plus + Dubbo 3.3
- 数据库：MySQL 8.0 + Redis
- 前端：Vue 3 + Element Plus

## Architecture Constraints
- 分层架构：Controller/DubboApi → Service → Mapper
- Entity 必须放在 xxx.api.entity 包下
- 禁止在 Controller/DubboApi 中写业务逻辑

## Coding Standards
- 使用 Spring Java Format 格式化代码
- 方法必须有 JavaDoc 注释
- 增删改操作必须添加 @Transactional

## Naming Rules
- Entity：大驼峰，如 UserInfo
- Service 接口：I{Entity}Service，如 IUserInfoService
- Mapper：{Entity}Mapper，如 UserInfoMapper

## Forbidden
- 禁止数据库 schema 迁移
- 禁止修改生产配置

## Manual Approval Required
- 新增第三方依赖
- CI/CD 配置修改
```

### 简化模板

```markdown
# Project Constitution

## Technical Stack
- 语言/框架：[如 Python 3.11 + FastAPI]
- 数据库：[如 PostgreSQL 15]
- 前端：[如 React 18 + TypeScript]

## Architecture Principles
- [如：分层架构、微服务、单体应用]
- [如：RESTful API、GraphQL]

## Coding Standards
- [如：代码风格、注释规范、测试要求]

## Constraints
- [如：性能要求、安全要求]
```

---

## Spec Template 模板

### 作用

定义"做什么"和"为什么"，包含用户故事、验收标准、需求列表和成功标准。

### 完整模板

```markdown
# Feature Specification: [FEATURE NAME]

**Feature Branch** : `[###-feature-name]`
**Created** : [DATE]
**Status** : Draft
**Input** : User description: "\$ARGUMENTS"

## User Scenarios & Testing *(mandatory)*

### User Story 1 - [Brief Title] (Priority: P1)

[Describe this user journey in plain language]

**Why this priority** : [Explain value and why it has this priority level]

**Independent Test** : [Describe how this can be tested independently]

**Acceptance Scenarios** :

1. **Given** [initial state], **When** [action], **Then** [expected outcome]

2. **Given** [initial state], **When** [action], **Then** [expected outcome]

### User Story 2 - [Brief Title] (Priority: P2)

[Describe this user journey in plain language]

**Why this priority** : [Explain the value and why it has this priority level]

**Independent Test** : [Describe how this can be tested independently]

**Acceptance Scenarios** :

1. **Given** [initial state], **When** [action], **Then** [expected outcome]

### Edge Cases

- What happens when [boundary condition]?

- How does system handle [error scenario]?

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001** : System MUST [specific capability]

- **FR-002** : System MUST [specific capability]

- **FR-003** : Users MUST be able to [key interaction]

### Key Entities *(include if feature involves data)*

- **[Entity 1]** : [What it represents, key attributes without implementation]

- **[Entity 2]** : [What it represents, relationships to other entities]

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001** : [Measurable metric, e.g., "Users can complete account creation in under 2 minutes"]

- **SC-002** : [Measurable metric, e.g., "System handles 1000 concurrent users without degradation"]

- **SC-003** : [User satisfaction metric, e.g., "90% of users successfully complete primary task on first attempt"]
```

### 简化模板

```markdown
# 功能规范：[功能名称]

## 背景
[简要描述为什么需要这个功能]

## 用户故事

### US1 - [故事标题] (Priority: P1)

作为[角色]，我希望[目标]，以便[价值]

**验收标准**：
1. [条件1]
2. [条件2]
3. [条件3]

## 功能需求

- [ ] 需求1
- [ ] 需求2
- [ ] 需求3

## 成功标准

- [ ] 成功标准1（可量化）
- [ ] 成功标准2（可量化）
```

---

## Plan Template 模板

### 作用

定义"怎么做"，包括技术栈、架构决策、API 设计、数据模型和项目结构。

### 完整模板

```markdown
# Implementation Plan: [FEATURE]

**Branch** : `[###-feature-name]`
**Date** : [DATE]
**Spec** : [link]
**Input** : Feature specification from `/specs/[###-feature-name]/spec.md`

**Note** : This template is filled in by `/speckit.plan` command.

## Summary

[Extract from feature spec: primary requirement + technical approach from research]

## Technical Context

**Language/Version** : [e.g., Python 3.11, Swift 5.9, Rust 1.75 or NEEDS CLARIFICATION]

**Primary Dependencies** : [e.g., FastAPI, UIKit, LLVM or NEEDS CLARIFICATION]

**Storage** : [if applicable, e.g., PostgreSQL, CoreData, files or N/A]

**Testing** : [e.g., pytest, XCTest, cargo test or NEEDS CLARIFICATION]

**Target Platform** : [e.g., Linux server, iOS 15+, WASM or NEEDS CLARIFICATION]

**Project Type** : [single/web/mobile - determines source structure]

**Performance Goals** : [domain-specific, e.g., 1000 req/s, 10k lines/sec, 60 fps or NEEDS CLARIFICATION]

**Constraints** : [domain-specific, e.g., <200ms p95, <100MB memory, offline-capable or NEEDS CLARIFICATION]

**Scale/Scope** : [domain-specific, e.g., 10k users, 1M LOC, 50 screens or NEEDS CLARIFICATION]

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

[Gates determined based on constitution file]

## Project Structure

### Documentation (this feature)

```
specs/[###-feature]/
├── plan.md # This file (/speckit.plan command output)
├── research.md # Phase 0 output (/speckit.plan command)
├── data-model.md # Phase 1 output (/speckit.plan command)
├── quickstart.md # Phase 1 output (/speckit.plan command)
└── contracts/ # Phase 1 output (/speckit.plan command)
```

### Source Code (repository root)

```
# Option 1: Single project (DEFAULT)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# Option 2: Web application (when "frontend" + "backend" detected)
backend/
├── src/
│ ├── models/
│ ├── services/
│ └── api/
└── tests/

frontend/
├── src/
│ ├── components/
│ ├── pages/
│ └── services/
└── tests/

# Option 3: Mobile + API (when "iOS/Android" detected)
api/
└── [same as backend above]

ios/ or android/
└── [platform-specific structure]
```

**Structure Decision** : [Document] the selected structure and reference to real directories captured above

## Complexity Tracking

**Fill ONLY if Constitution Check has violations that must be justified**

| Violation | Why Needed | Simpler Alternative Rejected Because |
|---|---|---|
| [e.g., 4th project] | [current need] | [why 3 projects insufficient] |
| [e.g., Repository pattern] | [specfic problem] | [why direct DB access insufficient] |
```

### 简化模板

```markdown
# 技术方案：[功能名称]

## 技术栈

**语言**：[如 Python 3.11]

**主要依赖**：[如 FastAPI, SQLAlchemy, PostgreSQL]

**测试框架**：[如 pytest]

## 架构决策

### 决策1：[标题]

**选择**：[具体方案]

**理由**：
- 理由1
- 理由2

### 决策2：[标题]

**选择**：[具体方案]

**理由**：
- 理由1
- 理由2

## API 设计

### [API 名称]

```
[METHOD] /api/v1/[endpoint]

Request:
{
  "field1": "value1",
  "field2": "value2"
}

Response:
{
  "success": true,
  "data": {}
}
```

## 数据模型

```python
# Example: User model
class User:
    id: int
    name: str
    email: str
    created_at: datetime
```

## 项目结构

```
src/
├── models/
├── services/
├── api/
└── utils/
```
```

---

## Tasks Template 模板

### 作用

将技术方案拆解为可执行的原子任务，标记依赖关系和并行执行机会。

### 完整模板

```markdown
# Tasks: [FEATURE NAME]

**Input** : Design documents from `/specs/[###-feature-name]/`
**Prerequisites** : plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests** : The examples below include test tasks. Tests are OPTIONAL.

**Organization** : Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]** : Can run in parallel (different files, no dependencies)
- **[Story]** : Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Single project** : `src/`, `tests/` at repository root
- **Web app** : `backend/src/`, `frontend/src/`
- **Mobile** : `api/src/`, `ios/src/` or `android/src/`

## Phase 1: Setup (Shared Infrastructure)

**Purpose** : Project initialization and basic structure

-  T001 Create project structure per implementation plan

-  T002 Initialize [language] project with [framework] dependencies

-  T003 [P] Configure linting and formatting tools

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose** : Core infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL** : No user story work can begin until this phase is complete

Examples of foundational tasks (adjust based on your project):

-  T004 Setup database schema and migrations framework

-  T005 [P] Implement authentication/authorization framework

-  T006 [P] Setup API routing and middleware structure

-  T007 Create base models/entities that all stories depend on

-  T008 Configure error handling and logging infrastructure

-  T009 Setup environment configuration management

**Checkpoint** : Foundation ready - user story implementation can now begin in parallel

## Phase 3: User Story 1 - [Title] (Priority: P1) 🎯 MVP

**Goal** : [Brief description of what this story delivers]

**Independent Test** : [How to verify this story works on its own]

### Tests for User Story 1 (OPTIONAL - only if tests requested) ⚠️

**NOTE: Write these tests FIRST, ensure they FAIL before implementation**

-  T010 [P] [US1] Contract test for [endpoint] in tests/contract/test_[name].py

-  T011 [P] [US1] Integration test for [user journey] in tests/integration/test_[name].py

### Implementation for User Story 1

-  T012 [P] [US1] Create [Entity1] model in src/models/[entity1].py

-  T013 [P] [US1] Create [Entity2] model in src/models/[entity2].py

-  T014 [US1] Implement [Service] in src/services/[service].py (depends on T012, T013)

-  T015 [US1] Implement [endpoint/feature] in src/[location]/[file].py

-  T016 [US1] Add validation and error handling

-  T017 [US1] Add logging for user story 1 operations

**Checkpoint** : At this point, User Story 1 should be fully functional and testable independently

## Phase 4: User Story 2 - [Title] (Priority: P2)

[Similar structure as Phase 3]

## Phase N: Polish & Cross-Cutting Concerns

**Purpose** : Improvements that affect multiple user stories

-  TXXX [P] Documentation updates in docs/

-  TXXX Code cleanup and refactoring

-  TXXX Performance optimization across all stories

-  TXXX [P] Additional unit tests (if requested) in tests/unit/

-  TXXX Security hardening

-  TXXX Run quickstart.md validation

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)** : No dependencies - can start immediately

- **Foundational (Phase 2)** : Depends on Setup completion - BLOCKS all user stories

- **User Stories (Phase 3+)** : All depend on Foundational phase completion

- **Polish (Final Phase)** : Depends on all desired user stories being complete

### Within Each User Story

- Tests (if included) MUST be written and FAIL before implementation

- Models before services

- Services before endpoints

- Core implementation before integration

- Story complete before moving to next priority

### Parallel Opportunities

- All Setup tasks marked [P] can run in parallel

- All Foundational tasks marked [P] can run in parallel (within Phase 2)

- Once Foundational phase completes, all user stories can start in parallel (if team capacity allows)
```

### 简化模板

```markdown
# 任务列表：[功能名称]

## Phase 1: Setup

-  T001 创建项目结构
-  T002 安装依赖

## Phase 2: Foundational (CRITICAL)

-  T003 配置数据库连接
-  T004 实现基础认证

**Checkpoint** ✅ 基础设施就绪

## Phase 3: User Story 1

-  T005 [P] [US1] 创建数据模型
-  T006 [US1] 实现业务逻辑
-  T007 [US1] 创建 API 接口

**Checkpoint** ✅ US1 完成

## Phase 4: Polish

-  T008 [P] 添加文档
-  T009 性能优化
```

---

## 模板使用指南

### 如何使用这些模板

#### 方法1：复制模板

直接复制上面的模板内容，粘贴到你的项目中，然后根据实际需求修改。

#### 方法2：使用 Spec-Kit 初始化

```bash
# 使用官方模板初始化项目
specify init my-project --ai claude

# 模板会自动复制到 .specify/templates/ 目录
```

#### 方法3：自定义模板

1. 修改 `.specify/templates/` 下的模板文件
2. 添加自定义章节和字段
3. 使用 `specify init --template custom` 应用自定义模板

---

### 自定义模板示例

#### 在 Spec Template 中添加安全章节

```markdown
# Feature Specification: [FEATURE NAME]

... [原有内容] ...

## Security Requirements *(自定义章节)*

- **SR-001** : 所有 API 请求必须进行身份验证
- **SR-002** : 敏感数据必须加密存储
- **SR-003** : 用户输入必须进行防注入处理

## Compliance Requirements *(自定义章节)*

- **CR-001** : 符合 GDPR 数据保护规范
- **CR-002** : 日志记录符合审计要求
```

---

### 模板最佳实践

1. **保持一致性**：在整个项目中使用相同的模板结构
2. **团队同步**：确保团队成员都了解模板的使用方式
3. **版本控制**：将自定义模板纳入版本控制
4. **定期更新**：根据项目演进调整模板内容
5. **文档化修改**：在模板中记录自定义章节的目的

---

*所有模板均来自 [GitHub Spec-Kit 官方仓库](https://github.com/github/spec-kit)，你可以根据项目需求进行定制。*
