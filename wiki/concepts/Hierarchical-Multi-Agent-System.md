---
type: concept
tags: [ai-agents, architecture, multi-agent, hierarchical, supervisor]
created: 2026-05-10
updated: 2026-05-10
sources: [buildingagents.pdf, https://www.anthropic.com/engineering/multi-agent-research-system, https://claude.com/blog/multi-agent-coordination-patterns]
---

# Hierarchical Multi-Agent System

**Domain**: AI agent architecture
**Also called**: supervisory system, orchestrator-agent pattern, supervisor-specialist, Orchestrator-Subagent

## Definition

A hierarchical multi-agent system uses a central supervisor agent to coordinate multiple specialist agents through intelligent task delegation. The supervisor analyzes incoming requests, routes them to appropriate specialists, synthesizes responses, and maintains a clear chain of responsibility. Subagents are treated as tools — the supervisor uses tool-calling to invoke them.

## How It Works

Structure:
- **Supervisor agent** — analyzes tasks, delegates to specialists, synthesizes results, maintains strategic alignment
- **Specialist agents** — focus on their domain, receive directives from the supervisor, return results
- **Communication** — supervisor calls specialists as tools; specialists don't communicate directly with each other (in the pure hierarchical form)

Example — marketing campaign development:
1. Client submits campaign brief
2. **Marketing director agent (supervisor)** — analyzes requirements, maps tasks to specialist agents
3. **Market research agent** — conducts audience analysis, competitive landscape
4. **Creative strategy agent** — develops messaging frameworks
5. **Content creation agent** — produces campaign materials
6. **Analytics agent** — defines success metrics
7. Supervisor synthesizes all outputs into a coherent campaign

### Production implementation: Anthropic Research system

The 5-step workflow used in Anthropic's live Research feature (from [[How We Built Our Multi-Agent Research System]]):

1. **Lead agent planning** — Claude Opus 4 analyzes the query, develops a research strategy, and persists the plan to memory
2. **Subagent creation** — spawns specialized Claude Sonnet 4 agents with specific research tasks, each with a clean context window
3. **Parallel research** — each subagent independently searches and evaluates results; parallel tool calls reduce research time by up to 90%
4. **Synthesis** — lead agent compiles findings and determines if additional research is needed
5. **Citation** — a dedicated citation agent identifies source locations for proper attribution

**Key design details:**
- Lead agents require detailed task descriptions specifying objectives, output formats, tool guidance, and clear task boundaries to prevent duplication across subagents
- Subagents store work to the filesystem rather than returning large outputs inline — prevents information loss and reduces orchestrator context overhead (see [[Durable Agent Execution]])
- Simple tasks: 1 agent, 3–10 tool calls. Complex research: 10+ subagents with divided responsibilities
- Measured result: Claude Opus 4 lead + Claude Sonnet 4 subagents outperformed single-agent Claude Opus 4 by **90.2%**; costs approximately **15× more tokens** than standard chat

**When to use:**
- Structured tasks with clear role boundaries
- When a single point of coordination and accountability is needed
- Organizational workflows that mirror human team hierarchies (specialists + manager)
- Tasks requiring strategic synthesis across multiple specialist outputs

**When to avoid:**
- When specialists need to collaborate directly (→ [[Collaborative Multi-Agent System]])
- When supervisory translation adds latency without value (→ Network/P2P)
- When the task is simple enough for a [[Single-Agent Architecture]]
- When subtasks require multi-step context continuity → [[Agent Teams]] is better suited

**Key limitation** (from [[Multi-Agent Coordination Patterns]]): the orchestrator becomes an **information bottleneck** when subagents discover cross-relevant findings — information that would benefit another subagent must route through the orchestrator rather than flowing directly. Critical details are also often lost through multiple handoffs. For tasks where subagents' discoveries should immediately influence each other, [[Shared State]] may be more appropriate.

## Relevance to This Wiki

The [[LLM Wiki Pattern]] query operation resembles a mini-hierarchical system: Claude acts as the orchestrator, reading `index.md` to identify which "specialist" wiki pages to read, synthesizing across them, then reporting back.

## Key Sources

[[Building Effective AI Agents]], [[How We Built Our Multi-Agent Research System]]

## Related Concepts

- [[Multi-Agent Architecture]] — the broader category
- [[Collaborative Multi-Agent System]] — alternative with distributed coordination
- [[Agent Skills]] — skills can be distributed across specialists for deep domain expertise
- [[Sequential Workflow]] — hierarchical systems often execute specialist tasks sequentially
- [[Parallel Workflow]] — hierarchical systems fan out to specialists in parallel; up to 90% time reduction
- [[Self-Improving Agent]] — tool-testing subagent pattern extends hierarchical systems with meta-improvement
- [[Durable Agent Execution]] — production infrastructure for running hierarchical systems reliably
- [[LLM-as-Judge]] — evaluation methodology for hierarchical system quality assessment
- [[Agent Teams]] — better choice when subtasks require multi-step context continuity
- [[Shared State]] — better choice when subagents need real-time discovery sharing
