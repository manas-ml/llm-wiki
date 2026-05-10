---
type: source
tags: [ai-agents, multi-agent, research, production, orchestrator, evaluation]
created: 2026-05-10
updated: 2026-05-10
sources: [https://www.anthropic.com/engineering/multi-agent-research-system]
---

# How We Built Our Multi-Agent Research System

**File**: `https://www.anthropic.com/engineering/multi-agent-research-system`
**Type**: article
**Author(s)**: Anthropic
**Date**: June 13, 2025
**Domain**: AI agent architecture, production engineering, evaluation

## Summary

An engineering deep-dive from Anthropic on the production multi-agent system behind Claude's Research feature. The system uses a hierarchical orchestrator-worker pattern: a lead Claude Opus 4 agent coordinates planning and synthesis while specialized Claude Sonnet 4 subagents handle parallel research tasks. The article covers the full engineering lifecycle — architecture decisions, prompting principles, evaluation methodology, and production challenges — bridging the gap between architecture patterns and real deployed systems.

## Key Claims

- A multi-agent system with Claude Opus 4 as lead and Claude Sonnet 4 subagents outperformed single-agent Claude Opus 4 by **90.2%** — confirming the figure in [[Building Effective AI Agents]] and specifying the exact model combination
- Multi-agent systems consume approximately **15× more tokens** than standard chat; tasks must have sufficient value to justify the cost
- The production workflow is 5 steps: lead agent plans and persists to memory → spawns specialized subagents → parallel research → synthesis → dedicated citation agent handles attribution
- [[Self-Improving Agent]] pattern: Claude models can diagnose their own failures and suggest prompt improvements autonomously; one tool-testing agent achieved a **40% reduction in task completion time** for future agents
- Running subagents in parallel plus parallel tool calls reduced research time by up to **90%**
- Scale effort to task: simple fact-finding = 1 agent, 3–10 tool calls; complex research = 10+ subagents with divided responsibilities
- Extended thinking improves instruction-following, reasoning, and efficiency for both planning and evaluation phases
- [[LLM-as-Judge]] evaluation against rubric criteria (factual accuracy, citation accuracy, completeness, source quality, tool efficiency) is effective at scale; human testing catches biases LLM judges miss (e.g. preference for SEO content over authoritative sources)
- [[Durable Agent Execution]] — production agents require state management, error handling, and resumption capabilities; costly restarts are not acceptable
- Rainbow deployments gradually shift traffic between versions to avoid disrupting active agents
- Subagents storing work independently to the filesystem prevents information loss and reduces token overhead from large outputs passing through conversation history

## Key Entities

[[Anthropic]]

## Key Concepts

[[Hierarchical Multi-Agent System]], [[Multi-Agent Architecture]], [[Parallel Workflow]], [[Self-Improving Agent]], [[LLM-as-Judge]], [[Durable Agent Execution]], [[Long-Running Agent]], [[Context Compaction]], [[Evaluator-Optimizer Workflow]]

## Notable Quotes

> "A multi-agent system with Claude Opus 4 as the lead agent and Claude Sonnet 4 subagents outperformed single-agent Claude Opus 4 by 90.2%."

> "Multi-agent systems use approximately 15 times more tokens than standard chat interactions. This requires tasks with sufficient value to justify increased computational costs."

> "One tool-testing agent achieved a 40% decrease in task completion time for future agents."

> "Running multiple subagents simultaneously and enabling parallel tool calls reduced research time by up to 90%."

## Contradicts or Challenges

None identified. This is fully consistent with [[Building Effective AI Agents]] (which introduced the orchestrator-worker and evaluator-optimizer patterns) and [[Effective Context Engineering for AI Agents]] (which described sub-agent context isolation, filesystem outputs, and compaction). This article is the production implementation of those patterns — it adds engineering detail, cost data, and the self-improving agent concept.

## Questions Raised

- How does the lead agent determine when synthesis is sufficient vs. when to spawn additional research subagents?
- How should teams balance the 15x token cost against task value — what are the practical thresholds?
- What is the right granularity for rainbow deployment traffic shifts for stateful long-running agents?
- Will asynchronous subagent execution (currently a noted gap) become standard practice, and how does it change orchestrator design?
