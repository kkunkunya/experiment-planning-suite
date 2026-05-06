# Experiment Planning Suite Workflow Profile

This profile is the high-level route map for `experiment-planning-suite`. The orchestrator should keep the workflow visible in its own body, then use this profile for compact reminders about modes, outputs, and handoffs.

## Plugin Interview

```yaml
primary_job: turn research claims into executable and verifiable experiment evidence plans
entry_skill: orchestrator
child_skills:
  - research-claim-snapshot
  - evidence-chain-planner
  - baseline-ablation-designer
  - figure-table-planner
  - feasibility-salvage-gate
  - req-experiment-handoff
accepted_inputs:
  - idea-only direction
  - topic and innovation claims
  - benchmark deconstruction or synthesis
  - half-finished experiments
  - code/results/logs/screenshots
  - target venue or thesis requirement
output_root: knowledge/experiment-plan/
human_outputs:
  - research-claim-snapshot.md
  - paper-level-assessment.md
  - claim-evidence-map.md
  - experiment-matrix.md
  - baseline-ablation-plan.md
  - figure-table-plan.md
  - feasibility-salvage-plan.md
  - req-handoff.md
machine_outputs:
  - machine/verifier-contracts.yaml
  - machine/experiment-artifacts.schema.json
  - machine/stop-conditions.yaml
  - data-schema/*.md
paper_level_bands:
  - coursework
  - bachelor-thesis
  - master-thesis
  - cn-general
  - cn-core
  - sci-low
  - sci-mid
  - sci-high
default_profiles:
  - generic
  - ai-ml
  - engineering-optimization
  - software-system
hard_gates:
  - do not overstate paper level
  - bind every figure/table to a claim
  - distinguish minimum-delivery baseline from publication-grade baseline
  - provide low-confidence plus minimum salvage instead of fake certainty
  - do not let execution loop tune forever; require stop conditions
must_not_do:
  - run P5 execution itself
  - create TASK-EXP files directly unless the user explicitly asks for req work too
  - use rejection-letter simulation as the main frame
  - hide machine contracts in human-facing prose
downstream_plugins:
  - req-suite
  - academic-suite
  - paper-positioning-suite
```

## Route Modes

| Mode | User state | Planning posture |
|---|---|---|
| `idea-only` | Direction and rough method only | Build a research-claim snapshot, mark assumptions, propose low-cost evidence path. |
| `topic-ready` | Research problem and innovation claims exist | Map claims to evidence, baseline, metrics, figures, and verifier contracts. |
| `benchmark-driven` | P2 benchmark experiment templates exist | Migrate benchmark modules into the user's experiment matrix. |
| `halfway-salvage` | Experiments/results already exist | Reposition contribution around what the evidence can support; add minimum supplements. |

## Support Level Rule

The plugin may estimate the paper level that the current plan can support, but it must phrase the result as a planning assessment, not a publication guarantee.

Use:

```text
current_support_level: <band>
confidence: high|medium|low
upgrade_path: <extra experiments needed for next level>
minimum_salvage_path: <lowest viable paper strategy>
```
