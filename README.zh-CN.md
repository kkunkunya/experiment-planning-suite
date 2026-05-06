# Experiment Planning Suite

Experiment Planning Suite 是一个面向学术项目的 agent plugin，用来把早期研究想法、已有材料、benchmark 笔记、半成品结果和资源约束，收敛成可验证的实验计划。

它不是实验运行器。它负责在项目里生成 `knowledge/experiment-plan/` 这类计划平面，再把执行层交给 `req-suite` 等任务包工作流。

作者：Kunkun

## 能做什么

- 提炼研究主张和预期证据强度。
- 把 claim 映射到实验、指标、验证契约和证据缺口。
- 设计 baseline、ablation、stop condition 和可救路线。
- 规划论文图、表、截图、caption 和数据 schema。
- 生成给下游任务包工具使用的 handoff prompt。

## Plugin 结构

```text
plugins/experiment-planning-suite/
├── .claude-plugin/plugin.json
├── .codex-plugin/plugin.json
├── README.md
├── references/workflow-profile.md
└── skills/
    ├── orchestrator/
    ├── research-claim-snapshot/
    ├── evidence-chain-planner/
    ├── baseline-ablation-designer/
    ├── figure-table-planner/
    ├── feasibility-salvage-gate/
    └── req-experiment-handoff/
```

## 快速开始

1. 把本仓作为本地 plugin marketplace 或 plugin 源暴露给你的 agent runtime。
2. 启用 `experiment-planning-suite`。
3. 在包含研究笔记、benchmark、草稿或需求材料的项目目录里，让 agent 调用 `experiment-planning-suite:orchestrator`。

示例请求：

```text
调用 experiment-planning-suite:orchestrator，读取这个项目并在 knowledge/experiment-plan/ 下写论文级实验计划。
```

## 默认输出

```text
knowledge/experiment-plan/
├── research-claim-snapshot.md
├── paper-level-assessment.md
├── claim-evidence-map.md
├── experiment-matrix.md
├── baseline-ablation-plan.md
├── figure-table-plan.md
├── feasibility-salvage-plan.md
├── req-handoff.md
├── machine/
│   ├── verifier-contracts.yaml
│   ├── experiment-artifacts.schema.json
│   └── stop-conditions.yaml
└── data-schema/
    ├── metrics-table.schema.md
    ├── figure-data-fields.md
    └── screenshot-evidence-schema.md
```

## 安全边界

本仓不包含维护者 API key、私有项目文件、客户材料或生成日志。用户需要在本地配置自己的运行环境和凭据。

参见：

- [安装说明](docs/INSTALLATION.zh-CN.md)
- [Agent 安装提示词](docs/AGENT_INSTALL_PROMPT.zh-CN.md)
- [Crawler 读取指南](docs/AGENT_CRAWLER_GUIDE.zh-CN.md)
- [API key 与本地配置](docs/API_KEYS_AND_LOCAL_CONFIG.zh-CN.md)
- [安全策略](SECURITY.zh-CN.md)

## License

MIT
