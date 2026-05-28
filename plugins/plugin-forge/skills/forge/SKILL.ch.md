---
name: forge
description: 一键创建 Claude Code 插件并提交到 free-sup 市场。全自动流水线：搜索候选 → 改造生成 → 安全检查 → 提交 PR。
origin: free-sup
---

# forge — 插件创建流水线

当用户说 "forge 帮我xxx" 时触发，自动编排：搜索候选 → 改造生成 → 安全检查 → 提交 PR。

## 触发条件

- "forge 帮我找一个 XX 插件"
- "forge 帮我创建一个 XX 插件"
- "forge 把 XX 做成插件"
- "forge 导入 XX"
- "/plugin-forge:forge <需求>"

## 工作流

### 步骤 1：确认需求

问用户：想要什么功能？是否有具体的 GitHub 仓库 URL？

- **有 URL** → 跳过搜索，直接走步骤 3
- **没 URL** → 走步骤 2 搜索

### 步骤 2：搜索候选

调用 Agent(search) 搜索外部市场。展示候选列表，让用户选择。

> **重要**：用户选定后，**必须走 adapt 改造流程**，不能直接 `/plugin install`。本插件的目的是将外部插件导入 free-sup 市场，不是安装。

### 步骤 3：改造生成

1. 调用 Agent(repo) 获取 free-sup 仓库现状
2. 调用 Agent(adapt) 改造源插件 → 输出到 `~/.claude/free/output/<name>/`
3. 等待返回预览摘要

### 步骤 4：安全检查

调用 Agent(check) 检查产物，返回安全 + 规范报告。

### 步骤 5：用户审核

展示预览 + 检查报告。

- **"可以"** → 继续
- **"不行"** → 回到步骤 3 修改 → 重新检查

### 步骤 6：提交 PR

调用 Agent(submit) 提交 PR，返回 PR 链接给用户。
