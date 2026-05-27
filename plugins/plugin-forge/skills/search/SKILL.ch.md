---
name: search
description: 搜索外部市场中匹配的 Claude Code 插件，对比候选差异，帮用户做出选择。不改造、不导入。
origin: free-sup
---

# search — 插件搜索

搜索外部市场和 GitHub，找到符合用户需求的 Claude Code 插件候选，深度评估后对比展示。

## 触发条件

- "有没有 X 的插件"
- "找个能做 Y 的插件"
- "/plugin-forge:search <需求>"
- "搜索 Claude Code 插件"

## 工作流

### 阶段 1：确认需求

先问清楚：
- 用户想要什么功能
- 有无偏好来源（默认优先 `affaan-m/everything-claude-code`）
- 是否是中文用户场景

### 阶段 2：搜索

按优先级搜索：

1. `affaan-m/everything-claude-code`（最高优先级）— WebFetch 获取 README
2. `anthropics/claude-plugins-official` — WebFetch 获取仓库列表
3. `ComposioHQ/awesome-claude-plugins` — WebFetch 获取 README
4. `gupsammy/Claudest` — WebFetch 获取 README
5. GitHub 通用搜索 — WebSearch: `claude-code-plugin` + 关键词

**聚合仓库**（README 是 Awesome List 格式、目录下有多个插件目录）：从列表中匹配符合需求的插件，标注每个的类型（skill / agent / hook / mcp / lsp）。

**独立仓库**：直接作为候选。聚合仓库中的条目可能是单一组件，也可能是完整的多组件插件。

**结果为空**：放宽关键词再搜一轮（去掉限定词、换英文搜索）。仍无结果则直说，提示用户直接提供仓库 URL 给 `/plugin-forge:adapt` 改造。

### 阶段 3：深度评估

对每个候选 WebFetch 获取：
- README（功能描述、使用方式）
- `.claude-plugin/plugin.json`（是否为标准 Claude Code 插件）
- 最近 commit 时间（维护状态）
- 仓库 stars/issues（社区活跃度）

### 阶段 4：对比展示

不只列表，要点出区别：

```markdown
## 候选插件对比

| # | 插件 | 来源 | 维护 | 匹配度 |
|---|------|------|------|--------|

### 各自特点

**1. xxx** — [推荐]
- 核心能力：...
- 优势：...（比 #2 好在哪）
- 劣势：...
- 格式：标准 Claude Code 插件 / 纯 README 描述 / 仅 skill
- 适用场景：...

### 推荐
选 #1 xxx，因为 [理由]。如果 [特殊情况]，也可以考虑 #2。
```

### 阶段 5：确认

- 用户回复编号、"就第一个"、或"都不好"
- "都不好" → 建议换关键词或直接给仓库 URL 走 `/plugin-forge:adapt`
- 选定后 → 返回候选 URL + 基本信息给主对话

## 质量规则

1. 每个候选必须 WebFetch 确认，不能仅凭搜索结果摘要判断
2. 对比必须点出区别（为什么选 A 不选 B），不能只列名字
3. 搜不到直说，不要编造候选
4. 标注格式状态（标准插件 vs 纯 README），影响改造难度

## 反模式

- 只看第一个搜索结果就下结论
- 不告诉用户聚合仓库和独立仓库的区别
- 推荐时不说理由
