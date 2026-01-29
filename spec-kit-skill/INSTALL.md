# Spec-Kit Skill 安装和使用指南

## 安装方式

### 方式1: 使用skills CLI安装 (推荐)

```bash
# 如果skill已发布到skills.sh
npx skills add [your-github-url]/spec-kit-skill

# 全局安装
npx skills add -g [your-github-url]/spec-kit-skill
```

### 方式2: 手动安装

1. 下载或克隆此skill到你的项目目录
2. 确保skill.json配置正确
3. AI助手会自动识别skill

## 快速开始

### 1. 初始化项目

在与AI助手对话时,说:

```
我想使用Spec-Kit进行开发
```

AI助手会引导你完成初始化。

### 2. 六步工作流

#### Step 1: 建立项目章程

```
/constitution 定义项目的核心原则和技术约束
```

示例:
```
/constitution
我们使用Python 3.11 + FastAPI开发后端API
数据库使用PostgreSQL
代码遵循PEP 8规范
禁止裸SQL,必须使用ORM
```

#### Step 2: 创建功能规范

```
/specify 描述要开发的功能
```

示例:
```
/specify
开发用户登录功能
支持手机号验证码登录
用户可以选择"记住我"保持7天登录状态
```

#### Step 3: 澄清需求 (可选)

如果有不明确的地方:

```
/clarify 解决规范中的模糊点
```

示例:
```
/clarify
验证码应该是6位纯数字
验证码有效期60秒
用户连续5次输入错误后锁定10分钟
```

#### Step 4: 制定技术方案

```
/plan 定义技术实现方案
```

示例:
```
/plan
使用FastAPI构建REST API
PostgreSQL存储用户数据
Redis缓存验证码
阿里云短信服务发送验证码
JWT Token实现认证
```

#### Step 5: 拆分任务

```
/tasks 将方案拆解为可执行任务
```

AI会自动生成任务列表。

#### Step 6: 执行实现

```
/implement 按照任务列表生成代码
```

AI会按照任务顺序生成代码。

#### Step 7: 质量检查 (可选但推荐)

```
/analyze 验证规范和代码的一致性
```

## 项目结构

使用Spec-Kit后,项目会生成以下结构:

```
your-project/
├── .specify/                          # Spec-Kit配置
│   ├── memory/
│   │   └── constitution.md            # 项目章程
│   └── templates/                     # 模板文件
│       ├── spec-template.md
│       ├── plan-template.md
│       └── tasks-template.md
├── specs/                             # 规范文档
│   └── 001-feature-name/
│       ├── spec.md                    # 功能规范
│       ├── plan.md                    # 技术方案
│       ├── tasks.md                   # 任务列表
│       ├── research.md                # 研究笔记
│       ├── data-model.md              # 数据模型
│       └── contracts/                 # API契约
└── src/                               # 源代码
```

## 完整示例

假设你要开发一个待办事项应用:

### 1. Constitution

```
/constitution
Python 3.10+ with FastAPI
SQLite database
PEP 8 code style
```

### 2. Specify

```
/specify
Build a simple todo application where users can:
- Add new tasks
- Mark tasks as complete
- Delete tasks
- View all tasks in a list
```

### 3. Clarify (如需要)

```
/clarify
Each task should have a title, description, and status (pending/completed).
Tasks should be ordered by creation date.
```

### 4. Plan

```
/plan
Use FastAPI with SQLite database.
The frontend should use HTML + JavaScript (Vanilla).
No external frameworks needed.
```

### 5. Tasks

```
/tasks
```

AI会生成类似:

```
## Phase 1: Setup
- T001 创建项目结构
- T002 安装FastAPI和依赖

## Phase 2: Foundational
- T003 创建数据库schema
- T004 创建Task模型

## Phase 3: User Story 1 - CRUD操作
- T005 创建添加任务API
- T006 创建列表任务API
- T007 创建标记完成API
- T008 创建删除任务API

## Phase 4: Polish
- T009 添加单元测试
- T010 添加文档
```

### 6. Implement

```
/implement
```

AI会按顺序生成所有代码。

### 7. Analyze

```
/analyze
```

AI会检查代码质量和一致性。

## 常见问题

### Q: 适合所有项目吗?

A: 不是。Spec-Kit适合:
- 复杂功能开发
- 团队协作项目
- 企业级应用
- 长期维护的系统

不适合:
- 简单Bug修复
- 临时脚本
- 纯文案修改
- 快速原型验证

### Q: 必须按顺序执行所有命令吗?

A: 是的。Spec-Kit是一个严格的工作流:
```
Constitution → Specify → Clarify → Plan → Tasks → Implement → Analyze
```

每个阶段都依赖前一个阶段的输出。

### Q: 可以修改生成的文档吗?

A: 可以,但需要:
1. 修改对应的文档(spec.md/plan.md/tasks.md)
2. 同步更新依赖该文档的其他部分
3. 使用`/analyze`验证一致性

### Q: Constitution可以改吗?

A: 可以,但要谨慎。Constitution是项目的基石,修改会影响所有内容。

### Q: 如何集成到现有项目?

A: 增量接入:
1. 先用Spec-Kit开发新功能
2. 逐步将旧代码迁移到Spec-Kit规范
3. 保持Constitution与现有代码库一致

## 高级用法

### 多功能开发

为每个功能创建独立的spec目录:

```
specs/
├── 001-user-login/
├── 002-product-catalog/
└── 003-shopping-cart/
```

### 团队协作

1. **PM**: Review spec.md
2. **架构师**: Review plan.md
3. **开发者**: Execute tasks.md
4. **QA**: Verify acceptance criteria

### CI/CD集成

```yaml
# .github/workflows/spec-kit.yml
name: Spec-Kit Validation

on: [push]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Validate Specs
        run: |
          # 验证规范文档
          spec-kit validate ./specs/**/*.md
```

## 获取帮助

- 查看[快速入门指南](../01-spec-kit-quickstart-392048ca42.md)
- 参考[官方模板](../02-spec-kit-templates-91b92b665c.md)
- 学习[最佳实践](../03-spec-kit-best-practices-0fdeff220f.md)
- 阅读[实战案例](../04-spec-kit-examples-20e7131eaa.md)
- 访问[GitHub官方仓库](https://github.com/github/spec-kit)

## 许可证

MIT License
