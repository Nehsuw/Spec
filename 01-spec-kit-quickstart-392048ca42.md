# Spec-Kit 快速入门指南

> 本文档帮助你快速了解 Spec-Kit 的核心理念和工作流程，是首次使用者的必读指南。

---

## 目录

1. [核心概念](#核心概念)
2. [为什么选择 Spec-Kit](#为什么选择-spec-kit)
3. [六阶段工作流程](#六阶段工作流程)
4. [安装与初始化](#安装与初始化)
5. [命令参考](#命令参考)
6. [目录结构](#目录结构)
7. [快速开始示例](#快速开始示例)
8. [常见问题](#常见问题)

---

## 核心概念

### Spec-Kit 是什么？

**Spec-Kit** 是 GitHub 官方开源的**规范驱动开发工具包**，通过"规范即源码"的理念，改变传统的 AI 编码方式。

**核心理念：**
- **规范即源码**：规范文件是系统设计的权威来源
- **设计先行**：详细设计在编码前完成
- **自动化驱动**：工具链自动从规范生成代码、测试、文档
- **契约测试**：确保实现严格遵循规范定义

---

### Spec First vs Code First

| 维度 | 传统开发 (Code First) | Spec-Kit (Spec First) |
|------|----------------------|------------------------|
| 开发起点 | 直接写代码 | 先写规范 |
| 规范位置 | 文档/口头约定 | 工程核心 |
| 约束 | 事后 Review | 事前定义 |
| AI 角色 | 独立写代码 | 遵循规范的协作者 |
| 风险控制 | 依赖人工 | Spec + 人工双重保障 |
| 可追溯性 | 需求与实现脱节 | 每行代码都可追溯到 spec |

---

## 为什么选择 Spec-Kit

### 适用场景

✅ **推荐使用**：
- 复杂功能开发（多用户故事、多 API 接口）
- 需要多人协作的团队项目
- 对代码质量有严格要求的系统
- 企业级应用（Web、微服务、移动应用）
- 长期维护的业务系统

❌ **不推荐使用**：
- 简单的 Bug 修复
- 临时脚本或一次性工具
- 纯文案修改
- 强探索性原型（Spec 尚不稳定）
- 极小团队、极短生命周期项目

---

## 六阶段工作流程

### 完整流程图

```
Constitution → Specify → Clarify → Plan → Tasks → Implement
     ↓            ↓         ↓        ↓       ↓         ↓
  项目章程    功能规范   需求澄清   技术方案  任务拆分   代码实现
```

### 阶段详解

#### Step 1: Establish Constitution（建立章程）

**命令**：`/constitution`

**目的**：定义项目的核心原则和约束，相当于项目的"宪法"

**示例**：
```
/constitution This project follows a "Security-First" approach.
All user inputs must be validated.
We use a microservices architecture.
Code must be fully documented.
```

**输出**：`.specify/memory/constitution.md`

---

#### Step 2: Create Specification（创建规范）

**命令**：`/specify`

**目的**：定义"做什么"和"为什么"，不涉及技术实现

**原则**：
- ✅ 明确描述功能、用户故事、验收标准
- ❌ 不要提及技术栈、数据库、框架

**示例**：
```
/specify Develop Taskify, a team productivity platform.
Allow users to create projects, add team members,
assign tasks, comment and move tasks between boards in Kanban style.
```

**输出**：`specs/[###-feature-name]/spec.md`

---

#### Step 3: Clarify Specification（澄清规范）

**命令**：`/clarify`

**目的**：解决规范中的模糊点

**何时使用**：
- AI 在生成 spec 时提出了问题
- 规范中有需要明确的边界条件
- 有多个可能的实现方案需要确认

**示例**：
```
/clarify I want to clarify the task card details.
For each task in the UI for a task card, you should be able to
change the current status of the task between the different columns.
```

**输出**：更新 `spec.md`，添加 Clarifications 部分

---

#### Step 4: Create Plan（制定方案）

**命令**：`/plan`

**目的**：定义"怎么做"，包括技术栈、架构、依赖

**必须包含**：
- 技术栈选择
- 架构决策
- API 设计
- 数据模型
- 数据库变更
- 安全考虑

**示例**：
```
/plan We are going to generate this using .NET Aspire, using Postgres as the database.
The frontend should use Blazor server with drag-and-drop task boards,
real-time updates. There should be a REST API created with a projects API,
tasks API, and a notifications API.
```

**输出**：
- `specs/[###-feature-name]/plan.md`
- `specs/[###-feature-name]/research.md`
- `specs/[###-feature-name]/data-model.md`
- `specs/[###-feature-name]/contracts/`

---

#### Step 5: Break Down Tasks（拆分任务）

**命令**：`/tasks`

**目的**：将方案拆解为可执行的原子任务

**任务格式**：`[ID] [P?] [Story] Description`

**示例**：
```
/tasks
```

**输出**：`specs/[###-feature-name]/tasks.md`

---

#### Step 6: Implement（执行实现）

**命令**：`/implement`

**目的**：执行任务列表，生成代码

**执行逻辑**：
1. 读取 tasks.md
2. 按顺序执行任务
3. 遵循依赖关系
4. 可并行执行标记为 [P] 的任务

**示例**：
```
/implement
```

**输出**：完整的源代码

---

## 安装与初始化

### 安装 Spec-Kit

#### 方式一：持久安装（推荐）

```bash
# 使用 uv 工具安装
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

#### 方式二：一次性使用

```bash
# 直接运行，无需安装
uvx --from git+https://github.com/github/spec-kit.git specify init my-project
```

---

### 初始化项目

#### 新项目

```bash
# 创建新项目，使用 Claude Code 作为 AI Agent
specify init my-project --ai claude
```

#### 在当前目录初始化

```bash
# 方式一
specify init . --ai claude

# 方式二（等价）
specify init --here --ai claude
```

#### 跳过 Git 初始化

```bash
specify init my-project --ai gemini --no-git
```

#### 启用调试输出

```bash
specify init my-project --ai claude --debug
```

---

### 支持的 AI Agent

Spec-Kit 支持多种 AI Agent，你可以根据团队习惯和需求选择：

| Agent | 状态 | 说明 |
|-------|------|------|
| **Claude Code** | ✅ 完全支持 | Anthropic 的强大 AI，擅长理解规范 |
| **GitHub Copilot** | ✅ 完全支持 | 原生 VS Code 集成 |
| **Cursor** | ✅ 完全支持 | AI-first 代码编辑器 |
| **Gemini CLI** | ✅ 完全支持 | Google 的 AI 模型 |
| **Windsurf** | ✅ 完全支持 | 高级 AI 编码助手 |
| **Roo Code** | ✅ 完全支持 |  |

---

## 命令参考

### Slash Commands（斜杠命令）

这些命令通常在与 AI 助手（如 Claude Code）对话时使用：

| 命令 | 描述 | 使用时机 | 输出物 |
|------|------|----------|----------|
| `/constitution` | 创建或更新项目章程 | 第一步，建立项目原则 | `.specify/memory/constitution.md` |
| `/specify` | 定义功能规范（WHAT） | 明确需求后 | `specs/[###-feature-name]/spec.md` |
| `/clarify` | 澄清模糊需求 | spec 生成后，plan 之前 | 更新 `spec.md` |
| `/plan` | 创建技术方案（HOW） | spec 澄清后 | `specs/[###-feature-name]/plan.md` 及相关文档 |
| `/tasks` | 拆分任务列表 | plan 完成后 | `specs/[###-feature-name]/tasks.md` |
| `/analyze` | 质量检查与一致性分析 | tasks 完成后，implement 之前 | 分析报告 |
| `/implement` | 执行代码实现 | tasks 准备就绪后 | 源代码 |

---

### Specify CLI 命令

这些命令在终端中直接运行：

```bash
# 初始化项目
specify init my-project --ai claude

# 检查环境依赖
specify check

# 验证规范
specify validate

# 生成代码
specify generate

# 查看版本
specify --version
```

---

### 环境变量

| 变量名 | 用途 | 示例 |
|--------|------|------|
| `SPECIFY_FEATURE` | 覆盖特性检测（非 Git 仓库） | `export SPECIFY_FEATURE=001-photo-albums` |
| `GITHUB_TOKEN` | GitHub API 令牌（企业环境） | `export GITHUB_TOKEN=ghp_your_token_here` |

---

## 目录结构

### 初始化后的项目结构

```
my-project/
├── .specify/                           # Spec-Kit 配置目录
│   ├── memory/
│   │   ├── constitution.md             # ⭐ 项目章程（非协商原则）
│   │   └── constitution_update_checklist.md
│   ├── scripts/
│   │   ├── bash/                     # Bash 脚本
│   │   └── powershell/               # PowerShell 脚本
│   └── templates/
│       ├── spec-template.md            # 规范模板
│       ├── plan-template.md            # 方案模板
│       └── tasks-template.md           # 任务模板
├── specs/                             # 规范文档存放目录
│   └── 001-user-login/
│       ├── spec.md                    # 功能规范
│       ├── plan.md                    # 技术方案
│       ├── tasks.md                   # 任务列表
│       ├── research.md                # 研究笔记
│       ├── data-model.md              # 数据模型
│       ├── quickstart.md               # 快速开始指南
│       └── contracts/                 # 契约定义
│           └── api-spec.json
├── .claude/                           # Claude Code 配置（如果使用）
│   └── commands/
│       ├── specify.md
│       ├── plan.md
│       ├── tasks.md
│       └── implement.md
└── src/                               # 源代码（实际开发）
    ├── models/
    ├── services/
    └── controllers/
```

---

### 特性目录命名规范

```
specs/
├── 001-user-login/           # 3位数字 + 功能名称（kebab-case）
├── 002-task-management/
└── 003-report-generation/
```

---

## 快速开始示例

### 示例：创建一个待办事项应用

#### 1. 建立章程

```
/constitution This is a simple todo app.
We use Python 3.10+ with FastAPI.
Code must follow PEP 8 style guide.
We use SQLite as the database.
```

#### 2. 定义规范

```
/specify Build a simple todo application where users can:
- Add new tasks
- Mark tasks as complete
- Delete tasks
- View all tasks in a list
```

#### 3. 澄清需求（如需要）

```
/clarify Each task should have a title, description, and status (pending/completed).
Tasks should be ordered by creation date.
```

#### 4. 制定方案

```
/plan Use FastAPI with SQLite database.
The frontend should use HTML + JavaScript (Vanilla).
No external frameworks needed.
```

#### 5. 拆分任务

```
/tasks
```

#### 6. 执行实现

```
/implement
```

---

## 常见问题

### Q: Spec-Kit 支持哪些编程语言？

A: Spec-Kit 是语言无关的，支持所有主流编程语言，包括 Python, Java, JavaScript, TypeScript, Go, Rust 等。它通过自然语言描述规范，因此不受限于特定编程语言。

---

### Q: 如何避免 AI 生成代码过度复杂？

A: Spec-Kit 的 plan.md 中有 "Complexity Tracking" 部分，如果 AI 添加了过度复杂的组件（不必要的设计模式、过多的抽象层等），可以在 Constitution Check 中明确拒绝，并说明为什么需要更简单的方案。

---

### Q: 可以同时支持多个 AI Agent 吗？

A: 可以。Spec-Kit 支持多种 AI Agent，你可以在不同的功能开发中使用不同的 Agent，甚至可以在团队中让不同成员使用各自习惯的 Agent，但仍然遵循统一的规范和流程。

---

### Q: 如何集成 CI/CD？

A: Spec-Kit 可以与 CI/CD 工具集成，确保所有提交的规范和代码都符合标准。示例：

```yaml
# .github/workflows/spec-kit.yml
name: Spec-Kit Validation

on: [push]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Validate Specifications
        run: |
          spec-kit validate ./specs/**/*.md
      - name: Lint Plan
        run: |
          spec-kit lint --rules api-standards
```

---

### Q: 如何迁移现有的 PRD（产品需求文档）到 Spec-Kit？

A: 可以使用 `/specify` 命令，将现有的 PRD 内容作为输入，让 AI 帮助转换为符合 Spec-Kit 模板的规范文档。重点是要将 PRD 中的功能需求映射到 User Stories 和 Acceptance Criteria。

---

## 下一步

恭喜你完成了快速入门！接下来你可以：

1. **深入学习模板**：查看 [官方模板文件](02-spec-kit-templates.md) 了解规范、方案、任务的详细结构
2. **学习最佳实践**：阅读 [最佳实践与架构哲学](03-spec-kit-best-practices.md) 掌握高级技巧
3. **参考实战案例**：通过 [实战案例](04-spec-kit-examples.md) 学习完整项目的实现流程

---

*本文档帮助你快速上手 Spec-Kit，如有疑问请参考其他详细文档或访问 [GitHub 官方仓库](https://github.com/github/spec-kit)。*
