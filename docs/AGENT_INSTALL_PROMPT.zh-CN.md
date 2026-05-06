# Agent 安装提示词

可以把下面这段给能读取 GitHub 仓库并配置本地 plugin 的 agent：

```text
从这个仓库安装本地 agent plugin，名称为 experiment-planning-suite。不要复制私有数据或生成日志。为当前项目启用该 plugin，并验证 experiment-planning-suite:orchestrator 可见。安装后做一次冒烟检查：说明默认规划路线和 knowledge/experiment-plan 的预期输出。
```

如果 agent 不能修改 runtime 配置，让它输出适用于本地 Claude Code 或 Codex 环境的手动安装步骤。
