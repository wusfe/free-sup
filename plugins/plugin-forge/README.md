# plugin-forge

External plugin import tool. Search, evaluate, and import Claude Code plugins from external marketplaces into free-sup format, with PR submission.

## Install

```bash
/plugin install plugin-forge@free-sup
```

## Usage

### Search

```
/plugin-forge:search code review
```

Search `affaan-m/everything-claude-code`, official marketplace, and GitHub for matching plugins.

### Direct Import

```
/plugin-forge:adapt https://github.com/xxx/awesome-plugin
```

Skip search, transform and import directly. Preview before commit.

### Update Existing

```
/plugin-forge:adapt add an agent to find-plugin
```

Append new components to an existing free-sup plugin.

## Pipeline

```
search → adapt (preview) → check (validate) → submit (PR)
```

Helpers: `repo` (repo state), `check` (security & spec)

## Spec

Generated plugins follow official Claude Code plugin spec, with one additional rule: every `.md` file has a corresponding `.ch.md` Chinese version.
