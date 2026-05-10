---
type: source
tags: [context-engineering, ai-agents, prompt-engineering, long-horizon, memory]
created: 2026-05-10
updated: 2026-05-10
sources: [https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents]
---

# Effective Context Engineering for AI Agents

**File**: `https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents`
**Type**: article
**Author(s)**: Anthropic Applied AI team — Prithvi Rajasekaran, Ethan Dixon, Carly Ryan, Jeremy Hadfield (with contributions from Rafi Ayub, Hannah Moran, Cal Rueb, Connor Jennings)
**Date**: 2026
**Domain**: AI agent architecture, LLM engineering, context management

## Summary

A technical article from Anthropic's Applied AI team introducing context engineering as the discipline successor to prompt engineering. The central argument: building capable multi-turn agents requires managing the entire token set at every inference step — not just writing good instructions. The article covers what belongs in effective context (system prompts, tools, examples, history), how agents should retrieve information just-in-time rather than front-loading it, and three concrete techniques for maintaining coherence on long-horizon tasks that exceed single context windows.

## Key Claims

- [[Context Engineering]] supersedes prompt engineering as agents grow more complex — it addresses the full token set (system prompt, tools, MCP, data, history) at each inference step, not just discrete instruction-writing
- [[Context Rot]] is a universal LLM phenomenon — recall accuracy degrades as token count grows due to transformer n² attention architecture; context must be treated as finite with diminishing marginal returns
- The core principle: find the "smallest possible set of high-signal tokens" maximizing desired outcome likelihood — applies to every context component
- Tools should be token-efficient, self-contained, and unambiguous; bloated tool sets with overlapping functionality are a primary failure mode
- [[Just-in-Time Context Retrieval]] is an emerging paradigm — agents maintain lightweight identifiers and load data dynamically at runtime, enabling progressive disclosure; Claude Code's glob/grep pattern is the running example
- Three techniques for long-horizon coherence: [[Context Compaction]], structured note-taking (agentic memory), and sub-agent architectures with isolated context windows
- Sub-agents return condensed summaries (1,000–2,000 tokens) to orchestrators, achieving separation of concerns at the context level
- CLAUDE.md files are explicitly cited as upfront context that enables JIT navigation — a validation of this wiki's architecture

## Key Entities

[[Anthropic]]

## Key Concepts

[[Context Engineering]], [[Context Rot]], [[Just-in-Time Context Retrieval]], [[Context Compaction]], [[Long-Running Agent]], [[Multi-Agent Architecture]], [[CLAUDE.md at Scale]]

## Notable Quotes

> "Context engineering is the art and science of curating what enters the limited context window from constantly evolving possible information."

> "Find the smallest possible set of high-signal tokens maximizing desired outcome likelihood."

> "Agents operating in loops generate increasingly relevant data that must be cyclically refined."

> "Do the simplest thing that works remains best advice for teams building Claude-based agents."

## Contradicts or Challenges

None identified. This article extends the agent architecture material in [[Building Effective AI Agents]] with a dedicated focus on the context layer. The sub-agent pattern described here aligns with [[Hierarchical Multi-Agent System]] and [[Multi-Agent Architecture]], adding the explicit context-isolation rationale. The CLAUDE.md-as-upfront-context example validates the [[CLAUDE.md Schema]] and [[CLAUDE.md at Scale]] patterns already in this wiki.

## Questions Raised

- At what token count does context rot become measurable for current Claude models — is there a practical threshold for different task types?
- How should compaction prompts be tuned differently for creative vs. technical long-horizon tasks?
- As models improve, how quickly will the engineering overhead of JIT retrieval become unnecessary?
