# 🚀 上传到 GitHub 完整指南

## 第 1 步: 在 GitHub 创建仓库

1. 访问 [GitHub](https://github.com/new)
1. **Repository name**: `chat-app` (或你喜欢的名字)
1. **Description**: “私密聊天应用 - 计算器伪装”
1. **Visibility**: 选择 `Private` (私密)
1. **Do NOT initialize** with README (我们已有)
1. 点击 **“Create repository”**

## 第 2 步: 本地初始化 Git

```bash
# 进入项目目录
cd chat-app-project

# 初始化 Git
git init

# 添加所有文件
git add .

# 提交代码
git commit -m "Initial commit: 私密聊天应用"

# 添加远程仓库 (替换 YOUR_USERNAME 和 YOUR_REPO)
git remote add origin https://github.com/YOUR_USERNAME/chat-app.git

# 推送到 GitHub
git branch -M main
git push -u origin main
```

## 第 3 步: 验证上传

1. 访问你的仓库: `https://github.com/YOUR_USERNAME/chat-app`
1. 确认以下文件存在:
- ✅ `frontend/index.html`
- ✅ `backend/cloud-function.js`
- ✅ `docs/DEPLOYMENT.md`
- ✅ `README.md`
- ✅ `.env.example`

## 第 4 步: 一键部署（以后使用）

当你需要在新服务器部署时：

```bash
# 1. 克隆仓库
git clone https://github.com/YOUR_USERNAME/chat-app.git
cd chat-app

# 2. 安装依赖
npm install

# 3. 一键部署
chmod +x deploy.sh
./deploy.sh

# 4. 按照提示完成部署
```

## 第 5 步: 保护敏感信息

⚠️ **重要**: 确保以下信息 **不要** 上传到 GitHub

- `.env` 文件（已在 `.gitignore` 中）
- CloudBase 环境 ID
- API 密钥
- 数据库配置

这些信息存储在 `.env` 文件中，只在本地使用。

## 常用 Git 命令

```bash
# 查看当前状态
git status

# 提交新的更改
git add .
git commit -m "描述你的更改"
git push

# 获取最新版本
git pull

# 查看提交历史
git log --oneline

# 回滚到上一个版本
git reset --hard HEAD~1
```

## 分支管理

### 创建开发分支

```bash
# 创建新分支
git checkout -b develop

# 推送到远程
git push -u origin develop

# 回到主分支
git checkout main
```

### 合并分支

```bash
# 在 main 分支上
git merge develop

# 推送合并
git push
```

## GitHub Actions 自动部署（可选）

创建 `.github/workflows/deploy.yml`:

```yaml
name: 自动部署

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: 安装 Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '18'
      
      - name: 部署到腾讯云
        env:
          CLOUDBASE_ENV_ID: ${{ secrets.CLOUDBASE_ENV_ID }}
          CLOUDBASE_API_KEY: ${{ secrets.CLOUDBASE_API_KEY }}
        run: |
          npm install -g @cloudbase/cli
          cloudbase hosting:deploy frontend -e $CLOUDBASE_ENV_ID
```

然后在 GitHub 的 Settings → Secrets 中添加密钥。

## 版本管理

推荐使用语义化版本：

```bash
# 标记版本
git tag v1.0.0
git push origin v1.0.0

# 查看所有标签
git tag -l
```

## 协作开发

如果多人开发：

1. **Fork** 仓库到自己的账户
1. 在自己的分支上开发
1. 发送 **Pull Request**
1. 审查后 **Merge**

## 常见问题

### Q: 如何更改已上传的敏感信息？

```bash
# 从历史中删除文件（但保留本地）
git rm --cached .env
git commit -m "Remove .env from tracking"
git push
```

### Q: 如何同步最新代码？

```bash
# 如果你 Fork 了仓库
git remote add upstream https://github.com/ORIGINAL_USER/chat-app.git
git fetch upstream
git merge upstream/main
git push
```

### Q: 提交了错误的文件怎么办？

```bash
# 撤销上一个提交（保留更改）
git reset --soft HEAD~1

# 撤销上一个提交（删除更改）
git reset --hard HEAD~1
```

## 安全建议

1. **设置仓库权限**
- Settings → Access → Collaborators
- 只邀请信任的人
1. **启用两步验证**
- Settings → Security and analysis
- 启用 2FA
1. **定期审查访问权限**
- 移除不再需要的协作者
1. **隐藏敏感信息**
- 使用 GitHub Secrets
- 从来不要提交 `.env` 文件

-----

**下一步**: 每次更新时，使用 `git push` 保持 GitHub 最新！

**需要帮助?** 查看 [GitHub 官方文档](https://docs.github.com)