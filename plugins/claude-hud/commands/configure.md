# claude-hud:configure

Guided configuration flow for Claude HUD display options.

## Triggers

- `/claude-hud:configure`
- User says "configure HUD", "change HUD settings", "customize statusline"

## Tools

Read, Write, Bash

## Workflow

### Determine Flow

Check if `~/.claude/plugins/claude-hud/config.json` exists:
- **Flow A (New User)**: Layout → Preset → Language → Turn Off → Turn On → Custom Line
- **Flow B (Update Config)**: Turn Off → Turn On → Git Style → Layout/Reset → Language → Custom Line

### Presets

Three presets available:
| Preset | What's Enabled |
|--------|---------------|
| **Full** | Everything: tools, agents, todos, git, usage, duration |
| **Essential** | Activity lines + git status, minimal info clutter |
| **Minimal** | Core only: model name + context bar |

Model name and context bar are always-on and not configurable.

### Layout Options

- **Expanded**: Multi-line, each element group on its own line
- **Compact**: Single line
- **Compact + Separators**: Single line with separator characters between elements

### Language

- English (`"en"`)
- 中文 (`"zh-Hans"`)

### Git Style

- Branch only
- Branch + dirty indicator (`*`)
- Full details (branch, dirty, ahead/behind `↑N ↓N`)
- File stats (adds `!M +A ✘D ?U` counts)

### Custom Line

Optional free-text line, max 80 characters. Appended after all other elements.

### Advanced Field Preservation

These fields are never edited by the guided flow and are always preserved: `colors.*`, `pathLevels`, time format settings, usage/environment/context thresholds, bar characters, merge groups, element order.

### Save

Merge changes into `~/.claude/plugins/claude-hud/config.json`. Auto-migrate legacy configs with `"layout": "default"` or `"layout": "separators"` to the new format.

## Quality Rules

1. Show a preview summary (layout, language, git changes + visual HUD mockup) before confirming write
2. Never write if user cancels (Esc) or no changes were made
3. Always merge with existing config — never overwrite blind
4. Preserve all advanced/manual fields that the guided flow doesn't touch

## Anti-Patterns

- Overwriting existing config.json without merging
- Editing advanced fields (colors, thresholds, time formats) in the guided flow
- Proceeding to write without showing a preview for user confirmation
- Applying defaults that override explicit user choices from a previous run
