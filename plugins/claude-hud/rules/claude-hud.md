# Claude HUD — Development Rules

## Triggers
This plugin activates whenever Claude Code requests statusline output (~300ms interval).

## Build Commands
- `npm ci` — Install dependencies
- `npm run build` — Compile TypeScript (tsc)
- `npm test` — Build + run tests
- `npm run test:coverage` — Build + run tests with coverage (c8)
- `npm run test:update-snapshots` — Update test snapshots
- `npm run test:stdin` — Test with sample stdin JSON

## Data Flow
1. Claude Code invokes the statusline binary every ~300ms
2. stdin receives JSON: { model, context_window, cost, rate_limits, effort, cwd, transcript_path }
3. Plugin reads transcript JSONL for tool/agent/todo entries
4. Plugin renders output lines to stdout
5. Config from `~/.claude/plugins/claude-hud/config.json`

## Implementation Guidelines
- Source files in `src/`: stdin parsing, transcript parsing, config reading, git status, rendering
- i18n support: English (`en.ts`) and Simplified Chinese (`zh-Hans.ts`)
- Context thresholds: <70% green, 70-85% yellow, >85% red
- Native stdin provides accurate token counts; transcript JSONL for tool/agent/todo extraction
- Output format: default expanded layout (2 lines), with opt-in lines for tools, agents, todos, environment

## Quality Standards
- TypeScript strict mode, targeting ES2022 with NodeNext module resolution
- Node.js 18+ compatibility
- Cross-platform: macOS, Linux, Windows (Git Bash, PowerShell)
- No external runtime dependencies beyond Node.js/Bun
- statusLine configuration goes into `~/.claude/settings.json`, NOT plugin.json

## Anti-Patterns
- Do NOT put statusLine config in plugin.json — it is not a valid field there
- Do NOT assume tmux — the statusline API works in any terminal
- Do NOT hardcode paths — use dynamic plugin version resolution
- Avoid platform-specific shell syntax without branching (e.g., \t in grep patterns)
- Avoid BOM in JSON output on PowerShell 5.1
- Do not use nested single quotes in Windows Git Bash commands
