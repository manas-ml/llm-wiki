---
type: concept
tags: [context-engineering, llm, attention, architecture, performance]
created: 2026-05-10
updated: 2026-05-10
sources: [https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents]
---

# Context Rot

**Domain**: LLM architecture, context engineering
**Also called**: context degradation, attention dilution, long-context performance degradation

## Definition

Context rot is the universal phenomenon whereby LLM recall accuracy and reasoning quality degrade as the token count in a context window increases. First observed through needle-in-a-haystack benchmarking, context rot means that more context does not linearly improve agent performance — beyond certain points, additional tokens actively dilute attention and reduce the model's ability to surface the right information at the right time.

## How It Works

### Architectural cause

LLMs use transformer architecture where every token attends to every other token across the entire context. This creates **n² pairwise attention relationships** for n tokens. As context lengthens, capturing these relationships stretches thin — the model's attention budget is finite, and each new token consumes a share of it.

Additionally, models develop attention patterns from training data where shorter sequences predominate. Fewer specialized parameters exist for context-wide dependencies, so performance on long-context tasks degrades relative to shorter ones.

### Gradient, not cliff

Context rot creates **performance gradients, not hard cliffs**. Models remain capable at longer contexts but show reduced precision for:
- Information retrieval from within the context
- Long-range reasoning across distant context sections
- Maintaining consistent focus on earlier constraints or goals

Position encoding interpolation allows models to handle longer sequences than their training context, but with some degradation in positional understanding.

### Attention budget framing

The practical implication is that LLMs should be treated as having a finite **attention budget** — like human working memory. Each token spends some of that budget. Tokens that don't contribute to the desired outcome represent wasted budget that could have gone to more relevant information.

This is why the core principle of [[Context Engineering]] — "smallest possible set of high-signal tokens" — is a response to an architectural constraint, not just a style preference.

## Implications for Agent Design

| Concern | Design response |
|---------|----------------|
| Long conversations accumulate noise | [[Context Compaction]] — summarize and reinitialize |
| Pre-loading all relevant data upfront | [[Just-in-Time Context Retrieval]] — load only what's needed, when it's needed |
| Large tool result payloads | Token-efficient tool design; store results externally, reference by pointer |
| Unbounded message history | Prune redundant tool outputs; clear raw results once processed |

## Relevance to This Wiki

Context rot is why the [[CLAUDE.md Schema]] Session Start Protocol reads only 3 files (CLAUDE.md, index.md, last 5 log entries) rather than loading every wiki page. Loading the full wiki into context would produce context rot — Claude would spend attention budget on hundreds of pages irrelevant to the current operation. The QUERY workflow's step-by-step page retrieval is also a response to this: read only candidate pages, not everything.

## Key Sources

[[Effective Context Engineering for AI Agents]]

## Related Concepts

- [[Context Engineering]] — the discipline that exists because of context rot
- [[Context Compaction]] — primary mitigation for long-horizon tasks
- [[Just-in-Time Context Retrieval]] — keeps context lean by loading data at runtime
- [[Long-Running Agent]] — the architecture most exposed to context rot
- [[AI Agent]] — context rot applies to any agent operating in a loop
