# Wiki Index

Master catalog of all wiki pages. Updated by Claude on every ingest.

Last updated: 2026-05-10
Total pages: 64
Total sources ingested: 9

---

## Sources

| Page | Summary | Ingested | Tags |
|------|---------|----------|------|
| [[LLM Wiki Pattern]] | Pattern document for building LLM-maintained personal knowledge bases; meta-specification for this wiki | 2026-05-10 | #llm #wiki #knowledge-management |
| [[Building Effective AI Agents]] | 30-page Anthropic whitepaper on AI agent architecture patterns and implementation frameworks | 2026-05-10 | #ai-agents #anthropic #architecture |
| [[2026 Agentic Coding Trends Report]] | 18-page Anthropic trends report on how coding agents reshape software engineering in 2026 | 2026-05-10 | #ai-agents #sdlc #trends #coding |
| [[Scaling Agentic Coding Across Your Organization]] | 39-page Anthropic guide on rolling out Claude Code from pilot to full org; rollout strategy, ROI, security | 2026-05-10 | #enterprise #rollout #claude-code #anthropic |
| [[Effective Context Engineering for AI Agents]] | Anthropic Applied AI team article on context engineering — the discipline of curating optimal token sets for capable agents | 2026-05-10 | #context-engineering #ai-agents #long-horizon |
| [[How We Built Our Multi-Agent Research System]] | Anthropic engineering deep-dive on the production orchestrator-worker system behind Claude's Research feature | 2026-05-10 | #multi-agent #production #evaluation #anthropic |
| [[Building Agents with the Claude Agent SDK]] | Anthropic article introducing the Claude Agent SDK (renamed from Claude Code SDK) and agent loop best practices | 2026-05-10 | #sdk #agent-loop #agentic-search #anthropic |
| [[Multi-Agent Coordination Patterns]] | Taxonomy of five coordination patterns (Generator-Verifier, Orchestrator-Subagent, Agent Teams, Message Bus, Shared State) with selection guidance | 2026-05-10 | #multi-agent #architecture #patterns #anthropic |
| [[Building Agents with Skills: Equipping Agents for Specialized Work]] | Anthropic article on Agent Skills — file-based domain expertise packages with three-tier progressive disclosure and four-layer architecture | 2026-05-10 | #skills #domain-expertise #context-engineering #anthropic |

## Entities

| Page | Type | Summary | Source count |
|------|------|---------|--------------|
| [[Vannevar Bush]] | person | Engineer who described the Memex in 1945 — conceptual precursor to this wiki | 1 |
| [[Obsidian]] | product | Markdown-based knowledge management app; recommended viewer for this wiki | 1 |
| [[qmd]] | tool | Local hybrid BM25/vector search engine for markdown files; optional at scale | 1 |
| [[Anthropic]] | company | AI safety company, creator of Claude; author of all agent sources in this wiki | 7 |
| [[Claude Agent SDK]] | product | Anthropic's agent platform (formerly Claude Code SDK); the runtime layer in the four-layer agent architecture | 1 |
| [[Coinbase]] | company | Crypto exchange; case study for enterprise agentic customer support at scale | 1 |
| [[LangGraph]] | framework | Python graph-based agent orchestration framework by LangChain | 1 |
| [[Fountain]] | company | HR-tech platform; case study for hierarchical multi-agent hiring orchestration | 1 |
| [[Brex]] | company | Fintech company; builds domain-specific agents on Claude Code SDK | 1 |
| [[Rakuten]] | company | Global e-commerce enterprise; builds domain-specific agents on Claude Code SDK | 1 |

## Concepts

| Page | Domain | Summary | Source count |
|------|--------|---------|--------------|
| [[LLM Wiki Pattern]] | Knowledge management | LLM incrementally builds a persistent wiki instead of re-deriving from raw sources | 1 |
| [[RAG vs Wiki]] | LLM systems | RAG re-derives on every query; the wiki compiles once and compounds; agentic search preferred over vector search | 2 |
| [[Knowledge Compounding]] | Knowledge management | Each ingest and filed query makes the wiki richer — value grows faster than page count | 1 |
| [[CLAUDE.md Schema]] | LLM tooling | Configuration document that turns the LLM into a disciplined wiki maintainer | 2 |
| [[Memex]] | History of computing | Bush's 1945 personal knowledge device with associative trails — precursor to this wiki | 1 |
| [[AI Agent]] | AI systems | LLM-powered system that autonomously solves open-ended multi-step problems | 2 |
| [[Agent Skills]] | Agent architecture | File-based domain expertise packages; three-tier progressive disclosure; four-layer architecture | 2 |
| [[Single-Agent Architecture]] | Agent architecture | One agent + prompt + tools; recommended starting point for any agentic system | 2 |
| [[Multi-Agent Architecture]] | Agent architecture | Multiple specialized agents coordinating; full pattern taxonomy with selection matrix | 5 |
| [[Hierarchical Multi-Agent System]] | Agent architecture | Orchestrator-Subagent pattern; 5-step production workflow; information bottleneck limitation | 4 |
| [[Collaborative Multi-Agent System]] | Agent architecture | Peer-to-peer agents; coordination emerges from interaction rather than central authority | 1 |
| [[Sequential Workflow]] | Agent architecture | Pipeline — each agent's output feeds the next; for linear stage-gated processes | 1 |
| [[Parallel Workflow]] | Agent architecture | Fan-out/fan-in — ephemeral concurrent agents; distinct from stateful Agent Teams | 3 |
| [[Agent Teams]] | Agent architecture | Persistent stateful workers claim from shared queue; accumulate domain specialization over time | 1 |
| [[Message Bus]] | Agent architecture | Pub/sub event-driven coordination; extensible without rewiring; risk of silent failures | 1 |
| [[Shared State]] | Agent architecture | Agents read/write shared store directly; immediate discovery sharing; no single point of failure | 1 |
| [[Evaluator-Optimizer Workflow]] | Agent architecture | Generator-Verifier pattern; "verifier only as good as its criteria"; end-state evaluation | 4 |
| [[Agentic SDLC]] | Software engineering | Agent-driven SDLC collapsing cycle time from weeks to hours; new abstraction layer | 1 |
| [[Engineer as Orchestrator]] | Software engineering | Engineer role shift from implementer to agent orchestrator and quality evaluator | 1 |
| [[Long-Running Agent]] | Agent architecture | Agents operating for days/weeks building complete systems with periodic human checkpoints | 3 |
| [[Human-in-the-Loop Oversight]] | AI safety | Humans review what matters; agents escalate uncertainty; agentic quality control at scale | 1 |
| [[Dual-Use AI Risk]] | AI safety | Same agent capabilities help defenders and attackers — security-first architecture required | 1 |
| [[Agentic Coding Rollout]] | Enterprise adoption | Phased strategy: power user pilot → champions → org-wide; builds expertise organically | 1 |
| [[CLAUDE.md at Scale]] | Enterprise tooling | Shared CLAUDE.md checked into git becomes team knowledge layer aligning all Claude sessions | 1 |
| [[Test-Driven Agent Development]] | Software engineering | TDD as guardrails for agents — write tests first, implement incrementally, avoid unbounded tasks | 1 |
| [[Agentic ROI Measurement]] | Enterprise metrics | Measuring agent impact beyond LOC: throughput, time-to-completion, satisfaction, onboarding speed | 1 |
| [[Context Engineering]] | LLM engineering | Curating optimal token set at inference; includes progressive disclosure pattern for skills libraries | 2 |
| [[Context Rot]] | LLM architecture | Universal phenomenon: recall degrades as context grows due to transformer n² attention constraints | 1 |
| [[Just-in-Time Context Retrieval]] | Agent architecture | Agents load data at runtime via lightweight identifiers rather than pre-loading everything upfront | 2 |
| [[Context Compaction]] | Agent architecture | Summarize conversation near context limit, reinitialize with summary; built into Claude Agent SDK | 2 |
| [[Self-Improving Agent]] | Agent architecture | Agent diagnoses its own failures and generates prompt/tool improvements for future runs | 1 |
| [[LLM-as-Judge]] | Evaluation | LLM scores agent outputs against rubrics at scale; best offline/batch, not per-output in-loop | 2 |
| [[Durable Agent Execution]] | Production systems | State management, error handling, resumption, and rainbow deployments for production agents | 1 |
| [[Agentic Search]] | Agent architecture | File-system exploration via bash/grep as primary retrieval; preferred over semantic/vector search | 1 |

## Syntheses

| Page | Original prompt | Filed |
|------|----------------|-------|
| [[Best Practices for Context Engineering]] | What are the best practices for context engineering? | 2026-05-10 |
