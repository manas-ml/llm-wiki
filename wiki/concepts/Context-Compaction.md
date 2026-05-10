---
type: concept
tags: [context-engineering, long-horizon, compaction, summarization, ai-agents]
created: 2026-05-10
updated: 2026-05-10
sources: [https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents, https://claude.com/blog/building-agents-with-the-claude-agent-sdk]
---

# Context Compaction

**Domain**: AI agent architecture, context engineering
**Also called**: context summarization, conversation compaction, context windowing

## Definition

Context compaction is a technique for long-horizon agent tasks where, as a conversation approaches the context window limit, the message history is summarized and a new context window is initialized with the summary. The agent continues from the compressed representation rather than hitting a hard cutoff. This preserves coherence across tasks that exceed any single context window.

## How It Works

### Mechanism

1. Agent detects (or is triggered when) the context window approaches its limit
2. Full message history is passed to a model with instructions to summarize and compress critical details
3. A new context window is initialized with the resulting summary
4. The agent continues from this compressed state

The model doing the compaction preserves:
- Architectural decisions made
- Unresolved bugs or open questions
- Implementation details still relevant to remaining work
- Key constraints established earlier in the conversation

It discards:
- Redundant tool outputs (raw results from tools already acted upon)
- Repeated failed attempts
- Verbose intermediate reasoning that's been superseded

### The art of compaction

Compaction involves a quality tradeoff: **recall vs. precision**.
- **Overly aggressive** compaction loses subtle context whose importance only emerges later
- **Insufficiently aggressive** compaction fails to meaningfully reduce the token load

Anthropic's guidance: tune compaction prompts on real complex agent traces. Start by maximizing recall (comprehensive capture), then iterate to improve precision (eliminate superfluous content). Tool result clearing — removing raw tool call outputs deep in history that the agent no longer needs — is offered as a lightweight compaction form.

### SDK built-in compact feature

[[Building Agents with the Claude Agent SDK]] notes that compaction is now built into the [[Claude Agent SDK]] as an automatic feature: when the context limit approaches, the SDK automatically summarizes previous messages and continues the session. This is built on Claude Code's compact slash command. For most agents built on the SDK, compaction is handled without custom implementation.

### Three long-horizon techniques compared

| Technique | Mechanism | Best for |
|-----------|-----------|---------|
| **Context Compaction** | Summarize + reinitialize at limit | Tasks with extensive back-and-forth conversation |
| Structured note-taking | Agent writes persistent notes to external files | Iterative development with clear milestones |
| Sub-agent architecture | Specialized agents with clean context windows return condensed summaries | Complex parallel research and analysis |

These techniques are complementary, not mutually exclusive.

### Sub-agent variant

Rather than compacting one agent's context, sub-agent architectures achieve context isolation by design: each sub-agent handles a focused task in a clean context window and returns a condensed summary (typically 1,000–2,000 tokens) to the orchestrating agent. The orchestrator never accumulates the sub-agent's full exploration context — it receives only the distilled result. This is [[Context Engineering]] applied at the architecture level.

## Relationship to Existing Patterns

Context compaction is the runtime mechanism that enables [[Long-Running Agent]] systems. Without it (or equivalent techniques), agents would either truncate important history or fail entirely on tasks exceeding their context window. The [[Evaluator-Optimizer Workflow]] can incorporate compaction: the evaluator pass compacts tool results from the generator before the next generation cycle.

## Relevance to This Wiki

The wiki's `log.md` is a lightweight analog to structured note-taking — an external, append-only record that persists operation state outside any single Claude session. The Session Start Protocol (read last 5 log entries) is a manual compaction read: Claude recovers recent context from the compressed log rather than re-reading everything that happened. The system handles context limits by design through structured external state.

## Key Sources

[[Effective Context Engineering for AI Agents]], [[Building Agents with the Claude Agent SDK]]

## Related Concepts

- [[Context Engineering]] — compaction is one of the three core long-horizon techniques
- [[Context Rot]] — compaction is the primary mitigation for context rot in long-running tasks
- [[Long-Running Agent]] — the primary use case for context compaction
- [[Just-in-Time Context Retrieval]] — complementary: JIT keeps data lean; compaction keeps history lean
- [[Evaluator-Optimizer Workflow]] — compaction can be embedded in the generate-evaluate-refine loop
- [[Multi-Agent Architecture]] — sub-agent architectures achieve context isolation by design
