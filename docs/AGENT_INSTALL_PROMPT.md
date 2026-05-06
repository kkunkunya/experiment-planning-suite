# Agent Install Prompt

Use this prompt with an agent that can read this GitHub repository and configure local plugins:

```text
Install the agent plugin from this repository as a local plugin named experiment-planning-suite. Do not copy private data or generated logs. Enable the plugin for this project, then verify that experiment-planning-suite:orchestrator is visible. After installation, run a smoke check by explaining the default planning route and expected knowledge/experiment-plan outputs.
```

If the agent cannot modify runtime config, ask it to provide exact manual install steps for your local Claude Code or Codex environment.
