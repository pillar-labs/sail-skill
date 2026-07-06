## **Appendix A \- Definitions of AI and Agentic System Components**

*Definitions are grouped into the same six layers. Within each layer, components are listed in the order most useful for navigation.*

| Core runtime |  |
| :---- | :---- |
| **AI Model** | The trained algorithm at the core of an AI system. Its weights are intellectual property and primary targets for theft, evasion, and poisoning. |
| **AI App (Application)** | The software application that embeds one or more AI models or agents and exposes them to users. Combines traditional application security with model- and agent-specific risks. |
| **AI Agent** | An LLM-based system that autonomously uses tools in a loop to pursue a goal. Every input surface — prompts, documents, tool responses — becomes a potential prompt-injection or goal-hijack vector. |
| **System Prompt** | Initial instructions to a model or agent that guide behavior, persona, and constraints. A common target for leakage, jailbreaking, and manipulation. |
| **Agent Config** | Configuration files, rules files (AGENTS.md, .cursor/rules), system prompts, and policy definitions that govern an agent's behavior and tools. Maliciously crafted configs (Rules File Backdoor pattern) lead to unauthorized actions or insecure output. |
| **Agent Harness** | The production runtime wrapper around an agent — context assembly, tool invocation, guardrails, observability — between the framework and the model. Most exploitable agent failures live in harness gaps, not model weaknesses. |
| **Model Metadata** | Descriptive information about a model — version, training sources, architecture, performance, licensing. Leaked metadata exposes training details and aids attacker reconnaissance. |
| **Model Files** | The digital files storing the trained model — architecture, weights, and dependencies. Primary targets for theft, tampering, and backdoor insertion (e.g., malicious pickles). |
| **Framework** | Libraries and toolkits for building agents — LangChain, LangGraph, LlamaIndex, CrewAI, AutoGen — that orchestrate model calls and tool integration. Frameworks define agent logic but don't enforce runtime policy. |
| **Model Inference Endpoint** | The API endpoint where a deployed model receives requests and returns responses. A primary attack surface for unauthorized access, denial-of-service, and model extraction. |
| **Capabilities** |  |
| **Tool** | An external capability an agent invokes — search, code execution, database queries, APIs. Over-permissive tool catalogs and poisoned descriptions are leading causes of agent-level RCE and exfiltration. |
| **Skill** | A packaged folder of instructions, scripts, and resources an agent loads to extend its capability (Anthropic Agent Skills open spec). A tampered Skill is a supply-chain code injection into the agent. |
| **MCP Server** | A server implementing the Model Context Protocol that exposes tools, resources, and data to agents via JSON-RPC. Authentication, tool-description integrity, and scope enforcement are the primary concerns. |
| **Agentic Platform** | Managed platforms for building and deploying agents — Copilot Studio, Agentforce, Vertex AI Agent Builder, Bedrock AgentCore — including low-code builders and vendor-hosted runtimes. Business users frequently deploy without security review, so platform-level controls drive most of the posture. |
| **Agents Plugins** | Third-party extensions an agent host loads at runtime to add tools or data sources — dominated by MCP servers, plus host-specific plugin formats. Each plugin executes within the agent's scope, making install a supply-chain trust decision. |
| **Agents Marketplaces** | Distribution platforms for pre-built agents, skills, and plugins — Salesforce AgentExchange, Cursor Marketplace, MCP registries, OpenAI plugin and GPT store. Concentrate supply-chain risk: a compromised listing can ship malicious tools into thousands of organizations. |
| **Identity & Authorization** |  |
| **AI Provider Access Credentials** | Tokens, keys, and OAuth grants used to access AI providers and their hosted models. Compromise enables unauthorized use, data exposure, or denial-of-wallet abuse. |
| **Agent Identity** | The non-human identity assigned to an agent — distinct from human users — carrying its authentication material, scopes, and accountable owner. Inherited user sessions and shared credentials create attribution gaps and over-permissioned blast radius. |
| **AI Policy** | The organizational rules governing AI and agentic systems — approval, data handling, identity, autonomy levels, vetting, authorization. The foundation every other component and control is evaluated against. |
| **Data & Context** |  |
| **Dataset / RAG** | Training, fine-tuning, and evaluation data, plus the runtime knowledge bases and vector stores RAG systems retrieve from. Integrity and provenance are critical to prevent poisoning, bias, and leakage. |
| **User Prompt** | Input provided by a user (or upstream agent) to an AI model. Maliciously crafted prompts drive direct prompt injection, jailbreaking, and abuse. |
| **Model Response** | The output a model returns — text, code, images, audio, structured data. Must be safe, accurate, and free of sensitive content, especially when consumed by downstream systems. |
| **Agent Memory / Cache** | Storage agents use to retain past interactions, context, and knowledge — short-term or persistent. A primary target for memory injection, cross-session leakage, and persistent context corruption. |
| **Operations & Containment** |  |
| **App Usage Log** | Logs of user interactions, model inputs and outputs, errors, and operational events from the AI application. Essential for monitoring and incident response, and protected because they often contain sensitive content. |
| **Agent Usage Log** | Agent-specific telemetry capturing not just actions but the reasoning behind them — planning, tool selection, intermediate prompts, token usage. Without per-step traces, agent incident response degenerates to guesswork. |
| **Agent Execution Sandbox** | Isolated runtime where an agent or its tool-generated code executes — Bedrock AgentCore microVMs, Anthropic and OpenAI code-interpreter sandboxes. The last-line containment against tool poisoning and RCE, and itself a target (sandbox escape demonstrated in 2025–26). |
| **Agentic Tasks / Schedule** | Agent invocations triggered automatically on a schedule or external event rather than by a user prompt. Standing credentials and no human in the loop make injected content a primary vector for delayed compromise. |
| **Inter-system** |  |
| **3rd-Party AI Integration** | External AI services, models, APIs, libraries, and data sources incorporated into the organization's systems. Introduces supply-chain, vulnerability, and privacy risks outside the consumer's control. |
| **Agent Communication Protocol (ACP)** | An open protocol for communication between agents across systems — originally from IBM, merging with Google's A2A as of late 2025\. Distinct from MCP (agent-to-tool); introduces transitive trust, prompt-injection propagation, and delegated-identity problems traditional API security doesn't cover. |
