# /claude-hud:setup

A slash command that performs initial setup of the Claude HUD statusline plugin.

## Triggers

- Invoked by user via `/claude-hud:setup` inside a Claude Code session
- First-time installation of the claude-hud plugin

## Execution

The command runs a multi-step process:

### Step 0 — Ghost Detection
Checks for orphaned cache/registry entries from failed installations. Provides cleanup commands for:
- macOS/Linux: remove stale `.claude/plugins/claude-hud` and registry entries
- Windows: remove stale directories and registry entries
- Linux EXDEV workaround: detects cross-device link issues with `/tmp` and recommends `TMPDIR=~/.cache/tmp`

### Step 1 — Platform Detection
Detects platform, shell, and JavaScript runtime (Node.js or Bun). Branches for:
- macOS/Linux with GNU grep
- Windows + Git Bash (BSD grep, `sort -V` instead of `\t` patterns)
- Windows + PowerShell (writes standalone `.ps1` wrapper, avoids `[Console]::WindowWidth` errors, BOM-free UTF-8)
- WSL

### Step 2 — Command Testing
Validates the generated statusline command for errors or hangs before writing to config.

### Step 2.5 — Existing Config Detection
Detects existing statusline configurations:
- claude-hud (reinstall): creates timestamped backup
- Known alternatives (claude-pace, cc-statusline): prompts before overwrite
- Custom scripts: saves previous command to `previous-statusline.txt`

### Step 3 — Config Writing
Writes the `statusLine` field to `~/.claude/settings.json` with proper JSON serialization (avoids BOM on PowerShell 5.1). Dynamically resolves the latest installed plugin version.

### Step 4 — Optional Features
Prompts to enable: tools activity, agents/todos, session info, session name, custom line. Writes to `~/.claude/plugins/claude-hud/config.json`.

### Step 5 — Verification
Asks if the HUD is visible. Offers to star the GitHub repo. Provides systematic debugging for:
- Restart required
- Wrong runtime path
- Shell mismatch
- Permission errors
- WSL confusion
- PowerShell execution policy

## Dependencies

- Node.js 18+ or Bun (macOS/Linux)
- Claude Code v1.0.80+
- Shell: bash, zsh, fish, PowerShell, or Git Bash
- `~/.claude/settings.json` must be writable

## Notes

- The `statusLine` field is a Claude Code native settings field, NOT a plugin.json field
- On Linux, tmpfs mount at `/tmp` may cause cross-device link errors
