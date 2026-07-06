---
name: sail
description: >-
  Apply the SAIL (Secure AI Lifecycle) V2 framework by Pillar Security to secure AI
  applications and agents. Use this skill whenever the user asks about AI or agent
  security — assessing an AI system or agent architecture for risks, building an AI
  security roadmap or maturity assessment, writing or reviewing an AI security policy,
  creating compliance checklists (ISO/IEC 42001, EU AI Act, OWASP LLM/Agentic, DASF,
  AIUC-1), prioritizing AI security controls, evaluating AI vendors or tools (RFPs,
  security questionnaires), securing MCP servers, agent identities, or LLM apps, or
  asking what "SAIL" or a "SAIL ID" (e.g., SAIL 5.17) means. Also use it when the user
  is building agentic features and wants to know which security risks apply — even if
  they never say "SAIL".
---

# SAIL V2 — Secure AI Lifecycle Framework

SAIL is Pillar Security's framework for building, deploying, and operating AI systems and agents securely. It is a working tool, not a reading exercise: users bring real systems, policies, and compliance obligations, and expect concrete, catalog-grounded answers. Whether they are writing their first AI policy or already operating hundreds of agents, SAIL is used to:

1. **Build an AI security roadmap** — walk the seven phases in order, assess coverage at each; the gaps, sequenced by phase, become the roadmap.
2. **Assess AI security maturity** — score each relevant risk (unaddressed / partially mitigated / mitigated with evidence) into a per-phase maturity profile trendable over time.
3. **Generate a compliance checklist** — filter the catalog by the framework the user is accountable to (ISO/IEC 42001, EU AI Act, OWASP, DASF, AIUC-1).
4. **Prioritize controls** — focus on risks that intersect the user's zones, assets, and weakest phase; autonomy tiers (SAIL 1.11) set control intensity per agent.
5. **Run vendor assessments and RFPs** — turn the catalog into a general RFP questionnaire (for security vendors or platform vendors), with questions mapped back to SAIL risk clusters.
6. **Align security, legal, compliance, and engineering** — SAIL IDs give all four teams one shared vocabulary for design reviews, risk registers, and exception requests.

Each has a workflow below.

## Core structure (memorize this, cite it constantly)

**Three zones** define *where* agentic risk materializes (drawn by where the risk lands, not where the process runs):

- **Zone 1 — AI Assets in Code & Pipeline**: models, datasets, prompts, agent configs, AI-generated code as artifacts in repos and CI/CD. Static surface; a poisoned artifact propagates into every build.
- **Zone 2 — Cloud Agents**: agents executing inside managed platforms (Copilot Studio, Agentforce, Bedrock AgentCore, etc.) under platform-issued identities.
- **Zone 3 — Endpoint Agents**: agents running on user devices with user credentials (Claude Code, Cursor, browser agents).

**Seven lifecycle phases** define *when* controls apply. Each phase has a numbered risk catalog — 91 risks total, each with an ID like `SAIL 3.4`:

| Phase | Name | Lifecycle stage | Risk IDs |
| :-- | :-- | :-- | :-- |
| 1 | AI Policy | Plan | SAIL 1.x |
| 2 | AI Discovery | Code/No Code | SAIL 2.x |
| 3 | Agentic Posture Management | Build | SAIL 3.x |
| 4 | Agentic Red Teaming | Test | SAIL 4.x |
| 5 | Runtime Controls | Deploy | SAIL 5.x |
| 6 | Sandbox | Operate | SAIL 6.x |
| 7 | Govern | Monitor & Retire | SAIL 7.x |

Every risk row in the catalog carries: description, concrete example, assets affected, mitigations, and standards mappings (ISO/IEC 42001, EU AI Act, OWASP LLM Top 10, OWASP Agentic, DASF, AIUC-1). SAIL IDs are the shared vocabulary — always cite them (e.g., "this maps to SAIL 5.17") so security, legal, compliance, and engineering can reference the same rows in risk registers and design reviews.

## Ground every answer in the catalog

Do not answer SAIL questions from general knowledge. The value of this skill is the actual catalog text — the mitigations, examples, and standards mappings the user's auditors and vendors will check against. Before making claims:

1. Read `references/risk-index.md` first — all 91 risks in one compact table (ID, name, one-line summary, phase, file). Use it to decide which risks are in scope.
2. Read the full phase file(s) for every risk you cite, so mitigations and standards mappings are quoted accurately, not paraphrased from memory.

## Reference files

| File | Contents | Read when |
| :-- | :-- | :-- |
| `references/risk-index.md` | All 91 risks: ID, name, summary, phase | Almost always — this is your map |
| `references/framework.md` | Executive summary, Chapter 1 (agentic workforce, the three zones with per-surface risk tables), Chapter 2 (phase overview, agentic stack components, how to use SAIL) | Explaining the framework, scoping zones, writing intros for deliverables |
| `references/phase-1-policy.md` … `phase-7-govern.md` | Full risk catalog per phase: descriptions, examples, assets, mitigations, standards mappings | Citing any risk in detail; building checklists, assessments, roadmaps |
| `references/definitions.md` | Appendix A: definitions of AI/agentic components (agent, MCP server, skill, agent identity, …) | Terminology disputes, glossaries for deliverables |
| `references/use-case.md` | Appendix B: worked FinTech example (agentic investment research platform) | User wants an example of SAIL applied end-to-end |

## Interactive intake — when invoked as `/sail` or the intent is unclear

> In Claude Code (plugin install), four commands skip this intake and run their workflow immediately: `/sail:assess`, `/sail:roadmap`, `/sail:comply`, `/sail:rfp`. If the user's request already matches one of those workflows, behave the same way — go straight to it.

If the user invoked this skill directly with no task attached (e.g., typed `/sail`), or their request doesn't clearly match one workflow, don't guess — run a short intake. Use the AskUserQuestion tool if available; otherwise ask in plain text. Batch related questions — don't interrogate one at a time.

**Step 0 — Detect the working context first.** Before asking anything, look at where the skill was invoked. Quickly scan the current repo for agent artifacts: agent framework imports (LangGraph, CrewAI, AutoGen, LlamaIndex, the Agents SDK), agent/tool configs (`agent.yaml`, `AGENTS.md`, `.cursor/rules`, MCP configs like `mcp.json` / `claude_desktop_config.json`), system-prompt files, or tool/MCP-server definitions.

**If the repo contains an agent, lead with assessing it.** Make the **first option** "Assess the agent in this repository (SAIL gap analysis)" — the user is standing in an agent project; the highest-value move is to run the gap-analysis workflow on the code in front of you (extract the component inventory from the repo yourself; don't ask the user to describe what you can read). Offer the other deliverables after it. A good option list in this context:
1. **Assess the agent in this repo** — full SAIL gap analysis of the code here *(default)*
2. **Create a vendor RFP** — generate a security-vendor or platform RFP questionnaire from SAIL
3. **Build a security roadmap / maturity assessment** for the broader AI program
4. **Generate a compliance checklist** (ISO/IEC 42001, EU AI Act, OWASP, DASF, AIUC-1)
5. **Prioritize controls** / team-alignment artifact
6. **Just ask questions about SAIL**

Pick the chosen deliverable, then jump to the workflow (for agent assessment, run "Assess a system / gap analysis" directly against the repo). Only ask the remaining scoping that workflow needs.

**If the repo has no agent (or the skill isn't in a code context), run the top-down program intake below** — establish the *program* first, then *what they're protecting*, then the deliverable.

**Question 1 — What kind of program is this? (single-select)** This frames everything: the same catalog is filtered and sequenced differently depending on the mandate. Offer:
- *AI governance & compliance* — policy, standards mapping, audit-readiness (leans Phases 1, 7 + compliance frameworks)
- *Agent / application security* — securing specific agents or AI apps you build or run (leans Phases 3–6, gap analysis)
- *AI transformation / adoption* — standing up AI safely at scale, building the security roadmap from scratch (leans full 7-phase roadmap + maturity)
- *Vendor / third-party assessment* — evaluating a platform, tool, or MCP server before adoption
- *Incident / red-team readiness* — testing and runtime defense posture (leans Phases 4–6)

**Question 2 — What do you have in your org to protect? (MULTI-select)** Users rarely have just one thing — capture the whole environment so the assessment scopes to their real asset mix. This maps directly to the zones and to the catalog's "Assets Affected" column. Offer (multi-select):
- Custom-built AI agents (LangGraph/CrewAI/etc.) — *Zone 2*
- Cloud/managed agent platforms (Copilot Studio, Agentforce, Bedrock…) — *Zone 2*
- Endpoint/coding agents (Claude Code, Cursor) — *Zone 3*
- LLM apps / SaaS with AI features — *Zone 2*
- MCP servers, tools, plugins & integrations — *Zone 1/2*
- AI models, datasets & RAG stores — *Zone 1*
- Prompts, agent configs & AI code in repos/CI-CD — *Zone 1*
- Not sure yet / help me discover what I have — *(routes to the AI Discovery workflow, Phase 2)*

**Question 3 — Which deliverable do you want?** Map the program + assets to a workflow and confirm which output they need: roadmap, maturity assessment, compliance checklist, control prioritization, vendor RFP, system gap analysis, team-alignment artifact, or just Q&A. Often Q1 already implies it — offer the best-fit as the default and let them redirect.

**Then ask only the remaining scoping the chosen workflow needs** (skip anything Q1/Q2 already answered):
- *Roadmap / maturity*: What's already in place (policy, discovery, red teaming, runtime controls)?
- *Compliance checklist*: Which framework — ISO/IEC 42001, EU AI Act, OWASP, DASF, AIUC-1?
- *Prioritize controls*: Where do they feel weakest? Any fully autonomous agents?
- *Vendor assessment / RFP*: Is this a **security vendor** (a product that protects your AI — the common case, → coverage questionnaire) or a **platform/product vendor** you'll build on (→ how-do-you-secure-it questionnaire)? Which specific vendor(s)? For platform vendors: what will agents built on it access?
- *System review*: Architecture — components, tools/MCP servers, data access, autonomy level, where prompts/configs live. If a codebase or design doc is available, offer to extract this yourself instead of asking.
- *Align teams*: Which artifact — design review template, risk register, exception request, glossary?

**Confirm scope in one sentence, then run the matching workflow.** Don't re-ask what they already told you.

If the user arrived with a fully-specified task, skip the intake entirely and go straight to the workflow.

## Workflows

Pick the workflow matching the user's intent. All of them share the same skeleton: scope by zone → select risks from the index → pull detail from phase files → deliver with SAIL IDs cited.

### Assess a system / gap analysis

This is the flagship deliverable — make it comprehensive. Two things make an assessment comprehensive: a component inventory in SAIL's own taxonomy, and a disposition for *every* risk in the catalog (not just highlights).

1. **Build the component inventory first.** Map every element of the user's system onto the SAIL component taxonomy — the 29 components in six layers defined in Appendix A (`references/definitions.md`; layer grouping in `references/framework.md` §2.2). If the user gave you a codebase or design doc, extract this yourself before asking questions. This inventory is not decoration: the catalog's "Assets Affected" column uses exactly these component names, so the inventory mechanically determines which risks apply.
2. **Chart the components.** Present the inventory as (a) a Mermaid diagram of the agent architecture, grouping components by zone with trust boundaries between them (user → agent harness → tools/MCP servers → data stores → external services), and (b) a table: user's element → SAIL component (Appendix A name) → layer → zone. Every SAIL component *not* present is a scoping fact worth one line — it excludes risk rows and shows the assessment considered them.
3. **Sweep the full catalog.** Walk all 91 rows of `risk-index.md` and give every risk a disposition — don't stop at the obvious hits:
   - **Gap** — applies, nothing addresses it
   - **Partial** — applies, some mitigation exists but incomplete
   - **Addressed** — applies and mitigated (say what mitigates it)
   - **Verify** — applies, but the user's description doesn't say either way; list the question to ask
   - **N/A** — affected assets absent from the inventory (one-line reason; group these compactly)
   A risk applies if its Assets Affected intersect the component inventory. Read the full phase-file row for every risk dispositioned Gap/Partial/Verify so mitigations are quoted from the catalog, not invented.
4. **Deliver** as a markdown doc: executive summary with per-phase disposition counts → component chart (diagram + table) → findings by phase (each with SAIL ID, disposition, evidence, catalog mitigation adapted to the user's stack) → prioritized recommendations (autonomy tier per SAIL 1.11 drives control intensity) → the compact N/A table last. State the coverage explicitly ("all 91 catalog risks dispositioned") so the reader knows nothing was skipped silently.

### Build an AI security roadmap

Walk the seven phases in order and assess coverage at each. The gaps, sequenced by phase, *are* the roadmap: Phase 1 gaps come first because later controls need policy backing (e.g., runtime action authorization in SAIL 5.x presumes the policy of SAIL 1.12 exists). Present as a phased plan with SAIL IDs, quick wins flagged separately from structural work.

### Assess AI security maturity

Score each relevant risk — **unaddressed / partially mitigated / mitigated with evidence** — to produce a per-phase maturity profile the user can trend over time. Use the three zones to determine which risks apply to their environment before scoring; scoring risks that can't apply inflates noise. Output a per-phase summary table plus the detailed scored rows.

### Generate a compliance checklist

Filter the catalog by the framework the user is accountable to (ISO/IEC 42001, EU AI Act, OWASP LLM/Agentic, DASF, AIUC-1). Grep the phase files for that framework's tag in the Standards Mapping column, then present the matching risks as a control checklist in *that framework's language* (article/control numbers first, SAIL IDs as cross-reference). State plainly: mappings indicate alignment, not automatic compliance.

### Prioritize controls

Focus on risks at the intersection of: the user's zones, the assets they actually have, and their weakest phase. Autonomy tiers (SAIL 1.11) set control intensity per agent — a fully autonomous agent warrants stricter controls than a supervised assistant for the same risk row. Deliver a ranked list with a one-line justification per item.

### Vendor assessment / RFP questions

The deliverable is a **markdown RFP questionnaire the user can send directly to vendors** — self-contained, professionally framed, with instructions and space for vendor responses.

**Do not emit one question per SAIL risk.** A 60-line questionnaire that mirrors the catalog row-for-row is unusable in a real procurement. Instead, write a tight set of **general, well-crafted RFP items** shaped by the user's stated focus, and **map each item to the SAIL cluster it covers**. The SAIL mapping is a *citation on the question*, not the question itself. A mapping can be:
- **Broad** — a whole phase, e.g. "SAIL Phase 1 (policy)" or "SAIL 5.x (runtime)"
- **A range/cluster** — e.g. "SAIL 5.17–5.20" for a group of related runtime-identity risks
- **A themed list** — e.g. "SAIL 1.13, 5.17, 5.20" pulled from across phases for one identity question

How to build it:
1. **Select the SAIL risks in scope** from the user's answers (their zones, assets, and program focus — e.g. an identity-focused engagement pulls the identity risks across phases). Don't include the whole catalog by default; include what their focus implies.
2. **Cluster those risks into procurement themes** — identity & authorization, tool/action permissions, data access & leakage, supply-chain & vetting, runtime enforcement & oversight, audit & governance, etc. Each theme becomes **one to a few** general questions, not one per underlying risk.
3. **Write the question at the buyer's altitude**, then annotate it with the SAIL cluster it maps to so answers still trace back to the risk register.

The framing flips by **which kind of vendor** is being assessed:
- **Security vendor** (a product whose *job* is to protect the user's AI — detection, guardrails, posture management, red-teaming, runtime defense). Most common case. Frame items as **coverage questions**: does the product address this cluster of risks, and how? Ask for *mechanism* (build-time scan / runtime enforcement / monitoring-only), *depth* (detect vs. prevent vs. remediate), and *evidence* (how coverage is demonstrated, not just claimed). Give each item a response template: **Covered fully / Partial / Not covered**, plus "how" and "evidence." Example item — *"How does your product govern agent identity and prevent privilege inheritance across agents and their makers?"* → maps to **SAIL 1.13, 5.17, 5.20**.
- **Platform / product vendor** (an agent platform or AI product the user will *build on or adopt* — Copilot Studio, Agentforce, a SaaS-with-AI). The vendor may *introduce* the risk, so ask how they control it in their own platform: *"How does your platform decouple a maker's identity from an agent's runtime identity, and scope agent permissions?"* → maps to **SAIL 5.17, 5.20**. Scope to the risks that platform's assets create (often Zone 2 + Phase 5).

Structure the output as: short intro (scope + how to respond) → themed sections, each with a few general questions carrying their SAIL-cluster mapping and (for security vendors) the coverage response template → a closing section naming the highest-severity risk clusters so the buyer knows which answers matter most. If it's unclear which kind of vendor, ask — it changes every question's framing.

### Align security, legal, compliance, and engineering

When the user needs cross-team artifacts — design review templates, risk register entries, exception requests, or a shared glossary — anchor every item to a SAIL ID so all four teams reference the same catalog row. A risk register entry should carry: SAIL ID, risk name, the team owning the mitigation, current status, and the standards mappings legal/compliance care about. For terminology disagreements, `references/definitions.md` is the arbiter. The point is one vocabulary: engineering argues about the mitigation, not about what the risk is called.

### Guide secure AI development

When the user is building agent/LLM features (with or without mentioning SAIL): identify which risks the code in front of you triggers (tool definitions → MCP vetting risks, agent memory → memory poisoning, new credentials → agent identity risks), name them by SAIL ID, and apply the catalog's mitigations in the implementation itself — not as a lecture afterward.

### Answer questions about SAIL

For "what is SAIL X.Y" or conceptual questions, read the relevant reference file and answer from it, quoting the catalog's own example when it helps. If asked about something the framework genuinely doesn't cover, say so rather than stretching a mapping.

## Output conventions

- Cite SAIL IDs inline everywhere a risk is referenced.
- For deliverables (assessments, checklists, roadmaps), default to a markdown document with: scope (zones + components), findings/controls table with SAIL IDs, and recommendations. Match the compliance framework's own vocabulary when one was specified.
- Keep the framework's stance: SAIL is practitioner-oriented and vendor-neutral in its guidance. Recommendations should be actionable controls, not marketing.
