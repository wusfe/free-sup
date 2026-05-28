# claude-hud:setup

Install and configure the Claude HUD statusline plugin.

## Triggers

- `/claude-hud:setup`
- User says "setup Claude HUD", "install HUD statusline", "configure HUD"

## Tools

Read, Write, Edit, Bash, Grep, Glob

## Workflow

### Step 0: Ghost Detection
Check for orphaned cache or stale registry entries from failed prior installs. On macOS/Linux, look for broken symlinks in the plugins directory. On Windows, check for orphaned plugin registry entries. Clean up if found.

### Step 1: Platform Detection
Determine OS and shell to generate the correct statusLine command:
- **macOS/Linux (bash/zsh)**: Use `bash -c` with awk/grep pipeline to locate the latest installed plugin version
- **Windows Git Bash**: Direct shell command with `sort -V`, no nested `bash -c` wrapper
- **Windows PowerShell**: Write a standalone `.ps1` wrapper file, reference with `-File` to avoid nested quoting issues

### Step 2: Test the Command
Run the generated command. Verify it produces valid statusline output. If it fails, debug and fix before proceeding.

### Step 3: Backup Existing Config
Read current `statusLine.command` from `settings.json`. Classify it (claude-hud, other plugin, custom, or empty). Create a timestamped backup of `settings.json` before any modification. Save previous statusline command to `previous-statusline.txt`.

### Step 4: Apply Configuration
Merge the `statusLine` config into `settings.json`, preserving all non-statusLine settings. On Windows PowerShell, write without BOM. Instruct user to restart Claude Code.

### Step 5: Optional Features
Prompt user to enable extra HUD features via multi-select:
- Tools activity (live read/edit/search)
- Agents & Todos tracking
- Session info (duration, cost, speed)
- Session name display
- Custom line

Write selected features to `config.json`.

### Step 6: Verification
After restart, confirm the HUD is visible. If not, debug: check stale symlinks, shell mismatches, PowerShell execution policy, console handle issues, WSL confusion. Offer to star the repo if user is satisfied.

## Quality Rules

1. Always backup `settings.json` before modifying
2. Preserve all existing non-statusLine settings during merge
3. Test the generated command before writing config
4. Detect and handle ghost/stale installations
5. Provide clear platform-specific commands — never use a Unix command on Windows or vice versa

## Anti-Patterns

- Writing statusLine config without testing the command first
- Overwriting settings.json without a timestamped backup
- Using nested `bash -c` on Windows Git Bash (causes quoting bugs)
- Forcing inline PowerShell script when a `.ps1` wrapper file is safer
- Skipping ghost detection — stale entries cause silent failures
