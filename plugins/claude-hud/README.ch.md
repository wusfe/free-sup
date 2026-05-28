# Claude HUD

Claude Code 实时状态栏 HUD — 上下文健康度、工具活动、代理跟踪和待办进度。

**版本：** 1.0.0 | **许可证：** MIT | **作者：** Jarrod Watts（原版）, wusfe（free-sup 转换）

## 概述

Claude HUD 是一个在 Claude Code 输入行下方显示持久多行状态栏的插件。它使用 Claude Code 原生 statusline API — 无需 tmux、无需独立窗口，在任何终端中都能工作。大约每 300ms 更新一次。

### 默认显示（2行）

第1行：`[模型徽章] /项目/路径 (main)`
第2行：`[上下文条 ████████░░ 72%] [使用量 ████░░ 38%]`

### 可选行（选择加入）

第3行：`工具：Read src/index.ts • Edit config.json • Search for "hud"`
第4行：`代理：agent-1 (分析中) • agent-2 (编写测试)`
第5行：`待办：[x] 设置完成 [ ] 添加测试 [ ] 部署`
第6行：`环境：2 CLAUDE.md • 5 规则 • 3 MCP • 1 钩子`

## 安装

```
/plugin marketplace add jarrodwatts/claude-hud
/plugin install claude-hud
/reload-plugins
/claude-hud:setup
```

重启 Claude Code 后生效。

## 配置

### 预设

| 预设 | 行数 | 适用场景 |
|--------|-------|----------|
| Minimal | 2 | 最大空间效率 |
| Essential | 3-4 | 日常使用，关注活动状态 |
| Full | 5-6 | 完整监控 |

## 要求

- Claude Code v1.0.80+
- Node.js 18+
- Bun（macOS/Linux 替代）
