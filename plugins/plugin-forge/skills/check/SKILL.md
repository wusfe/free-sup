---
name: check
description: Inspect adapt output for security risks and spec compliance. Reports only, never modifies.
origin: free-sup
---

# check — Security & Spec Validation

Inspect files under `~/.claude/free/output/<name>/`. Report security risks and spec compliance issues. Read-only.

## Invocation

Auto-invoked by main conversation after adapt produces output.

## Checks

### Security

| Check | Detail |
|-------|--------|
| `exec`/`eval` | Dynamic code execution |
| Obfuscated code | Nonsensical strings, encoding tricks |
| Suspicious URLs | Non-standard domains |
| Hardcoded credentials | API keys, tokens, passwords |

Risks marked `⚠️`, don't block.

### Spec Compliance

| Check | Standard |
|-------|----------|
| plugin.json valid | Valid JSON, kebab-case `name` |
| SKILL frontmatter | Has `name` and `description` |
| Directory structure | Only `plugin.json` in `.claude-plugin/` |
| .ch rule | Every `.md` has `.ch.md`, `.claude-plugin/` has `plugin.ch.json` |

## Output

Report with security status (✅/⚠️), spec status (✅/⚠️), and precise locations for any issues.

## Quality Rules

1. Read-only, no modifications
2. Precise file + line number locations
3. Don't miss any .ch.md checks
