# Agent Crawler Guide

This guide is for crawler-style agents that can only read a GitHub URL.

## Reading Order

1. `README.md`
2. `plugins/experiment-planning-suite/README.md`
3. `plugins/experiment-planning-suite/skills/orchestrator/SKILL.md`
4. `plugins/experiment-planning-suite/references/workflow-profile.md`
5. The child `SKILL.md` files selected by the orchestrator
6. `docs/API_KEYS_AND_LOCAL_CONFIG.md`

## What To Assume

- This is a planning plugin, not an experiment runner.
- The main entrypoint is `experiment-planning-suite:orchestrator`.
- Outputs belong in the target project, usually under `knowledge/experiment-plan/`.
- Downstream execution planning should be handed to a task-ledger system such as `req-suite`.

## What Not To Assume

- Do not assume this repository contains private project data.
- Do not assume any maintainer API key is available.
- Do not invent experimental results, benchmark numbers, or feasibility claims.
- Do not flatten `references/` into `SKILL.md`; preserve resource fidelity.
