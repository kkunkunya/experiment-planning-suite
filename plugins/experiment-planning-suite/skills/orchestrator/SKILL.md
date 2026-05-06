---
name: orchestrator
description: Experiment-planning-suite 主入口。用于 P4 实验规划、实验矩阵、图表证据链、做一半能不能发、怎么交给 req 拆实验任务时路由。不用于真正跑实验（交给 req-suite 执行）。
---

# Experiment Planning Orchestrator

## Role

本 skill 是 `experiment-planning-suite` 的 P4 workflow conductor。它要先读项目已有材料，判断入口模式，再按参考工作流程组织子 skill。它不直接跑实验、不创建 `TASK-EXP-*`，也不把机器判定契约塞进人读报告正文。

## Method-call

```text
/experiment-planning-orchestrator(
  project_root?,
  materials?,
  target_level?,
  route_mode?: idea-only|topic-ready|benchmark-driven|halfway-salvage|auto,
  output_root = knowledge/experiment-plan/
) -> experiment_planning_package
```

## Reference Workflow

先参考 `../../references/workflow-profile.md`，但 workflow 本身以本节为准。

```text
0. Scope project and output root
1. Scan existing materials
2. Route mode
3. research-claim-snapshot
4. evidence-chain-planner
5. baseline-ablation-designer
6. figure-table-planner
7. feasibility-salvage-gate
8. req-experiment-handoff
```

### Step 0: Scope Project and Output Root

默认在当前项目根写：

```text
knowledge/experiment-plan/
```

如果当前目录不是用户项目，先用用户给的路径或最近的项目根。规划资产不能混进 `knowledge/tasks/`；执行任务交给后续 `req-suite`。

### Step 1: Scan Existing Materials

先读项目已有材料，再问缺口。优先级：

1. `knowledge/experiment-plan/` 已有草案或历史规划。
2. `knowledge/project-specs/`、`knowledge/TASKS.md`、`knowledge/tasks/` 中的研究目标和边界。
3. `knowledge/paper-positioning/`、P2/P3 标杆拆解、topic、storyline、fit 结果。
4. `knowledge/literature/`、`references.md`、文献综述和目标期刊信息。
5. `results/`、`experiments/`、`logs/`、`screenshots/`、代码 README、现有图表。
6. 用户当轮提供的方向、方法、资源、期刊等级、已有结果。

不要把缺失的 P2/P3 当作立即阻塞；先标 `unknown` 或 `low-confidence`，再给最低补救方案。

### Step 2: Route Mode

| Signal | Mode | Next posture |
|---|---|---|
| 只有方向和大概方法 | `idea-only` | 先生成 research claim snapshot，约束最小可验证贡献。 |
| 有研究问题/创新点 | `topic-ready` | 直接构建 claim-evidence map 和 experiment matrix。 |
| 有标杆实验模板 | `benchmark-driven` | 迁移标杆实验模块，分出必做/可选/投稿增强。 |
| 已有结果或做一半 | `halfway-salvage` | 先判断现有证据能支撑什么级别，再做补救规划。 |

### Step 3: Snapshot

调用 `experiment-planning-suite:research-claim-snapshot`。输出：

- `research-claim-snapshot.md`
- `paper-level-assessment.md`

必须包含当前计划预计支撑的论文级别：

```text
coursework | bachelor-thesis | master-thesis | cn-general | cn-core | sci-low | sci-mid | sci-high
```

### Step 4: Evidence Chain

调用 `experiment-planning-suite:evidence-chain-planner`。输出：

- `claim-evidence-map.md`
- `experiment-matrix.md`
- `machine/verifier-contracts.yaml`

每条 claim 必须绑定实验、指标、成功/失败判定和失败动作。

### Step 5: Baseline and Ablation

调用 `experiment-planning-suite:baseline-ablation-designer`。输出：

- `baseline-ablation-plan.md`
- `machine/stop-conditions.yaml`

必须区分：

- 最低可交付 baseline
- 投稿级强 baseline

### Step 6: Figure, Table, Screenshot Evidence

调用 `experiment-planning-suite:figure-table-planner`。输出：

- `figure-table-plan.md`
- `data-schema/metrics-table.schema.md`
- `data-schema/figure-data-fields.md`
- `data-schema/screenshot-evidence-schema.md`

每张图/表/截图必须写清：结构、数据字段、caption、论文位置、证明哪一句 claim。

### Step 7: Feasibility and Salvage

调用 `experiment-planning-suite:feasibility-salvage-gate`。输出：

- `feasibility-salvage-plan.md`
- `machine/experiment-artifacts.schema.json`

如果证据弱，输出 `low-confidence` 加最低补救方案。补救方向优先是论文策略：收窄场景、换指标、换贡献角度、补关键资源，而不是无止境调参。

### Step 8: Req Handoff

调用 `experiment-planning-suite:req-experiment-handoff`。输出：

- `req-handoff.md`

该文件必须包含可复制给 `req-suite` 的下一棒 prompt，用于生成 `TASK-EXP-*`。

## Output Contract

```text
route_mode: <idea-only|topic-ready|benchmark-driven|halfway-salvage>
output_root: knowledge/experiment-plan/
current_support_level: <paper-level-band>
confidence: high|medium|low
written_files: <expected file list or existing file list>
next_skill: experiment-planning-suite:<child-skill>
req_ready: yes|no|partial
```

## Negative Boundary

- P2/P3 论文定位、标杆拆解和 storyline 定型优先走 `paper-positioning-suite`。
- 文献综述正文走 `literature-review-suite`。
- 真正跑实验、建任务、调度执行走 `req-suite`。
- 学术图像生成走 `academic-figure-suite:orchestra`，本 plugin 只规划图表。

## 本 skill 的 deletion-spec

- **触发删除条件**：当 plugin-native routing 能稳定识别 P4 入口模式并按完整 workflow 调用子 skill 时，本 orchestrator 可下线。
- **禁用方式**：删除 `plugins/experiment-planning-suite/skills/orchestrator/`，bump plugin 版本，刷新 marketplace/cache。
- **卸载清单**：同步检查 `README.md`、`.claude-plugin/plugin.json`、`.codex-plugin/plugin.json`、`plugins/registry.json` 和任何引用 `experiment-planning-suite:orchestrator` 的 prompt-core 路由。
