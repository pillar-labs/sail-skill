## Use Case: Real-Time Posture for an Agentic Investment Research Platform

## **Customer: FinTech / investment house** 

## **Primary users:** CISO, AppSec, AI platform team, investment technology team, compliance/risk AI stack: Claude Code for development, custom investment research agent in production, MCP/tools, market-data feeds, internal portfolio/risk systems, CI/CD, SIEM.

Use Case

An investment house wants to safely build and operate a custom AI investment agent that helps analysts research companies, summarize filings, compare portfolio exposure, draft investment memos, and prepare approval workflows. Developers use Claude Code to build and maintain the agent, prompts, tools, and integrations.

The security challenge is that the posture of this stack changes constantly. Claude Code may add or modify prompts, tool definitions, MCP configs, dependencies, and CI/CD artifacts. The production investment agent may gain new tools, access new data feeds, inherit broader permissions, or connect to additional internal systems. A once-approved agent can drift into a higher-risk state without a formal deployment event.

**How SAIL Helps Govern and Secure the Fleet \-** Pillar provides real-time posture visibility across the agentic lifecycle: what agents exist, what they can access, which tools they can invoke, what changed, what risk was introduced, and which SAIL controls are missing.

| Layer | Real-Time Posture Question | SAIL Coverage |
| :---- | :---- | :---- |
| AI asset discovery  | Which agents, models, MCP servers, and identities exist across the organization — including shadow ones? | Phase 2 |
| Developer endpoints | Which developers are using Claude Code, and with what local authority? | Phase 2, 3, 6 |
| Code & pipeline | What changed in the agent stack before it reaches production? | Phase 2, 3, 4 |
| Agent build posture | What can the investment agent reach or modify? | Phase 2, 3 |
| Runtime posture | Has the live agent drifted from its approved posture? | Phase 5, 6 |
| Action exposure | Which business actions are possible if the agent is misused? | Phase 1, 3, 6 |
| Monitoring & audit | Can security reconstruct posture changes over time? |  |
