# Spec-Kit 实战案例

> 本文档通过三个完整的项目案例，展示 Spec-Kit 在实际项目中的应用流程和最佳实践。

---

## 目录

1. [案例1：Taskify 团队生产力平台](#案例1taskify-团队生产力平台)
2. [案例2：Photo Album Organizer 照片相册](#案例2photo-album-organizer-照片相册)
3. [案例3：Bookstore E-commerce 书店电商](#案例3bookstore-e-commerce-书店电商)
4. [案例对比分析](#案例对比分析)
5. [关键经验总结](#关键经验总结)

---

## 案例1：Taskify 团队生产力平台

### 项目背景

**目标**：开发一个看板式项目管理工具，支持多用户、拖拽、实时协作。

**核心功能**：
- 用户管理（5个预定义用户）
- 项目管理（3个示例项目）
- 看板管理（To Do → In Progress → In Review → Done）
- 任务拖拽
- 评论系统
- 实时通知

---

### 完整流程

#### Step 1: Constitution

```markdown
# Project Constitution

## Technical Stack
- 后端：.NET Aspire + PostgreSQL
- 前端：Blazor Server
- 实时通信：SignalR

## Architecture Constraints
- 微服务架构
- RESTful API
- 实时更新优先

## Coding Standards
- 使用 C# 12.0
- 遵循 .NET Coding Conventions
- 所有公共 API 必须有 XML 注释

## Performance Requirements
- 支持 1000+ 并发用户
- P95 响应时间 < 200ms
- 实时更新延迟 < 100ms
```

---

#### Step 2: Specify

```markdown
# Feature Specification: Taskify Platform

**Feature Branch**: `001-taskify-platform`
**Created**: 2026-01-27
**Status**: Draft

## User Scenarios & Testing

### User Story 1 - 用户选择与项目列表 (Priority: P1)

作为用户，我希望从预定义的用户列表中选择登录，无需密码，以便快速访问系统。

**Why this priority**: 核心功能，其他所有功能依赖登录

**Independent Test**: 启动应用 → 显示5个用户 → 点击用户 → 进入项目列表

**Acceptance Scenarios**:
1. Given 应用启动, When 页面加载完成, Then 显示5个预定义用户卡片
2. Given 用户点击某个用户卡片, When 点击事件触发, Then 直接进入该用户的项目列表
3. Given 用户未登录, When 访问任何页面, Then 重定向到用户选择页面

### User Story 2 - 看板视图 (Priority: P1)

作为用户，我希望在Kanban看板上查看和管理任务，以便跟踪工作进度。

**Why this priority**: 核心功能，是用户日常工作界面

**Independent Test**: 选择项目 → 查看看板 → 拖拽任务卡片

**Acceptance Scenarios**:
1. Given 用户进入项目, When 看板加载完成, Then 显示4个列: To Do, In Progress, In Review, Done
2. Given 每个列有5-15个任务, When 任务加载完成, Then 每个任务卡片显示标题、描述、负责人、截止日期
3. Given 用户拖拽任务卡片, When 拖拽到另一列, Then 任务状态自动更新

### User Story 3 - 任务评论 (Priority: P2)

作为用户，我希望为任务添加评论并编辑自己的评论，以便团队协作沟通。

**Why this priority**: 重要功能，提升团队协作效率

**Independent Test**: 打开任务详情 → 添加评论 → 编辑/删除自己的评论

**Acceptance Scenarios**:
1. Given 用户打开任务详情, When 点击添加评论, Then 可以输入评论内容
2. Given 用户提交评论, When 保存成功, Then 评论显示在评论列表
3. Given 用户编辑自己的评论, When 保存成功, Then 评论内容更新
4. Given 用户尝试删除他人评论, When 操作执行, Then 显示"无权限"提示

## Requirements

### Functional Requirements
- **FR-001**: 系统必须支持5个预定义用户（1个PM + 4个工程师）
- **FR-002**: 每个项目必须包含5-15个随机分布的任务
- **FR-003**: 看板必须支持4个状态列：To Do, In Progress, In Review, Done
- **FR-004**: 任务卡片必须显示标题、描述、负责人、截止日期
- **FR-005**: 用户只能编辑/删除自己创建的评论
- **FR-006**: 拖拽任务到新列时，必须自动更新任务状态

### Key Entities
- **User**: 用户实体，包含ID、姓名、角色（PM/Engineer）
- **Project**: 项目实体，包含ID、名称、描述、所有者
- **Task**: 任务实体，包含ID、标题、描述、状态、负责人、截止日期
- **Comment**: 评论实体，包含ID、内容、创建者、创建时间

## Success Criteria
- **SC-001**: 用户从选择到进入看板，平均时长 < 5秒
- **SC-002**: 拖拽任务的响应时间 < 200ms
- **SC-003**: 实时通知延迟 < 500ms
```

---

#### Step 3: Clarify

```markdown
# Clarifications

## 任务分布规则

**Q**: 5-15个任务如何分布在不同列中？

**A**: 对于每个示例项目：
- 随机生成5-15个任务
- 随机分配到4个列中
- 确保每个列至少有1个任务

## 实时更新机制

**Q**: 当一个用户拖拽任务后，其他用户如何看到更新？

**A**: 使用 SignalR 实现实时推送：
- 拖拽事件触发后，服务器广播更新
- 所有连接的客户端自动接收并更新UI
- 无需手动刷新

## 评论权限

**Q**: PM是否可以编辑/删除任何评论？

**A**: 为简化实现，初始版本：
- 所有用户只能编辑/删除自己创建的评论
- PM权限在后续版本中实现
```

---

#### Step 4: Plan

```markdown
# Implementation Plan: Taskify Platform

**Branch**: `001-taskify-platform`
**Date**: 2026-01-27
**Spec**: [链接到 spec.md]

## Summary

开发一个基于 .NET Aspire 的团队生产力平台，前端使用 Blazor Server，后端提供 REST API，使用 PostgreSQL 存储，通过 SignalR 实现实时更新。

## Technical Context

**Language/Version**: C# 12.0 / .NET 8.0

**Primary Dependencies**: .NET Aspire, Blazor Server, PostgreSQL, SignalR, Entity Framework Core

**Storage**: PostgreSQL 15

**Testing**: xUnit, Moq, Blazor Testing

**Target Platform**: Linux server (Docker container)

**Project Type**: web (backend API + frontend Blazor)

**Performance Goals**: P95 响应时间 < 200ms, 支持 1000+ 并发用户

**Constraints**: 实时更新延迟 < 100ms, 任务拖拽流畅无延迟

**Scale/Scope**: 初始支持 100 个用户，50 个项目

## Constitution Check

✅ 技术栈符合 Constitution.md 规定
✅ 架构遵循微服务模式
✅ 编码规范符合 .NET Coding Conventions

## Architecture Decisions

### 1. 前端架构

**决策**: Blazor Server with SignalR

**理由**:
- Blazor Server 提供完整的 C# 开发体验
- SignalR 原生支持实时通信
- 避免客户端 JavaScript 复杂度
- 符合团队技术栈（.NET）

**架构**:
```
Blazor Server (Razor Components)
    ↓ (SignalR)
    Backend API
```

### 2. 后端架构

**决策**: 微服务架构，分层设计

**理由**:
- 符合 Constitution.md 的微服务要求
- 便于独立扩展各模块
- 清晰的职责分离

**架构**:
```
API Layer (Controllers)
    ↓
Service Layer (Business Logic)
    ↓
Repository Layer (Data Access)
    ↓
PostgreSQL
```

### 3. 实时通信方案

**决策**: SignalR

**理由**:
- 原生 .NET 支持，集成简单
- 自动处理连接管理
- 支持多种传输协议
- 性能优秀，延迟低

**设计**:
- Hub: TaskHub（任务变更通知）
- Events: TaskMoved, TaskUpdated, CommentAdded
- Groups: 按项目分组，只推送相关更新

## API Design

### 1. 获取用户列表

```
GET /api/users

Response:
{
  "users": [
    {
      "id": 1,
      "name": "Alice (PM)",
      "role": "PM"
    },
    ...
  ]
}
```

### 2. 获取用户的项目列表

```
GET /api/users/{userId}/projects

Response:
{
  "projects": [
    {
      "id": 1,
      "name": "Project Alpha",
      "description": "Initial release",
      "taskCount": 12
    },
    ...
  ]
}
```

### 3. 获取看板任务

```
GET /api/projects/{projectId}/tasks

Response:
{
  "tasks": [
    {
      "id": 1,
      "title": "Design homepage",
      "description": "Create wireframes",
      "status": "In Progress",
      "assigneeId": 2,
      "assigneeName": "Bob",
      "dueDate": "2026-02-01",
      "commentCount": 3
    },
    ...
  ]
}
```

### 4. 更新任务状态（拖拽）

```
PUT /api/tasks/{taskId}/status

Request:
{
  "status": "Done"
}

Response:
{
  "success": true
}
```

### 5. 添加评论

```
POST /api/tasks/{taskId}/comments

Request:
{
  "content": "Great work!",
  "userId": 1
}

Response:
{
  "id": 101,
  "content": "Great work!",
  "authorName": "Alice (PM)",
  "createdAt": "2026-01-27T10:30:00Z"
}
```

## Data Model

### PostgreSQL Schema

```sql
CREATE TABLE Users (
    Id INT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Role VARCHAR(20) NOT NULL CHECK (Role IN ('PM', 'Engineer'))
);

CREATE TABLE Projects (
    Id INT PRIMARY KEY,
    Name VARCHAR(200) NOT NULL,
    Description TEXT,
    OwnerId INT NOT NULL,
    FOREIGN KEY (OwnerId) REFERENCES Users(Id)
);

CREATE TABLE Tasks (
    Id INT PRIMARY KEY,
    ProjectId INT NOT NULL,
    Title VARCHAR(200) NOT NULL,
    Description TEXT,
    Status VARCHAR(20) NOT NULL CHECK (Status IN ('To Do', 'In Progress', 'In Review', 'Done')),
    AssigneeId INT,
    DueDate DATE,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (ProjectId) REFERENCES Projects(Id),
    FOREIGN KEY (AssigneeId) REFERENCES Users(Id)
);

CREATE TABLE Comments (
    Id INT PRIMARY KEY,
    TaskId INT NOT NULL,
    Content TEXT NOT NULL,
    UserId INT NOT NULL,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (TaskId) REFERENCES Tasks(Id),
    FOREIGN KEY (UserId) REFERENCES Users(Id)
);
```

## SignalR Events

### TaskHub

```csharp
public class TaskHub : Hub
{
    // 加入项目组
    public async Task JoinProject(int projectId)
    {
        await Groups.AddToGroupAsync(Context.ConnectionId, $"project_{projectId}");
    }

    // 离开项目组
    public async Task LeaveProject(int projectId)
    {
        await Groups.RemoveFromGroupAsync(Context.ConnectionId, $"project_{projectId}");
    }
}
```

### 事件定义

```csharp
// 任务移动事件
public class TaskMovedEvent
{
    public int TaskId { get; set; }
    public string NewStatus { get; set; }
}

// 评论添加事件
public class CommentAddedEvent
{
    public int TaskId { get; set; }
    public CommentDto Comment { get; set; }
}
```

## Security Considerations

1. **认证简化**: 初始版本使用预定义用户，无需密码
2. **权限控制**: 评论只允许编辑/删除自己的
3. **输入验证**: 所有用户输入必须验证
4. **SQL注入防护**: 使用参数化查询（EF Core）
5. **CORS配置**: 允许Blazor Server访问API
```

---

#### Step 5: Tasks

```markdown
# Tasks: Taskify Platform

**Prerequisites**: plan.md, spec.md

## Phase 1: Setup

- T001 创建解决方案结构（Taskify.sln）
- T002 初始化 .NET Aspire 项目（Aspire.AppHost）
- T003 [P] 初始化 Blazor Server 项目（Taskify.Web）
- T004 [P] 初始化 API 项目（Taskify.Api）
- T005 安装 NuGet 包依赖
- T006 配置 PostgreSQL 连接
- T007 [P] 配置 SignalR

## Phase 2: Foundational (CRITICAL)

- T008 创建数据库模型（Models/Entities）
- T009 配置 Entity Framework Core
- T010 创建数据库迁移
- T011 初始化数据库（5个用户 + 3个项目 + 任务）
- T012 实现 Repository 模式
- T013 [P] 实现 Service 基类
- T014 配置 SignalR Hub

**Checkpoint** ✅ 基础设施就绪

## Phase 3: User Story 1 - 用户选择与项目列表

- T015 [US1] 创建 User API Controller（获取用户列表）
- T016 [US1] 创建 Project API Controller（获取用户项目）
- T017 [US1] 创建 UserList.razor 组件（用户选择页面）
- T018 [US1] 创建 ProjectList.razor 组件（项目列表页面）
- T019 [US1] 实现路由配置（/ → UserList → ProjectList）
- T020 [US1] 添加用户卡片样式

**Checkpoint** ✅ US1 完成

## Phase 4: User Story 2 - 看板视图

- T021 [US2] 创建 Task API Controller（获取项目任务）
- T022 [US2] 创建 TaskController（更新任务状态）
- T023 [US2] 创建 KanbanBoard.razor 组件（4个列）
- T024 [US2] 创建 TaskCard.razor 组件（任务卡片）
- T025 [US2] 实现拖拽功能（使用 MudBlazor Drag & Drop）
- T026 [US2] 集成 SignalR 实时更新
- T027 [US2] 测试拖拽流畅度

**Checkpoint** ✅ US2 完成

## Phase 5: User Story 3 - 任务评论

- T028 [US3] 创建 Comment API Controller（添加/编辑/删除评论）
- T029 [US3] 创建 Comment.razor 组件（评论列表）
- T030 [US3] 创建 CommentForm.razor 组件（添加/编辑评论）
- T031 [US3] 实现评论权限验证（只能编辑自己的）
- T032 [US3] 集成 SignalR 评论推送
- T033 [US3] 添加评论样式和动画

**Checkpoint** ✅ US3 完成

## Phase 6: Polish

- T034 [P] 添加单元测试
- T035 [P] 添加集成测试
- T036 性能优化（数据库查询、SignalR连接）
- T037 错误处理和日志记录
- T038 添加文档（API文档、用户手册）
- T039 Docker 容器化

## Dependencies

```
T001 → T002 → T003 → T004 → T005 → T006 → T007
       ↓
T008 → T009 → T010 → T011 → T012 → T013 → T014
       ↓
T015 → T016 → T017 → T018 → T019 → T020
       ↓
T021 → T022 → T023 → T024 → T025 → T026 → T027
       ↓
T028 → T029 → T030 → T031 → T032 → T033
       ↓
T034 → T035 → T036 → T037 → T038 → T039
```

## Parallel Opportunities

- T003, T004, T007 可以并行执行
- T034, T035, T036 可以并行执行
```

---

#### Step 6: Implement

```bash
/implement
```

---

### 项目结构

```
Taskify/
├── Taskify.sln                    # 解决方案文件
├── Taskify.AppHost/              # .NET Aspire 主机
│   ├── Program.cs
│   └── appsettings.json
├── Taskify.Api/                  # 后端 API
│   ├── Controllers/
│   │   ├── UsersController.cs
│   │   ├── ProjectsController.cs
│   │   ├── TasksController.cs
│   │   └── CommentsController.cs
│   ├── Services/
│   │   ├── Interfaces/
│   │   ├── TaskService.cs
│   │   └── ProjectService.cs
│   ├── Models/
│   │   ├── Entities/
│   │   ├── DTOs/
│   │   └── Events/
│   ├── Hubs/
│   │   └── TaskHub.cs
│   ├── Data/
│   │   ├── AppDbContext.cs
│   │   └── Migrations/
│   └── Program.cs
├── Taskify.Web/                  # Blazor 前端
│   ├── Pages/
│   │   ├── UserList.razor
│   │   ├── ProjectList.razor
│   │   └── KanbanBoard.razor
│   ├── Components/
│   │   ├── TaskCard.razor
│   │   ├── CommentList.razor
│   │   └── CommentForm.razor
│   ├── Services/
│   │   └── SignalRService.cs
│   └── Program.cs
└── specs/
    └── 001-taskify-platform/
        ├── spec.md
        ├── plan.md
        └── tasks.md
```

---

## 案例2：Photo Album Organizer 照片相册

### 项目背景

**目标**：开发一个本地照片管理应用，支持按日期分组、拖拽排序。

**核心功能**：
- 读取本地照片
- 按日期自动分组
- 拖拽调整相册顺序
- 照片预览
- 无需上传到云端

---

### 完整流程

#### Step 1: Constitution

```markdown
# Project Constitution

## Technical Stack
- 前端：Vite + Vanilla JS
- 存储：本地 SQLite
- 构建工具：Vite

## Architecture Constraints
- 轻量级设计，最小依赖
- 无需后端服务器
- 数据本地存储

## Coding Standards
- 使用现代 ES6+ JavaScript
- 遵循 Airbnb JavaScript Style Guide
- 所有函数必须有 JSDoc 注释
```

---

#### Step 2: Specify

```markdown
# Feature Specification: Photo Album Organizer

**Feature Branch**: `002-photo-album-organizer`
**Created**: 2026-01-27
**Status**: Draft

## User Scenarios & Testing

### User Story 1 - 照片导入与分组 (Priority: P1)

作为用户，我希望导入本地照片并按日期自动分组为相册，以便整理我的照片。

**Why this priority**: 核心功能，整个应用的基础

**Independent Test**: 选择照片文件夹 → 系统扫描 → 自动按日期分组 → 显示相册列表

**Acceptance Scenarios**:
1. Given 用户选择照片文件夹, When 扫描完成, Then 照片按拍摄日期分组为多个相册
2. Given 照片没有EXIF日期, When 分组时, Then 使用文件修改日期
3. Given 同一天有多张照片, When 分组时, Then 归入同一个相册

### User Story 2 - 拖拽排序 (Priority: P1)

作为用户，我希望拖拽相册调整顺序，以便按我喜欢的顺序浏览照片。

**Why this priority**: 核心交互功能

**Independent Test**: 拖拽相册A到相册B位置 → 松开 → 相册顺序改变

**Acceptance Scenarios**:
1. Given 用户拖拽相册, When 拖拽到新位置, Then 相册列表更新顺序
2. Given 用户拖拽相册, When 松开鼠标, Then 新顺序保存到数据库
3. Given 用户刷新页面, When 页面加载完成, Then 相册顺序保持不变

### User Story 3 - 照片预览 (Priority: P2)

作为用户，我希望点击照片查看大图，以便仔细查看照片细节。

**Why this priority**: 重要功能，提升用户体验

**Independent Test**: 点击照片 → 显示大图 → 点击关闭 → 返回相册视图

**Acceptance Scenarios**:
1. Given 用户点击照片, When 照片加载完成, Then 显示大图预览
2. Given 用户在大图预览界面, When 点击关闭按钮, Then 返回相册视图
3. Given 用户切换相册, When 预览界面打开, Then 只显示当前相册的照片

## Requirements

### Functional Requirements
- **FR-001**: 支持导入本地文件夹中的照片（JPG, PNG, WEBP）
- **FR-002**: 自动按拍摄日期或文件修改日期分组
- **FR-003**: 支持拖拽相册调整顺序
- **FR-004**: 支持照片大图预览
- **FR-005**: 相册顺序持久化存储

### Key Entities
- **Album**: 相册实体，包含ID、日期、照片数量、排序顺序
- **Photo**: 照片实体，包含ID、文件路径、拍摄日期、相册ID

## Success Criteria
- **SC-001**: 导入1000张照片的分组时间 < 5秒
- **SC-002**: 拖拽响应时间 < 100ms
- **SC-003**: 照片预览加载时间 < 1秒
```

---

#### Step 3: Clarify

```markdown
# Clarifications

## EXIF 日期处理

**Q**: 如果照片的 EXIF 日期不可用怎么办？

**A**: 降级策略：
1. 优先使用 EXIF 中的 DateTimeOriginal
2. 如果不可用，使用文件修改日期
3. 如果都不存在，使用导入日期

## 拖拽实现

**Q**: 使用什么库实现拖拽？

**A**: 使用原生 HTML5 Drag and Drop API，无需第三方库，保持轻量级。

## 数据库初始化

**Q**: SQLite 数据库文件存放在哪里？

**A**: 存放在应用数据目录：
- Windows: `%APPDATA%/PhotoAlbum/albums.db`
- macOS: `~/Library/Application Support/PhotoAlbum/albums.db`
- Linux: `~/.config/PhotoAlbum/albums.db`
```

---

#### Step 4: Plan

```markdown
# Implementation Plan: Photo Album Organizer

**Branch**: `002-photo-album-organizer`
**Date**: 2026-01-27
**Spec**: [链接到 spec.md]

## Summary

开发一个基于 Vite + Vanilla JS 的照片管理应用，使用 SQLite 存储元数据，原生 HTML5 拖拽 API，无后端服务器。

## Technical Context

**Language/Version**: JavaScript ES6+

**Primary Dependencies**: Vite, better-sqlite3, exif-js

**Storage**: SQLite

**Testing**: Vitest, Playwright

**Target Platform**: Desktop browser (Chrome, Firefox, Safari, Edge)

**Project Type**: web (single-page app)

**Performance Goals**: 导入1000张照片 < 5秒, 拖拽响应 < 100ms

**Constraints**: 无第三方UI库, 最小依赖包, <5MB 打包大小

**Scale/Scope**: 支持 10,000+ 张照片, 1,000+ 个相册

## Constitution Check

✅ 技术栈符合 Constitution.md 规定
✅ 轻量级设计，最小依赖
✅ 数据本地存储

## Architecture Decisions

### 1. 前端架构

**决策**: 单页应用（SPA） + Vanilla JS

**理由**:
- 轻量级，无框架开销
- Vite 提供快速开发和构建
- Vanilla JS 保持代码简洁

**架构**:
```
index.html
    ↓
main.js (入口)
    ↓
modules/
    ├── album-manager.js  (相册管理)
    ├── photo-scanner.js  (照片扫描)
    ├── drag-drop.js      (拖拽逻辑)
    └── ui-renderer.js    (UI 渲染)
```

### 2. 数据库设计

**决策**: SQLite + better-sqlite3

**理由**:
- 轻量级，无需服务器
- better-sqlite3 性能优秀
- 适合桌面应用

**Schema**:
```sql
CREATE TABLE albums (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    date TEXT NOT NULL,  -- YYYY-MM-DD
    photo_count INTEGER DEFAULT 0,
    sort_order INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE photos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    album_id INTEGER NOT NULL,
    file_path TEXT NOT NULL UNIQUE,
    file_name TEXT NOT NULL,
    capture_date TIMESTAMP,
    file_size INTEGER,
    width INTEGER,
    height INTEGER,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (album_id) REFERENCES albums(id)
);

CREATE INDEX idx_photos_album ON photos(album_id);
CREATE INDEX idx_photos_date ON photos(capture_date);
```

### 3. 拖拽实现

**决策**: 原生 HTML5 Drag and Drop

**理由**:
- 无需第三方库
- 浏览器原生支持
- 性能良好

**设计**:
- 相册列表容器 `ul` 支持 `dragover` 和 `drop`
- 每个相册项 `li` 设置 `draggable="true"`
- 使用 `data-index` 跟踪位置

## Data Model

### JavaScript Classes

```javascript
class Album {
    constructor(id, date, photoCount, sortOrder) {
        this.id = id;
        this.date = date;
        this.photoCount = photoCount;
        this.sortOrder = sortOrder;
    }
}

class Photo {
    constructor(id, albumId, filePath, fileName, captureDate, fileSize, width, height) {
        this.id = id;
        this.albumId = albumId;
        this.filePath = filePath;
        this.fileName = fileName;
        this.captureDate = captureDate;
        this.fileSize = fileSize;
        this.width = width;
        this.height = height;
    }
}
```

## UI Components

### 主布局

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Photo Album Organizer</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>📷 Photo Album Organizer</h1>
        <button id="importBtn">📂 Import Photos</button>
    </header>

    <main>
        <ul id="albumList" class="album-list">
            <!-- 相册列表由 JS 动态生成 -->
        </ul>
    </main>

    <div id="photoModal" class="modal hidden">
        <div class="modal-content">
            <span class="close-btn">&times;</span>
            <img id="previewImage" src="" alt="Photo Preview">
        </div>
    </div>

    <script type="module" src="main.js"></script>
</body>
</html>
```

## API Design (Internal)

### PhotoScanner

```javascript
class PhotoScanner {
    async scanDirectory(dirPath) {
        // 扫描目录，返回照片文件列表
    }

    async extractExifDate(filePath) {
        // 提取 EXIF 日期
    }

    async getModifiedDate(filePath) {
        // 获取文件修改日期
    }
}
```

### AlbumManager

```javascript
class AlbumManager {
    async importPhotos(photoPaths) {
        // 导入照片，按日期分组，创建相册
    }

    async getAlbums() {
        // 获取所有相册（按 sort_order 排序）
    }

    async getPhotos(albumId) {
        // 获取相册的所有照片
    }

    async updateAlbumOrder(albumIds) {
        // 更新相册顺序
    }
}
```

### DragDropManager

```javascript
class DragDropManager {
    constructor(albumListElement, onReorder) {
        this.element = albumListElement;
        this.onReorder = onReorder;
        this.setupEventListeners();
    }

    setupEventListeners() {
        // 设置拖拽事件监听
    }

    handleDragStart(e) {
        // 处理拖拽开始
    }

    handleDragOver(e) {
        // 处理拖拽悬停
    }

    handleDrop(e) {
        // 处理放置
    }
}
```

## Performance Considerations

1. **批量导入**: 使用批量插入，避免逐条插入
2. **懒加载**: 滚动时加载照片缩略图
3. **虚拟列表**: 大量相册时使用虚拟滚动
4. **Web Worker**: 照片扫描在 Web Worker 中执行，不阻塞UI

## Security Considerations

1. **文件访问**: 使用 File System Access API（如果支持）
2. **路径注入**: 验证文件路径，防止路径遍历攻击
3. **XSS防护**: 照片文件名需 HTML 转义
4. **数据存储**: SQLite 数据库文件加密（可选）
```

---

#### Step 5: Tasks

```markdown
# Tasks: Photo Album Organizer

**Prerequisites**: plan.md, spec.md

## Phase 1: Setup

- T001 创建 Vite 项目（vanilla-js template）
- T002 安装依赖（better-sqlite3, exif-js）
- T003 配置项目结构（modules/, db/, styles/）
- T004 初始化 SQLite 数据库

## Phase 2: Foundational (CRITICAL)

- T005 创建数据库 schema（albums, photos）
- T006 实现数据库连接池
- T007 创建 Album 和 Photo 模型类
- T008 配置 Web Worker 环境支持

**Checkpoint** ✅ 基础设施就绪

## Phase 3: User Story 1 - 照片导入与分组

- T009 [US1] 实现 PhotoScanner.scanDirectory
- T010 [US1] 实现 PhotoScanner.extractExifDate
- T011 [US1] 实现 AlbumManager.importPhotos（按日期分组）
- T012 [US1] 创建文件选择器 UI（<input type="file">）
- T013 [US1] 实现导入进度显示
- T014 [US1] 实现相册列表渲染

**Checkpoint** ✅ US1 完成

## Phase 4: User Story 2 - 拖拽排序

- T015 [US2] 实现 DragDropManager 类
- T016 [US2] 实现拖拽事件处理（dragstart, dragover, drop）
- T017 [US2] 实现视觉反馈（拖拽时的高亮效果）
- T018 [US2] 实现 AlbumManager.updateAlbumOrder
- T019 [US2] 测试拖拽流畅度

**Checkpoint** ✅ US2 完成

## Phase 5: User Story 3 - 照片预览

- T020 [US3] 实现照片缩略图渲染（CSS object-fit）
- T021 [US3] 创建照片预览 Modal（HTML + CSS）
- T022 [US3] 实现大图加载（点击照片 → 显示大图）
- T023 [US3] 实现关闭功能（点击关闭/背景 → 返回）
- T024 [US3] 实现照片切换（上一张/下一张）

**Checkpoint** ✅ US3 完成

## Phase 6: Polish

- T025 [P] 添加响应式设计（移动端适配）
- T026 [P] 添加键盘快捷键（方向键浏览照片）
- T027 性能优化（虚拟滚动、懒加载）
- T028 添加错误处理（无效照片格式、损坏的文件）
- T029 添加单元测试（Vitest）
- T030 添加 E2E 测试（Playwright）

## Dependencies

```
T001 → T002 → T003 → T004
       ↓
T005 → T006 → T007 → T008
       ↓
T009 → T010 → T011 → T012 → T013 → T014
       ↓
T015 → T016 → T017 → T018 → T019
       ↓
T020 → T021 → T022 → T023 → T024
       ↓
T025 → T026 → T027 → T028 → T029 → T030
```

## Parallel Opportunities

- T025, T026, T029 可以并行执行
```

---

#### Step 6: Implement

```bash
/implement
```

---

### 项目结构

```
photo-album-organizer/
├── index.html
├── style.css
├── main.js
├── modules/
│   ├── album-manager.js
│   ├── photo-scanner.js
│   ├── drag-drop.js
│   ├── ui-renderer.js
│   └── db.js
├── db/
│   ├── schema.sql
│   └── albums.db
├── worker/
│   └── photo-scanner.worker.js
├── styles/
│   ├── main.css
│   ├── modal.css
│   └── drag-drop.css
├── tests/
│   ├── unit/
│   └── e2e/
├── vite.config.js
└── package.json
```

---

## 案例3：Bookstore E-commerce 书店电商

### 项目背景

**目标**：开发一个书店电商前端，包含商品列表、详情页、购物车。

**核心功能**：
- 商品列表展示
- 商品详情查看
- 购物车管理
- Mock API 数据

---

### 完整流程

#### Step 1: Constitution

```markdown
# Project Constitution

## Technical Stack
- 前端：React 18 + TypeScript + Vite
- UI框架：Tailwind CSS
- HTTP客户端：Axios
- 状态管理：useState, useReducer, Context API
- 路由：React Router

## Architecture Constraints
- 组件化设计
- TypeScript 严格模式
- RESTful API 架构

## Coding Standards
- ESLint + Prettier
- 使用函数组件 + Hooks
- 所有组件必须有 TypeScript 类型定义
```

---

#### Step 2: Specify

```markdown
# Feature Specification: Bookstore E-commerce

**Feature Branch**: `003-bookstore-ecommerce`
**Created**: 2026-01-27
**Status**: Draft

## User Scenarios & Testing

### User Story 1 - 商品列表页 (Priority: P1)

作为用户，我希望浏览书籍列表，以便找到我感兴趣的书籍。

**Why this priority**: 核心功能，用户入口

**Independent Test**: 打开首页 → 显示书籍网格 → 查看书籍信息

**Acceptance Scenarios**:
1. Given 用户访问首页, When 页面加载完成, Then 显示书籍网格列表
2. Given 书籍列表加载, When 每本书加载完成, Then 显示封面、标题、作者、价格
3. Given 用户点击书籍, When 点击事件触发, Then 跳转到书籍详情页

### User Story 2 - 商品详情页 (Priority: P1)

作为用户，我希望查看书籍详细信息并加入购物车，以便了解书籍并购买。

**Why this priority**: 核心功能，购买流程的关键

**Independent Test**: 点击书籍 → 显示详情 → 点击"加入购物车" → 成功提示

**Acceptance Scenarios**:
1. Given 用户点击书籍, When 详情页加载完成, Then 显示大封面、标题、作者、描述、价格、"加入购物车"按钮
2. Given 用户点击"加入购物车", When 操作成功, Then 显示"已添加到购物车"提示
3. Given 用户点击"返回列表", When 点击事件触发, Then 返回书籍列表页

### User Story 3 - 购物车 (Priority: P1)

作为用户，我希望查看购物车并调整数量，以便管理我要购买的书籍。

**Why this priority**: 核心功能，购买流程的关键

**Independent Test**: 点击购物车图标 → 显示购物车 → 调整数量 → 显示总价

**Acceptance Scenarios**:
1. Given 用户点击购物车, When 购物车页面加载完成, Then 显示已添加的书籍列表
2. Given 用户增加书籍数量, When 数量更新, Then 单行小计和总价自动更新
3. Given 用户减少数量到0, When 数量更新, Then 该书籍从购物车移除
4. Given 用户点击"结算", When 点击事件触发, Then 显示"结算功能开发中"提示

## Requirements

### Functional Requirements
- **FR-001**: 书籍列表页显示网格布局，响应式设计
- **FR-002**: 书籍详情页显示完整信息（封面、标题、作者、描述、价格）
- **FR-003**: 购物车支持添加、修改数量、删除商品
- **FR-004**: 购物车实时计算总价
- **FR-005**: 使用 Mock API 提供书籍数据

### Key Entities
- **Book**: 书籍实体，包含ID、标题、作者、描述、价格、封面URL
- **CartItem**: 购物车项实体，包含书籍ID、数量

## Success Criteria
- **SC-001**: 书籍列表页加载时间 < 1秒
- **SC-002**: 加入购物车操作响应时间 < 200ms
- **SC-003**: 购物车状态在所有页面同步（Context API）
```

---

#### Step 3: Clarify

```markdown
# Clarifications

## Mock API 数据

**Q**: Mock API 返回多少本书籍数据？

**A**: 初始版本返回 12 本书籍，格式如下：

```json
[
  {
    "id": 1,
    "title": "The Great Gatsby",
    "author": "F. Scott Fitzgerald",
    "description": "A novel set in the Jazz Age...",
    "price": 12.99,
    "coverUrl": "https://example.com/covers/gatsby.jpg"
  },
  ...
]
```

## 购物车持久化

**Q**: 购物车数据是否持久化到本地存储？

**A**: 初始版本使用内存存储（Context API），购物车在刷新页面后清空。后续版本可添加 localStorage 支持。

## 路由配置

**Q**: 使用什么路由模式？

**A**: 使用 Hash Router（React Router HashRouter），无需服务器配置，适合静态部署。
```

---

#### Step 4: Plan

```markdown
# Implementation Plan: Bookstore E-commerce

**Branch**: `003-bookstore-ecommerce`
**Date**: 2026-01-27
**Spec**: [链接到 spec.md]

## Summary

开发一个基于 React 18 + TypeScript 的书店电商前端，使用 Tailwind CSS 样式，Axios 调用 Mock API，Context API 管理购物车状态。

## Technical Context

**Language/Version**: TypeScript 5.0+ / React 18

**Primary Dependencies**: React 18, React Router, Axios, Tailwind CSS

**Storage**: Mock API (json-server 或内存)

**Testing**: React Testing Library, Vitest

**Target Platform**: Web browser

**Project Type**: web (frontend only)

**Performance Goals**: 列表加载 < 1秒, 购物车操作 < 200ms

**Constraints**: 无后端依赖, 静态部署友好, SEO 可选

**Scale/Scope**: 初始 100 本书籍, 支持 10,000 购物车并发

## Constitution Check

✅ 技术栈符合 Constitution.md 规定
✅ 组件化设计
✅ TypeScript 严格模式

## Architecture Decisions

### 1. 前端架构

**决策**: React 18 + TypeScript + Vite

**理由**:
- Vite 提供快速的开发体验
- TypeScript 提供类型安全
- React 18 支持并发特性

**架构**:
```
src/
├── components/       # 可复用组件
│   ├── BookCard.tsx
│   ├── CartItem.tsx
│   └── Button.tsx
├── pages/          # 页面组件
│   ├── BookListPage.tsx
│   ├── BookDetailPage.tsx
│   └── CartPage.tsx
├── contexts/       # React Context
│   └── CartContext.tsx
├── services/       # API 服务
│   └── bookService.ts
├── types/          # TypeScript 类型
│   └── index.ts
└── App.tsx
```

### 2. 状态管理

**决策**: Context API + useReducer

**理由**:
- 轻量级，无需 Redux
- 足够管理购物车状态
- TypeScript 类型安全

**设计**:
```typescript
interface CartState {
  items: CartItem[];
  total: number;
}

interface CartContextValue {
  state: CartState;
  addToCart: (book: Book) => void;
  removeFromCart: (bookId: number) => void;
  updateQuantity: (bookId: number, quantity: number) => void;
  clearCart: () => void;
}
```

### 3. 路由设计

**决策**: React Router HashRouter

**理由**:
- 无需服务器配置
- 适合静态部署
- SEO 可选（后续可用 BrowserRouter）

**路由**:
```typescript
<Routes>
  <Route path="/" element={<BookListPage />} />
  <Route path="/book/:id" element={<BookDetailPage />} />
  <Route path="/cart" element={<CartPage />} />
</Routes>
```

## API Design

### Mock API (内存或 json-server)

### 1. 获取书籍列表

```
GET /api/books

Response:
{
  "books": [
    {
      "id": 1,
      "title": "The Great Gatsby",
      "author": "F. Scott Fitzgerald",
      "description": "A novel set in the Jazz Age...",
      "price": 12.99,
      "coverUrl": "https://example.com/covers/gatsby.jpg"
    },
    ...
  ]
}
```

### 2. 获取书籍详情

```
GET /api/books/:id

Response:
{
  "id": 1,
  "title": "The Great Gatsby",
  "author": "F. Scott Fitzgerald",
  "description": "A novel set in the Jazz Age...",
  "price": 12.99,
  "coverUrl": "https://example.com/covers/gatsby.jpg",
  "isbn": "978-0743273565",
  "pages": 180,
  "publisher": "Scribner"
}
```

## Data Model

### TypeScript Types

```typescript
interface Book {
  id: number;
  title: string;
  author: string;
  description: string;
  price: number;
  coverUrl: string;
  isbn?: string;
  pages?: number;
  publisher?: string;
}

interface CartItem {
  book: Book;
  quantity: number;
}

interface CartState {
  items: CartItem[];
  total: number;
}
```

## Component Design

### BookCard Component

```typescript
interface BookCardProps {
  book: Book;
  onAddToCart: (book: Book) => void;
}

const BookCard: React.FC<BookCardProps> = ({ book, onAddToCart }) => {
  return (
    <div className="book-card">
      <img src={book.coverUrl} alt={book.title} />
      <h3>{book.title}</h3>
      <p>{book.author}</p>
      <p>${book.price.toFixed(2)}</p>
      <Button onClick={() => onAddToCart(book)}>
        Add to Cart
      </Button>
    </div>
  );
};
```

### CartContext

```typescript
import React, { createContext, useReducer, useContext } from 'react';

const CartContext = createContext<CartContextValue | undefined>(undefined);

const cartReducer = (state: CartState, action: CartAction): CartState => {
  switch (action.type) {
    case 'ADD_ITEM':
      // 逻辑...
    case 'REMOVE_ITEM':
      // 逻辑...
    case 'UPDATE_QUANTITY':
      // 逻辑...
    case 'CLEAR_CART':
      // 逻辑...
    default:
      return state;
  }
};

export const CartProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [state, dispatch] = useReducer(cartReducer, initialState);

  const value = {
    state,
    addToCart: (book) => dispatch({ type: 'ADD_ITEM', payload: book }),
    removeFromCart: (bookId) => dispatch({ type: 'REMOVE_ITEM', payload: bookId }),
    updateQuantity: (bookId, quantity) => dispatch({ type: 'UPDATE_QUANTITY', payload: { bookId, quantity } }),
    clearCart: () => dispatch({ type: 'CLEAR_CART' }),
  };

  return <CartContext.Provider value={value}>{children}</CartContext.Provider>;
};

export const useCart = () => {
  const context = useContext(CartContext);
  if (!context) {
    throw new Error('useCart must be used within a CartProvider');
  }
  return context;
};
```

## UI/UX Design

### Tailwind CSS 配置

```css
/* tailwind.config.js */
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: '#3b82f6',
        secondary: '#6366f1',
      },
    },
  },
};
```

### 响应式设计

```html
<!-- 书籍列表网格 -->
<div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
  {books.map((book) => (
    <BookCard key={book.id} book={book} />
  ))}
</div>
```

## Performance Considerations

1. **代码分割**: 使用 React.lazy() 和 Suspense
2. **图片优化**: 使用 WebP 格式，懒加载
3. **缓存策略**: 使用 React Query 或 SWR 缓存 API 数据
4. **Bundle 大小**: Tree-shaking，移除未使用的代码

## Security Considerations

1. **XSS防护**: React 默认转义 HTML，避免使用 dangerouslySetInnerHTML
2. **输入验证**: 所有用户输入验证
3. **CORS配置**: Mock API 允许跨域
4. **HTTPS**: 生产环境强制 HTTPS
```

---

#### Step 5: Tasks

```markdown
# Tasks: Bookstore E-commerce

**Prerequisites**: plan.md, spec.md

## Phase 1: Setup

- T001 创建 Vite + React + TypeScript 项目
- T002 安装依赖（React Router, Axios, Tailwind CSS）
- T003 配置 Tailwind CSS
- T004 配置 ESLint + Prettier
- T005 创建项目结构（components/, pages/, contexts/, services/, types/）

## Phase 2: Foundational (CRITICAL)

- T006 创建 TypeScript 类型定义（types/index.ts）
- T007 创建 Mock API 服务（services/bookService.ts）
- T008 配置 Mock 数据（12本书籍）
- T009 创建 CartContext（contexts/CartContext.tsx）
- T010 配置 React Router（App.tsx）

**Checkpoint** ✅ 基础设施就绪

## Phase 3: User Story 1 - 商品列表页

- T011 [US1] 创建 BookListPage 组件（pages/BookListPage.tsx）
- T012 [US1] 创建 BookCard 组件（components/BookCard.tsx）
- T013 [US1] 创建 Button 组件（components/Button.tsx）
- T014 [US1] 实现书籍列表 API 调用
- T015 [US1] 实现书籍网格布局（Tailwind CSS）
- T016 [US1] 实现书籍卡片点击跳转

**Checkpoint** ✅ US1 完成

## Phase 4: User Story 2 - 商品详情页

- T017 [US2] 创建 BookDetailPage 组件（pages/BookDetailPage.tsx）
- T018 [US2] 实现书籍详情 API 调用
- T019 [US2] 实现详情页布局（大封面 + 信息 + "加入购物车"按钮）
- T020 [US2] 集成 CartContext.addToCart
- T021 [US2] 实现"已添加到购物车"提示
- T022 [US2] 实现"返回列表"功能

**Checkpoint** ✅ US2 完成

## Phase 5: User Story 3 - 购物车

- T023 [US3] 创建 CartPage 组件（pages/CartPage.tsx）
- T024 [US3] 创建 CartItem 组件（components/CartItem.tsx）
- T025 [US3] 实现购物车列表渲染
- T026 [US3] 实现数量调整（+/-按钮）
- T027 [US3] 实现删除功能
- T028 [US3] 实现总价计算
- T029 [US3] 实现"结算"按钮（显示提示）

**Checkpoint** ✅ US3 完成

## Phase 6: Polish

- T030 [P] 添加响应式设计（移动端适配）
- T031 [P] 添加加载状态（Skeleton Screen）
- T032 [P] 添加错误处理（API 失败提示）
- T033 [P] 添加单元测试（React Testing Library）
- T034 [P] 添加 E2E 测试（Playwright）
- T035 性能优化（代码分割、图片懒加载）

## Dependencies

```
T001 → T002 → T003 → T004 → T005
       ↓
T006 → T007 → T008 → T009 → T010
       ↓
T011 → T012 → T013 → T014 → T015 → T016
       ↓
T017 → T018 → T019 → T020 → T021 → T022
       ↓
T023 → T024 → T025 → T026 → T027 → T028 → T029
       ↓
T030 → T031 → T032 → T033 → T034 → T035
```

## Parallel Opportunities

- T030, T031, T033 可以并行执行
```

---

#### Step 6: Implement

```bash
/implement
```

---

### 项目结构

```
bookstore-ecommerce/
├── index.html
├── package.json
├── tsconfig.json
├── vite.config.ts
├── tailwind.config.js
├── src/
│   ├── main.tsx
│   ├── App.tsx
│   ├── components/
│   │   ├── BookCard.tsx
│   │   ├── CartItem.tsx
│   │   └── Button.tsx
│   ├── pages/
│   │   ├── BookListPage.tsx
│   │   ├── BookDetailPage.tsx
│   │   └── CartPage.tsx
│   ├── contexts/
│   │   └── CartContext.tsx
│   ├── services/
│   │   └── bookService.ts
│   ├── types/
│   │   └── index.ts
│   └── styles/
│       └── index.css
├── public/
│   └── images/
└── tests/
    ├── unit/
    └── e2e/
```

---

## 案例对比分析

### 三个案例的异同点

| 维度 | Taskify | Photo Album | Bookstore |
|------|---------|-------------|-----------|
| **技术栈** | .NET Aspire + Blazor | Vite + Vanilla JS | React + TypeScript |
| **架构模式** | 微服务 + SignalR | 单页应用 + SQLite | 前端 + Mock API |
| **数据存储** | PostgreSQL | SQLite | 内存/LocalStorage |
| **实时通信** | SignalR | 无 | 无 |
| **复杂度** | 高 | 中 | 中 |
| **团队规模** | 5-10人 | 1-2人 | 3-5人 |
| **开发周期** | 3-6个月 | 2-4周 | 1-2个月 |

### 适用场景分析

#### Taskify（微服务架构）

**适合**：
- 多用户实时协作
- 复杂业务逻辑
- 长期维护的企业应用

**不适合**：
- 快速原型验证
- 单人项目
- 资源受限环境

---

#### Photo Album（轻量级设计）

**适合**：
- 桌面应用
- 单用户场景
- 本地数据管理

**不适合**：
- 多用户协作
- 云端同步需求
- 复杂业务逻辑

---

#### Bookstore（前端开发）

**适合**：
- 电商网站
- 内容展示
- 静态部署

**不适合**：
- 复杂后端逻辑
- 实时功能需求
- 大数据处理

---

## 关键经验总结

### 1. Spec-Kit 的价值体现

**三个案例共同点**：
- 通过明确的 Spec，减少了需求理解偏差
- 通过 Plan，确保技术方案的一致性
- 通过 Tasks，将复杂需求拆解为可执行的任务
- 通过 Implement，加速了代码生成

---

### 2. 技术选型的灵活性

Spec-Kit 不绑定任何技术栈，三个案例使用了不同的技术组合：
- **.NET**：企业级应用
- **Vanilla JS**：轻量级应用
- **React**：现代前端应用

---

### 3. 复杂度的可管理性

| 案例复杂度 | Spec-Kit 带来的价值 |
|------------|---------------------|
| **高**（Taskify） | 通过 Constitution 和 Plan 约束复杂度，通过 Tasks 明确依赖关系 |
| **中**（Photo Album） | 通过 Clarify 明确模糊需求，通过 Plan 确定轻量级方案 |
| **中**（Bookstore） | 通过 Spec 定义清晰的验收标准，通过 Tasks 合理拆分任务 |

---

### 4. 团队协作的标准化

无论团队规模大小，Spec-Kit 提供了标准化的协作流程：
- **PM**：关注 Spec 的验收标准
- **架构师**：关注 Plan 的技术方案
- **开发者**：关注 Tasks 的具体实现

---

### 5. 可追溯性

每个代码变更都可以追溯到：
- **Spec**：为什么需要这个功能
- **Plan**：为什么选择这个方案
- **Tasks**：为什么这样拆分任务

---

### 6. 质量保证

Spec-Kit 通过以下机制确保质量：
- **Constitution Check**：确保符合项目约束
- **Clarify**：消除需求模糊
- **Analyze**：验证一致性和完整性
- **Tests**：自动化测试覆盖

---

## 总结

**三个实战案例展示了 Spec-Kit 在不同场景下的应用**：

1. **Taskify**：企业级微服务应用，展示复杂架构和实时通信
2. **Photo Album**：轻量级桌面应用，展示最小依赖和本地存储
3. **Bookstore**：现代前端应用，展示组件化和状态管理

**共同经验**：
- 明确的 Spec 是成功的基础
- 合理的 Plan 决定技术方案的质量
- 清晰的 Tasks 让代码生成更高效
- 持续的验证确保质量

**Spec-Kit 不是"银弹"，而是"工程方法论"**：
- 它不自动解决问题
- 但提供了解决问题的框架
- 需要团队理解和实践
- 才能发挥最大价值

---

*本文档通过三个完整的项目案例，展示了 Spec-Kit 在实际项目中的应用流程和最佳实践。*
