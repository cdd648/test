# GitHub 代码上传指南

> 作者: cdd648  
> 日期: 2026-03-22  
> 仓库: https://github.com/cdd648/test

---

## 目录

1. [前置准备](#前置准备)
2. [配置 Git](#配置-git)
3. [初始化本地仓库](#初始化本地仓库)
4. [创建 GitHub 远程仓库](#创建-github-远程仓库)
5. [关联并推送代码](#关联并推送代码)
6. [常见问题](#常见问题)
7. [后续更新代码](#后续更新代码)

---

## 前置准备

### 1. 安装 Git

下载地址: https://git-scm.com/downloads

验证安装:
```bash
git --version
```

### 2. 注册 GitHub 账号

访问: https://github.com

---

## 配置 Git

### 设置用户名和邮箱

```bash
# 配置用户名（使用 GitHub 用户名）
git config --global user.name "cdd648"

# 配置邮箱（使用 GitHub 注册邮箱）
git config --global user.email "1061563811@qq.com"
```

### 验证配置

```bash
# 查看所有配置
git config --list

# 查看用户名
git config user.name

# 查看邮箱
git config user.email
```

---

## 初始化本地仓库

### 1. 进入项目目录

```bash
cd d:\Users\trae项目\cc
```

### 2. 初始化 Git 仓库

```bash
git init
```

输出:
```
Initialized empty Git repository in D:/Users/trae项目/cc/.git/
```

### 3. 创建 .gitignore 文件

创建 `.gitignore` 文件，添加以下内容:

```gitignore
# 依赖目录
node_modules/

# 构建输出
dist/
build/
out/

# 日志文件
*.log
logs/

# 系统文件
.DS_Store
Thumbs.db

# IDE 配置
.idea/
.vscode/
*.swp
*.swo

# 环境变量文件
.env
.env.local
.env.*.local

# 临时文件
*.tmp
*.temp
temp/
tmp/

# 缓存
.cache/
.npm/
```

### 4. 添加文件到暂存区

```bash
# 添加所有文件
git add .

# 或添加指定文件
git add filename.txt
```

### 5. 查看状态

```bash
git status
```

### 6. 创建首次提交

```bash
git commit -m "Initial commit: 项目初始化"
```

---

## 创建 GitHub 远程仓库

### 步骤

1. 登录 https://github.com
2. 点击右上角 **+** → **New repository**
3. 填写信息:
   - **Repository name**: `test`（仓库名称）
   - **Description**: （可选）项目描述
   - **Visibility**: 选择 Public（公开）或 Private（私有）
   - **不要勾选** "Add a README file"
   - **不要勾选** "Add .gitignore"
   - **不要勾选** "Choose a license"
4. 点击 **Create repository**

---

## 关联并推送代码

### 1. 添加远程仓库地址

```bash
git remote add origin https://github.com/cdd648/test.git
```

### 2. 验证远程地址

```bash
git remote -v
```

### 3. 推送代码到 GitHub

```bash
# 设置分支名称为 main
git branch -M main

# 推送到远程仓库
git push -u origin main
```

### 4. 使用 Personal Access Token 认证

如果推送时提示需要认证，需要创建 Personal Access Token:

#### 创建 Token

1. 访问: https://github.com/settings/tokens
2. 点击 **Generate new token** → **Generate new token (classic)**
3. 填写信息:
   - **Note**: `git-push`（Token 名称）
   - **Expiration**: 选择有效期（建议 90 天）
   - **Scopes**: 勾选 **repo**（完整仓库访问权限）
4. 点击 **Generate token**
5. **立即复制生成的 Token**（只显示一次）

Token 格式:
```
ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

#### 使用 Token 推送

```bash
# 修改远程地址，加入 Token
git remote set-url origin https://用户名:TOKEN@github.com/用户名/仓库名.git

# 示例（将 YOUR_TOKEN 替换为你的真实 Token）
git remote set-url origin https://cdd648:YOUR_TOKEN@github.com/cdd648/test.git

# 推送
git push -u origin main
```

---

## 常见问题

### 问题 1: 推送被拒绝（远程有更新）

**错误信息:**
```
! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/...'
```

**解决方法:**
```bash
# 先拉取远程更新
git pull origin main --allow-unrelated-histories

# 再推送
git push -u origin main
```

### 问题 2: 未配置用户信息

**错误信息:**
```
Author identity unknown
*** Please tell me who you are.
```

**解决方法:**
```bash
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱"
```

### 问题 3: 命令未找到

**错误信息:**
```
'git' 不是内部或外部命令
```

**解决方法:**
- 检查 Git 是否正确安装
- 将 Git 安装目录添加到系统 PATH 环境变量

---

## 后续更新代码

### 基本流程

```bash
# 1. 查看修改状态
git status

# 2. 添加修改的文件
git add .

# 3. 提交修改（写清楚修改内容）
git commit -m "修改说明"

# 4. 推送到 GitHub
git push
```

### 常用命令速查

| 命令 | 说明 |
|------|------|
| `git status` | 查看仓库状态 |
| `git add .` | 添加所有修改到暂存区 |
| `git add filename` | 添加指定文件到暂存区 |
| `git commit -m "msg"` | 提交修改 |
| `git push` | 推送到远程仓库 |
| `git pull` | 拉取远程更新 |
| `git log` | 查看提交历史 |
| `git remote -v` | 查看远程仓库地址 |

---

## 参考链接

- Git 官方文档: https://git-scm.com/doc
- GitHub 文档: https://docs.github.com/
- GitHub Token 管理: https://github.com/settings/tokens

---

## 本次操作记录

- **本地仓库**: `d:\Users\trae项目\cc`
- **远程仓库**: https://github.com/cdd648/test
- **上传文件**:
  - `.gitignore`
  - `claude-code-usage.md`
  - `claude-workflow-guide.md`
  - `README.md`
