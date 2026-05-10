# Activity Log

Append-only chronological record of all wiki operations.

Parse last 10 entries: `grep "^## \[" wiki/log.md | tail -10`
Parse all ingests:     `grep "^## \[.*ingest" wiki/log.md`
Parse all queries:     `grep "^## \[.*query" wiki/log.md`
Parse all lints:       `grep "^## \[.*lint" wiki/log.md`

---

## [2026-05-10] ingest | The Linux Command Line *(removed)*

- **Source**: linuxcommands.pdf
- **Pages created**: [[The Linux Command Line]], [[William E. Shotts Jr.]], [[LinuxCommand.org]], [[No Starch Press]], [[Unix Philosophy]], [[Everything Is a File]], [[Bash Scripting]], [[Readline Keyboard Shortcuts]], [[I/O Redirection]], [[Regular Expressions]], [[Shell Expansion]], [[Text Processing Pipeline]]
- **Pages updated**: [[index.md]]
- **Contradictions flagged**: no
- **Notes**: 538-page book, 4 parts. *(All pages from this ingest removed — see removal entry below.)*

## [2026-05-10] removal | The Linux Command Line

- **Source removed**: linuxcommands.pdf
- **Pages deleted**: [[The Linux Command Line]], [[William E. Shotts Jr.]], [[LinuxCommand.org]], [[No Starch Press]], [[Unix Philosophy]], [[Everything Is a File]], [[Bash Scripting]], [[Readline Keyboard Shortcuts]], [[I/O Redirection]], [[Regular Expressions]], [[Shell Expansion]], [[Text Processing Pipeline]]
- **Pages updated**: [[Sequential Workflow]] (Unix Philosophy cross-ref removed), [[Knowledge Compounding]] (example updated), [[Building Effective AI Agents]] (Unix Philosophy link removed), [[index.md]]
- **Notes**: User requested removal of all content derived from linuxcommands.pdf. Raw file left intact in raw/. 3 surviving pages had broken cross-references cleaned up.

## [2026-05-10] ingest | LLM Wiki Pattern

- **Source**: wiki.md
- **Pages created**: [[LLM Wiki Pattern]] (source), [[LLM Wiki Pattern]] (concept), [[RAG vs Wiki]], [[Knowledge Compounding]], [[CLAUDE.md Schema]], [[Memex]], [[Vannevar Bush]], [[Obsidian]], [[qmd]]
- **Pages updated**: [[index.md]]
- **Contradictions flagged**: no
- **Notes**: Meta-source — describes the system this wiki implements. Self-referential: this wiki is an instance of the pattern. Open questions: schema evolution triggers, multi-user conflict handling, index.md scale ceiling.

## [2026-05-10] ingest | Building Effective AI Agents

- **Source**: buildingagents.pdf
- **Pages created**: [[Building Effective AI Agents]], [[AI Agent]], [[Agent Skills]], [[Single-Agent Architecture]], [[Multi-Agent Architecture]], [[Hierarchical Multi-Agent System]], [[Collaborative Multi-Agent System]], [[Sequential Workflow]], [[Parallel Workflow]], [[Evaluator-Optimizer Workflow]], [[Anthropic]], [[Coinbase]], [[LangGraph]]
- **Pages updated**: [[index.md]]
- **Contradictions flagged**: no
- **Notes**: 30-page Anthropic whitepaper. Key cross-links: [[Unix Philosophy]] ↔ [[Sequential Workflow]] (composability analogy); [[LLM Wiki Pattern]] ↔ [[Single-Agent Architecture]] (this wiki is a single-agent instance); [[CLAUDE.md Schema]] ↔ [[Agent Skills]] (schema as a skill package). Open question: evaluator loop termination criteria not specified in source.

## [2026-05-10] ingest | 2026 Agentic Coding Trends Report

- **Source**: agentictrends.pdf
- **Pages created**: [[2026 Agentic Coding Trends Report]], [[Agentic SDLC]], [[Engineer as Orchestrator]], [[Long-Running Agent]], [[Human-in-the-Loop Oversight]], [[Dual-Use AI Risk]], [[Fountain]]
- **Pages updated**: [[index.md]], [[Anthropic]] (second source added)
- **Contradictions flagged**: no
- **Notes**: 18-page Anthropic trends report. Complementary to [[Building Effective AI Agents]] — patterns + trajectory. Key new cross-links: [[LLM Wiki Pattern]] ↔ [[Agentic SDLC]] (wiki maintenance as agentic SDLC for knowledge); [[CLAUDE.md Schema]] ↔ [[Agent Skills]] reinforced; [[Human-in-the-Loop Oversight]] ↔ INGEST step 2 (discuss takeaways = checkpoint). Strong emphasis on human collaboration over full autonomy — slightly more cautious than [[Building Effective AI Agents]] framing.

## [2026-05-10] ingest | Scaling Agentic Coding Across Your Organization

- **Source**: scalingagents.pdf
- **Pages created**: [[Scaling Agentic Coding Across Your Organization]], [[Agentic Coding Rollout]], [[CLAUDE.md at Scale]], [[Test-Driven Agent Development]], [[Agentic ROI Measurement]], [[Brex]], [[Rakuten]]
- **Pages updated**: [[CLAUDE.md Schema]], [[index.md]]
- **Contradictions flagged**: no
- **Notes**: 39-page Anthropic guide on organizational rollout of Claude Code — from pilot to full-org. Key cross-links: [[CLAUDE.md Schema]] ↔ [[CLAUDE.md at Scale]] (shared schema as team knowledge layer); [[Test-Driven Agent Development]] ↔ [[Evaluator-Optimizer Workflow]] (TDD as evaluator guardrail); all three Anthropic sources align on start-simple, human oversight, security-first. Open questions: CLAUDE.md drift management across teams, MCP audit cadence, satisfaction measurement methodology.

## [2026-05-10] ingest | Effective Context Engineering for AI Agents

- **Source**: https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- **Pages created**: [[Effective Context Engineering for AI Agents]], [[Context Engineering]], [[Context Rot]], [[Just-in-Time Context Retrieval]], [[Context Compaction]]
- **Pages updated**: [[Long-Running Agent]] (three long-horizon techniques added), [[Multi-Agent Architecture]] (context isolation rationale added), [[index.md]]
- **Contradictions flagged**: no
- **Notes**: Anthropic Applied AI team article. Introduces context engineering as prompt engineering's successor — covering the full token set at every inference step. Key new cross-links: [[Context Rot]] explains why [[Just-in-Time Context Retrieval]] and [[Context Compaction]] exist (transformer n² attention constraint); [[CLAUDE.md Schema]] is explicitly validated as upfront-context pattern; this wiki's QUERY workflow is a working instance of JIT retrieval. Strong cross-wiki resonance: Session Start Protocol, QUERY operation, and log.md are all described by the article's patterns without having been built with them in mind.

## [2026-05-10] ingest | How We Built Our Multi-Agent Research System

- **Source**: https://www.anthropic.com/engineering/multi-agent-research-system
- **Pages created**: [[How We Built Our Multi-Agent Research System]], [[Self-Improving Agent]], [[LLM-as-Judge]], [[Durable Agent Execution]]
- **Pages updated**: [[Hierarchical Multi-Agent System]] (5-step production workflow added), [[Evaluator-Optimizer Workflow]] (end-state evaluation + LLM-as-Judge added), [[Parallel Workflow]] (90% time reduction stat added), [[index.md]]
- **Contradictions flagged**: no
- **Notes**: Anthropic engineering article on the production system behind Claude's Research feature. Bridges the whitepaper patterns in [[Building Effective AI Agents]] with real deployment engineering. Key additions: 90.2% performance gain now attributed to specific model combination (Opus 4 lead + Sonnet 4 subagents); 15x token cost makes task-value justification a real design constraint; [[Self-Improving Agent]] is a genuinely new concept (40% task completion improvement from tool-testing agent); [[Durable Agent Execution]] fills the production engineering gap not covered by any prior source. Strong cross-links: [[Context Compaction]] ↔ [[Durable Agent Execution]] (filesystem outputs as state externalization); [[LLM-as-Judge]] ↔ [[Evaluator-Optimizer Workflow]] (out-of-loop vs. in-loop evaluation).

## [2026-05-10] ingest | Building Agents with the Claude Agent SDK

- **Source**: https://claude.com/blog/building-agents-with-the-claude-agent-sdk
- **Pages created**: [[Building Agents with the Claude Agent SDK]], [[Claude Agent SDK]], [[Agentic Search]]
- **Pages updated**: [[RAG vs Wiki]] (agentic search vs. semantic search comparison added), [[Just-in-Time Context Retrieval]] (agentic search as concrete implementation), [[LLM-as-Judge]] (robustness caveat + pipeline vs. in-loop distinction), [[Context Compaction]] (SDK built-in compact feature noted), [[index.md]]
- **Contradictions flagged**: yes — [[LLM-as-Judge]]: [[How We Built Our Multi-Agent Research System]] endorses it for pipeline evaluation; this article calls it "generally not a very robust method" for in-loop use. Flagged with context in [[LLM-as-Judge]] page — the two positions are reconcilable by use-case but the tension is real.
- **Notes**: September 2025 article announcing the Claude Code SDK → Claude Agent SDK renaming. Most valuable additions: [[Agentic Search]] as a named, explicitly-recommended pattern (file-system exploration over vector search); [[Claude Agent SDK]] entity page; honest LLM-as-Judge caveat that creates productive tension with the Research system article. The "folder structure as context engineering" observation is a concrete design principle not found in prior sources. Strong consistency with the wiki's own architecture throughout.

## [2026-05-10] ingest | Multi-Agent Coordination Patterns

- **Source**: https://claude.com/blog/multi-agent-coordination-patterns
- **Pages created**: [[Multi-Agent Coordination Patterns]], [[Agent Teams]], [[Message Bus]], [[Shared State]]
- **Pages updated**: [[Multi-Agent Architecture]] (full 8-pattern table + selection matrix), [[Evaluator-Optimizer Workflow]] (Generator-Verifier alias + criteria limitation), [[Hierarchical Multi-Agent System]] (Orchestrator-Subagent alias + information bottleneck weakness), [[Parallel Workflow]] (distinction from Agent Teams), [[index.md]]
- **Contradictions flagged**: no
- **Notes**: April 2026 article completing the multi-agent pattern taxonomy. Two genuinely new concepts: [[Message Bus]] (pub/sub, event-driven, extensible) and [[Shared State]] (no coordinator, immediate discovery sharing, termination design is the key challenge). [[Agent Teams]] was implicitly present in [[Parallel Workflow]] but is now named and properly distinguished — persistent stateful workers vs. ephemeral fan-out. Key design principle: "combine patterns" in production — this resolves the either/or framing in earlier sources. [[Multi-Agent Architecture]] is now the most cross-referenced page in the wiki (5 sources).

## [2026-05-10] ingest | Building Agents with Skills: Equipping Agents for Specialized Work

- **Source**: https://claude.com/blog/building-agents-with-skills-equipping-agents-for-specialized-work
- **Pages created**: [[Building Agents with Skills: Equipping Agents for Specialized Work]]
- **Pages updated**: [[Agent Skills]] (major rewrite — file structure, three-tier progressive disclosure, script packaging, ecosystem types, four-layer architecture, open standard), [[Context Engineering]] (progressive disclosure as a named pattern), [[index.md]]
- **Contradictions flagged**: no
- **Notes**: January 2026 article that transforms [[Agent Skills]] from a one-paragraph mention into a fully developed pattern. Most valuable additions: three-tier progressive disclosure (metadata ~50 tokens → full skill ~500 tokens → reference docs 2,000+ tokens) as a concrete context engineering technique; skills packaging executable scripts that agents can modify; the complete four-layer architecture (Loop + Runtime + MCP + Skills); skills as an open standard. Strong connection to existing wiki: [[CLAUDE.md Schema]] is already a skills instance; the wiki's QUERY operation already implements progressive disclosure (index.md metadata → candidate pages on demand). [[Agent Skills]] is now the most substantially updated page in the wiki.

## [2026-05-10] query | Best practices for context engineering

- **Source**: what are the best practices for context engineering?
- **Pages created**: [[Best Practices for Context Engineering]] (synthesis)
- **Pages updated**: [[index.md]] (Syntheses section populated for the first time)
- **Contradictions flagged**: no
- **Notes**: First filed synthesis in the wiki. Drew from [[Context Engineering]], [[Context Rot]], [[Just-in-Time Context Retrieval]], [[Context Compaction]], [[Agentic Search]], [[Agent Skills]], [[Long-Running Agent]], [[Claude Agent SDK]]. Seven practices organized around the governing principle "smallest possible set of high-signal tokens." High confidence — all three context engineering sources are mutually reinforcing. Main gap: no quantitative thresholds for when context rot becomes practically significant.
