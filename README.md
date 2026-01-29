# Spec-Kit Skill

Spec-Kit 开发规范文档生成 Skill,用于规范驱动的软件开发流程。

## 📖 介绍

**Spec-Kit** 是 GitHub 官方开源的**规范驱动开发工具包**,通过"规范即源码"的理念,改变传统的 AI 编码方式。

### 核心理念

- **规范即源码**: 规范文件是系统设计的权威来源
- **设计先行**: 详细设计在编码前完成
- **自动化驱动**: 工具链自动从规范生成代码、测试、文档
- **契约测试**: 确保实现严格遵循规范定义

## 🚀 快速开始

### 六阶段工作流程

```
Constitution → Specify → Clarify → Plan → Tasks → Implement
     ↓            ↓         ↓        ↓       ↓         ↓
  项目章程    功能规范   需求澄清   技术方案  任务拆分   代码实现
```

### 基本用法

1. **建立项目章程**
```
/constitution 定义项目的核心原则和技术约束
```

2. **创建功能规范**
```
/specify 描述要开发的功能
```

3. **澄清需求**
```
/clarify 解决规范中的模糊点
```

4. **制定技术方案**
```
/plan 定义技术实现方案
```

5. **拆分任务**
```
/tasks 将方案拆解为可执行任务
```

6. **执行实现**
```
/implement 生成代码实现
```

7. **质量分析**
```
/analyze 验证规范和代码的一致性
```

## 📋 命令列表

| 命令 | 描述 | 使用时机 |
|------|------|----------|
| `/constitution` | 创建项目章程 | 第一步,建立项目原则 |
| `/specify` | 定义功能规范 | 明确需求后 |
| `/clarify` | 澄清模糊需求 | spec生成后,plan之前 |
| `/plan` | 创建技术方案 | spec澄清后 |
| `/tasks` | 拆分任务列表 | plan完成后 |
| `/implement` | 执行代码实现 | tasks准备就绪后 |
| `/analyze` | 质量检查分析 | 任何阶段验证一致性 |

## 🎯 适用场景

### ✅ 推荐使用

- 复杂功能开发(多用户故事、多API接口)
- 需要多人协作的团队项目
- 对代码质量有严格要求的系统
- 企业级应用
- 长期维护的业务系统

### ❌ 不推荐使用

- 简单的Bug修复
- 临时脚本或一次性工具
- 纯文案修改
- 强探索性原型
- 极小团队、极短生命周期项目

## 📁 目录结构

使用此skill后,项目会生成以下结构:

```
your-project/
├── .specify/                           # Spec-Kit配置目录
│   ├── memory/
│   │   └── constitution.md             # 项目章程
│   └── templates/
│       ├── spec-template.md            # 规范模板
│       ├── plan-template.md            # 方案模板
│       └── tasks-template.md           # 任务模板
├── specs/                              # 规范文档存放目录
│   └── 001-feature-name/
│       ├── spec.md                     # 功能规范
│       ├── plan.md                     # 技术方案
│       ├── tasks.md                    # 任务列表
│       ├── research.md                 # 研究笔记
│       ├── data-model.md               # 数据模型
│       └── contracts/                  # 契约定义
└── src/                                # 源代码
```

## 📚 参考资源

- [Spec-Kit 官方仓库](https://github.com/github/spec-kit)
- [快速入门指南](../01-spec-kit-quickstart-392048ca42.md)
- [官方模板](../02-spec-kit-templates-91b92b665c.md)
- [最佳实践](../03-spec-kit-best-practices-0fdeff220f.md)
- [实战案例](../04-spec-kit-examples-20e7131eaa.md)

## 🤝 贡献

欢迎提交 Issue 和 Pull Request!

## 📄 许可证

MIT License
