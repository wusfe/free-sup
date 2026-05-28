---
name: forge
description: One-click Claude Code plugin creation and PR submission to free-sup marketplace. Automated pipeline: search candidates → adapt → security check → submit PR.
origin: free-sup
---

# forge — Plugin Creation Pipeline

Triggered by "forge ..." prefixed requests. Automatically orchestrates: search → adapt → check → submit.

## Triggers

- "forge find me a X plugin"
- "forge create an X plugin"
- "forge import X"
- "/plugin-forge:forge <need>"

## Workflow

### Step 1: Confirm Need

Ask: what functionality? Have a specific GitHub repo URL?

- **Has URL** → skip search, go to step 3
- **No URL** → proceed to step 2

### Step 2: Search

Call Agent(search) to search marketplaces. Present candidates, let user pick.

### Step 3: Transform

1. Call Agent(repo) for repository state
2. Call Agent(adapt) to transform → output to `~/.claude/free/output/<name>/`
3. Return preview

### Step 4: Security Check

Call Agent(check) to inspect output. Return security + spec report.

### Step 5: User Review

Show preview + check report.

- "OK" → continue
- "Not OK" → back to step 3 → re-check

### Step 6: Submit PR

Call Agent(submit) to commit + push + create PR. Return PR link.
