# Constitution 命令

## 目的

创建或更新项目章程,定义项目的核心原则和约束,相当于项目的"宪法"。

## 使用场景

- 项目初始化时,第一步建立项目原则
- 需要更新技术栈或架构约束时
- 团队需要统一开发规范时

## 输出文件

`.specify/memory/constitution.md`

## 执行步骤

### 1. 收集信息

询问用户以下信息:

**Runtime & Technical Stack**:
- 编程语言和版本
- 框架和主要依赖
- 数据库类型
- 开发工具

**Architecture Constraints**:
- 架构模式(分层、微服务、单体等)
- 模块划分规则
- 通信方式

**Coding Standards**:
- 代码风格规范
- 注释规范
- 测试要求
- 文档要求

**Naming Rules**:
- 文件命名规则
- 类/函数命名规则
- 变量命名规则

**Forbidden Items**:
- 禁止的操作(如数据库schema迁移)
- 禁止的技术或模式
- 安全禁止项

**Manual Approval Required**:
- 需要人工确认的操作(如新增依赖)
- 需要审批的变更

### 2. 生成Constitution

使用以下模板生成 `constitution.md`:

```markdown
# Project Constitution

## Runtime
- [语言和运行时版本]

## Technical Stack
- 后端: [技术栈]
- 数据库: [数据库]
- 前端: [前端技术] (如适用)

## Architecture Constraints
- [架构约束1]
- [架构约束2]
- [架构约束3]

## Coding Standards
- [编码标准1]
- [编码标准2]
- [编码标准3]

## Naming Rules
- [命名规则1]
- [命名规则2]
- [命名规则3]

## Forbidden
- [禁止项1]
- [禁止项2]
- [禁止项3]

## Manual Approval Required
- [需人工确认项1]
- [需人工确认项2]
```

### 3. 验证Constitution

检查清单:
- [ ] 技术栈是否明确?
- [ ] 架构约束是否清晰?
- [ ] 禁止项是否列出?
- [ ] 需要人工确认的项是否标注?
- [ ] 所有约束是否可执行?

### 4. 创建文件

创建 `.specify/memory/constitution.md` 文件,保存生成的内容。

### 5. 下一步引导

完成后,告知用户:

```
✅ Constitution 已创建!

下一步:
- 使用 `/specify` 命令定义功能规范
- 在后续阶段,所有技术决策都将参照此Constitution进行验证
```

## 示例

### 示例1: Python Web应用

```markdown
# Project Constitution

## Runtime
- Python 3.11

## Technical Stack
- 后端: FastAPI + SQLAlchemy
- 数据库: PostgreSQL 15
- 前端: React 18 + TypeScript

## Architecture Constraints
- 分层架构: API Layer → Service Layer → Repository Layer
- RESTful API 设计
- 前后端分离

## Coding Standards
- 遵循 PEP 8 代码规范
- 使用 Black 格式化代码
- 所有公共函数必须有 docstring
- 测试覆盖率 > 80%

## Naming Rules
- 文件名: snake_case
- 类名: PascalCase
- 函数/变量: snake_case
- 常量: UPPER_CASE

## Forbidden
- 禁止直接在Controller中写业务逻辑
- 禁止裸SQL,必须使用ORM
- 禁止硬编码配置信息

## Manual Approval Required
- 新增第三方依赖
- 数据库schema变更
- API接口变更(破坏性)
```

### 示例2: .NET微服务

```markdown
# Project Constitution

## Runtime
- .NET 8.0
- Docker container

## Technical Stack
- 后端: ASP.NET Core + Entity Framework Core
- 数据库: SQL Server 2022
- 消息队列: RabbitMQ
- 缓存: Redis

## Architecture Constraints
- 微服务架构
- DDD(Domain-Driven Design)
- CQRS模式
- Event-Driven通信

## Coding Standards
- 遵循 .NET Coding Conventions
- 使用 C# 12 新特性
- 所有公共API必须有XML注释
- 单元测试使用 xUnit

## Naming Rules
- 命名空间: PascalCase
- 类名: PascalCase
- 接口: IPascalCase
- 私有字段: _camelCase

## Forbidden
- 禁止服务间直接数据库访问
- 禁止同步调用外部服务
- 禁止全局状态

## Manual Approval Required
- 新增微服务
- 跨服务数据模型变更
- 消息契约变更
```

## 常见问题

### Q: Constitution可以修改吗?

A: 可以,但需要谨慎。Constitution是项目的基石,修改前应该:
1. 评估影响范围
2. 团队讨论确认
3. 更新所有受影响的Spec/Plan/Tasks
4. 使用 `/analyze` 验证一致性

### Q: Constitution应该多详细?

A: 足够详细以确保团队理解和遵守,但不要过度。关注核心约束和非协商原则。

### Q: 如果Plan违反了Constitution怎么办?

A: 有两个选择:
1. 修改Plan使其符合Constitution
2. 如果Constitution确实不合理,更新Constitution并记录原因

## 注意事项

⚠️ **重要**:
- Constitution定义的是"不能做什么",而不是"要做什么"
- Constitution应该相对稳定,不要频繁修改
- 所有技术决策都必须参照Constitution进行验证
- Constitution是非协商的,除非有充分理由更新
