---
type: concept
tags: [context-engineering, prompt-engineering, llm, ai-agents, inference]
created: 2026-05-10
updated: 2026-05-10
sources: [https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents, https://claude.com/blog/building-agents-with-skills-equipping-agents-for-specialized-work]
---

# Context Engineering

**Domain**: LLM engineering, AI agent architecture
**Also called**: context curation, token curation, context management

## Definition

Context engineering is the discipline of curating and maintaining the optimal token set during LLM inference to consistently achieve desired outcomes. It encompasses the system prompt, tools, MCP integrations, external data, and message history — everything that enters the context window at each sampling step. Where prompt engineering concerns discrete instruction-writing tasks, context engineering operates iteratively: curation occurs every time information passes to the model.

Anthropic defines it as "the art and science of curating what enters the limited context window from constantly evolving possible information."

## How It Works

### The core principle

Find the **smallest possible set of high-signal tokens** that maximizes desired outcome likelihood. This applies to every component:

| Component | High-signal practice | Failure mode |
|-----------|---------------------|--------------|
| System prompt | Specific enough to guide, flexible enough to handle variation | Too vague (false shared context) or too brittle (hardcoded logic) |
| Tools | Self-contained, token-efficient returns, unambiguous inputs | Bloated tool sets; overlapping functionality agents can't decide between |
| Examples (few-shot) | Diverse canonical cases showing expected behavior | Edge-case stuffing; examples that don't represent the real distribution |
| Message history | Pruned, compacted, or externalized as tasks complete | Full conversation history accumulating noise and stale data |

### System prompt altitude

System prompts should be written at the right "altitude" — specific enough to produce concrete behavior, flexible enough to provide strong heuristics rather than brittle rules. Structure them with clear sections (`<background_information>`, `<instructions>`, `## Tool guidance`) using XML tags or Markdown headers. Start minimal and add based on observed failure modes rather than anticipating every edge case.

### Tools as context contracts

Tools define the agent-environment contract. They must promote efficiency: token-efficient information returns, clear intended use, unambiguous parameters. If engineers cannot definitively specify when to use a tool, agents cannot perform better than random guessing between options. Bloated tool sets with overlapping functionality are a primary agent failure mode.

### Iterative nature

Unlike prompt engineering (a one-time task), context engineering is ongoing. Each inference step is a fresh curation decision — what to pass forward, what to compact, what to load just-in-time. This makes it a runtime engineering discipline, not a setup task.

### Progressive disclosure as a context engineering pattern

[[Building Agents with Skills: Equipping Agents for Specialized Work]] introduces **progressive disclosure** as a concrete context engineering technique for skills libraries. Rather than loading all skill content upfront, skills expose content in three tiers:

1. **Metadata** (~50 tokens) — name and description always present, so the agent knows the skill exists
2. **Full skill file** (~500 tokens) — loaded when the agent determines the skill is needed
3. **Reference docs** (2,000+ tokens) — loaded on demand during execution

This pattern enables equipping agents with hundreds of skills without context overload. Each tier is the minimum needed for the agent's current decision — a direct application of "smallest possible set of high-signal tokens" to expertise delivery. The same principle applies to any large knowledge library: index metadata upfront, load content just-in-time.

## Relationship to Prompt Engineering

Prompt engineering was dominant when most LLM use cases were one-shot classification or generation. Context engineering emerges when building multi-turn agents over extended horizons — the prompt is just one input to a continuously evolving context state.

## Relevance to This Wiki

The [[CLAUDE.md Schema]] is an instance of context engineering applied to wiki maintenance. Each section — directory map, templates, operation workflows — is a context component curated to give Claude the minimum high-signal information it needs per session. The Session Start Protocol (read CLAUDE.md → index.md → log.md) is a structured context loading sequence, not just initialization.

## Key Sources

[[Effective Context Engineering for AI Agents]], [[Building Agents with Skills: Equipping Agents for Specialized Work]]

## Related Concepts

- [[Context Rot]] — the architectural constraint that makes context engineering necessary
- [[Just-in-Time Context Retrieval]] — the runtime retrieval pattern that keeps context lean
- [[Context Compaction]] — technique for managing context in long-horizon tasks
- [[CLAUDE.md Schema]] — a real instance of context engineering for knowledge management
- [[CLAUDE.md at Scale]] — shared CLAUDE.md as team-level context engineering
- [[Agent Skills]] — three-tier progressive disclosure is context engineering applied to expertise delivery
- [[AI Agent]] — context engineering is primarily relevant for agents in loops
- [[Long-Running Agent]] — where context engineering complexity is highest
