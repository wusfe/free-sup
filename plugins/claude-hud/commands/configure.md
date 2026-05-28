# /claude-hud:configure

A slash command that provides interactive guided configuration for the Claude HUD statusline.

## Triggers

- Invoked by user via `/claude-hud:configure` inside a Claude Code session
- User wants to customize HUD appearance or behavior

## Flow A — New Users (6 questions)

1. **Layout** — Choose display layout: `expanded` (default, multiline) or `compact` (single line)
2. **Preset** — Choose from three presets:
   - `Full`: Everything enabled (tools, agents, todos, environment)
   - `Essential`: Activity lines + git status
   - `Minimal`: Just model name + context bar
3. **Language** — Choose interface language: `en` (English), `zh` or `zh-Hans` (Chinese)
4. **Elements** — Toggle individual display elements on/off (model, context bar, usage, tools, agents, todos, cost, duration, speed, memory, prompt cache, config counts)
5. **Custom Line** — Optional custom text line at the bottom
6. **Preview** — Shows a text preview of the resulting HUD layout before applying

## Flow B — Returning Users (6 questions)

1. **Toggle Elements** — Turn on/off specific elements
2. **Git Style** — Choose git status display style
3. **Layout/Reset** — Change layout or reset to defaults
4. **Language** — Change interface language
5. **Custom Line** — Set or modify the custom line
6. **Preview** — Show preview before applying

## Output

Writes configuration to `~/.claude/plugins/claude-hud/config.json`. Applies after Claude Code restart.

## Mappings

Configuration options map to:
- `lineLayout`: "expanded" | "compact"
- `display.showModel`, `display.showTools`, `display.showAgents`, etc.: boolean
- `language`: "en" | "zh" | "zh-Hans"
- `gitStatus.style`, `gitStatus.showAheadBehind`, etc.
- Custom color overrides: `context`, `usage`, `model`, `project`, `git`, `critical`, etc.
- `customLine.text`: string or null

## Notes

- Presets are independent from element toggles - presets set defaults, toggles override
- Manual editing of config.json supports all options not covered in the guided flow
