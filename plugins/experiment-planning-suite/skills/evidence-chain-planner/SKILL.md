---
name: evidence-chain-planner
description: 用于把论文贡献主张映射成实验矩阵、指标、成功判定、失败动作和机器 verifier 契约。不用于 baseline 细化或图表设计。
---

# Evidence Chain Planner

## Role

本 skill 把“我做出了什么贡献”转成“哪些实验、指标、artifact 和判定能证明它”。核心产物是 claim-evidence map 和 experiment matrix。

## Method-call

```text
/evidence-chain-planner(
  claim_snapshot,
  target_level?,
  output_root = knowledge/experiment-plan/
) -> evidence_chain
```

## Step 1: Normalize Claims

每条 claim 写成可验证形式：

```text
In <scene>, <method/system> improves or enables <effect> compared with <baseline>, measured by <metric/evidence>.
```

不能测量的 claim 降级为 background / motivation / future work，不进入 contribution claim。

## Step 2: Map Claim to Evidence

每条 claim 至少绑定：

- experiment id
- experiment purpose
- dataset / case / scenario
- metric or observable evidence
- expected artifact path
- success criteria
- failure interpretation
- fallback or salvage action

输出总表：

```text
knowledge/experiment-plan/claim-evidence-map.md
```

## Step 3: Build Experiment Matrix

实验矩阵必须覆盖：

- baseline reproduction or baseline alignment
- main comparison
- ablation
- generalization / robustness / scenario transfer when relevant
- qualitative or system evidence when relevant
- figure/table/screenshot output hooks

列建议：

| Column | Meaning |
|---|---|
| experiment_id | Stable id such as EXP-001 |
| claim | Which claim this proves |
| purpose | Why this experiment exists |
| inputs | Dataset, case, logs, screenshots, or simulation condition |
| baselines | Minimum and strong baselines if known |
| metrics | Quantitative or qualitative indicators |
| success_rule | Human-readable rule |
| verifier_hint | Script, command, notebook, manual rubric, or verifier_todo |
| artifact | Expected machine-readable result |
| failure_action | Claim narrowing, metric switch, resource acquisition, or stop |
| priority | must / should / optional |
| feasibility | high / medium / low |

## Step 4: Machine Contract

Write an initial machine contract to:

```text
knowledge/experiment-plan/machine/verifier-contracts.yaml
```

Keep YAML concise and machine-readable. Do not expose this structure as the main human explanation. If a verifier does not exist, use `verifier_todo` and make that visible to req handoff.

## Step 5: Output Files

Write or update:

```text
knowledge/experiment-plan/claim-evidence-map.md
knowledge/experiment-plan/experiment-matrix.md
knowledge/experiment-plan/machine/verifier-contracts.yaml
```

## 本 skill 的 deletion-spec

- **触发删除条件**：当 experiment matrix generation and verifier-contract creation are fully covered by a more specific planner, this skill can be removed.
- **禁用方式**：删除 `plugins/experiment-planning-suite/skills/evidence-chain-planner/`，bump plugin 版本，刷新 marketplace/cache。
- **卸载清单**：同步检查 orchestrator、baseline-ablation-designer、figure-table-planner、req-experiment-handoff 对 `claim-evidence-map.md` 和 `verifier-contracts.yaml` 的引用。
