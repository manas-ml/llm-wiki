---
type: concept
tags: [retrieval, ai-agents, context-engineering, file-system, search]
created: 2026-05-10
updated: 2026-05-10
sources: [https://claude.com/blog/building-agents-with-the-claude-agent-sdk]
---

# Agentic Search

**Domain**: AI agent architecture, context engineering, information retrieval
**Also called**: file-system search, bash-based retrieval, agent-driven search

## Definition

Agentic search is a retrieval approach where agents explore information sources — primarily the file system — autonomously using tools like bash, grep, and tail, rather than relying on pre-computed embeddings or vector indexes. Rather than querying a vector store for semantically similar chunks, the agent navigates structure (folder hierarchies, naming conventions, file sizes, timestamps) and executes targeted queries to find what it needs at runtime.

Anthropic's guidance: **start with agentic search; only add semantic/vector search if you need faster results or more variations.**

## How It Works

### The mechanism

The agent receives access to a file system (or equivalent structured information store) and uses tools to navigate it:

- `grep` — search file contents for keywords or patterns
- `tail` / `head` — inspect large files without loading them fully into context
- `find` — locate files by name, type, or date
- `ls` — survey directory structure

For large files (logs, user uploads), the agent decides *how* to load content — partial reads, targeted searches, or progressive exploration — rather than loading everything at once.

### Folder structure as context engineering

The article makes a key observation: **the folder and file structure of an agent becomes a form of context engineering.** Naming conventions, directory hierarchies, and timestamps carry semantic signal before any content is read:

- `conversations/` folder implies email or chat history
- `test_utils.py` in `tests/` vs. `src/core_logic/` implies different purposes
- A timestamp of yesterday vs. 3 years ago proxies for relevance

Designing an agent's storage layout is therefore also designing its retrieval behavior.

### Contrast with semantic/vector search

| Dimension | Agentic Search | Semantic Search |
|-----------|---------------|-----------------|
| Mechanism | Runtime file system exploration via tools | Pre-computed embeddings + vector similarity query |
| Accuracy | Higher — exact matches, full content available | Lower — chunking and embedding lose precision |
| Transparency | High — agent explains what it searched and found | Low — similarity scores are opaque |
| Maintenance | Low — no index to update as files change | High — index must be kept in sync |
| Speed | Slower — runtime I/O and tool calls | Faster — pre-computed similarity |
| Flexibility | High — agent adapts search strategy dynamically | Lower — fixed chunking and embedding approach |

Recommendation: start with agentic search. The accuracy, transparency, and maintainability advantages outweigh the speed disadvantage for most use cases. Add semantic search only when speed is a hard constraint or you need to handle many query variations over large corpora.

### In the agent loop

Agentic search is the primary mechanism for the **gather context** phase of the agent loop (gather context → take action → verify work). The email agent example from the article stores previous conversations in a `conversations/` folder, then searches these when relevant context is needed — agentic search, not a vector store.

Subagents extend this: the lead agent can spin off multiple search subagents in parallel, each running different queries against the same information source, then aggregating only the relevant excerpts rather than loading everything into one context window.

## Relationship to [[Just-in-Time Context Retrieval]]

Agentic search is the concrete file-system implementation of the [[Just-in-Time Context Retrieval]] paradigm. JIT retrieval is the broader principle (load data at runtime via lightweight identifiers); agentic search is the specific technique (navigate file systems via bash tools). The same principles apply — progressive disclosure, reference metadata as signal, hybrid with upfront loading for stable context.

## Relevance to This Wiki

The wiki's QUERY operation uses agentic search: the index.md is the directory map, and reading candidate pages on demand is file-system navigation at the wiki level. The LINT operation is also agentic search — grepping for wikilink patterns across all pages to find orphans and broken links. Neither operation uses vector embeddings.

## Key Sources

[[Building Agents with the Claude Agent SDK]]

## Related Concepts

- [[Just-in-Time Context Retrieval]] — the broader paradigm; agentic search is its file-system implementation
- [[Context Engineering]] — folder structure as context engineering is a design insight
- [[RAG vs Wiki]] — agentic search extends the RAG skepticism: even for agents, prefer exploration over pre-computed embeddings
- [[Claude Agent SDK]] — the platform where agentic search is the recommended default
- [[Multi-Agent Architecture]] — subagents can run parallel agentic searches efficiently
- [[Context Rot]] — agentic search avoids context rot by loading only relevant content on demand
