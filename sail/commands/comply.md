---
description: SAIL V2 compliance checklist — ISO/IEC 42001, EU AI Act, OWASP LLM/Agentic, DASF, AIUC-1. All five frameworks by default, or only those named.
argument-hint: "[framework(s) — e.g. eu-ai-act iso-42001 owasp dasf aiuc-1]"
---

Run the SAIL V2 **"Generate a compliance checklist"** workflow immediately. Do NOT run the interactive intake and do NOT ask scoping questions.

**Frameworks:** those named in "$ARGUMENTS" if non-empty. Accept loose spellings (`eu-ai-act`/`euaiact`, `iso-42001`/`iso42001`, `owasp`, `dasf`, `aiuc-1`/`aiuc1`); if an argument doesn't match any of the five, use the closest framework and note the interpretation in the output header. If "$ARGUMENTS" is empty, cover **all five frameworks** — one section per framework from a single catalog sweep.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/sail/SKILL.md`, section **"Generate a compliance checklist"**:

1. Read `${CLAUDE_PLUGIN_ROOT}/skills/sail/references/risk-index.md`, then search the phase files under `${CLAUDE_PLUGIN_ROOT}/skills/sail/references/` for each framework's tag in the Standards Mapping column.
2. Present the matching risks as a control checklist in **that framework's own language** — article/control numbers first, SAIL IDs as cross-reference.
3. State plainly in the document: mappings indicate alignment, not automatic compliance.
