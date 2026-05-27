---
name: submit
description: 从 output 目录读取文件，写入 free-sup 仓库并提交 PR。纯执行，无判断。
origin: free-sup
---

# submit — 提交 PR

用户确认后，读取 `~/.claude/free/output/<name>/` 中的产物，写入 free-sup 仓库，执行 git commit + push + PR。

## 触发方式

- 用户审核通过后由主对话自动调用
- 手动：`/plugin-forge:submit <name>`

## 环境准备

1. **检查 `gh`**：
   ```bash
   gh auth status
   ```
   不可用 → 提示安装并退出：
   - Windows: `winget install GitHub.cli`
   - macOS: `brew install gh`
   - 安装后运行 `gh auth login`

2. **进入仓库**：检测当前目录是否在 free-sup 仓库内。不在则用 `~/.claude/free/free-sup/`（不存在就调 Agent(repo) clone）

## 执行流程

### 1. 新增/更新文件

仓库内模式先 `git stash`。

```bash
git checkout -b add-plugin-<name>
# 或更新模式: git checkout -b update-plugin-<name>
```

分支名冲突 → 追加时间戳：`add-plugin-<name>-20260527`

### 2. 复制文件

**全新模式**：
```bash
cp -r ~/.claude/free/output/<name>/* plugins/<name>/
```

**更新模式**：
```bash
# 新增文件
cp -r ~/.claude/free/output/<name>/new/* plugins/<name>/
# 有修改的文件（覆盖）
cp -r ~/.claude/free/output/<name>/diff/* plugins/<name>/
# unchanged/ 跳过
```

### 3. 更新 marketplace.json

- **全新**：在 `.claude-plugin/marketplace.json` 的 `plugins` 数组中追加条目
- **更新**：如果 adapt 修改了条目则覆盖

### 4. 更新 README.md

- **全新**：在顶层 `README.md` 和 `README.ch.md` 的插件表格中追加一行
- **更新**：不需要改动

### 5. 提交

```bash
# 先用本地 git 配置作为提交者身份
NAME=$(git config user.name)
EMAIL=$(git config user.email)

git add plugins/<name>/ .claude-plugin/marketplace.json README.md README.ch.md
git -c user.name="$NAME" -c user.email="$EMAIL" commit -m "feat: import <plugin-name> from <source-url>

- 创建 plugin.json / SKILL.md / SKILL.ch.md
- 更新 marketplace.json / README.md

Source: <source-url>"
```

### 5. Push + PR

```bash
git push -u origin <branch>
gh pr create \
  --title "feat: add <name> plugin" \
  --body "## 变更说明
从 <source-url> 导入 <plugin-name>，已按 free-sup 格式改造。

## 变更内容
- [ ] plugin.json
- [ ] SKILL.md + SKILL.ch.md（改写后）
- [ ] marketplace.json + README.md 已更新

## 来源信息
- 源仓库: <url>
- 原作者: <author>
- 许可: <license>

## 待手动操作
- [ ] 查看生成文件内容
- [ ] rules/ 需手动移植到 .claude/rules/（如有）"
```

### 6. 收尾

```bash
git checkout main
git stash pop  # 仓库内模式
```

输出 PR 链接给用户。

## 失败处理

| 场景 | 处理 |
|------|------|
| gh 不可用 | 提示安装，报错退出 |
| 操作中途失败 | `git reset --hard && git checkout main && git branch -D <branch> && git stash pop` |
| push 冲突 | `git pull --rebase` 后重试，仍冲突则提示确认后 `--force-with-lease` |
| PR 已存在 | 提示已有 PR URL，询问是否更新（关闭旧 PR 重新提交） |

## 质量规则

1. 纯执行，不做判断
2. 失败必须回滚（reset + checkout main + delete branch + pop stash）
3. PR 描述包含完整的来源和待操作信息
4. 提交信息标注源 URL
