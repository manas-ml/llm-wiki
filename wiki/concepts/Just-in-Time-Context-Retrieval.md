---
type: concept
tags: [context-engineering, retrieval, ai-agents, progressive-disclosure, rag]
created: 2026-05-10
updated: 2026-05-10
sources: [https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents, https://claude.com/blog/building-agents-with-the-claude-agent-sdk]
---

# Just-in-Time Context Retrieval

**Domain**: AI agent architecture, context engineering
**Also called**: JIT retrieval, agentic search, runtime context loading, progressive disclosure

## Definition

Just-in-Time Context Retrieval is a context management paradigm where agents maintain lightweight identifiers (file paths, stored queries, web links) and dynamically load data at runtime using tools, rather than pre-loading all relevant information upfront. Instead of embedding-based pre-inference retrieval that preprocesses data before the agent runs, JIT retrieval lets the agent decide what to load and when — assembling context layer by layer through exploration.

## How It Works

### Contrast with upfront loading

| Approach | When data loads | Agent control | Speed | Context efficiency |
|----------|----------------|--------------|-------|-------------------|
| Upfront / RAG | Before inference, pre-computed | None — engineer decides | Fast (cached) | Low — loads regardless of relevance |
| Just-in-time | During inference, at runtime | Full — agent decides | Slower (runtime I/O) | High — loads only what's needed |
| Hybrid | Both | Partial | Balanced | Balanced |

### Progressive disclosure

Rather than loading everything at once, JIT retrieval enables **progressive disclosure** — agents incrementally discover relevant context through exploration. Each interaction yields signals informing next decisions:
- File sizes suggest complexity
- Naming conventions hint at purpose (`test_utils.py` in `tests/` vs. `src/core_logic/`)
- Timestamps proxy relevance
- Folder hierarchies encode domain structure

Agents assemble understanding layer-by-layer, maintaining only working memory essentials in context while using tools to pull additional details on demand.

### Reference metadata as signal

Lightweight identifiers carry information beyond their content. A file path like `src/auth/jwt_validator.py` tells an agent about location, domain, and likely function before a single byte of content is read. This means agents operating on identifiers are already doing useful reasoning before any data is loaded.

### Claude Code as the canonical example

The article describes Claude Code's approach as the reference implementation:
- **CLAUDE.md** drops into context upfront — stable, infrequently-changing project context
- **glob and grep primitives** handle JIT file retrieval — dynamic, query-driven
- The combination bypasses stale indexing and complex syntax tree dependencies

This hybrid mirrors the wiki's own architecture: CLAUDE.md and index.md load upfront; individual wiki pages load on demand during QUERY operations.

### Trade-offs

**Advantages:**
- Context stays lean — no attention budget wasted on irrelevant data
- Handles dynamic environments where pre-indexing would go stale
- Enables complex exploration that can't be anticipated upfront

**Disadvantages:**
- Slower than pre-computed retrieval (runtime I/O per load)
- Requires well-designed tools with clear heuristics — without guidance, agents waste context chasing dead-ends
- Engineers must ensure the right tools and navigation signals exist in the environment

### Hybrid strategies

Most effective agents use **hybrid strategies**: upfront loading for stable, high-confidence context (CLAUDE.md, project metadata, system prompts) combined with JIT retrieval for dynamic, task-specific content. The right balance depends on how dynamic the environment is and how predictable the information needs are.

## Relevance to This Wiki

The QUERY operation in [[CLAUDE.md Schema]] is a JIT retrieval workflow: Step 1 loads index.md (lightweight identifier catalog), Step 2 loads only candidate pages (runtime, query-driven). The wiki never loads all 50+ pages into context for a single query — it identifies candidates from the index and retrieves selectively. This is JIT retrieval applied to knowledge management.

## Key Sources

[[Effective Context Engineering for AI Agents]], [[Building Agents with the Claude Agent SDK]]

## Related Concepts

- [[Context Engineering]] — JIT retrieval is one of the core context engineering techniques
- [[Context Rot]] — JIT retrieval is a response to the attention budget constraint
- [[Context Compaction]] — compaction manages history; JIT manages data loading
- [[RAG vs Wiki]] — RAG is upfront/pre-computed; this wiki uses a hybrid JIT approach
- [[CLAUDE.md Schema]] — the wiki's QUERY workflow implements JIT retrieval
- [[Long-Running Agent]] — JIT retrieval is especially valuable for long-horizon tasks
- [[Agentic Search]] — the concrete file-system implementation of JIT retrieval
- [[Claude Agent SDK]] — the platform where agentic search is the recommended default
