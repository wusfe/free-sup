# claude-hud:setup

安装并配置 Claude HUD 状态栏插件。

## 触发条件

- `/claude-hud:setup`
- 用户说"安装 HUD"、"配置状态栏"、"setup Claude HUD"

## 可用工具

Read, Write, Edit, Bash, Grep, Glob

## 工作流程

### 第 0 步：残留检测
检查之前失败安装留下的孤立缓存或过期注册表项。macOS/Linux 上查找插件目录中的损坏符号链接，Windows 上检查孤立的插件注册表项。如存在则清理。

### 第 1 步：平台检测
根据 OS 和 Shell 生成正确的 statusLine 命令：
- **macOS/Linux (bash/zsh)**：使用 `bash -c` 配合 awk/grep 管道定位最新安装的插件版本
- **Windows Git Bash**：直接使用 shell 命令加 `sort -V`，不要嵌套 `bash -c`
- **Windows PowerShell**：写入独立的 `.ps1` 包装文件，通过 `-File` 引用以避开嵌套引号问题

### 第 2 步：测试命令
执行生成的命令，验证能产生有效的 statusline 输出。如失败则调试修复。

### 第 3 步：备份现有配置
读取 `settings.json` 中当前的 `statusLine.command`。分类判断（claude-hud、其他插件、自定义、空）。修改前创建带时间戳的 `settings.json` 备份。将旧的 statusline 命令保存到 `previous-statusline.txt`。

### 第 4 步：应用配置
将 `statusLine` 配置合并到 `settings.json`，保留所有非 statusLine 设置。Windows PowerShell 下以无 BOM 方式写入。提示用户重启 Claude Code。

### 第 5 步：可选功能
通过多选提示让用户启用额外功能：
- 工具活动（实时读写搜索操作）
- Agent 和 Todo 追踪
- 会话信息（时长、费用、速度）
- 会话名称
- 自定义行

将选中功能写入 `config.json`。

### 第 6 步：验证
重启后确认 HUD 是否可见。如不可见则排查：过期符号链接、Shell 不匹配、PowerShell 执行策略、控制台句柄问题、WSL 混淆。如用户满意则提示可给仓库点星。

## 质量规则

1. 修改前必须备份 `settings.json`
2. 合并时保留所有非 statusLine 的现有设置
3. 写入配置前必须先测试生成的命令
4. 检测并处理残留/过期安装
5. 提供正确的平台特定命令——禁止在 Windows 上使用 Unix 命令或反之

## 反模式

- 未测试命令就直接写入 statusLine 配置
- 不带时间戳备份就覆盖 settings.json
- 在 Windows Git Bash 上嵌套使用 `bash -c`（会导致引号 bug）
- 能用 `.ps1` 包装文件时强行使用内联 PowerShell 脚本
- 跳过残留检测——过期条目会导致静默失败
