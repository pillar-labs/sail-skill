---
description: Vendor RFP questionnaire from the SAIL V2 catalog — security-vendor coverage questionnaire by default, platform-vendor framing on request.
argument-hint: "[security|platform] [vendor name / focus area]"
---

Run the SAIL V2 **"Vendor assessment / RFP questions"** workflow immediately. Do NOT run the interactive intake and do NOT ask scoping questions.

**Vendor type:** if "$ARGUMENTS" starts with `platform`, use **platform/product-vendor** framing (how does the vendor secure its own platform — scope to the risks that platform's assets create, often Zone 2 + Phase 5). Otherwise default to **security vendor** — a coverage questionnaire with the response template *Covered fully / Partial / Not covered*, plus mechanism (build-time scan / runtime enforcement / monitoring-only), depth (detect vs prevent vs remediate), and evidence. Any remaining argument text names the vendor and/or focus area and scopes the themes.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/sail/SKILL.md`, section **"Vendor assessment / RFP questions"**:

1. Read `${CLAUDE_PLUGIN_ROOT}/skills/sail/references/risk-index.md` and select the risks the stated focus implies (all zones if none given).
2. Cluster them into procurement themes — identity & authorization, tool/action permissions, data access & leakage, supply chain & vetting, runtime enforcement & oversight, audit & governance. Write **one to a few general questions per theme** — never one question per SAIL risk.
3. Annotate every question with the SAIL cluster it maps to (a phase, a range like SAIL 5.17–5.20, or a themed list).
4. Structure: short intro (scope + how to respond) → themed sections → closing section naming the highest-severity risk clusters.

Deliver a self-contained markdown questionnaire the user can send directly to vendors.
