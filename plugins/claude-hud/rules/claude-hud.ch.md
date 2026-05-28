# Claude HUD — 开发规则

## 触发条件
每次 Claude Code 请求状态栏输出时激活（约 300ms 间隔）。

## 构建命令
- `npm ci` — 安装依赖
- `npm run build` — 编译 TypeScript
- `npm test` — 构建 + 运行测试
- `npm run test:coverage` — 构建 + 运行测试覆盖率
- `npm run test:stdin` — 使用示例 stdin JSON 测试

## 数据流
1. Claude Code 每 ~300ms 调用状态栏程序
2. stdin 接收 JSON：{ model, context_window, cost, rate_limits, effort, cwd, transcript_path }
3. 插件解析 transcript JSONL 获取工具/代理/待办条目
4. 插件将渲染行输出到 stdout
5. 配置存储在 `~/.claude/plugins/claude-hud/config.json`

## 实现指南
- 源文件在 `src/`：stdin 解析、transcript 解析、配置读取、git 状态、渲染
- 国际化：英文和简体中文
- 上下文阈值：<70% 绿色，70-85% 黄色，>85% 红色
- statusLine 配置写入 `~/.claude/settings.json`，不是 plugin.json

## 反模式
- 不要在 plugin.json 中放置 statusLine 配置
- 不要假设 tmux 环境
- 不要硬编码路径
- 避免不跨平台的 Shell 语法
