# **Executive Summary**

## **SAIL V1: Adoption and Approach**

The Secure AI Lifecycle (SAIL) Framework is a structured way to build, deploy, and operate AI systems with security, safety, and governance controls embedded end to end. SAIL v1 launched in July 2025\. Within ten months it recorded over 50,000 downloads and was adopted by security teams, CISOs, and AI practitioners across enterprises, startups, and government organizations worldwide. They shared with us how they implemented and translated SAIL into their working roadmaps.

The framework was born through extensive collaboration with AI and cybersecurity leaders, from startups to Fortune 500 enterprises, who identified a common gap: the need for a unifying framework that could translate high-level security principles into practical, actionable guidance across the entire AI lifecycle.

The SAIL Framework addresses this need by embracing a process-oriented approach that both harmonizes with and enhances the valuable contributions of existing standards. Its unique strength lies in embedding security actions into each phase of the agents lifecycle, complementing the strategic risk management of NIST AI RMF, the management system structures of ISO 42001, the vulnerability identification of OWASP, and the component-level risk analysis of frameworks like DASF. 

By synthesizing these diverse perspectives through a lifecycle lens, SAIL provides an operational guide that empowers organizations to transform security knowledge into actionable practices.

**Why V2, and Why Now**

Over the past year, AI crossed a line. It evolved from a tool employees use and into a worker that operates alongside them. A new kind of workforce already operates inside most organizations today.

These digital workers hold access to your most sensitive systems, reason through problems, chain tool calls, and take real actions at machine speed. They operate inside your CI/CD, get built on agentic and SaaS platforms, and run on every developer machine in your organization.

The security stack we built over the last 20 years assumed humans make decisions and software executes them. AI agents break that assumption. The dangerous failures live in the reasoning layer, where no firewall or EDR can see. Static policy can't govern a non-deterministic actor. And the shadow agents your teams already shipped sit outside the perimeter you're protecting.

Agentic security is not only access control; it is also action control. It's understanding the intent and broader context that leads to those actions at machine speed. Authorize every action, not just every session. Observe agent intent, plans, tool calls, and policy decisions continuously. Discover the agents nobody told you about.

SAIL V2 addresses this next phase of enterprise cybersecurity, helping AI development, MLOps, LLMOps, AgentOps, security, and governance teams secure the agentic workforce, from policy through runtime enforcement and continuous governance.

In practice, teams use SAIL to: 

* Build an AI security roadmap and benchmark maturity, phase by phase   
* Turn the standards mappings into compliance checklists (EU AI Act, ISO 42001, OWASP, DASF, AIUC-1)   
* Prioritize controls by zone, asset, and agent autonomy tier   
* Drive vendor assessments and RFPs with agentic-literate questions   
* Give security, legal, compliance, and engineering one shared vocabulary 

# **Chapter 1: The Shape of the Agentic Attack Surface**

We are standing at the precipice of a fundamental inversion in cybersecurity. AI agents are one of the fastest-growing attack surfaces in the enterprise. We enter the era of highly capable, autonomous AI, that is also the least visible and least governed. They plan, call tools, hold memory, and chain actions across systems that were never built to negotiate with a probabilistic caller. Data itself has become executable: prompts, configurations, and datasets now function as active instructions that command software behavior, where a single malicious or misinterpreted input can trigger operations that are difficult or impossible to roll back. The limitation is how rapidly human defenders can verify, triage, and patch agentic systems before someone can exploit them

The chapter starts with the force driving that surface, the agentic workforce, then maps the zones where risk concentrates.

---

**1.1 The Agentic Workforce**

Before diving into specific risks and attack vectors, it is worth understanding the shift that created them. 

The AI system itself has become an actor, not just a component. An autonomous agent monitors a CRM pipeline, drafts follow-up emails, queries an internal database, calls an external API, and triggers a downstream workflow without a human approving each step. That is the agentic workforce: autonomous actors that hold access, chain reasoning, invoke tools, and take actions with real consequences on production systems.

Security architectures built over the last two decades assumed humans decide and software executes deterministically. 

Agents break that assumption in three ways: 

**Hidden logic, zero visibility:** Many agent actions depend on internal planning and tool-selection logic: which tools to call, what data to access, and in what order. Most existing controls see actions and logs, not the full intent, plan, and tool-decision context. An attacker who hijacks that reasoning produces behavior that looks legitimate at the action layer while a hidden payload executes underneath. Securing agents means inspecting reasoning in real time. Few enterprise controls do this consistently today.

**Full speed, no safety net:** When a human acts suspiciously, the security stack gets multiple chances to intervene. When an agent goes rogue, nothing pauses it mid-execution or routes a pending action for approval. Static policy applies deterministic governance to a non-deterministic actor. An agent can stay within its permitted access boundaries while composing a chain of actions that becomes collectively harmful. Action control, authorizing every action and not only every session, replaces access control as the primary enforcement layer.

**Too fast, too many:**  High-volume agents can execute tool calls at machine speed across many concurrent sessions. Many enterprises already maintain large populations of non-human identities relative to human users.And the problem compounds through shadow agents: developers spin up agents in notebooks, business users build them in low-code platforms, coding assistants connect to community MCP servers outside corporate infrastructure. A security program that governs only the agents it knows about covers a fraction of the actual attack surface.

The three problems above frame the attack surface. The next section maps where it lives.

---

## **1.2 Where the Surface Lives \- Establishing a Common Understanding of Agentic Risks** 

Agentic assets now exist everywhere an enterprise builds, buys, or runs software. Three zones dominate, each with a different risk signature, different owners, and different controls. Several recurring concerns \- agent identity, multi-agent orchestration, memory and retrieval, and human-in-the-loop interfaces \- traverse all three zones. They appear as patterns across the SAIL catalog and have dedicated mitigation guidance in the relevant phases.

Zones are drawn by where the risk materializes, not by where the agent process runs. A coding assistant executes on a developer's laptop with the developer's credentials, so the endpoint is governed in Zone 3 — while the repository artifacts it reads and writes are governed in Zone 1\. Agents in Zone 3 read and write Zone 1 artifacts; agents in Zone 2 consume them at runtime. Controls must span these handoffs, not just the zones

SAIL V2 addresses risks across all three zones and across every phase of the agent lifecycle, from design through deployment, runtime, and incident response.

### **Zone 1 — AI Assets in Code & Pipeline**

**Definition.** Where AI components \-  models, datasets, prompts, agent configurations, and AI-generated code or IaC \-  exist as artifacts in source control, build, and deployment pipelines. The risk surface is static: no agent is executing here. The danger lives in what the artifacts will do once another agent or runtime picks them up. A poisoned artifact committed once gets consumed by every downstream build, every deployed agent, every dependent application.

**Representative tools and products.** GitHub, GitLab, Bitbucket, Azure DevOps Repos; GitHub Actions, GitLab CI, Hugging Face, AWS SageMaker Registry, and others. 

| Surface | Representative Risks |
| :---- | :---- |
| **Model files & registries** | Malicious pickles and safetensors, model backdoors, poisoned public datasets quietly ingested into training and fine-tuning pipelines, unverified Hugging Face pulls in CI, missing model provenance and SBOM. |
| **AI scanners and tooling in CI** | Compromised tool binaries stealing runner credentials, force-pushed action tags pointing to malicious versions, vendor service-account compromise propagating downstream, typosquatted update domains. |
| **Server-side coding and automation agents** | Autonomous merge or deploy without review, over-scoped service-account permissions, runtime tool and MCP poisoning, secret exfiltration from runner memory. |
| **Agent configs, rules files & system prompts as code** | Cursor rules, AGENTS.md, agent.yaml; hidden-Unicode prompt injection; the Rules File Backdoor pattern published by Pillar Security in March 2025\. |
| **Skills, plugins, CLIs and MCP integrations** | Unvetted installs from public marketplaces, description poisoning in tool and skill manifests, hidden instructions in agent CLI configs, malicious tool-definition smuggling, project-scoped installs without review. |

***Primary failure mode.** A poisoned artifact propagates from one commit into every build, every deployed agent, and every downstream consumer — without any agent ever needing to be compromised at runtime.*

### **Zone 2 — Cloud Agents**

**Definition.** Where agents execute inside managed productivity, CRM, ITSM, Agentic platforms,  and cloud platforms, running under platform-issued identities, embedded into applications most security teams don't instrument the same way as infrastructure. Frequently built and deployed by non-security users: business analysts in low-code builders, app teams in managed cloud agent services. Risk concentrates because delegated authority outpaces visibility — agents act on behalf of users with credentials, scopes, and trust boundaries the user never explicitly granted.

**Representative tools and products.** Microsoft Copilot Studio, Salesforce Agentforce, Anthropic Managed Agents, Microsoft 365 Copilot, AWS Bedrock AgentCore, Azure AI Foundry, Google Gemini

| Surface | Representative Risks |
| :---- | :---- |
| **Managed agent platforms (low-code and vendor-hosted)** | Maker-identity inheritance giving consumers the builder's access, unsanctioned agent sprawl across teams and tenants, default-permissive tool and connector scopes, missing audit trail across consumers, zero-click exfiltration chains. |
| **Internet-exposed agents** | Public endpoints reachable from outside the corporate boundary, indirect prompt injection via shared documents and tickets, customer-facing agents abused for jailbreaks, sandbox escape via runtime DNS or filesystem.  |
| **Agent access to sensitive data (read & write)** | Cross-workspace and cross-tenant data exfiltration, vector database leakage, agent memory leakage across sessions, write actions invoked without action-level authorization. |
| **Inter-agent protocols**  | Unauthenticated handoffs between agents, full-tool-catalog delegation, instruction smuggling across agent chains, cross-agent identity impersonation, session smuggling between orchestrated agents. |
| **External and internal agent identities** | Maker-identity inheritance in low-code platforms, over-scoped OAuth and connector grants, missing workload identities for service-side agents, excessive IAM role assumption in cloud runtimes, identity impersonation across A2A handoffs. |

***Primary failure mode.** Agents operate with delegated authority that exceeds the delegating user's awareness, and compromise propagates through SaaS data planes that security teams don't instrument the same way as infrastructure.*

### **Zone 3 — Endpoint Agents**

**Definition.** Where AI agents execute on developer workstations, employee laptops, and personal devices running autonomous loops under the user's local OS identity, with the user's local secrets, browser cookies, and credential stores in reach. Every file, repo, page, and tool description these agents read is an indirect prompt-injection vector and with local credentials in reach, injection is a direct escalation path. The most under-governed zone: EDR, DLP, and IAM were built to model a human operator, not an agent acting on their behalf at machine speed. Coding assistants, agentic browsers, computer-use agents, and desktop MCP bridges all live here. What good looks like: endpoint agents running under scoped, time-bound non-human identities with per-action approval, allowlisted tools, and full telemetry.

**Representative tools and products:** Cursor, Claude Code, DeepSeek-Coder, Kimi K2, Qwen-Coder; Perplexity Comet, OpenAI Codex, Opencode and others

| Surface | Representative Risks |
| :---- | :---- |
| **Endpoint coding assistants** | Sandbox bypass via skip-permissions and YOLO modes, wildcard tool allowlists, prompt injection via repo files and MCP descriptions, uncontrolled outbound network access, credential exfiltration from local config, RCE through crafted repo files. |
| **Agents in user-managed cloud workloads** | Cloud development environments (Codespaces, dev VMs, cloud shells) running agents with full user IAM, sandbox escape via runtime DNS or filesystem, long-lived keys in agent config, missing audit telemetry on agent tool invocations. |
| **Local & shadow AI runtimes** | Unmanaged local model runners and shadow CLIs bypassing corporate proxies, personal AI agents brought into corporate environments, no SIEM telemetry, uncontrolled traffic to unapproved destinations. |
| **OS-level and computer-use agents** | Operation via accessibility APIs and screen capture outside the browser sandbox, untrusted local tool integrations and extensions, credential harvesting from local secret stores, data exfiltration via uncontrolled outbound access, local execution outside EDR's behavioral model, agentic browsers, extensions, and desktop-agent integrations inheriting authenticated SaaS/browser sessions via the cookie jar and acting as indirect-injection conduits from any page, email, or document the user opens. |

***Primary failure mode.*** The endpoint has become an autonomous execution environment, but EDR, DLP, and IAM were built for a human operator. An agent compromised here moves at machine speed, harvests local secrets, mutates Zone 1 artifacts, and chains into Zone 2 SaaS authority,  all under the user's identity, looking legitimate the entire wa*y*. The counter is dedicated, time-bound non-human identities, per-action authorization, and tamper-evident logging, and treating the agent itself as a potential adversary that may attempt to erase its trail, expand its own permissions, or solicit access from other agents.

## **Chapter 2: The SAIL (Secure AI Lifecycle) Framework – Navigating the Waters**

The AI agent lifecycle maps how agents move from policy and discovery through build, test, deployment, and continuous governance. It shares its skeleton with the traditional Software Development Lifecycle, but autonomy changes what's at stake: agents reason, hold memory, invoke tools, and chain actions on their own. That capability is also the attack surface  it's what lets a single prompt injection escalate into credential abuse or cross-zone movement. Because the agent can act at every phase, security has to be present at every phase too. SAIL provides the security operating model for the agent loop itself.

The two lifecycles interweave but don't replace one another. The software loop \-  application code, infrastructure, CI/CD pipeline hardening \-  has its own established disciplines (AppSec, DevSecOps, supply-chain security). SAIL covers the agent loop: the agentic-specific risks and controls that AppSec wasn't built to handle. 

Chapter 1 mapped *where* agentic risk lives — three zones. The seven SAIL phases map *when* security work happens. The framework is structured around their intersection. The phases are AI Policy (Plan), AI Discovery (Code/No Code), Agentic Posture Management (Build), Agentic Red Teaming (Test), Runtime Controls (Deploy), Sandbox (Operate), and Govern (Monitor & Retire).

**2.1 Overview of the SAIL Phases**

**AI Policy (Plan).** This foundational phase establishes the AI and agentic security policy framework, aligned with business objectives, regulatory requirements, and overall AI governance. It covers identifying agent use cases, assessing compliance needs (EU AI Act, NIST AI RMF, ISO 42001), defining risk-based protection levels, and setting up secure experimentation environments. The phase incorporates dedicated threat modeling for agent-specific risks — reasoning manipulation, tool misuse, privilege escalation through delegation chains, and cascading failures across multi-agent workflows — and formalizes how new models, tools, MCP servers, and data sources get vetted before they enter the environment.

**AI Discovery (Code/No Code):** The Discovery phase focuses on identifying, cataloging, and vetting every agent and AI asset in the environment, whether built in-house, embedded in SaaS platforms, or spun up by developers and business users without security involvement. The inventory covers models, datasets, agent configurations, tool connections, MCP servers, and no-code agent builders. Shadow agents make this phase especially critical: you cannot secure what you cannot see, and the gap between known agents and actual agents is where risk concentrates.

**Agentic Posture (Build).** The Build phase is dedicated to performing a deep posture analysis of the agents and AI assets identified in Discovery. It involves understanding, mapping, and graphing the agent landscape \- identity, tool and connector scopes, memory and RAG dependencies, A2A and MCP edges, and the platforms underneath \- to establish a clear picture of the system's security posture and the attack paths that span it. Using the protection requirements from the Plan phase, organizations prioritize security controls per asset based on risk and document residual exposure.

**Agentic Red Teaming (Test).** In the Test phase, agents undergo rigorous security assessments that simulate adversarial behaviors to uncover vulnerabilities, weaknesses, and risks. Unlike traditional AI testing focused on functionality and performance, agentic red teaming goes beyond standard validation to include goal hijacking, prompt injection across tool chains, privilege escalation through delegation, memory poisoning, inter-agent instruction smuggling, and cascading failure propagation. The depth and intensity of testing align with the protection requirements of the supported business processes and cover all three zones from Chapter 1: code and pipeline, SaaS and cloud, and endpoints.

**Runtime Controls (Deploy).** The Deploy phase ensures that agents are released into production with runtime controls and active enforcement in place. These controls validate every action against policy, enforce least-privilege tool access per invocation, and provide the ability to pause, redirect, or terminate an agent mid-execution. Static allow/deny lists are a starting point, not the finish line: action-level authorization, input/output filtering, and behavioral baselines must be active before an agent goes live.

**Sandbox (Operate).** During the Operate phase, agents run within isolated execution environments that limit blast radius. Sandboxing and zero-trust strategies separate agents from critical infrastructure and sensitive data while keeping them productive. For coding agents, MCP servers, and other endpoint-based agents, the sandbox defines what file systems, networks, APIs, and credentials the agent can reach. Isolation applies to agent-to-agent communication as well: a compromised agent should not be able to pivot into another's scope.

**Govern (Monitor & Retire).** This phase continuously monitors agent activity in production and governs the end-of-life of every agent. On the monitor side, it collects telemetry, maintains audit trails, and tracks reasoning chains, tool invocations, data access patterns, and inter-agent communication for anomalies that signal drift, misuse, or compromise — validating behavior against the AI policy and feeding signals back into the rest of the lifecycle. On the retire side, it owns decommissioning: revoking the non-human identity, rotating credentials, wiping or archiving memory and cache per data-retention policy, removing scheduled triggers, and capturing an end-of-life audit record.

**2.2 Components of the Agentic Stack**

At its core, SAIL V2 is structured around seven lifecycle phases, addressing more than 90 mapped risks across the AI and agentic system lifecycle. These help define the key capabilities needed to build a robust agentic security roadmap.

To effectively understand and address these risks across the SAIL phases, it's essential to recognize the core components that form the building blocks of AI and agentic systems, as each presents its own potential attack surface. The list below outlines these fundamental AI and agentic assets, which are central to the risk discussions and 'Assets Affected' field within each detailed phase description that follows. 

*The 29 components below are grouped into six layers. Each layer concentrates a distinct class of agentic risk and is defined individually in* Appendix A.

* **Core runtime.** AI Model · AI App · AI Agent · System Prompt · Agent Config · Agent Harness · Framework · Model Files · Model Metadata · Model Inference Endpoint  
* **Capabilities.** Tool · Skill · MCP Server · Agents Plugins · Agents Marketplaces · Agentic Platform  
* **Identity & Authorization.** Agent Identity · AI Provider Access Credentials · AI Policy  
* **Data & Context.** Dataset · User Prompt · Model Response · Agent Memory / Cache  
* **Operations & Containment.** App Usage Log · Agent Usage Log · Agentic Tasks / Schedule · Agent Execution Sandbox  
* **Inter-system.** Agent Communication Protocol (ACP) · 3rd-Party AI Integration

## **2.3 How to Use SAIL**

SAIL is a working tool, not a reading exercise. Whether you are writing your first AI policy or already operating hundreds of agents, you can use SAIL for:

* **Build an AI security roadmap.** Walk the seven phases in order and assess coverage at each. The gaps, sequenced by phase, become the roadmap.  
* **Assess AI security maturity.** Score each relevant risk \- unaddressed, partially mitigated, or mitigated with evidence \- to produce a per-phase maturity profile you can trend over time. The three zones in Chapter 1 define which risks apply to your environment.  
* **Generate a compliance checklist.** Filter the catalog by the framework you are accountable to (ISO/IEC 42001, EU AI Act, OWASP, DASF, AIUC-1) to get a control checklist in that framework’s language. Mappings indicate alignment, not automatic compliance.  
* **Prioritize controls.** Focus on risks that intersect your zones, your assets, and your weakest phase. Autonomy tiers (SAIL 1.11) set control intensity per agent.  
* **Run vendor assessments and RFPs.** Convert risk rows into vendor questions \- SAIL 5.17 becomes “How is the maker’s identity decoupled from the runtime identity?”  
* **Align security, legal, compliance, and engineering.** SAIL IDs give all four teams one shared vocabulary for design reviews, risk registers, and exception requests.

### **Chapter 3: SAIL V2 \-  Complete Risk Catalog**

This chapter contains the full SAIL V2 risk catalog \-  91 risks across the seven lifecycle phases.

Each risk row follows a consistent structure:

* **ID**: a stable identifier (SAIL X.Y) used for cross-reference within and outside the framework.  
* **Risk** \- the failure mode in two to five words.  
* **Description**: one to two sentences defining the risk and the conditions under which it manifests.  
* **Example**: a concrete scenario illustrating how the risk plays out in practice.  
* **Assets Affected**: the components from the SAIL V2 taxonomy (Section 2.2, defined in Appendix A) most directly involved.  
* **Mitigations**: the controls and practices that reduce the risk.  
* **Standards Mapping**: citations to the most authoritative external frameworks that address the same failure mode.

Standards mappings cover six frameworks:

* ISO/IEC 42001:2023  
* NIST RMF  
* OWASP Top 10 for Agentic Applications (2026)   
* OWASP Top 10 for LLM Applications (2025)  
* EU AI Act (Regulation 2024/1689)   
* Databricks AI Security Framework (DASF) v3.0   
* AIUC-1 

Where a framework doesn't directly cover a SAIL risk, that framework is omitted from the row rather than forced.

To keep the catalog usable, each risk cites at most five mappings \-  the strongest fit per framework. 

Note on the EU AI Act: article mappings assume the AI system qualifies as high-risk under Art.

---
