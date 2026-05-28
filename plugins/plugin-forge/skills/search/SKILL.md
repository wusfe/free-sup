---
name: search
description: Search external marketplaces for matching Claude Code plugins. Compare candidates and help users decide. No transformation, no importing.
origin: free-sup
---

# search — Plugin Search

Search external marketplaces and GitHub for Claude Code plugins matching user needs. Deep evaluation with comparative presentation.

## When to Activate

- "Is there a plugin for X"
- "Find me a plugin that does Y"
- "/plugin-forge:search <need>"
- "Search Claude Code plugins"

## Workflow

### Phase 1: Confirm Needs

Clarify: what functionality, any preferred source (default priority: `affaan-m/everything-claude-code`), Chinese user scenario.

### Phase 2: Search

Priority order:

1. General GitHub search (highest) — WebSearch: `claude-code-plugin` + keywords, `"claude code" plugin <keyword> github`
2. `affaan-m/everything-claude-code` — WebFetch README
3. `anthropics/claude-plugins-official` — WebFetch repo listing
4. `ComposioHQ/awesome-claude-plugins` — WebFetch README
5. `gupsammy/Claudest` — WebFetch README

**Collection repos** (README in Awesome List format): match relevant plugins from the list, label each with component type (skill/agent/hook/mcp/lsp).

**Single repos**: direct candidate. Entries in collection repos may be single components or full multi-component plugins.

**Empty results**: relax keywords (remove qualifiers, English search). If still nothing, tell user and suggest providing a repo URL to `/plugin-forge:adapt`.

### Phase 3: Deep Evaluation

WebFetch each candidate for: README, `.claude-plugin/plugin.json`, last commit, stars/issues.

### Phase 4: Comparative Display

Table + individual analysis with key differentiators and recommendation with reasoning.

### Phase 5: Confirmation

User picks a number, "first one", or "none". If "none", suggest different keywords or direct URL to adapt. Selected candidate's URL + info returned to main conversation.

## Quality Rules

1. WebFetch every candidate, don't rely on search snippets alone
2. Point out differences — why A over B
3. Don't fabricate candidates if nothing found
4. Label format status (standard plugin vs README-only)

## Anti-Patterns

- Jumping to conclusions after first result
- Not explaining collection vs standalone repos
- Recommending without reasoning
