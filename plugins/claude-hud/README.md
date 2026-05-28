# Claude HUD

Real-time heads-up display for Claude Code. Shows context health, tool activity, agent tracking, and todo progress — always visible below your input line.

Uses Claude Code's native **statusline API** — no separate window, no tmux required.

## What It Shows

| Element | Detail |
|---------|--------|
| **Project path** | Configurable 1–3 directory levels |
| **Context health** | Visual bar with percentage, green→yellow→red |
| **Tool activity** | Live read/edit/search operations |
| **Agent tracking** | Sub-agent name, model, task, duration |
| **Todo progress** | Real-time completion counts |
| **Usage limits** | 5-hour and 7-day rate limit consumption with reset countdowns |
| **Git status** | Branch, dirty indicator, ahead/behind, file stats |
| **Session info** | Duration, cost, token speed, memory, cache countdown |

## Display

**Expanded (default 2-line):**
```
[Opus] │ my-project git:(main*)
Context █████░░░░░ 45% │ Usage ██░░░░░░░░ 25% (1h 30m / 5h)
```

**Optional activity lines:**
```
◐ Edit: auth.ts | ✓ Read ×3 | ✓ Grep ×2
◐ explore [haiku]: Finding auth code (2m 15s)
▸ Fix authentication bug (2/5)
```

## Installation

```
/plugin marketplace add jarrodwatts/claude-hud
/plugin install claude-hud
/reload-plugins
/claude-hud:setup
```

Restart Claude Code after setup.

## Configuration

Run `/claude-hud:configure` for guided setup. Three presets:

| Preset | Scope |
|--------|-------|
| **Full** | Everything enabled |
| **Essential** | Activity lines + git, minimal clutter |
| **Minimal** | Model name + context bar only |

Layout: `expanded` (multi-line) or `compact` (single-line). Language: English or 中文 (`zh-Hans`).

Advanced settings in `~/.claude/plugins/claude-hud/config.json`: colors, bar characters, thresholds, element order, time formats.

## Requirements

- Claude Code v1.0.80+
- Node.js 18+ (macOS/Linux) or Node.js 18+ (Windows)

## License

MIT. Originally created by [Jarrod Watts](https://github.com/jarrodwatts/claude-hud).
