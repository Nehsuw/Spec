# Analyze 命令

## 目的

质量检查与一致性分析,验证规范和代码的一致性。

## 使用场景

- 代码实现完成后,验证质量
- 需求或技术方案变更后,检查一致性
- Code Review前,自动化检查
- 定期质量审计

## 检查范围

### 1. Constitution一致性
检查代码是否符合Constitution约束

### 2. Spec-Plan一致性
检查Plan是否覆盖了Spec的所有需求

### 3. Plan-Tasks一致性
检查Tasks是否覆盖了Plan的所有设计

### 4. Tasks-Implementation一致性
检查代码是否实现了Tasks的所有任务

### 5. 代码质量
检查代码质量、测试覆盖率、文档完整性

## 执行步骤

### 1. Constitution Check

**检查项**:
- [ ] 技术栈是否符合规定
- [ ] 架构模式是否遵守
- [ ] 编码规范是否执行
- [ ] 命名规则是否遵循
- [ ] 禁止项是否被违反
- [ ] 需人工确认项是否经过审批

**示例**:
```markdown
## Constitution Check

✅ 技术栈: Python 3.11 + FastAPI ✓
✅ 架构: RESTful API ✓
✅ 编码规范: PEP 8 ✓
❌ 命名: 发现3个文件未使用snake_case
❌ 禁止项违反: 发现1处裸SQL,应使用ORM
```

### 2. Spec-Plan一致性

**检查项**:
- [ ] Spec中的每个用户故事是否在Plan中有对应的设计
- [ ] Spec中的功能需求是否在Plan中有实现方案
- [ ] Spec中的成功标准是否在Plan中有性能设计

**生成报告**:
```markdown
## Spec-Plan Consistency

### Covered User Stories
✅ US1 - 手机号验证码登录 (Plan中有对应API设计)
✅ US2 - 自动登录 (Plan中有Token机制设计)

### Covered Functional Requirements
✅ FR-001: 支持中国大陆手机号 (Plan中有验证器设计)
✅ FR-002: 验证码6位随机数字 (Plan中有生成逻辑设计)
✅ FR-003: 验证码有效期60秒 (Plan中使用Redis TTL)
✅ FR-004: 发送频率限制 (Plan中有Redis计数器设计)
✅ FR-005: 登录状态保持7天 (Plan中JWT有效期7天)
✅ FR-006: 连续5次错误锁定 (Plan中有锁定机制设计)

### Success Criteria Coverage
✅ SC-001: 30秒内完成登录 (Plan中性能目标<200ms)
✅ SC-002: 验证码发送成功率>99% (Plan中双服务商设计)
✅ SC-003: 验证准确率100% (Plan中验证逻辑清晰)

### Missing Items
❌ Spec中的边界情况"短信服务不可用"未在Plan中详细设计
```

### 3. Plan-Tasks一致性

**检查项**:
- [ ] Plan中的每个API是否有对应的任务
- [ ] Plan中的数据模型是否有创建任务
- [ ] Plan中的架构决策是否有实现任务

**生成报告**:
```markdown
## Plan-Tasks Consistency

### API Coverage
✅ POST /auth/send-code → T016
✅ POST /auth/login → T017
✅ POST /auth/refresh → T019
✅ GET /auth/me → T021

### Data Model Coverage
✅ users表 → T005
✅ login_attempts表 → T005
✅ User模型 → T008
✅ LoginAttempt模型 → T009

### Service Coverage
✅ SMSService → T013
✅ AuthService → T014
✅ UserService → T015

### Missing Tasks
❌ Plan中的"备用短信服务商"未在Tasks中体现
❌ Plan中的"IP限流"功能未在Tasks中列出
```

### 4. Tasks-Implementation一致性

**检查项**:
- [ ] Tasks中的每个任务是否已实现
- [ ] 实现的代码是否在Tasks中有对应任务
- [ ] 文件路径是否与Tasks中描述一致

**生成报告**:
```markdown
## Tasks-Implementation Consistency

### Completed Tasks (30/35)

**Phase 1: Setup** ✅ (4/4)
- T001 ✅ 项目结构已创建
- T002 ✅ 依赖已安装
- T003 ✅ pytest配置完成
- T004 ✅ black + flake8配置完成

**Phase 2: Foundational** ✅ (8/8)
- T005 ✅ 数据库schema已创建
- T006 ✅ PostgreSQL连接已配置
- T007 ✅ Redis连接已配置
- T008 ✅ User模型已创建
- T009 ✅ LoginAttempt模型已创建
- T010 ✅ JWT工具已实现
- T011 ✅ 手机号验证器已实现
- T012 ✅ FastAPI应用已配置

**Phase 3: User Story 1** ✅ (6/6)
- T013 ✅ SMSService已实现 (src/services/sms_service.py)
- T014 ✅ AuthService已实现 (src/services/auth_service.py)
- T015 ✅ UserService已实现 (src/services/user_service.py)
- T016 ✅ 发送验证码API已实现 (src/api/auth.py)
- T017 ✅ 登录API已实现 (src/api/auth.py)
- T018 ✅ 错误处理和日志已添加

**Phase 4: User Story 2** ⚠️ (3/4)
- T019 ✅ Token刷新API已实现
- T020 ✅ 认证依赖已实现
- T021 ✅ 获取当前用户API已实现
- T022 ❌ Token过期处理未实现

**Phase 5: Polish** ⚠️ (9/13)
- T023 ✅ 防暴力破解已实现
- T024 ❌ IP限流未实现
- T025 ✅ 单元测试已添加
- T026 ✅ 集成测试已添加
- T027 ✅ 性能优化已完成
- T028 ✅ 代码清理和文档已完成
- T029 ✅ 配置文件管理已完成
- T030 ❌ Docker容器化未完成
- ...

### Incomplete Tasks (5)
❌ T022: Token过期处理
❌ T024: IP限流
❌ T030: Docker容器化
❌ T032: API文档
❌ T034: E2E测试

### Implementation vs Tasks
✅ 所有实现的代码都有对应任务
❌ 发现新增文件 `src/utils/logger.py`,未在Tasks中列出
```

### 5. 代码质量检查

**检查项**:
- [ ] Linter错误
- [ ] Type错误
- [ ] 测试覆盖率
- [ ] 文档完整性
- [ ] 代码复杂度

**生成报告**:
```markdown
## Code Quality

### Linter (Black + Flake8)
✅ 无格式问题
✅ 无PEP 8违反

### Type Checking (mypy)
⚠️ 发现3处类型提示缺失
- src/services/sms_service.py:45
- src/api/auth.py:78
- src/utils/redis.py:12

### Test Coverage
⚠️ 总覆盖率: 75% (目标>80%)
✅ models: 95%
✅ services: 85%
⚠️ api: 65%
❌ utils: 45%

### Documentation
✅ README.md存在且完整
⚠️ API文档缺失
✅ 所有公共函数有docstring
❌ 部分模块缺少模块级注释

### Code Complexity
✅ 平均圈复杂度: 3.2 (良好)
⚠️ 1个函数复杂度>10: auth_service.py:login_user
```

### 6. 验收标准检查

**检查项**:
- [ ] Spec中的验收标准是否可测试
- [ ] 是否有对应的测试用例
- [ ] 测试是否通过

**生成报告**:
```markdown
## Acceptance Criteria Verification

### User Story 1 - 手机号验证码登录

**AC1**: Given 用户在登录页面, When 输入有效手机号并发送验证码, Then 收到6位数字验证码
✅ 测试: test_send_verification_code_success ✓

**AC2**: Given 用户收到验证码, When 60秒内输入正确验证码登录, Then 验证通过跳转首页
✅ 测试: test_login_with_valid_code ✓

**AC3**: Given 用户输入错误验证码, When 点击登录, Then 显示"验证码错误"
✅ 测试: test_login_with_invalid_code ✓

**AC4**: Given 验证码超过60秒, When 尝试登录, Then 显示"验证码已过期"
✅ 测试: test_login_with_expired_code ✓

### User Story 2 - 自动登录

**AC1**: Given 用户登录成功, When 勾选"记住我", Then 系统保存登录状态
✅ 测试: test_remember_me ✓

**AC2**: Given 登录状态已保存, When 7天内再次打开, Then 自动登录
✅ 测试: test_auto_login_within_7_days ✓

**AC3**: Given 超过7天, When 打开应用, Then 登录状态失效
✅ 测试: test_auto_login_expired ✓
```

### 7. 性能验证

**检查项**:
- [ ] Spec中的性能要求是否达到
- [ ] 是否有性能测试
- [ ] 性能瓶颈识别

**生成报告**:
```markdown
## Performance Verification

### Success Criteria
**SC-001**: 30秒内完成登录
- 实测: 平均2.3秒 ✅ (远优于目标)
- P95: 3.8秒 ✅
- P99: 5.2秒 ✅

**SC-002**: 验证码发送成功率>99%
- 实测: 99.7% ✅
- 阿里云: 99.8%
- 腾讯云(备用): 99.5%

**SC-003**: 验证准确率100%
- 实测: 100% ✅

### Performance Goals (from Plan)
**验证码发送P95 < 1s**
- 实测P95: 850ms ✅

**登录验证P95 < 200ms**
- 实测P95: 180ms ✅

**支持1000 QPS**
- 实测: 1200 QPS ✅ (有余量)

### Bottlenecks
⚠️ Redis连接偶尔超时(P99: 1.2s)
建议: 增加连接池大小或优化连接策略
```

### 8. 生成完整报告

汇总所有检查结果:

```markdown
# Spec-Kit Analysis Report

**Feature**: User Login
**Date**: 2026-01-29
**Analyzer**: System

## Executive Summary

✅ **Overall Status**: Good (85% complete)

**Highlights**:
- Constitution constraints respected
- Core functionality implemented and tested
- Performance goals exceeded

**Issues**:
- 5 tasks incomplete
- Test coverage below target (75% vs 80%)
- IP rate limiting not implemented

## Detailed Analysis

### 1. Constitution Check ✅
[详细内容]

### 2. Spec-Plan Consistency ✅
[详细内容]

### 3. Plan-Tasks Consistency ⚠️
[详细内容]

### 4. Tasks-Implementation Consistency ⚠️
[详细内容]

### 5. Code Quality ⚠️
[详细内容]

### 6. Acceptance Criteria Verification ✅
[详细内容]

### 7. Performance Verification ✅
[详细内容]

## Recommendations

### High Priority
1. 完成未完成的任务(T022, T024, T030等)
2. 提升测试覆盖率到80%以上
3. 添加缺失的类型提示

### Medium Priority
1. 完善API文档
2. 优化Redis连接池
3. 添加更多边界情况测试

### Low Priority
1. 降低auth_service.login_user的复杂度
2. 添加性能监控
3. 完善错误日志

## Conclusion

项目整体质量良好,核心功能已实现并通过测试。建议完成剩余任务后再发布。
```

## 下一步引导

完成后,告知用户:

```
✅ Analysis 完成!

总体评分: 85/100

优点:
- Constitution约束遵守良好
- 核心功能完整
- 性能超出预期

需要改进:
- 5个任务未完成
- 测试覆盖率需提升
- 部分文档缺失

建议:
1. 完成未完成的任务
2. 提升测试覆盖率
3. Code Review后可准备发布
```

## 常见问题

### Q: Analyze应该多久执行一次?

A: 
- 代码实现完成后必须执行
- 需求或方案变更后执行
- Code Review前执行
- 定期质量审计(如每周)

### Q: 如何处理不一致问题?

A:
1. 确认是文档落后还是代码错误
2. 更新文档或修复代码
3. 再次运行`/analyze`验证

### Q: 性能测试如何做?

A:
- 使用压测工具(如locust、JMeter)
- 测试关键接口的P95、P99延迟
- 测试并发能力(QPS)
- 监控资源使用(CPU、内存)

### Q: 测试覆盖率多少算合格?

A: 
- 一般项目: >70%
- 质量要求高: >80%
- 关键系统: >90%
- 核心业务逻辑: 100%

## 注意事项

⚠️ **重要**:
- **Analyze不能代替人工Code Review**
- **发现问题必须修复,不能忽视**
- **不一致问题可能指向需求变更**
- **性能问题应在实际环境验证**
- **测试覆盖率不是唯一指标,质量更重要**
- **定期执行,而不是只在最后执行**
