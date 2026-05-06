---
name: research-claim-snapshot
description: 用于 P4 实验规划前先读项目材料，生成研究主张快照、资源边界、预计论文级别和 low-confidence 缺口。不用于拆 req 任务。
---

# Research Claim Snapshot

## Role

本 skill 负责把项目现有材料压缩成 P4 可用的研究主张快照。它先读已有调研和实验材料，再判断当前计划预计能支撑的论文级别。

## Method-call

```text
/research-claim-snapshot(
  project_root,
  materials?,
  target_level?,
  output_root = knowledge/experiment-plan/
) -> claim_snapshot
```

## Step 1: Material Inventory

列出已读材料，按证据类型分组：

- research direction / requirement
- target journal or thesis requirement
- P2 benchmark deconstruction or synthesis
- P3 topic and innovation claims
- literature review or reference pool
- method notes / code / dataset / existing results
- screenshots / demo evidence / system artifacts

缺失项写 `missing`，不脑补。

## Step 2: Research Claim Snapshot

把材料转成可验证主张：

| Field | Question |
|---|---|
| research_scene | 在哪个领域、场景、区域或约束下做贡献？ |
| problem | 要解决什么问题？ |
| method | 实际方法、系统或流程是什么？ |
| contribution_candidates | 可能的贡献点有哪些？ |
| measurable_effect | 贡献能通过什么指标或证据证明？ |
| available_resources | 数据、代码、环境、算力、时间、人工资源有什么？ |
| missing_inputs | 缺 P2/P3/结果/数据/期刊信息的哪一块？ |

## Step 3: Paper Level Assessment

输出当前实验计划预计支撑级别，不做发表保证：

```text
current_support_level: coursework|bachelor-thesis|master-thesis|cn-general|cn-core|sci-low|sci-mid|sci-high
confidence: high|medium|low
why_this_level: <evidence-backed explanation>
upgrade_path: <what is needed for next level>
minimum_salvage_path: <lowest viable paper strategy>
```

判断依据：

- claim novelty and clarity
- baseline strength
- ablation completeness
- dataset / case / scenario breadth
- metric and verifier clarity
- target venue fit
- whether current evidence can support the claim without overclaiming

## Step 4: Output Files

Write or update:

```text
knowledge/experiment-plan/research-claim-snapshot.md
knowledge/experiment-plan/paper-level-assessment.md
```

Human-facing prose must stay readable. Put machine contracts in later skills, not in this snapshot.

## 本 skill 的 deletion-spec

- **触发删除条件**：当 P4 orchestrator can reliably infer project claims and paper-level support without a dedicated snapshot pass, this skill can be removed.
- **禁用方式**：删除 `plugins/experiment-planning-suite/skills/research-claim-snapshot/`，bump plugin 版本，刷新 marketplace/cache。
- **卸载清单**：同步检查 orchestrator workflow、README、`feasibility-salvage-gate` 和 `req-experiment-handoff` 对 snapshot 文件的引用。
