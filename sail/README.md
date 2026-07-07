# SAIL — Claude Code Plugin

Applies **SAIL V2** (Secure AI Lifecycle) by [Pillar Security](https://www.pillar.security) to secure AI applications and agents. Bundles the `sail` skill, the full 91-risk SAIL catalog, and four workflow commands.

## Install

```
/plugin marketplace add pillar-labs/sail-skill
/plugin install sail@pillar-security
```

You get `/sail` (guided launcher) plus `/sail:assess`, `/sail:roadmap`, `/sail:comply`, `/sail:rfp` — each runs its workflow immediately, no intake questions. The skill also triggers automatically whenever you work on AI/agent security.

**Full documentation — workflows, commands, install for every other harness (Cowork, Claude.ai, Codex, ChatGPT, opencode):** see the [repository README](../README.md).
