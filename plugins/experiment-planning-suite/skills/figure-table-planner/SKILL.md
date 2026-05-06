---
name: figure-table-planner
description: 用于 P4 设计论文图、表、截图/系统演示证据，绑定 claim、章节位置、caption 和数据字段 schema。不用于实际画图。
---

# Figure Table Planner

## Role

本 skill 负责把实验矩阵转成论文可用的图、表、截图和数据 schema。每个视觉证据必须绑定一条论文主张，不允许为了凑图而设计图。

## Method-call

```text
/figure-table-planner(
  claim_evidence_map,
  experiment_matrix,
  paper_type?,
  output_root = knowledge/experiment-plan/
) -> figure_table_plan
```

## Step 1: Evidence Types

根据项目类型选择证据：

- Figure: pipeline, architecture, teaser, trend line, bar chart, heatmap, qualitative comparison.
- Table: main comparison, ablation, generalization, complexity, resource cost, feature checklist.
- Screenshot/system evidence: app flow, database state, admin/user workflow, simulation result, model interface, deployment evidence.
- Diagram generation plan: when structure/framework/process diagrams are needed, route generation later to `academic-figure-suite:orchestra`.

## Step 2: Claim Binding

每个 figure/table/screenshot 必须写：

| Field | Requirement |
|---|---|
| evidence_id | FIG-001 / TAB-001 / SCR-001 |
| linked_claim | Which claim it proves |
| paper_location | Method / Experiments / Results / Discussion / Appendix |
| structure | Axes, columns, panels, screenshot sequence, or layout |
| data_fields | Required fields and data source |
| caption_draft | Caption that can stand alone |
| success_message | What the reader should conclude |
| failure_message | What it means if data does not show the expected pattern |

## Step 3: Data Schema

Write schema notes for downstream agents:

```text
knowledge/experiment-plan/data-schema/metrics-table.schema.md
knowledge/experiment-plan/data-schema/figure-data-fields.md
knowledge/experiment-plan/data-schema/screenshot-evidence-schema.md
```

Schema should be readable by humans and specific enough for req/code agents to generate tables and plots.

## Step 4: Avoid Visual Overclaiming

If a chart cannot actually prove the linked claim, mark it:

```text
evidence_status: illustrative_only
claim_support: weak
required_fix: <extra metric/table/experiment>
```

Illustrative evidence may remain in the paper, but cannot be counted as core proof.

## Step 5: Output Files

Write or update:

```text
knowledge/experiment-plan/figure-table-plan.md
knowledge/experiment-plan/data-schema/metrics-table.schema.md
knowledge/experiment-plan/data-schema/figure-data-fields.md
knowledge/experiment-plan/data-schema/screenshot-evidence-schema.md
```

## 本 skill 的 deletion-spec

- **触发删除条件**：当 a later paper-writing or figure-generation suite owns claim-bound figure/table planning end to end, this skill can be removed.
- **禁用方式**：删除 `plugins/experiment-planning-suite/skills/figure-table-planner/`，bump plugin 版本，刷新 marketplace/cache。
- **卸载清单**：同步检查 orchestrator、README、req-experiment-handoff 和任何 route to `academic-figure-suite:orchestra` 的引用。
