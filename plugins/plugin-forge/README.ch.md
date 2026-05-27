# plugin-forge

外部插件导入工具。搜索、评估外部市场的 Claude Code 插件，改造为 free-sup 格式并提交 PR。

## 安装

```bash
/plugin install plugin-forge@free-sup
```

## 使用

### 搜索插件

```
/plugin-forge:search 代码审查
```

在 `affaan-m/everything-claude-code`、官方市场和 GitHub 中搜索匹配的插件，对比展示候选差异。

### 直接导入

```
/plugin-forge:adapt https://github.com/xxx/awesome-plugin
```

跳过搜索，直接改造并导入。生成预览后等用户确认。

### 更新已有插件

```
/plugin-forge:adapt 给 find-plugin 加个 agent
```

追加新组件到已有 free-sup 插件。

## 工作流

```
search（搜市场）→ adapt（改造预览）→ check（安全检查）→ submit（提交 PR）
```

辅助：`repo`（查仓库现状）、`check`（安全规范检查）

## 规范

生成的插件严格遵循 Claude Code 官方插件规范，唯一附加规则：每个 `.md` 文件都有对应的 `.ch.md` 中文版。
