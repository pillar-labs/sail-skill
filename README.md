# SAIL Skill — Secure AI Lifecycle by Pillar Security

An agent skill that applies **SAIL V2** — [Pillar Security](https://www.pillar.security)'s Secure AI Lifecycle framework — to real systems. It bundles the full SAIL V2 risk catalog (**91 risks across 7 lifecycle phases**, each with mitigations and standards mappings) as reference files the agent reads on demand, so every answer is grounded in the actual catalog: real SAIL IDs, real mappings, no hallucinated controls.

This repository is also a **Claude Code plugin marketplace** — one-command install for Claude Code, prebuilt archives for everything else.

## What it does

The skill triggers whenever you work on AI/agent security. It can:

- **Assess an agent or system** — full SAIL gap analysis with a component chart and all 91 risks dispositioned
- **Build a security roadmap** and **maturity assessment**
- **Generate compliance checklists** (ISO/IEC 42001, EU AI Act, OWASP LLM/Agentic, DASF, AIUC-1)
- **Prioritize controls** and produce **cross-team alignment artifacts**
- **Create vendor RFPs** (security-vendor coverage questionnaires or platform-vendor security questionnaires)
- **Answer questions about SAIL** — "What does SAIL 5.17 cover?" cites the catalog row

Example prompts (any harness — the skill triggers automatically):

- *"We're deploying an autonomous support agent built on LangGraph with MCP servers for Zendesk and Postgres — do a security review I can share with our CISO."*
- *"Generate a security control checklist organized by EU AI Act articles."*
- *"Build me an AI security roadmap — we have agents in production but no AI policy yet."*
- *"Write the security section of an RFP for a managed agent platform."*

In **Claude Code** you additionally get five entry points:

| Command | What it does |
| :-- | :-- |
| `/sail` | Interactive launcher — guided intake, picks the right workflow |
| `/sail:assess [path]` | Full gap analysis of the agent in the current repo (or `path`) — all 91 risks dispositioned, written to `SAIL-assessment.md` |
| `/sail:roadmap [hints]` | 7-phase security roadmap + per-phase maturity profile |
| `/sail:comply [frameworks]` | Compliance checklist — all five frameworks, or just those named (`eu-ai-act`, `iso-42001`, `owasp`, `dasf`, `aiuc-1`) |
| `/sail:rfp [security\|platform] [vendor]` | Vendor RFP questionnaire — security-vendor coverage by default |

The `/sail:*` commands run immediately with smart defaults — no intake questions.

## Installation

Pick your harness/software in the matrix, then follow its section below.

| Harness / software | Install | Details |
| :-- | :-- | :-- |
| **Claude Code** (CLI / IDE / desktop) | `/plugin marketplace add pillar-security/sail-skill`<br>`/plugin install sail@pillar-security` | [§ Claude Code](#claude-code) |
| **Claude.ai / Desktop / Cowork** | **Customize → Skills** → upload `dist/sail-skill.zip` | [§ Claude.ai / Desktop / Cowork](#claudeai--desktop--cowork) |
| **OpenAI Codex CLI** | `$skill-installer install https://github.com/pillar-security/sail-skill/tree/main/sail/skills/sail` | [§ OpenAI Codex CLI](#openai-codex-cli) |
| **Google Antigravity** (IDE / CLI) | `npx skills add https://github.com/pillar-security/sail-skill/tree/main/sail/skills/sail -a antigravity -g -y` | [§ Google Antigravity](#google-antigravity) |
| **pi** | `pi install https://github.com/pillar-security/sail-skill` | [§ pi](#pi) |
| **ChatGPT** (all plans) | [chatgpt.com/skills](https://chatgpt.com/skills) → **New skill** → upload `dist/sail-skill.zip` | [§ ChatGPT](#chatgpt) |
| **opencode** | `cp -r sail/skills/sail ~/.config/opencode/skills/sail` | [§ opencode](#opencode) |
| **Other SKILL.md-compatible agents** | `npx skills add https://github.com/pillar-security/sail-skill/tree/main/sail/skills/sail -g -y` | [§ Other agents](#other-skillmd-compatible-agents) |

> **Slash commands are Claude Code-only.** Everywhere else you get the full skill — it triggers automatically on AI/agent-security intent, and the guided intake replaces the commands.
>
> 🤖 **AI agents:** reading this README to install the skill yourself? Jump straight to [§ For agents](#for-agents).

### For agents

> **If you are an AI agent reading this README to install the skill, follow these steps — no human input needed.**
>
> 1. **Locate the skill source.** If you are inside a clone of this repo, the skill folder is `sail/skills/sail/` (it contains `SKILL.md` and `references/`). If not, clone the repo first, or unzip `dist/sail-skill.zip` — it extracts to a top-level `sail/` skill folder.
> 2. **If your harness has a skill installer, prefer it:**
>
> ```
> # OpenAI Codex (in-session):
> $skill-installer install https://github.com/pillar-security/sail-skill/tree/main/sail/skills/sail
> # Google Antigravity (and 20+ other agents via Vercel's skills CLI):
> npx skills add https://github.com/pillar-security/sail-skill/tree/main/sail/skills/sail -a antigravity -g -y
> # pi:
> pi install https://github.com/pillar-security/sail-skill
> ```
>
>    **Otherwise, copy the skill folder to your harness's skills directory:**
>
> ```bash
> # Claude Code (or if unsure — most harnesses read this path):
> mkdir -p ~/.claude/skills && cp -r sail/skills/sail ~/.claude/skills/sail
>
> # Shared directory read by Codex, Antigravity, pi, and opencode:
> mkdir -p ~/.agents/skills && cp -r sail/skills/sail ~/.agents/skills/sail
>
> # opencode (native path):
> mkdir -p ~/.config/opencode/skills && cp -r sail/skills/sail ~/.config/opencode/skills/sail
>
> # Project-scoped variants: .claude/skills/ · .agents/skills/ · .opencode/skills/ at the repo root
> ```
>
> 3. **Verify:** the file `<skills-dir>/sail/SKILL.md` exists and its frontmatter `name` is `sail`. The skill loads in the **next session** — tell your user to restart/reload, then test with *"What does SAIL 5.17 cover?"* (expected: cites *Maker-Identity Inheritance in Low-Code Agents*).
> 4. **Claude Code bonus:** for the `/sail:*` commands, tell your user to run `/plugin marketplace add pillar-security/sail-skill` and `/plugin install sail@pillar-security` instead of (or in addition to) the copy — plugin slash commands can't be installed by copying files.
> 5. **Sandboxed harnesses with no filesystem skills dir** (Claude.ai, Cowork, ChatGPT): you cannot self-install; point your user to the matching section below and hand them `dist/sail-skill.zip`.

### Claude Code

```
/plugin marketplace add pillar-security/sail-skill
/plugin install sail@pillar-security
```


Manual copy (no plugin system):

```bash
cp -r sail/skills/sail ~/.claude/skills/sail         # personal, all projects
cp -r sail/skills/sail <project>/.claude/skills/sail # per-project
```

### Claude.ai / Desktop / Cowork

One skills system across claude.ai, the Claude Desktop app, and Cowork — install once, it syncs with your claude.ai account and works everywhere.

1. Enable **Code execution** under **Settings → Capabilities** (once).
2. Go to **Customize → Skills → + → Upload a skill** (in Cowork: **Customize** in the left sidebar → **Skills**).
3. Upload `dist/sail-skill.zip`. (If you use `dist/sail.skill`, rename it to `.zip` first — same archive.)
4. Claude applies the skill automatically; in Cowork you can also type `/` and pick **sail**. Test with *"What does SAIL 5.17 cover?"* — the answer should cite the catalog row. Team/Enterprise admins can share it org-wide (Organization settings → Skills).

### OpenAI Codex CLI

Easiest — inside a Codex session, the built-in installer takes this repo's URL directly:

```
$skill-installer install https://github.com/pillar-security/sail-skill/tree/main/sail/skills/sail
```

Codex picks up new skills automatically (restart if it doesn't appear). Manual alternative — Codex discovers skills in `~/.agents/skills` (personal) or `.agents/skills` (project):

```bash
mkdir -p ~/.agents/skills && cp -r sail/skills/sail ~/.agents/skills/sail
```

Invoke explicitly with `$sail` (type `$` or `/skills` in the TUI) or just ask about AI/agent security and let the description trigger it.

### Google Antigravity

Antigravity (IDE and CLI) reads SKILL.md skills from `.agents/skills` (workspace) and `~/.agents/skills` (user). Google's codelabs endorse the `skills` CLI for installation:

```bash
npx skills add https://github.com/pillar-security/sail-skill/tree/main/sail/skills/sail -a antigravity -g -y
```

(Drop `-g` for a workspace-level install.) On older CLI builds that only read `~/.gemini/antigravity-cli/skills/`, move or copy the installed `sail/` folder there. Manual alternative: `cp -r sail/skills/sail ~/.agents/skills/sail`.

### pi

The repo ships a pi package manifest, so it's a one-liner:

```bash
pi install https://github.com/pillar-security/sail-skill
```

pi also reads `~/.agents/skills`, so a Codex or Antigravity install covers pi too (and vice versa via the shared directory).

### ChatGPT

Skills are available on **all ChatGPT plans** at **[chatgpt.com/skills](https://chatgpt.com/skills)**:

1. Open [chatgpt.com/skills](https://chatgpt.com/skills) → **New skill** → **Upload from your computer**.
2. Upload `dist/sail-skill.zip`.
3. ChatGPT scans the upload and the skill becomes available; workspace admins (Business/Enterprise/Edu) can share it org-wide.

### opencode

```bash
# per-project
mkdir -p <project>/.opencode/skills && cp -r sail/skills/sail <project>/.opencode/skills/sail
# global
mkdir -p ~/.config/opencode/skills && cp -r sail/skills/sail ~/.config/opencode/skills/sail
```

opencode also reads `~/.claude/skills` and `.agents/skills`, so a Claude Code or Codex install covers it too.

### Other SKILL.md-compatible agents

The skill is a plain folder — `SKILL.md` + `references/` — following the open agent-skills format. Vercel's `skills` CLI installs it for 20+ agents:

```bash
npx skills add https://github.com/pillar-security/sail-skill/tree/main/sail/skills/sail -g -y
```

Or copy `sail/skills/sail/` into whatever skills directory your tool documents / unzip `dist/sail-skill.zip` there.

## Usage

**Guided mode** (Claude Code): type `/sail` with no arguments.

- **Inside a repo that contains an agent**, the skill detects it and leads with **"Assess the agent in this repository"** — a full SAIL gap analysis of the code in front of it.
- **Elsewhere**, it runs a short top-down intake: what kind of program this is (governance, agent security, AI transformation, vendor assessment, red-team readiness) → what you have to protect → which deliverable you want.

**Direct mode** (any harness): just describe your task — the skill triggers automatically on AI/agent-security intent.

## What's inside the skill

```
sail/skills/sail/
├── SKILL.md                      # entry point: framework structure + workflows
├── README.md                     # thin pointer to this page
└── references/
    ├── risk-index.md             # all 91 risks in one table (the navigation map)
    ├── framework.md              # exec summary, the 3 zones, the 7 phases, how to use SAIL
    ├── phase-1-policy.md         # full risk catalog, one file per phase:
    ├── …                         #   descriptions, examples, assets affected,
    ├── phase-7-govern.md         #   mitigations, standards mappings
    ├── definitions.md            # Appendix A: component definitions
    └── use-case.md               # Appendix B: worked FinTech example
```

## Repository layout

```
sail-skill/                        marketplace repo root
├── .claude-plugin/
│   └── marketplace.json           marketplace manifest
├── sail/                          the "sail" plugin
│   ├── .claude-plugin/plugin.json plugin manifest
│   ├── commands/                  /sail:assess · /sail:roadmap · /sail:comply · /sail:rfp
│   ├── README.md                  thin plugin readme
│   └── skills/sail/               the skill itself (see above)
├── dist/                          prebuilt zip / .skill archives (rebuild: dist/BUILD.md)
└── package.json                   pi package manifest (`pi install <repo-url>`)
```

After editing the skill, rebuild `dist/sail-skill.zip` / `dist/sail.skill` — see [`dist/BUILD.md`](dist/BUILD.md).

## About SAIL

SAIL (Secure AI Lifecycle) V2 covers the agentic attack surface across three zones (code & pipeline, cloud agents, endpoint agents) and seven lifecycle phases (Policy → Discovery → Posture → Red Teaming → Runtime Controls → Sandbox → Govern), with 91 risks mapped to external standards (ISO/IEC 42001, EU AI Act, OWASP LLM & Agentic Top 10, DASF, AIUC-1). Standards mappings indicate alignment, not automatic compliance. Learn more at [pillar.security](https://www.pillar.security).
