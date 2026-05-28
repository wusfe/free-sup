# Claude HUD

Real-time statusline HUD for Claude Code — context health, tool activity, agent tracking, and todo progress.

**Version:** 1.0.0 | **License:** MIT | **Author:** Jarrod Watts (original), wusfe (free-sup transform)

## Overview

Claude HUD is a plugin that displays a persistent multi-line statusline below the input line in Claude Code. It uses Claude Code's native statusline API — no tmux, no separate window, works in any terminal. Updates approximately every 300ms.

### Default Display (2 lines)

Line 1: `[Model Badge] /project/path (main)`
Line 2: `[Context Bar ████████░░ 72%] [Usage ████░░ 38%]`

### Optional Lines (opt-in)

Line 3: `Tools: Read src/index.ts • Edit config.json • Search for "hud"`
Line 4: `Agents: agent-1 (analyzing) • agent-2 (writing tests)`
Line 5: `Todos: [x] Setup complete [ ] Add tests [ ] Deploy`
Line 6: `Env: 2 CLAUDE.md • 5 rules • 3 MCPs • 1 hook`

## Installation

```
/plugin marketplace add jarrodwatts/claude-hud
/plugin install claude-hud
/reload-plugins
/claude-hud:setup
```

Restart Claude Code after setup.

## Configuration

### Presets

| Preset | Lines | Best for |
|--------|-------|----------|
| Minimal | 2 | Maximum space efficiency |
| Essential | 3-4 | Daily use with activity awareness |
| Full | 5-6 | Complete monitoring |

### Key Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| lineLayout | "expanded" / "compact" | "expanded" | Single or multi-line layout |
| display.showTools | boolean | false | Show real-time tool activity |
| display.showAgents | boolean | false | Show running sub-agents |
| display.showTodos | boolean | false | Show todo progress |
| display.showModel | boolean | true | Show model name/badge |
| language | "en" / "zh" | "en" | Interface language |
| usageValue | "percentage" / "remaining" | "percentage" | Usage display style |

## Requirements

- Claude Code v1.0.80+
- Node.js 18+ (all platforms)
- Bun (macOS/Linux alternative)
