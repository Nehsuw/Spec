# Spec-Kit Skill Deployment Guide

## 文件结构检查

确保所有文件已创建:

```
spec-kit-skill/
├── skill.json                         ✅ 配置文件
├── README.md                          ✅ 说明文档
├── INSTALL.md                         ✅ 安装指南
├── prompt.md                          ✅ 主提示词
├── commands/                          ✅ 命令目录
│   ├── constitution.md                ✅
│   ├── specify.md                     ✅
│   ├── clarify.md                     ✅
│   ├── plan.md                        ✅
│   ├── tasks.md                       ✅
│   ├── implement.md                   ✅
│   └── analyze.md                     ✅
└── templates/                         ✅ 模板目录
    ├── constitution-template.md       ✅
    ├── spec-template.md               ✅
    ├── plan-template.md               ✅
    └── tasks-template.md              ✅
```

## 部署步骤

### 方式1: 发布到GitHub (推荐)

1. **创建Git仓库**

```bash
cd spec-kit-skill
git init
git add .
git commit -m "Initial commit: Spec-Kit skill v1.0.0"
```

2. **推送到GitHub**

```bash
# 创建GitHub仓库后
git remote add origin https://github.com/YOUR_USERNAME/spec-kit-skill.git
git branch -M main
git push -u origin main
```

3. **发布到skills.sh**

访问 https://skills.sh/ 并提交你的skill

### 方式2: 本地使用

直接将skill文件夹放在项目中,AI助手会自动识别。

### 方式3: 使用skills CLI

```bash
# 从GitHub安装
npx skills add YOUR_USERNAME/spec-kit-skill

# 全局安装
npx skills add -g YOUR_USERNAME/spec-kit-skill
```

## 验证部署

### 1. 检查skill.json

确保所有命令都正确配置:

```bash
cat skill.json
```

### 2. 测试命令可用性

与AI助手对话,测试每个命令:

```
/constitution
/specify
/clarify
/plan
/tasks
/implement
/analyze
```

### 3. 完整工作流测试

运行一个完整的示例项目:

```
1. /constitution Python 3.11 + FastAPI
2. /specify 简单的待办事项应用
3. /plan FastAPI + SQLite
4. /tasks
5. /implement
6. /analyze
```

## 更新版本

修改skill.json中的version字段:

```json
{
  "version": "1.0.1",
  ...
}
```

然后提交更新:

```bash
git add .
git commit -m "Update to v1.0.1"
git push
```

## 故障排除

### 问题1: AI助手无法识别命令

**解决**:
- 检查skill.json格式是否正确
- 确保commands目录下的文件存在
- 重启AI助手

### 问题2: 模板文件未找到

**解决**:
- 检查templates目录是否存在
- 确保文件路径正确
- 检查文件权限

### 问题3: 命令执行失败

**解决**:
- 查看命令文件内容是否完整
- 检查是否按照正确顺序执行
- 确认前置条件是否满足

## 维护建议

1. **定期更新**: 根据用户反馈更新skill
2. **版本控制**: 使用语义化版本号
3. **文档同步**: 保持文档与代码同步
4. **社区反馈**: 收集并回应用户问题
5. **兼容性**: 确保与最新AI助手兼容

## 贡献指南

欢迎提交Issue和Pull Request!

### 贡献流程

1. Fork仓库
2. 创建特性分支
3. 提交更改
4. 推送到分支
5. 创建Pull Request

### 代码规范

- 遵循Markdown格式规范
- 保持文档清晰易读
- 添加充分的示例
- 更新相关文档

## 许可证

MIT License

## 联系方式

- GitHub Issues: [仓库地址]/issues
- Email: [你的邮箱]

---

祝你使用Spec-Kit愉快! 🎉
