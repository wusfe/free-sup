---
name: adapt
description: 理解源插件仓库后改造成 free-sup 格式，生成文件预览。不提交、不 push。
origin: free-sup
---

# adapt — 插件改造

拿到源仓库 URL，深入理解其内容后按 free-sup 规范改造，生成完整文件预览供用户审核。不涉及任何 git 操作。

## 触发条件

- "导入这个仓库 https://..."
- "把这个做成 free-sup 插件"
- "/plugin-forge:adapt https://github.com/xxx/plugin"
- "帮我改造这个插件"
- "给 free-sup 的 xx 插件加个 skill"（更新模式，请用户提供源 URL 或功能描述）
- 从 search 选定候选后自动衔接

## 改造流程

### 0. 准备工作区

产物写入 `~/.claude/free/output/<name>/`。**目录结构必须和 `plugins/<name>/` 完全一致**：

```
~/.claude/free/output/<name>/
├── .claude-plugin/
│   ├── plugin.json
│   └── plugin.ch.json
├── skills/<skill-name>/SKILL.md + SKILL.ch.md
├── agents/<agent-name>.md + .ch.md       # 如有
├── hooks/hooks.json                       # 如有
├── .mcp.json / .lsp.json                  # 如有
├── README.md + README.ch.md
└── rules/<name>.md + <name>.ch.md         # 如有
```

- **首次**：创建新目录
- **重改时**：Read 已有 output 内容作为上下文，在此基础上修改，不从头覆盖
- 调用 Agent(repo) 获取仓库现状（marketplace.json + plugins/ 目录树）
- 对比源插件名与 marketplace.json：不存在 → 全新模式，存在 → 更新模式
- repo clone 失败时终止并报告用户

### 1. 提取源信息

WebFetch 源仓库：
- README（功能描述、使用方式）
- `.claude-plugin/plugin.json`（标准插件标识）
- 目录结构（识别组件）

**源仓库不可访问**（404/私有/鉴权）→ 报告错误，提示用户：手动提供 README 内容，或验证 URL。不做无根据的改造。

**识别所有组件**：
- `skills/` → SKILL.md
- `commands/` → 旧格式命令
- `agents/` → 子代理
- `hooks/hooks.json` → 事件钩子
- `.mcp.json` 或 `mcpServers` → MCP 服务器
- `.lsp.json` 或 `lspServers` → LSP 服务器
- `rules/` → 规则文件（收藏到 rules/ 收纳）

### 2. 理解源插件

通读所有源内容：
- 核心功能是什么
- 什么时候触发、怎么用
- 依赖什么工具
- 提取每个组件的本质

### 3. 确定基础信息

- **插件名**：源 `plugin.json` 的 name 或源仓库名转 kebab-case。和 marketplace.json 中已有插件冲突 → 提示用户重命名
- **author**：`wusfe`，源作者在 PR 描述中标注
- **license**：优先源 license，无则默认 MIT
- **version**：默认 `1.0.0`

### 4. 改造生成

**改造原则**：

| 源组件 | 改造方式 | 生成文件 |
|--------|---------|---------|
| skill | 理解 → 重写为五段式 | `SKILL.md` + `SKILL.ch.md` |
| command | 同上 | `<name>.md` + `<name>.ch.md` |
| agent | 适配官方 YAML 格式 | `<name>.md` + `<name>.ch.md` |
| hook | 适配路径变量 `${CLAUDE_PLUGIN_ROOT}` | `hooks/hooks.json` |
| mcpServer | 适配路径变量 | `.mcp.json` 或写入 plugin.json |
| lspServer | 适配路径变量 | `.lsp.json` 或写入 plugin.json |
| README | 重写为中文使用场景 | `README.md` + `README.ch.md` |
| rules | 原样收藏 | `rules/<name>.md` + `<name>.ch.md` |

**内容重写规范**：

每个 skill/command/agent 重写为五段式：

```
# <title>

<一句话简介>

## 触发条件
- "..."（自然语言触发短语）

## 核心工具
（需要用到的工具及其用法）

## 工作流
### 阶段 1：...
### 阶段 2：...

## 质量规则
1. ...
2. ...

## 反模式
- ...
```

**通用规则**：
- 每个 `.md` 必须有对应的 `.ch.md`，中文自然不生硬
- `.claude-plugin/plugin.json` 必须有对应的 `plugin.ch.json`（中文注释版，字段含义用 `_comment` 标注）
- YAML frontmatter 必须有 `name` 和 `description`
- 源信息不足时推断并标注"（根据源仓库推断）"

**全新模式**：
- 创建所有文件
- 生成 marketplace.json 追加条目（模板见下）

**更新模式**：
- 仅追加源有但本地缺少的新组件
- 不覆盖已有文件
- output 中按 `new/`（新增）、`diff/`（修改对比）、`unchanged/`（跳过）分类存放
- marketplace.json 已有条目不动（除非 description 等需要更新）

### 5. 输出预览

列出所有文件路径 + 内容摘要：

```markdown
## 改造预览

### 5.1 先打包 zip（在展示内容之前）

**必须执行**，按 OS 选择命令：

```bash
# Linux/macOS
zip -r ~/.claude/free/output/<name>.zip ~/.claude/free/output/<name>/

# Windows (Git Bash 中)
powershell -Command "Compress-Archive -Path '$HOME/.claude/free/output/<name>' -DestinationPath '$HOME/.claude/free/output/<name>.zip' -Force"
```

不可用则回退：
```bash
# Windows 用 tar
tar -czf ~/.claude/free/output/<name>.tar.gz -C ~/.claude/free/output/ <name>
```

### 5.2 展示预览

```markdown
## 改造预览

**模式**：全新导入 / 更新追加
**产物路径**：~/.claude/free/output/<name>/
**下载**：~/.claude/free/output/<name>.zip

### 文件清单
- `plugin.json` — <描述>
- `skills/xxx/SKILL.md` — <核心功能摘要>
- ...

### 检查清单
- [ ] 每个 .md 都有 .ch.md
- [ ] YAML frontmatter 完整
- [ ] 插件名 kebab-case
```

**必须等用户说"可以"才能进入下一步。**

### 6. 用户不满意时

- 用户说明原因（功能偏差 / 格式不对 / 细节调整）
- Read 已有 output 作为上下文，在此基础上继续改
- 用户也可选择回到 search 换一个候选
- 直到用户说"可以"或"放弃"
- 放弃时 → 删除 `~/.claude/free/output/<name>/`

## 质量规则

1. 不凭空编造功能，一切基于源仓库内容
2. 推断内容必须标注
3. 中文自然流畅，不能机翻
4. 每个文件生成前确认格式正确
5. marketplace.json 必须合法 JSON

## 反模式

- 在不理解源插件的情况下盲目改写
- 跳过 repo Agent 判断模式（全新 vs 更新）
- 更新模式下覆盖已有文件
- 不展示预览就直接让用户确认
