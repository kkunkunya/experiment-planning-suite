# 安装说明

本仓把 `experiment-planning-suite` 作为本地 agent plugin 导出。

## Claude Code

使用你本地的 Claude Code plugin 工作流注册本仓，或把 plugin 目录复制到已配置的 plugin marketplace。启用：

```text
experiment-planning-suite
```

然后新开会话，让 runtime 重新发现 skill。

## Codex

把本仓注册为本地 marketplace，或把 `plugins/experiment-planning-suite` 复制到现有 Codex plugin marketplace。启用：

```toml
[plugins."experiment-planning-suite@local"]
enabled = true
```

具体 marketplace 名称取决于你的本地 runtime 配置。

## 冒烟测试

让 agent 执行：

```text
列出 experiment-planning-suite 的可见 skills，并说明什么时候使用 experiment-planning-suite:orchestrator。
```

如果 runtime 看不到 plugin，检查 marketplace 注册、plugin enablement，以及是否重开了会话。
