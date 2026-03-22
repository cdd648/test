# Claude Code 完整工作流程指南

> 版本: 1.0.0  
> 更新日期: 2026-03-22

---

## 目录

1. [环境准备](#一环境准备)
2. [切换工作目录](#二切换工作目录)
3. [启动 Claude Code](#三启动-claude-code)
4. [常用任务场景](#四常用任务场景)
5. [会话管理](#五会话管理)
6. [实用技巧](#六实用技巧)
7. [常见问题排查](#七常见问题排查)

---

## 一、环境准备

### 1.1 确认安装

```bash
# 检查是否已安装
claude --version

# 如未安装，执行安装
npm install -g @anthropic-ai/claude-code
```

### 1.2 首次认证

```bash
# 启动认证流程
claude auth
```

按提示完成登录和授权。

### 1.3 环境变量配置（Windows）

如果提示 `claude: command not found`，需要添加 npm 全局目录到 PATH：

```powershell
# 临时生效（当前会话）
$env:PATH += ";D:\nodejs\node_global"

# 永久生效
[Environment]::SetEnvironmentVariable(
    "PATH",
    [Environment]::GetEnvironmentVariable("PATH", "User") + ";D:\nodejs\node_global",
    "User"
)
```

---

## 二、切换工作目录

### 2.1 基础目录切换

```powershell
# 方式1: 直接 cd 到目标目录
cd d:\Users\trae项目\cc
claude

# 方式2: 使用绝对路径启动（无需先 cd）
claude --add-dir d:\Users\trae项目\cc
```

### 2.2 常用项目目录快速切换

```powershell
# 创建快捷函数（添加到 PowerShell Profile）
function proj-cc { cd d:\Users\trae项目\cc }
function proj-work { cd d:\work }

# 使用
proj-cc
claude
```

### 2.3 多目录项目处理

```bash
# 添加多个允许访问的目录
claude --add-dir d:\project1 --add-dir d:\project2 --add-dir d:\shared-lib

# 或使用配置文件
claude --settings project-settings.json
```

### 2.4 目录结构检查

```powershell
# 进入目录后，先查看结构
ls
# 或
tree /f
```

---

## 三、启动 Claude Code

### 3.1 基础启动方式

```bash
# 方式1: 交互式会话（推荐）
claude

# 方式2: 直接提问后退出
claude "分析这个项目的代码结构"

# 方式3: 打印输出后退出（适合脚本）
claude -p "列出所有 JavaScript 文件"
```

### 3.2 按场景选择启动参数

| 场景 | 命令 |
|------|------|
| 继续上次对话 | `claude -c` |
| 恢复历史会话 | `claude -r` |
| 指定模型 | `claude --model sonnet` |
| 规划模式 | `claude --permission-mode plan` |
| 自动接受编辑 | `claude --permission-mode acceptEdits` |
| 调试模式 | `claude --debug` |

### 3.3 启动流程示例

```powershell
# 完整启动流程示例
PS D:\> cd d:\Users\trae项目\cc
PS d:\Users\trae项目\cc> claude --model sonnet

# 等待出现 Claude 提示符
╭────────────────────────────────────────╮
│ ✻ Welcome to Claude Code!              │
│                                        │
│   Tips:                                │
│   • Type /help for commands            │
│   • Type /exit to exit                 │
╰────────────────────────────────────────╯

> 你的任务在这里输入...
```

---

## 四、常用任务场景

### 4.1 代码分析与理解

```
# 分析项目结构
> 分析这个项目的整体架构

# 理解特定文件
> 解释 src/utils/helper.js 的作用

# 查找依赖关系
> 找出哪些文件依赖于 config.js

# 代码审查
> 审查 src/api/user.js 的代码质量
```

### 4.2 代码生成与修改

```
# 生成新功能
> 帮我创建一个用户登录的 API 接口

# 重构代码
> 将回调函数重构为 async/await

# 添加注释
> 为 src/services/ 下的所有函数添加 JSDoc 注释

# 批量修改
> 将所有 var 声明改为 const 或 let
```

### 4.3 调试与排错

```
# 分析错误
> 这个错误是什么意思: TypeError: Cannot read property 'x' of undefined

# 查找问题
> 为什么这个函数返回 null？

# 日志分析
> 分析 error.log 中的错误模式
```

### 4.4 测试相关

```
# 生成测试
> 为 src/utils/calculator.js 生成单元测试

# 分析测试覆盖
> 哪些函数还没有测试覆盖？

# 修复测试
> 修复失败的测试用例
```

### 4.5 文档生成

```
# 生成 README
> 根据代码生成项目 README

# 生成 API 文档
> 为所有 API 路由生成文档

# 代码注释
> 为复杂的算法添加详细注释
```

---

## 五、会话管理

### 5.1 查看会话历史

```bash
# 列出所有会话
claude -r

# 输出示例:
# ? Select a conversation to resume:
#   abc123 - 2026-03-22 - 重构用户模块
#   def456 - 2026-03-21 - 添加支付功能
# > ghi789 - 2026-03-20 - 修复登录bug
```

### 5.2 恢复会话

```bash
# 交互式选择
claude -r

# 按 ID 恢复
claude -r abc123

# 继续当前目录最近的会话
claude -c
```

### 5.3 命名会话

```bash
# 启动时命名
claude -n "用户模块重构任务"

# 方便后续查找和恢复
```

### 5.4 会话中的命令

在 Claude Code 交互模式中：

```
# 常用斜杠命令
/help          # 显示帮助
/exit          # 退出会话
/clear         # 清空屏幕
/model         # 切换模型
/compact       # 压缩对话历史
```

---

## 六、实用技巧

### 6.1 管道操作

```bash
# 代码审查流水线
git diff | claude -p "审查这些改动"

# 日志分析
cat app.log | claude -p "总结错误和警告"

# 文件处理
find . -name "*.js" | claude -p "分析这些 JS 文件"
```

### 6.2 权限模式选择

| 模式 | 适用场景 | 命令 |
|------|----------|------|
| default | 默认，每次询问 | 无需指定 |
| acceptEdits | 自动接受文件编辑 | `--permission-mode acceptEdits` |
| plan | 先出方案再执行 | `--permission-mode plan` |
| dontAsk | 几乎不询问（谨慎） | `--permission-mode dontAsk` |

### 6.3 工具限制

```bash
# 只允许特定工具
claude --allowedTools "Bash(git:*) Edit Read"

# 禁用危险工具
claude --disallowedTools "Bash(rm:*) Bash(mv:*)"
```

### 6.4 输出格式

```bash
# JSON 输出（适合程序处理）
claude -p "列出所有函数" --output-format json

# 简洁模式
claude --brief
```

### 6.5 配置文件

创建 `claude-settings.json`：

```json
{
  "model": "sonnet",
  "permissionMode": "acceptEdits",
  "allowedTools": ["Edit", "Read", "Bash"],
  "systemPrompt": "你是一个专业的代码审查助手"
}
```

使用：

```bash
claude --settings claude-settings.json
```

---

## 七、常见问题排查

### 7.1 启动问题

| 问题 | 解决方案 |
|------|----------|
| `command not found` | 检查 PATH 环境变量 |
| 认证失败 | 运行 `claude auth` 重新认证 |
| 版本过旧 | 运行 `claude update` |

### 7.2 使用问题

| 问题 | 解决方案 |
|------|----------|
| 响应慢 | 使用 `--model sonnet` 替代 `opus` |
| 超出上下文 | 使用 `/compact` 压缩历史 |
| 权限被拒绝 | 检查 `--permission-mode` 设置 |

### 7.3 调试方法

```bash
# 启用调试输出
claude --debug

# 调试特定模块
claude --debug "api,tools"

# 输出到文件
claude --debug-file debug.log
```

---

## 八、完整工作流示例

### 场景：新功能开发

```powershell
# Step 1: 切换到项目目录
cd d:\Users\trae项目\cc

# Step 2: 检查当前状态
git status

# Step 3: 启动 Claude Code（规划模式）
claude --permission-mode plan -n "添加订单导出功能"

# Step 4: 在 Claude 中交互
> 帮我设计一个订单导出功能，支持 Excel 和 CSV 格式

# Step 5: 审查生成的代码
> 审查刚生成的代码，检查是否有安全问题

# Step 6: 生成测试
> 为订单导出功能生成单元测试

# Step 7: 运行测试（在另一个终端）
npm test

# Step 8: 生成文档
> 为订单导出 API 生成文档

# Step 9: 提交代码
git add .
git commit -m "feat: 添加订单导出功能"

# Step 10: 退出 Claude Code
/exit
```

---

## 参考

- [Claude Code 官方文档](https://docs.anthropic.com/)
- [完整命令参考](./claude-code-usage.md)
