# Implement 命令

## 目的

执行任务列表,生成代码实现。

## 使用场景

- Tasks已经准备就绪
- 准备开始编码时
- 需要按规范生成代码时

## 输入文件

- `specs/[###-feature-name]/spec.md`
- `specs/[###-feature-name]/plan.md`
- `specs/[###-feature-name]/tasks.md`
- `.specify/memory/constitution.md`

## 输出

完整的源代码实现

## 执行原则

### 核心原则

1. **严格遵循Tasks**: 按照tasks.md的顺序和依赖关系执行
2. **遵守Constitution**: 所有代码必须符合Constitution约束
3. **可追溯性**: 每个代码变更都能追溯到Tasks
4. **质量优先**: 生成的代码必须可运行、可测试、可维护

### 执行逻辑

```
读取tasks.md
  ↓
按Phase顺序执行
  ↓
遵循依赖关系
  ↓
并行执行[P]标记的任务
  ↓
完成Checkpoint验证
  ↓
继续下一个Phase
```

## 执行步骤

### 1. 准备阶段

**检查前置条件**:
- [ ] Constitution.md存在
- [ ] Spec.md完整
- [ ] Plan.md完整
- [ ] Tasks.md完整
- [ ] 项目环境已配置

**理解上下文**:
- 阅读Constitution,理解约束
- 阅读Spec,理解需求
- 阅读Plan,理解技术方案
- 阅读Tasks,理解任务依赖

### 2. Phase 1: Setup

执行所有Setup任务:

```markdown
示例:
- T001 创建项目结构
  → 创建所有必要的目录

- T002 安装依赖
  → 安装package.json/requirements.txt/pom.xml中的依赖

- T003 [P] 配置工具
  → 配置linter、formatter等
```

**Checkpoint**:
- [ ] 项目结构正确
- [ ] 依赖安装成功
- [ ] 工具配置生效

### 3. Phase 2: Foundational ⚠️ CRITICAL

执行所有Foundational任务,这是**阻塞性前置条件**:

```markdown
示例:
- T005 创建数据库schema
  → 生成migration文件或SQL脚本

- T006 创建基础模型
  → 生成model文件

- T007 实现认证框架
  → 生成auth相关代码
```

**Checkpoint**:
- [ ] 数据库schema正确
- [ ] 基础模型可用
- [ ] 认证框架工作
- [ ] 所有基础设施就绪

**⚠️ 重要**: 必须通过Checkpoint才能继续!

### 4. Phase 3+: User Stories

按优先级顺序实现每个用户故事:

**对于每个用户故事**:

1. **理解目标**: 阅读用户故事的Goal和Independent Test
2. **执行任务**: 按顺序执行该用户故事的所有任务
3. **验证完成**: 使用Independent Test验证功能
4. **标记Checkpoint**: 确认用户故事完成

```markdown
示例 - User Story 1:

- T013 [US1] 实现短信服务
  → 生成 src/services/sms_service.py
  → 包含发送验证码方法
  → 包含备用服务商切换逻辑

- T014 [US1] 实现认证服务
  → 生成 src/services/auth_service.py
  → 包含验证码生成/验证方法
  → 包含JWT Token生成/验证方法

- T015 [US1] 创建API
  → 生成 src/api/auth.py
  → 实现POST /auth/send-code接口
  → 实现POST /auth/login接口

**Checkpoint** ✅ US1 complete
  → 测试: 调用API,验证登录流程
```

### 5. Phase N: Polish

执行所有优化和完善任务:

```markdown
示例:
- T030 [P] 添加单元测试
  → 生成test文件

- T031 性能优化
  → 优化代码性能

- T032 文档
  → 生成README、API文档
```

### 6. 代码质量检查

**自动检查**:
- [ ] Linter检查通过
- [ ] Type检查通过(如TypeScript、Python类型提示)
- [ ] 单元测试通过
- [ ] 集成测试通过

**手动检查**:
- [ ] 代码符合Constitution约束
- [ ] 命名符合规范
- [ ] 注释完整
- [ ] 错误处理充分

### 7. 下一步引导

完成后,告知用户:

```
✅ Implementation 完成!

完成情况:
- Setup: ✅
- Foundational: ✅
- User Story 1: ✅
- User Story 2: ✅
- Polish: ✅

建议:
- 运行测试验证功能
- 使用 `/analyze` 命令检查一致性
- Code Review
```

## 代码生成规范

### 文件头注释

每个文件应包含:

```python
"""
Module: [模块名称]
Description: [简要描述]
Author: [作者]
Created: [日期]
"""
```

### 函数注释

每个函数应包含docstring:

```python
def send_verification_code(phone: str) -> bool:
    """
    发送验证码到指定手机号
    
    Args:
        phone: 11位手机号
        
    Returns:
        bool: 发送成功返回True,失败返回False
        
    Raises:
        ValueError: 手机号格式错误
        SMSServiceError: 短信服务不可用
        
    Example:
        >>> send_verification_code("13800138000")
        True
    """
    pass
```

### 错误处理

所有错误都应该被处理:

```python
try:
    result = risky_operation()
except SpecificError as e:
    logger.error(f"Operation failed: {e}")
    raise CustomError(f"Failed to ...: {e}")
```

### 日志记录

关键操作记录日志:

```python
logger.info(f"User {user_id} logged in successfully")
logger.warning(f"Failed login attempt for phone {phone}")
logger.error(f"SMS service failed: {error}")
```

### 安全考虑

- 不硬编码敏感信息
- 使用参数化查询防止SQL注入
- 验证所有用户输入
- 记录日志时脱敏敏感信息

## 示例

### 示例1: 实现发送验证码API

**任务**: T016 [US1] 创建发送验证码API

**输入**:
- Plan中的API设计
- Tasks中的任务描述
- Constitution中的编码规范

**输出**: `src/api/auth.py`

```python
"""
Module: auth
Description: Authentication API endpoints
Author: System
Created: 2026-01-29
"""

from fastapi import APIRouter, HTTPException, status
from pydantic import BaseModel, validator
import re
from typing import Dict

from src.services.auth_service import AuthService
from src.services.sms_service import SMSService
from src.utils.validators import validate_phone
from src.utils.redis import redis_client
from src.utils.logger import logger

router = APIRouter(prefix="/api/v1/auth", tags=["auth"])


class SendCodeRequest(BaseModel):
    """发送验证码请求"""
    phone: str
    
    @validator('phone')
    def validate_phone_format(cls, v):
        """验证手机号格式"""
        if not validate_phone(v):
            raise ValueError("Invalid phone number format")
        return v


class SendCodeResponse(BaseModel):
    """发送验证码响应"""
    success: bool
    message: str
    expire_seconds: int


@router.post("/send-code", response_model=SendCodeResponse)
async def send_verification_code(request: SendCodeRequest) -> Dict:
    """
    发送验证码到指定手机号
    
    Args:
        request: 发送验证码请求,包含手机号
        
    Returns:
        SendCodeResponse: 发送结果
        
    Raises:
        HTTPException 400: 手机号格式错误
        HTTPException 429: 请求过于频繁
        HTTPException 500: 短信服务不可用
    """
    phone = request.phone
    
    # 检查发送频率限制(1分钟内只能发送1次)
    rate_key = f"sms:rate:{phone}"
    if await redis_client.exists(rate_key):
        logger.warning(f"Rate limit exceeded for phone {phone}")
        raise HTTPException(
            status_code=status.HTTP_429_TOO_MANY_REQUESTS,
            detail="Please wait 60 seconds before requesting another code"
        )
    
    try:
        # 生成验证码
        auth_service = AuthService()
        code = await auth_service.generate_verification_code(phone)
        
        # 保存到Redis,TTL 60秒
        code_key = f"sms:code:{phone}"
        await redis_client.setex(code_key, 60, code)
        
        # 设置频率限制,TTL 60秒
        await redis_client.setex(rate_key, 60, "1")
        
        # 发送短信
        sms_service = SMSService()
        success = await sms_service.send_code(phone, code)
        
        if success:
            logger.info(f"Verification code sent successfully to {phone}")
            return {
                "success": True,
                "message": "Verification code sent successfully",
                "expire_seconds": 60
            }
        else:
            logger.error(f"Failed to send verification code to {phone}")
            raise HTTPException(
                status_code=status.HTTP_500_INTERNAL_SERVER_ERROR,
                detail="SMS service unavailable"
            )
            
    except Exception as e:
        logger.error(f"Error sending verification code: {e}")
        raise HTTPException(
            status_code=status.HTTP_500_INTERNAL_SERVER_ERROR,
            detail="Internal server error"
        )
```

### 示例2: 实现短信服务

**任务**: T013 [US1] 实现短信服务

**输出**: `src/services/sms_service.py`

```python
"""
Module: sms_service
Description: SMS service for sending verification codes
Author: System
Created: 2026-01-29
"""

from aliyunsdkcore.client import AcsClient
from aliyunsdkcore.request import CommonRequest
from tencentcloud.common import credential
from tencentcloud.sms.v20210111 import sms_client, models
import os
from typing import Optional

from src.utils.logger import logger


class SMSService:
    """短信服务,支持主备服务商切换"""
    
    def __init__(self):
        """初始化SMS服务"""
        # 阿里云配置(主服务商)
        self.aliyun_access_key = os.getenv("ALIYUN_ACCESS_KEY")
        self.aliyun_access_secret = os.getenv("ALIYUN_ACCESS_SECRET")
        self.aliyun_sign_name = os.getenv("ALIYUN_SIGN_NAME")
        self.aliyun_template_code = os.getenv("ALIYUN_TEMPLATE_CODE")
        
        # 腾讯云配置(备用服务商)
        self.tencent_secret_id = os.getenv("TENCENT_SECRET_ID")
        self.tencent_secret_key = os.getenv("TENCENT_SECRET_KEY")
        self.tencent_sms_sdk_app_id = os.getenv("TENCENT_SMS_SDK_APP_ID")
        self.tencent_sign_name = os.getenv("TENCENT_SIGN_NAME")
        self.tencent_template_id = os.getenv("TENCENT_TEMPLATE_ID")
    
    async def send_code(self, phone: str, code: str) -> bool:
        """
        发送验证码,自动切换主备服务商
        
        Args:
            phone: 11位手机号
            code: 6位验证码
            
        Returns:
            bool: 发送成功返回True,失败返回False
        """
        # 尝试主服务商(阿里云)
        success = await self._send_via_aliyun(phone, code)
        if success:
            return True
        
        logger.warning("Aliyun SMS failed, switching to Tencent")
        
        # 主服务商失败,切换到备用服务商(腾讯云)
        success = await self._send_via_tencent(phone, code)
        if success:
            return True
        
        logger.error("Both SMS providers failed")
        return False
    
    async def _send_via_aliyun(self, phone: str, code: str) -> bool:
        """
        通过阿里云发送验证码
        
        Args:
            phone: 手机号
            code: 验证码
            
        Returns:
            bool: 发送成功返回True
        """
        try:
            client = AcsClient(self.aliyun_access_key, 
                             self.aliyun_access_secret, 
                             'cn-hangzhou')
            
            request = CommonRequest()
            request.set_accept_format('json')
            request.set_domain('dysmsapi.aliyuncs.com')
            request.set_method('POST')
            request.set_protocol_type('https')
            request.set_version('2017-05-25')
            request.set_action_name('SendSms')
            
            request.add_query_param('PhoneNumbers', phone)
            request.add_query_param('SignName', self.aliyun_sign_name)
            request.add_query_param('TemplateCode', self.aliyun_template_code)
            request.add_query_param('TemplateParam', f'{{"code":"{code}"}}')
            
            response = client.do_action_with_exception(request)
            logger.info(f"Aliyun SMS sent to {phone}: {response}")
            return True
            
        except Exception as e:
            logger.error(f"Aliyun SMS error: {e}")
            return False
    
    async def _send_via_tencent(self, phone: str, code: str) -> bool:
        """
        通过腾讯云发送验证码
        
        Args:
            phone: 手机号
            code: 验证码
            
        Returns:
            bool: 发送成功返回True
        """
        try:
            cred = credential.Credential(self.tencent_secret_id, 
                                        self.tencent_secret_key)
            client = sms_client.SmsClient(cred, "ap-guangzhou")
            
            req = models.SendSmsRequest()
            req.SmsSdkAppId = self.tencent_sms_sdk_app_id
            req.SignName = self.tencent_sign_name
            req.TemplateId = self.tencent_template_id
            req.TemplateParamSet = [code]
            req.PhoneNumberSet = [f"+86{phone}"]
            
            resp = client.SendSms(req)
            logger.info(f"Tencent SMS sent to {phone}: {resp}")
            return True
            
        except Exception as e:
            logger.error(f"Tencent SMS error: {e}")
            return False
```

## 常见问题

### Q: 如果任务太大无法一次完成怎么办?

A: 将任务拆分为更小的子任务。理想情况下,每个任务1-2小时可完成。

### Q: 遇到bugs怎么办?

A: 
1. 记录bug
2. 修复bug
3. 添加测试确保不再出现
4. 更新tasks.md记录修复

### Q: 生成的代码不符合需求怎么办?

A: 
1. 检查是否理解错误
2. 修改代码
3. 考虑是否需要更新Spec/Plan/Tasks
4. 使用`/analyze`验证一致性

### Q: 可以跳过某些任务吗?

A: 不推荐。每个任务都有其目的。如果确实不需要,应该更新tasks.md删除该任务。

## 注意事项

⚠️ **重要**:
- **严格按照tasks.md执行**
- **遵守Constitution约束**
- **Foundational阶段必须完全完成**
- **每个Checkpoint都要验证**
- **生成的代码必须可运行**
- **添加充分的注释和文档**
- **包含错误处理和日志记录**
- **代码质量优先于速度**
