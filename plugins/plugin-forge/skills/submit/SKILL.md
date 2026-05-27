---
name: submit
description: Read files from output directory, write to free-sup repo, and submit PR. Pure execution, no judgment.
origin: free-sup
---

# submit — PR Submission

After user approval, read artifacts from `~/.claude/free/output/<name>/`, write to free-sup repository, and execute git commit + push + PR creation.

## Invocation

- Auto-invoked by main conversation after user approval
- Manual: `/plugin-forge:submit <name>`

## Environment Setup

1. **Check `gh`**: `gh auth status`. If unavailable: prompt install and exit.
2. **Enter repo**: Check if in free-sup repo. If not, use `~/.claude/free/free-sup/` (call Agent(repo) if missing).

## Execution

1. **Stash** (in-repo mode): `git stash`
2. **Branch**: `git checkout -b add-plugin-<name>` or `update-plugin-<name>`. Append timestamp on conflict.
3. **Copy files**: New mode → copy all. Update mode → copy `new/`, overwrite `diff/`, skip `unchanged/`.
4. **Update marketplace.json**: Append entry (new) or overwrite entry (update).
5. **Update README.md**: Append plugin row to top-level `README.md` and `README.ch.md` (new mode only, skip for update).
6. **Commit**: Read local git config (`user.name`, `user.email`), then `git -c user.name="$NAME" -c user.email="$EMAIL" commit` with structured message. This ensures commits are attributed to you, not Claude.
7. **Push + PR**: `git push -u origin <branch>` + `gh pr create`.
8. **Cleanup**: `git checkout main` + `git stash pop`.

## Error Handling

| Scenario | Action |
|----------|--------|
| gh unavailable | Install prompt, exit |
| Mid-operation failure | `git reset --hard && git checkout main && git branch -D <branch> && git stash pop` |
| Push conflict | `git pull --rebase` then retry; if still conflicted, prompt for `--force-with-lease` |
| PR exists | Show existing PR URL, ask if update |

## Quality Rules

1. Pure execution, no judgment
2. Always rollback on failure
3. PR description must include complete source info
4. Commit message must reference source URL
