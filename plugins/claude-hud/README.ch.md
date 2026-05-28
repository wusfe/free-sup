# Claude HUD

Claude Code 实时状态栏插件。在输入行下方始终显示上下文健康度、工具活动、子代理追踪和任务进度。

使用 Claude Code 原生 **statusline API** — 无需额外窗口，无需 tmux。

## 显示内容

| 元素 | 说明 |
|------|------|
| **项目路径** | 1–3 级目录深度可配 |
| **上下文健康度** | 可视化进度条 + 百分比，绿→黄→红 |
| **工具活动** | 实时读写搜索操作 |
| **Agent 追踪** | 子代理名称、模型、任务、耗时 |
| **Todo 进度** | 实时任务完成计数 |
| **用量限额** | 5 小时和 7 天用量消耗 + 重置倒计时 |
| **Git 状态** | 分支、脏标记、超前/落后、文件统计 |
| **会话信息** | 耗时、费用、Token 速度、内存、缓存倒计时 |

## 显示效果

**默认双行布局：**
```
[Opus] │ my-project git:(main*)
上下文 █████░░░░░ 45% │ 用量 ██░░░░░░░░ 25% (1h 30m / 5h)
```

**可选活动行：**
```
◐ 编辑: auth.ts | ✓ 读取 ×3 | ✓ 搜索 ×2
◐ explore [haiku]: 查找认证代码 (2m 15s)
▸ 修复认证 bug (2/5)
```

## 安装

```
/plugin marketplace add jarrodwatts/claude-hud
/plugin install claude-hud
/reload-plugins
/claude-hud:setup
```

安装后重启 Claude Code。

## 配置

运行 `/claude-hud:configure` 进行引导式配置。三种预设：

| 预设 | 范围 |
|------|------|
| **Full** | 全部启用 |
| **Essential** | 活动行 + Git，信息精简 |
| **Minimal** | 仅模型名称 + 上下文进度条 |

布局：`expanded`（多行）或 `compact`（单行）。语言：英文或中文（`zh-Hans`）。

高级设置位于 `~/.claude/plugins/claude-hud/config.json`：颜色、进度条字符、阈值、元素排序、时间格式。

## 环境要求

- Claude Code v1.0.80+
- Node.js 18+（macOS/Linux）或 Node.js 18+（Windows）

## 许可证

MIT。原作者 [Jarrod Watts](https://github.com/jarrodwatts/claude-hud)。
