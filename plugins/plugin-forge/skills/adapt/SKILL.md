---
name: adapt
description: Transform a source plugin repository into free-sup format. Generates file previews for user review. No commits, no push.
origin: free-sup
---

# adapt — Plugin Adaptation

Take a source repository URL, deeply understand its content, and transform it into free-sup format per Claude Code official spec + .ch.md custom rule.

## When to Activate

- "Import this repo https://..."
- "Make this into a free-sup plugin"
- "/plugin-forge:adapt https://github.com/xxx/plugin"
- "Transform this plugin for me"
- "Add a skill to free-sup's xx plugin" (update mode, ask for source URL or description)
- Auto-transition from search selection

## Workflow

### 0. Prepare Workspace

Write output to `~/.claude/free/output/<name>/`. **Directory structure must mirror `plugins/<name>/` exactly**:

```
~/.claude/free/output/<name>/
├── .claude-plugin/plugin.json + plugin.ch.json
├── skills/<skill-name>/SKILL.md + SKILL.ch.md
├── agents/<agent-name>.md + .ch.md
├── hooks/hooks.json
├── .mcp.json / .lsp.json
├── README.md + README.ch.md
└── rules/<name>.md + <name>.ch.md
```

- **First time**: create new directory
- **Re-edit**: Read existing output as context, modify in place

Call Agent(repo) to get repository state. Compare source name against marketplace.json: not found → new mode, found → update mode. Terminate if repo clone fails.

### 1. Extract Source Info

WebFetch source repo: README, `.claude-plugin/plugin.json`, directory structure. If inaccessible (404/private/auth): report error, ask user for README content.

Identify all components: skills/, commands/, agents/, hooks/, mcpServers, lspServers, rules/.

### 2. Understand Source

Read all content. Extract the essence of each component: what it does, when to use, dependencies.

### 3. Determine Metadata

- Name: kebab-case from source. Conflict with marketplace.json → prompt user
- Author: `wusfe`, source author in PR description
- License: source's license or default MIT
- Version: `1.0.0`

### 4. Transform & Generate

Each `.md` must have `.ch.md`. `.claude-plugin/plugin.json` must have `plugin.ch.json`. Rewrite skills/commands/agents into five-section format (triggers → tools → workflow → quality rules → anti-patterns).

**New mode**: create all files + marketplace.json entry.
**Update mode**: only add new components, don't overwrite. Structure output as `new/`, `diff/`, `unchanged/`.

Required output files per component:

| Source | Action | Output |
|--------|--------|--------|
| skill | Rewrite 5-section | SKILL.md + SKILL.ch.md |
| command | Same | `<name>.md` + `<name>.ch.md` |
| agent | Adapt YAML | `<name>.md` + `<name>.ch.md` |
| hook | Adapt path vars | hooks/hooks.json |
| mcpServer | Adapt path vars | .mcp.json or plugin.json |
| lspServer | Adapt path vars | .lsp.json or plugin.json |
| README | Extract + rewrite cleanly | README.md + README.ch.md |
| rules | Archive as-is | rules/`<name>`.md + `<name>`.ch.md |

**README rules**: Don't copy the source README verbatim. Extract key info and rewrite concisely in free-sup style:

- **README.md**: Plugin name, one-line summary, install command. **Every component type must have usage docs**:
  - skills: One section each with natural-language trigger phrases, slash commands, and concrete dialog examples
  - agents: When Claude auto-invokes them, plus manual trigger command
  - hooks: What event triggers them and what they do automatically
  - MCP/LSP: Note they start automatically on install
  - Component list with all types, names, and descriptions
  - Source attribution
- **README.ch.md**: Same structure in Chinese, with source info appended.
- Strip CI badges, contribution guides, changelogs, and other irrelevant content from the source.
- Feature descriptions must be based on actual transformed output, not inflated claims.

### 5. Output Preview

**5.1 Create zip first** (before showing content). Must execute — use `zip -r` on Linux/macOS, `powershell Compress-Archive` on Windows, fall back to `tar -czf`.

**5.2 Show preview** with file list, content summary, and the zip download path. **Wait for user approval.**

### 6. User Dissatisfied

User explains issue. Read existing output as context, modify in place. User can also go back to search. Loop until "ok" or "abandon". On abandon: delete `output/<name>/`.

## Quality Rules

1. No fabricated features — everything based on source content
2. Inferred content must be labeled
3. Every file checked for format correctness before writing
4. marketplace.json must be valid JSON
5. Must call repo agent to determine new vs update mode

## Anti-Patterns

- Blindly rewriting without understanding the source plugin
- Skipping repo agent for mode determination
- Overwriting existing files in update mode
- Asking for user confirmation without showing preview
