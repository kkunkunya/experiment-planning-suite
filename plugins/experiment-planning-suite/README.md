# experiment-planning-suite

P4 experiment planning toolkit for academic papers, theses, and student projects.

This plugin is not an experiment runner. It turns a research direction, claims, benchmark-derived experiment templates, half-finished results, and resource constraints into a detailed, verifiable experiment plan under:

```text
knowledge/experiment-plan/
```

The execution layer stays in `req-suite`: this plugin writes a detailed plan and a copyable handoff prompt for `TASK-EXP-*` task generation.

## Core Philosophy

A paper is publishable when it can make a contribution in a specific field, scene, constraint, or evidence boundary. Bad or incomplete experiment results are not automatically failure; they may require claim narrowing, scene repositioning, metric switching, extra resources, or a lower-level paper strategy.

## Skills

| Skill | Purpose |
|---|---|
| `experiment-planning-suite:orchestrator` | Main P4 workflow entrypoint. Scans project materials, chooses route mode, and sequences the planning flow. |
| `experiment-planning-suite:research-claim-snapshot` | Reads existing materials and writes the research claim snapshot plus expected support level. |
| `experiment-planning-suite:evidence-chain-planner` | Maps claims to experiments, metrics, verifier contracts, and evidence gaps. |
| `experiment-planning-suite:baseline-ablation-designer` | Designs minimum-delivery and publication-grade baselines, ablations, metrics, and stop conditions. |
| `experiment-planning-suite:figure-table-planner` | Designs figures, tables, screenshots, captions, paper locations, and data schemas. |
| `experiment-planning-suite:feasibility-salvage-gate` | Judges feasibility, paper-level support, low-confidence cases, and salvage strategies. |
| `experiment-planning-suite:req-experiment-handoff` | Writes the `req-suite` prompt for turning the plan into `TASK-EXP-*` work packages. |

## Default Route

```text
project material scan
  -> research-claim-snapshot
  -> evidence-chain-planner
  -> baseline-ablation-designer
  -> figure-table-planner
  -> feasibility-salvage-gate
  -> req-experiment-handoff
```

## Output Files

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

## Collaboration

Upstream:

- `journal-research-en` / `journal-research-cn` for target venue constraints.
- `paper-positioning-suite` for P2/P3 benchmark, topic, fit, and storyline inputs.
- `literature-review-suite` / `academic-suite` for literature and paper metadata.

Downstream:

- `req-suite` for task packages and execution scheduling.
- `academic-figure-suite:orchestra` for thesis-grade structure/framework/process figures when visual generation is needed.
