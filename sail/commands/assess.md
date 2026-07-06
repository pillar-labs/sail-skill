---
description: Full SAIL V2 gap analysis of the AI agent/system in the current repo — component inventory, all 91 catalog risks dispositioned, prioritized recommendations. Writes SAIL-assessment.md.
argument-hint: "[path or design doc — defaults to the current repo]"
---

Run the SAIL V2 **"Assess a system / gap analysis"** workflow immediately. Do NOT run the interactive intake and do NOT ask scoping questions.

**Target:** "$ARGUMENTS" if non-empty (a repo path or a design document); otherwise the current repository.

Follow the workflow exactly as specified in `${CLAUDE_PLUGIN_ROOT}/skills/sail/SKILL.md`, section **"Assess a system / gap analysis"**:

1. Read `${CLAUDE_PLUGIN_ROOT}/skills/sail/references/risk-index.md` first — it is the map.
2. Extract the component inventory from the target yourself (agent-framework imports, agent/tool configs, MCP configs, system prompts, credential handling, schedules) and map it onto the 29-component taxonomy in `${CLAUDE_PLUGIN_ROOT}/skills/sail/references/definitions.md`.
3. Chart the components: a Mermaid diagram grouped by zone with trust boundaries, plus the element → SAIL component → layer → zone table.
4. Sweep all 91 risks in the index and give every one a disposition — Gap / Partial / Addressed / Verify / N/A. Read the full phase file for every risk dispositioned Gap, Partial, or Verify so mitigations are quoted from the catalog.
5. Deliver per the workflow's document structure, citing SAIL IDs everywhere. If the target is a git repository, write the deliverable to `SAIL-assessment.md` at its root; otherwise emit the document in the conversation.

If the target contains no agent artifacts, say so, assess what does exist (LLM app, pipeline assets), and mark absent components N/A — still disposition all 91 risks.
