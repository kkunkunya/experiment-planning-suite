# Agent Crawler 读取指南

本指南给只能读取 GitHub URL 的 crawler-style agent 使用。

## 读取顺序

1. `README.zh-CN.md`
2. `plugins/experiment-planning-suite/README.md`
3. `plugins/experiment-planning-suite/skills/orchestrator/SKILL.md`
4. `plugins/experiment-planning-suite/references/workflow-profile.md`
5. orchestrator 选中的子 `SKILL.md`
6. `docs/API_KEYS_AND_LOCAL_CONFIG.zh-CN.md`

## 可以假设什么

- 这是实验规划 plugin，不是实验运行器。
- 主入口是 `experiment-planning-suite:orchestrator`。
- 输出应写入目标项目，通常是 `knowledge/experiment-plan/`。
- 后续执行任务应交给 `req-suite` 这类 task-ledger 系统。

## 不要假设什么

- 不要假设本仓包含私有项目数据。
- 不要假设维护者 API key 可用。
- 不要编造实验结果、benchmark 数字或可行性结论。
- 不要把 `references/` 压平进 `SKILL.md`；应保留资源结构。
