# /claude-hud:setup

Claude HUD 状态栏插件的初始化设置命令。

## 触发方式

- 用户在 Claude Code 会话中输入 `/claude-hud:setup`
- 首次安装 claude-hud 插件后自动提示

## 执行流程

### 第0步 — 残留检测
检查是否存在失败的安装残留，提供清理命令：
- macOS/Linux：删除残留的 `.claude/plugins/claude-hud` 和注册表条目
- Windows：删除残留目录和注册表条目
- Linux EXDEV 解决方法：检测 `/tmp` 跨设备链接问题，建议 `TMPDIR=~/.cache/tmp`

### 第1步 — 平台检测
检测操作系统、Shell 和 JavaScript 运行时（Node.js 或 Bun），按平台分支处理：
- macOS/Linux（GNU grep）
- Windows + Git Bash（BSD grep）
- Windows + PowerShell（独立 `.ps1` 包装器）
- WSL

### 第2步 — 命令测试
在写入配置前验证生成的状态栏命令是否可用。

### 第2.5步 — 现有配置检测
检测现有状态栏配置：
- claude-hud（重装）：创建带时间戳的备份
- 已知替代品（claude-pace, cc-statusline）：覆盖前提示
- 自定义脚本：将之前的命令保存到 `previous-statusline.txt`

### 第3步 — 写入配置
将 `statusLine` 字段写入 `~/.claude/settings.json`，自动解析最新安装的插件版本。

### 第4步 — 可选功能
询问是否启用：工具活动、代理/待办、会话信息、会话名称、自定义行。

### 第5步 — 验证
询问 HUD 是否可见。提供系统化调试指南。

## 依赖

- Node.js 18+ 或 Bun（macOS/Linux）
- Claude Code v1.0.80+
- Shell：bash、zsh、fish、PowerShell 或 Git Bash
- 需要 `~/.claude/settings.json` 可写
