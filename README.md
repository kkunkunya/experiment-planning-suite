# Experiment Planning Suite

## Portfolio positioning

This is a portfolio-facing academic research Agent/Plugin project for experiment planning, claim-evidence mapping, baselines, ablations, and task handoff. It shows how research planning can be turned into reusable agent skills.


Experiment Planning Suite is an agent plugin for turning early academic ideas, partial evidence, benchmark notes, and resource constraints into a concrete experiment plan.

It is not an experiment runner. It helps an agent write a verifiable planning surface under `knowledge/experiment-plan/`, then hands execution planning to a task-ledger workflow such as `req-suite`.

Author: Kunkun

## What It Does

- Extracts the research claim and expected evidence level.
- Maps claims to experiments, metrics, verifier contracts, and evidence gaps.
- Designs baselines, ablations, stop conditions, and feasibility salvage paths.
- Plans paper figures, tables, screenshots, captions, and data schemas.
- Produces a handoff prompt for downstream task-package generation.

## Plugin Layout

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

## Quick Start

1. Install or expose this repository as a local plugin marketplace for your agent runtime.
2. Enable `experiment-planning-suite`.
3. Ask the agent to use `experiment-planning-suite:orchestrator` on a project folder that contains research notes, benchmark notes, drafts, or early requirements.

Example request:

```text
Use experiment-planning-suite:orchestrator to read this project and write a paper-level experiment plan under knowledge/experiment-plan/.
```

## Expected Output

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

## Safety

This repository does not include maintainer-owned API keys, private project files, customer materials, or generated experiment logs. Users should configure any required tools locally.

See:

- [Installation](docs/INSTALLATION.md)
- [Agent install prompt](docs/AGENT_INSTALL_PROMPT.md)
- [Crawler guide](docs/AGENT_CRAWLER_GUIDE.md)
- [API keys and local config](docs/API_KEYS_AND_LOCAL_CONFIG.md)
- [Security policy](SECURITY.md)

## License

MIT
