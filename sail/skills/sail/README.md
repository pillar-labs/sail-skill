# SAIL Skill — Secure AI Lifecycle Framework

An agent skill that applies **SAIL V2** — [Pillar Security](https://www.pillar.security)'s Secure AI Lifecycle framework — to real systems: gap analyses, security roadmaps, maturity assessments, compliance checklists (ISO/IEC 42001, EU AI Act, OWASP, DASF, AIUC-1), control prioritization, and vendor RFPs. The full 91-risk catalog ships in `references/` and is read on demand, so every answer cites real SAIL IDs and real mappings.

## Usage

Just describe your task — the skill triggers automatically on AI/agent-security intent. Examples:

- *"We're deploying an autonomous support agent built on LangGraph with MCP servers for Zendesk and Postgres — do a security review I can share with our CISO."*
- *"Generate a security control checklist organized by EU AI Act articles."*
- *"Build me an AI security roadmap — we have agents in production but no AI policy yet."*
- *"What does SAIL 1.11 mean and why does autonomy tier matter?"*

In Claude Code, type `/sail` for a guided intake, or use the direct commands (`/sail:assess`, `/sail:roadmap`, `/sail:comply`, `/sail:rfp`) from the plugin install.

## Install

Installation instructions for **all harnesses** — Claude Code, Claude.ai/Desktop/Cowork, OpenAI Codex CLI, Google Antigravity, ChatGPT, pi, opencode — live in the repository README: **[github.com/pillar-security/sail-skill](https://github.com/pillar-security/sail-skill)**.

## About SAIL

SAIL V2 covers the agentic attack surface across three zones (code & pipeline, cloud agents, endpoint agents) and seven lifecycle phases (Policy → Discovery → Posture → Red Teaming → Runtime Controls → Sandbox → Govern). Standards mappings indicate alignment, not automatic compliance.
