---
name: req-experiment-handoff
description: 用于把 knowledge/experiment-plan/ 的 P4 实验规划转成 req-suite 下一棒 prompt，要求拆 TASK-EXP-* 并绑定 verifier。不用于亲自创建任务文件。
---

# Req Experiment Handoff

## Role

本 skill 把实验规划包交给 `req-suite`。它不直接创建 `TASK-EXP-*`，除非用户明确要求本轮继续调用 req-suite 写任务文件。

## Method-call

```text
/req-experiment-handoff(
  experiment_plan_root = knowledge/experiment-plan/,
  target_req_mode?: task-author|task-scheduler|auto
) -> req_handoff_prompt
```

## Step 1: Check Required Inputs

确认以下文件存在或标明缺口：

- `research-claim-snapshot.md`
- `paper-level-assessment.md`
- `claim-evidence-map.md`
- `experiment-matrix.md`
- `baseline-ablation-plan.md`
- `figure-table-plan.md`
- `feasibility-salvage-plan.md`
- `machine/verifier-contracts.yaml`
- `machine/stop-conditions.yaml`

缺 verifier 时，不把执行任务标成 ready；写 `verifier_todo`。

## Step 2: Task Split Rules

建议 `req-suite` 拆成：

- `TASK-EXP-BASELINE-*`
- `TASK-EXP-MAIN-*`
- `TASK-EXP-ABLATION-*`
- `TASK-EXP-GENERALIZATION-*`
- `TASK-EXP-FIGURE-*`
- `TASK-EXP-SCREENSHOT-*`
- `TASK-EXP-SALVAGE-CHECK`

每个 task 必须绑定：

- claim id
- experiment id
- expected artifact
- verifier or verifier_todo
- stop condition
- failure action from `feasibility-salvage-plan.md`

## Step 3: Handoff Prompt

Write a copyable prompt to:

```text
knowledge/experiment-plan/req-handoff.md
```

Prompt skeleton:

```text
请调用 req-suite，把 knowledge/experiment-plan/ 下的实验规划拆成 TASK-EXP-*。

要求：
1. 每个 TASK 必须绑定 claim-evidence-map.md 中的一个 Claim 或 figure-table-plan.md 中的一个 Figure/Table/Screenshot。
2. 每个 TASK 必须引用 machine/verifier-contracts.yaml 和 machine/stop-conditions.yaml。
3. 如果 verifier 不存在，任务状态不能写 done，只能写 verifier_todo 或 blocked。
4. 每个 TASK 输出 artifact 路径、指标结果、是否满足 stop condition。
5. 失败时按 feasibility-salvage-plan.md 切换策略，不要无限调参。
6. 任务文件写入 knowledge/tasks/，运行证据写入 knowledge/runs/。
```

## Step 4: Output Contract

```text
req_ready: yes|partial|no
reason: <missing files or ready summary>
next_plugin: req-suite
recommended_skills: task-author, task-scheduler, task-integrity-auditor
handoff_file: knowledge/experiment-plan/req-handoff.md
```

## 本 skill 的 deletion-spec

- **触发删除条件**：当 req-suite natively consumes `knowledge/experiment-plan/` and creates experiment tasks without a prompt handoff, this skill can be removed.
- **禁用方式**：删除 `plugins/experiment-planning-suite/skills/req-experiment-handoff/`，bump plugin 版本，刷新 marketplace/cache。
- **卸载清单**：同步检查 orchestrator、README、req-suite docs, and any project templates that mention `req-handoff.md`。
