---
name: check
description: 检查 adapt 生成的产物，给出安全 + 规范报告。只报告、不修改。
origin: free-sup
---

# check — 安全检查

检查 `~/.claude/free/output/<name>/` 下的 adapt 产物，输出安全风险 + 规范合规报告。不修改任何文件。

## 触发方式

由主对话在 adapt 的预览输出后自动调用。用户不满意时可通过 `check 重新检查` 再次触发。

## 检查项

### 安全检查

| 检查项 | 说明 |
|--------|------|
| `exec` / `eval` | 动态代码执行，标注位置 |
| 混淆代码 | 大量无意义字符串、编码转换 |
| 可疑 URL | 非 GitHub/知名域名的外连 |
| 硬编码凭据 | API key、token、密码 |

风险标注 `⚠️`，不阻断流程（用户自行判断）。

### 规范检查

| 检查项 | 通过标准 |
|--------|---------|
| plugin.json 合法性 | 合法 JSON，`name` 为 kebab-case |
| SKILL frontmatter | 有 `name` 和 `description` |
| 目录结构 | `.claude-plugin/` 下只有 `plugin.json` |
| .ch 规则 | 每个 `.md` 都有 `.ch.md`，`.claude-plugin/` 下有 `plugin.ch.json` |

## 输出

```
## Check 报告

### 安全
✅ 通过
或
⚠️ N 项风险
  - `skills/xxx/SKILL.md:42` — <风险描述>

### 规范
✅ 通过
或
⚠️ N 项问题
  - `plugin.json` — name 字段不是 kebab-case
  - `skills/xxx/` — 缺少 SKILL.ch.md
```

## 后续动作

- **全部通过** → 用户确认后进入 submit
- **有问题** → 用户审查 → 可回 adapt 修改 → 重新 check
- **安全风险** → 标注但不阻断，用户决定是否继续

## 质量规则

1. 只读不写
2. 只报告不决策
3. 问题标注位置要精确到文件和行号
4. 不遗漏任何 .md 文件的 .ch.md 检查
