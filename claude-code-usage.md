# Claude Code 使用指南

> 版本: 2.1.81  
> 更新日期: 2026-03-22

## 简介

Claude Code 是 Anthropic 官方推出的命令行 AI 编程助手，支持代码生成、重构、调试、解释、批量修改等功能，可直接对接终端、IDE 与项目目录。

---

## 安装

```bash
npm install -g @anthropic-ai/claude-code
```

**前置要求**: Node.js >= 18

---

## 基本用法

```bash
claude                           # 启动交互式会话
claude "你的问题"                 # 直接提问
claude -p "解释这个函数"          # 打印输出后退出（适合管道）
claude -c                        # 继续最近的对话
claude -r                        # 恢复历史会话
```

---

## 命令行选项

### 通用选项

| 选项 | 说明 |
|------|------|
| `-v, --version` | 显示版本号 |
| `-h, --help` | 显示帮助信息 |
| `-p, --print` | 打印响应后退出（适合管道操作） |
| `-c, --continue` | 继续当前目录最近的对话 |
| `-r, --resume [value]` | 恢复会话（按 ID 或交互选择） |
| `-n, --name <name>` | 设置会话显示名称 |

### 模型与代理

| 选项 | 说明 |
|------|------|
| `--model <model>` | 指定模型（如 `sonnet`、`opus` 或完整名称） |
| `--agent <agent>` | 指定代理类型 |
| `--agents <json>` | 自定义代理配置（JSON 格式） |
| `--fallback-model <model>` | 默认模型过载时自动切换备用模型 |

### 权限与安全

| 选项 | 说明 |
|------|------|
| `--permission-mode <mode>` | 权限模式：`default`、`acceptEdits`、`bypassPermissions`、`dontAsk`、`plan`、`auto` |
| `--dangerously-skip-permissions` | 跳过所有权限检查（仅限沙盒环境） |
| `--allow-dangerously-skip-permissions` | 允许跳过权限检查选项（不默认启用） |
| `--allowedTools <tools...>` | 允许的工具列表 |
| `--disallowedTools <tools...>` | 禁用的工具列表 |

### 输入输出

| 选项 | 说明 |
|------|------|
| `--output-format <format>` | 输出格式：`text`、`json`、`stream-json` |
| `--input-format <format>` | 输入格式：`text`、`stream-json` |
| `--json-schema <schema>` | JSON Schema 结构化输出验证 |
| `--verbose` | 详细输出模式 |
| `--brief` | 简洁模式（启用 SendUserMessage 工具） |

### 目录与文件

| 选项 | 说明 |
|------|------|
| `--add-dir <directories...>` | 添加允许工具访问的目录 |
| `--file <specs...>` | 启动时下载文件资源 |
| `-w, --worktree [name]` | 创建新的 git worktree |
| `--tmux` | 创建 tmux 会话（需配合 --worktree） |

### 配置与设置

| 选项 | 说明 |
|------|------|
| `--settings <file-or-json>` | 加载设置文件或 JSON 配置 |
| `--setting-sources <sources>` | 设置来源：`user`、`project`、`local` |
| `--mcp-config <configs...>` | 加载 MCP 服务器配置 |
| `--strict-mcp-config` | 仅使用 --mcp-config 中的 MCP 配置 |
| `--plugin-dir <path>` | 加载插件目录 |

### 提示词

| 选项 | 说明 |
|------|------|
| `--system-prompt <prompt>` | 设置系统提示词 |
| `--system-prompt-file <file>` | 从文件加载系统提示词 |
| `--append-system-prompt <prompt>` | 追加系统提示词 |
| `--append-system-prompt-file <file>` | 从文件追加系统提示词 |

### 其他选项

| 选项 | 说明 |
|------|------|
| `--ide` | 自动连接 IDE |
| `--chrome` | 启用 Chrome 集成 |
| `--no-chrome` | 禁用 Chrome 集成 |
| `--bare` | 最小模式（跳过钩子、LSP、插件等） |
| `--debug [filter]` | 启用调试模式 |
| `--debug-file <path>` | 调试日志输出到文件 |
| `--effort <level>` | 努力级别：`low`、`medium`、`high`、`max` |
| `--max-budget-usd <amount>` | API 调用最大花费（美元） |
| `--session-id <uuid>` | 指定会话 ID |
| `--fork-session` | 恢复时创建新会话 ID |
| `--from-pr [value]` | 从 PR 恢复会话 |

---

## 子命令

### auth - 认证管理

```bash
claude auth          # 管理认证状态
```

### mcp - MCP 服务器管理

```bash
claude mcp           # 配置和管理 MCP 服务器
```

### agents - 代理列表

```bash
claude agents        # 列出已配置的代理
```

### plugin / plugins - 插件管理

```bash
claude plugin        # 管理 Claude Code 插件
claude plugins       # 同上
```

### update / upgrade - 更新

```bash
claude update        # 检查并安装更新
claude upgrade       # 同上
```

### install - 安装原生版本

```bash
claude install [target]    # 安装原生版本
                           # target: stable, latest, 或特定版本
```

### doctor - 健康检查

```bash
claude doctor        # 检查自动更新器健康状态
```

### setup-token - 设置令牌

```bash
claude setup-token   # 设置长期认证令牌（需 Claude 订阅）
```

### auto-mode - 自动模式检查

```bash
claude auto-mode     # 检查自动模式分类器配置
```

---

## 使用示例

### 交互式会话

```bash
# 启动交互式会话
claude

# 指定模型
claude --model opus

# 继续上次对话
claude -c
```

### 非交互模式（管道操作）

```bash
# 打印输出后退出
claude -p "解释这段代码的作用"

# JSON 输出
claude -p "列出项目结构" --output-format json

# 配合管道使用
cat app.js | claude -p "审查这段代码"
```

### 恢复会话

```bash
# 交互式选择历史会话
claude -r

# 按 ID 恢复
claude -r <session-id>

# 从 PR 恢复
claude --from-pr 123
```

### 权限控制

```bash
# 自动接受编辑
claude --permission-mode acceptEdits

# 规划模式
claude --permission-mode plan

# 仅允许特定工具
claude --allowedTools "Bash(git:*) Edit Read"
```

### 调试模式

```bash
# 启用调试
claude --debug

# 调试特定类别
claude --debug "api,hooks"

# 调试输出到文件
claude --debug-file debug.log
```

---

## 常见问题

### 命令未找到

如果出现 `claude: command not found`，需要将 npm 全局目录添加到 PATH：

```powershell
# Windows PowerShell（临时）
$env:PATH += ";D:\nodejs\node_global"

# Windows PowerShell（永久）
[Environment]::SetEnvironmentVariable("PATH", [Environment]::GetEnvironmentVariable("PATH", "User") + ";D:\nodejs\node_global", "User")
```

### 认证问题

```bash
# 重新认证
claude auth

# 设置 API Token
claude setup-token
```

### 更新 Claude Code

```bash
claude update
# 或
npm update -g @anthropic-ai/claude-code
```

---

## 参考链接

- 官方文档: https://docs.anthropic.com/
- npm 包: https://www.npmjs.com/package/@anthropic-ai/claude-code
