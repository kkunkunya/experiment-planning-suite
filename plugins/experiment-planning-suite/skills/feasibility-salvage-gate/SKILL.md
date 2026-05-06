---
name: feasibility-salvage-gate
description: 用于 P4 判断实验计划可实施性、预计论文级别、low-confidence 风险、失败后贡献重定位和最低补救方案。不用于拒稿信预演。
---

# Feasibility Salvage Gate

## Role

本 skill 是 P4 的策略门。它判断当前计划能不能做、能支撑什么论文级别、哪里低置信，以及失败后怎样换论文策略。

## Method-call

```text
/feasibility-salvage-gate(
  snapshot,
  experiment_matrix,
  baseline_ablation_plan,
  figure_table_plan,
  target_level?,
  output_root = knowledge/experiment-plan/
) -> feasibility_salvage_plan
```

## Step 1: Feasibility Scan

按维度打标：

| Dimension | Question |
|---|---|
| data | 数据、案例、工况、截图是否可获取？ |
| code/environment | 代码、工具、仿真、平台是否能跑？ |
| baseline | 最低 baseline 和强 baseline 是否可实现？ |
| metric | 指标是否可自动或半自动判定？ |
| figure/table | 图表数据能否产出？ |
| time/resource | 当前资源是否够做 must-have 实验？ |
| paper_level | 证据链是否匹配目标级别？ |

## Step 2: Confidence and Level

输出：

```text
current_support_level: <band>
target_level: <band or unknown>
confidence: high|medium|low
blocking_gaps: [...]
upgrade_path: [...]
minimum_salvage_path: [...]
```

`low-confidence` 不是失败；它必须附带最低补救方案。

## Step 3: Salvage Strategy

优先从论文策略上补救：

- narrow scene: 从全局有效改为某场景/区域/约束下有效。
- switch metric: 精度不赢时转效率、鲁棒性、成本、稳定性、可解释性、可部署性。
- lower claim: 从方法创新改为应用验证、系统工程、对比改进、案例资源。
- acquire resource: 补数据、补 baseline、补日志、补截图、补仿真工况。
- downgrade paper level: 目标从 sci-mid 降到 sci-low / cn-general / master-thesis 等。
- ask user: 需要付费资源、不可逆改动、真实投稿目标裁决时让用户决定。

## Step 4: Machine Artifact Schema

Write artifact expectations to:

```text
knowledge/experiment-plan/machine/experiment-artifacts.schema.json
```

This schema should tell harness agents how to recognize results, plots, screenshots, metric files, and verifier outputs.

## Step 5: Output Files

Write or update:

```text
knowledge/experiment-plan/feasibility-salvage-plan.md
knowledge/experiment-plan/machine/experiment-artifacts.schema.json
```

## Negative Boundary

Do not make rejection-letter simulation the core framing. The gate can mention likely reviewer concerns only when they directly map to evidence gaps, but the main frame is contribution repositioning and feasibility.

## 本 skill 的 deletion-spec

- **触发删除条件**：当 paper-positioning plus req execution can jointly perform support-level, feasibility, and salvage judgment without a separate P4 gate, this skill can be removed.
- **禁用方式**：删除 `plugins/experiment-planning-suite/skills/feasibility-salvage-gate/`，bump plugin 版本，刷新 marketplace/cache。
- **卸载清单**：同步检查 orchestrator、README、req-experiment-handoff 和 any future P8 review/revision suite references。
