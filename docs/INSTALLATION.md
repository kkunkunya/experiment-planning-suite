# Installation

This repository exports `experiment-planning-suite` as a local agent plugin.

## Claude Code

Use your local Claude Code plugin workflow to register this repository or copy the plugin folder into a configured plugin marketplace. Enable:

```text
experiment-planning-suite
```

Then start a new agent session so the runtime can discover the skills.

## Codex

Register the repository as a local marketplace or copy `plugins/experiment-planning-suite` into your existing Codex plugin marketplace. Enable:

```toml
[plugins."experiment-planning-suite@local"]
enabled = true
```

The exact marketplace name depends on your local runtime configuration.

## Smoke Test

Ask the agent:

```text
List the visible skills for experiment-planning-suite and explain when to use experiment-planning-suite:orchestrator.
```

If the runtime cannot see the plugin, re-check marketplace registration, plugin enablement, and whether the session was restarted.
