# Plan 命令

## 目的

创建技术方案,定义"怎么做",包括技术栈、架构、API设计、数据模型。

## 使用场景

- Spec已经明确,开始技术设计时
- 需要选择技术栈和架构时
- 准备向团队说明技术方案时

## 输出文件

- `specs/[###-feature-name]/plan.md` (主要文件)
- `specs/[###-feature-name]/research.md` (研究笔记)
- `specs/[###-feature-name]/data-model.md` (数据模型)
- `specs/[###-feature-name]/contracts/` (API契约)

## 执行步骤

### 1. Constitution Check (GATE)

**必须先通过Constitution检查!**

检查以下内容是否符合Constitution:
- [ ] 技术栈选择
- [ ] 架构模式
- [ ] 编码规范
- [ ] 命名规则
- [ ] 禁止项
- [ ] 需人工确认项

如果违反Constitution:
- **选项1**: 修改Plan使其符合Constitution
- **选项2**: 更新Constitution并记录原因

### 2. 确定技术上下文

收集以下信息:

**Technical Context**:
- Language/Version
- Primary Dependencies
- Storage (数据库/文件系统)
- Testing (测试框架)
- Target Platform
- Project Type (single/web/mobile)
- Performance Goals
- Constraints
- Scale/Scope

### 3. 架构决策

记录每个重要的架构决策:

```markdown
### 决策X: [标题]

**决策**: [具体方案]

**理由**:
- 理由1
- 理由2
- 理由3

**替代方案**: [被拒绝的方案及原因]

**Trade-offs**: [优缺点分析]
```

### 4. API设计

设计所有API接口:

```markdown
### API名称

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

Errors:
- 400: 参数错误
- 401: 未授权
- 500: 服务器错误
```
```

### 5. 数据模型设计

定义所有数据模型:

```markdown
## Data Model

### Entity1

```sql
CREATE TABLE entity1 (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

或

```python
class Entity1:
    id: int
    name: str
    created_at: datetime
```
```

### 6. 项目结构

定义清晰的项目结构:

```markdown
## Project Structure

### Documentation
```
specs/[###-feature]/
├── plan.md
├── research.md
├── data-model.md
└── contracts/
```

### Source Code
```
src/
├── models/
├── services/
├── api/
└── utils/
```
```

### 7. Complexity Tracking

如果Plan违反了Constitution的复杂度约束,必须justify:

```markdown
## Complexity Tracking

| Violation | Why Needed | Simpler Alternative Rejected Because |
|---|---|---|
| [例如: 引入新框架] | [具体原因] | [为什么简单方案不够] |
```

### 8. 生成Plan文档

使用完整模板生成 `plan.md`:

```markdown
# Implementation Plan: [FEATURE]

**Branch**: `[###-feature-name]`
**Date**: [DATE]
**Spec**: [link to spec.md]

## Summary

[简要总结: 从spec提取主要需求 + 技术方案]

## Technical Context

**Language/Version**: [如 Python 3.11]

**Primary Dependencies**: [如 FastAPI, SQLAlchemy]

**Storage**: [如 PostgreSQL 15]

**Testing**: [如 pytest]

**Target Platform**: [如 Linux server]

**Project Type**: [single/web/mobile]

**Performance Goals**: [具体指标]

**Constraints**: [约束条件]

**Scale/Scope**: [规模范围]

## Constitution Check

[检查结果,如:]
✅ 技术栈符合 Constitution
✅ 架构遵循约束
❌ 需要justify的违反项

## Architecture Decisions

### 决策1: [标题]

**决策**: [具体方案]

**理由**:
- 理由1
- 理由2

### 决策2: [标题]

...

## API Design

### API 1

```
GET /api/v1/users

Response:
{
  "users": []
}
```

### API 2

...

## Data Model

### Entity1

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    ...
);
```

## Project Structure

[详细的目录结构]

## Security Considerations

- [安全考虑1]
- [安全考虑2]

## Performance Considerations

- [性能优化1]
- [性能优化2]

## Complexity Tracking

[如果有Constitution违反项]
```

### 9. 验证Plan

检查清单:
- [ ] Constitution Check是否通过?
- [ ] 技术栈选择是否有充分理由?
- [ ] 所有架构决策是否记录?
- [ ] API设计是否完整?
- [ ] 数据模型是否清晰?
- [ ] 项目结构是否明确?
- [ ] 安全和性能考虑是否充分?
- [ ] 过度复杂性是否被justify?

### 10. 下一步引导

完成后,告知用户:

```
✅ Plan 已创建!

检查点:
- [ ] Constitution Check是否通过?
- [ ] 架构决策是否合理?
- [ ] API设计是否完整?

下一步:
- 使用 `/tasks` 命令拆分任务列表
```

## 示例

### 示例1: 用户登录功能Plan

```markdown
# Implementation Plan: User Login

**Branch**: `001-user-login`
**Date**: 2026-01-29
**Spec**: [link to spec.md]

## Summary

实现基于手机号验证码的用户登录功能,使用FastAPI构建REST API,PostgreSQL存储用户数据,Redis缓存验证码,阿里云短信服务发送验证码。

## Technical Context

**Language/Version**: Python 3.11

**Primary Dependencies**: FastAPI, SQLAlchemy, Redis, aliyun-python-sdk-dysmsapi

**Storage**: PostgreSQL 15 + Redis 7

**Testing**: pytest, pytest-asyncio

**Target Platform**: Linux server (Docker container)

**Project Type**: single (backend API)

**Performance Goals**: 
- 验证码发送P95 < 1s
- 登录验证P95 < 200ms
- 支持1000 QPS

**Constraints**: 
- 验证码必须6位数字
- 短信成本控制: 每月<10000条

**Scale/Scope**: 
- 初期支持10万用户
- 日活1万

## Constitution Check

✅ 技术栈符合 Constitution (Python 3.11 + FastAPI)
✅ 架构遵循RESTful API设计
✅ 数据库使用PostgreSQL,符合要求
✅ 编码规范遵循PEP 8

## Architecture Decisions

### 决策1: 验证码存储方案

**决策**: 使用Redis存储验证码,TTL设置为60秒

**理由**:
- Redis支持原生TTL,验证码自动过期
- 内存存储,读写速度快
- 减轻数据库压力

**替代方案**: PostgreSQL + 定时任务清理
- 被拒绝原因: 性能较差,需要额外的清理任务

**Trade-offs**:
- 优点: 性能优秀,实现简单
- 缺点: 需要额外的Redis服务

---

### 决策2: 短信服务商选择

**决策**: 主服务商阿里云,备用服务商腾讯云

**理由**:
- 阿里云短信服务稳定,价格合理
- 双服务商提升可用性
- 自动切换降低故障影响

**替代方案**: 单一服务商
- 被拒绝原因: 可用性风险

---

### 决策3: 登录Token方案

**决策**: JWT Token,有效期7天

**理由**:
- JWT无状态,易于扩展
- 7天有效期平衡安全性和用户体验
- 支持刷新机制

**Token内容**:
```json
{
  "user_id": 123,
  "phone": "13800138000",
  "exp": 1706501234
}
```

## API Design

### 1. 发送验证码

```
POST /api/v1/auth/send-code

Request:
{
  "phone": "13800138000"
}

Response:
{
  "success": true,
  "message": "验证码已发送",
  "expire_seconds": 60
}

Errors:
- 400: 手机号格式错误
- 429: 请求过于频繁(1分钟内只能发送1次)
- 500: 短信服务不可用
```

### 2. 验证登录

```
POST /api/v1/auth/login

Request:
{
  "phone": "13800138000",
  "code": "123456",
  "remember_me": true
}

Response:
{
  "success": true,
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": 123,
      "phone": "13800138000",
      "created_at": "2026-01-29T10:00:00Z"
    }
  }
}

Errors:
- 400: 验证码错误或已过期
- 423: 账号已锁定(连续错误5次)
- 500: 服务器错误
```

### 3. 刷新Token

```
POST /api/v1/auth/refresh

Headers:
Authorization: Bearer <token>

Response:
{
  "success": true,
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}

Errors:
- 401: Token无效或已过期
```

## Data Model

### PostgreSQL Schema

```sql
-- 用户表
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    phone VARCHAR(11) NOT NULL UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    last_login_at TIMESTAMP,
    INDEX idx_phone (phone)
);

-- 登录尝试记录(用于锁定机制)
CREATE TABLE login_attempts (
    id SERIAL PRIMARY KEY,
    phone VARCHAR(11) NOT NULL,
    success BOOLEAN NOT NULL,
    ip_address VARCHAR(45),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_phone_created (phone, created_at)
);
```

### Redis Key Schema

```
# 验证码
Key: sms:code:{phone}
Value: "123456"
TTL: 60秒

# 发送频率限制
Key: sms:rate:{phone}
Value: 1
TTL: 60秒

# 登录锁定
Key: login:lock:{phone}
Value: 1
TTL: 600秒(10分钟)

# 错误计数
Key: login:error:{phone}
Value: 3
TTL: 3600秒(1小时)
```

## Project Structure

```
src/
├── api/                    # API层
│   ├── __init__.py
│   ├── auth.py            # 认证相关API
│   └── deps.py            # 依赖注入
├── models/                # 数据模型
│   ├── __init__.py
│   ├── user.py           # User模型
│   └── login_attempt.py  # LoginAttempt模型
├── schemas/               # Pydantic Schemas
│   ├── __init__.py
│   ├── auth.py           # 认证相关Schema
│   └── user.py           # 用户Schema
├── services/              # 业务逻辑
│   ├── __init__.py
│   ├── auth_service.py   # 认证服务
│   ├── sms_service.py    # 短信服务
│   └── user_service.py   # 用户服务
├── utils/                 # 工具函数
│   ├── __init__.py
│   ├── jwt.py            # JWT工具
│   ├── redis.py          # Redis工具
│   └── validators.py     # 验证器
├── config.py              # 配置文件
├── database.py            # 数据库连接
└── main.py                # 应用入口

tests/
├── unit/                  # 单元测试
│   ├── test_auth_service.py
│   ├── test_sms_service.py
│   └── test_validators.py
├── integration/           # 集成测试
│   └── test_auth_api.py
└── conftest.py           # pytest配置
```

## Security Considerations

1. **验证码安全**
   - 使用加密随机数生成器
   - 验证码只能使用一次
   - 限制发送频率防止轰炸

2. **Token安全**
   - 使用HMAC-SHA256签名
   - Secret Key从环境变量读取
   - Token不包含敏感信息

3. **防暴力破解**
   - 连续5次错误锁定10分钟
   - 记录IP地址,监控异常行为

4. **数据传输安全**
   - 生产环境强制HTTPS
   - Token通过Authorization Header传输

5. **日志安全**
   - 不记录验证码明文
   - 不记录用户密码(虽然本功能无密码)
   - 脱敏手机号(138****8000)

## Performance Considerations

1. **验证码发送**
   - 异步发送短信,不阻塞请求
   - 使用消息队列(Celery)处理短信任务
   - 失败自动重试,最多3次

2. **Redis连接池**
   - 使用连接池,避免频繁建立连接
   - 连接池大小: min=10, max=50

3. **数据库查询优化**
   - 手机号字段建立索引
   - 使用SQLAlchemy异步引擎

4. **Token验证**
   - JWT验证无需查询数据库
   - 减少数据库压力

## Complexity Tracking

无需追踪 - Plan完全符合Constitution约束
```

## 常见问题

### Q: Plan应该多详细?

A: 详细到开发者可以直接开始编码。所有技术决策都应该有明确的理由。

### Q: 如果找不到合适的技术方案怎么办?

A: 可以创建 `research.md` 记录调研过程,对比不同方案,最终选择最优方案。

### Q: Constitution Check失败怎么办?

A: 
1. 首选修改Plan使其符合Constitution
2. 如果确实需要违反,必须在Complexity Tracking中justify

### Q: API设计需要包含所有细节吗?

A: 是的。Request/Response格式、错误码、HTTP方法都应该明确。

## 注意事项

⚠️ **重要**:
- **必须先通过Constitution Check**
- 每个架构决策都要有明确理由
- API设计要完整,包含错误处理
- 数据模型要清晰,包含索引
- 安全和性能考虑不能遗漏
- 过度复杂性必须justify
