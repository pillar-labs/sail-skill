---
description: 7-phase SAIL V2 security roadmap plus per-phase maturity profile, built from the current repo and conversation context.
argument-hint: '[scope hints — e.g. "org-wide", controls already in place]'
---

Run the SAIL V2 **"Build an AI security roadmap"** and **"Assess AI security maturity"** workflows immediately. Do NOT run the interactive intake and do NOT ask scoping questions.

**Scope:** the current repository plus whatever this conversation has already established. Treat "$ARGUMENTS" (if non-empty) as scope hints — e.g. org-wide vs this system, or controls already in place.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/sail/SKILL.md`, sections **"Build an AI security roadmap"** and **"Assess AI security maturity"**:

1. Read `${CLAUDE_PLUGIN_ROOT}/skills/sail/references/risk-index.md`; use the three zones to determine which risks apply before scoring — scoring risks that can't apply inflates noise.
2. Walk the seven phases in order, scoring each applicable risk **unaddressed / partially mitigated / mitigated with evidence**. Where the repo and conversation give no evidence either way, mark the risk **Verify** and record the question to ask — do not ask it now.
3. Deliver one markdown document: per-phase maturity summary table → the phased roadmap (gaps sequenced by phase — Phase 1 gaps first, because later controls need policy backing) → quick wins flagged separately from structural work. Cite SAIL IDs throughout.
