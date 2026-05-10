---
type: concept
tags: [rag, llm, knowledge-management, retrieval, comparison]
created: 2026-05-10
updated: 2026-05-10
sources: [wiki.md, https://claude.com/blog/building-agents-with-the-claude-agent-sdk]
---

# RAG vs Wiki

**Domain**: knowledge management, LLM systems
**Also called**: retrieval-augmented generation vs. compiled knowledge base

## Definition

RAG (Retrieval-Augmented Generation) and the LLM Wiki Pattern are two fundamentally different approaches to using LLMs with a document collection. RAG retrieves and re-derives on every query; the wiki compiles once and compounds.

## How It Works

### RAG approach
1. Documents are chunked and embedded into a vector store at index time.
2. At query time, relevant chunks are retrieved by similarity search.
3. The LLM reads the retrieved chunks and generates an answer from scratch.
4. Nothing is built up — the next query starts from zero again.

**Examples**: NotebookLM, ChatGPT file uploads, most RAG frameworks.

### LLM Wiki approach
1. At ingest time, the LLM reads each source and writes structured wiki pages — entity pages, concept pages, summaries, cross-references.
2. At query time, the LLM reads the already-compiled wiki pages and synthesizes from structured knowledge.
3. Each ingest makes the wiki richer. Each filed query answer adds to it further.

### Comparison

| Dimension | RAG | LLM Wiki |
|-----------|-----|----------|
| Knowledge compilation | At query time (re-derived) | At ingest time (persistent) |
| Cross-references | Re-discovered each query | Already built |
| Contradictions | Not tracked | Flagged and recorded |
| Synthesis | Re-generated each time | Accumulated and fileable |
| Infrastructure | Vector DB, embeddings required | Plain markdown files |
| Compounding | None — stateless | Yes — richer with every source |
| Multi-doc synthesis | Requires all chunks to be retrieved | Already compiled across sources |
| Scalability ceiling | Higher (vector search scales) | ~100s of sources before needing search tooling |

### When RAG is better
- Very large document collections (thousands of sources) where compiling every page is impractical
- Read-only corpora that don't need ongoing synthesis
- When freshness matters more than synthesis depth (e.g., real-time data)

### When the wiki is better
- Active knowledge accumulation over weeks or months
- Cross-source synthesis is the primary goal
- The human wants to *browse and understand*, not just retrieve answers
- No vector DB infrastructure available or desired

## Extended: agentic search vs. semantic search

[[Building Agents with the Claude Agent SDK]] extends this debate into agent design. For agents retrieving information at runtime, Anthropic recommends **[[Agentic Search]]** (file-system exploration via bash/grep) over **semantic/vector search** for the same reasons the wiki pattern beats RAG:

| Dimension | Semantic/Vector Search | Agentic Search |
|-----------|----------------------|----------------|
| Accuracy | Lower (chunking loses precision) | Higher (exact matches, full content) |
| Transparency | Low (similarity scores opaque) | High (agent explains what it found) |
| Maintenance | High (index must stay in sync) | Low (no index to update) |
| Speed | Faster | Slower |

Recommendation: start with agentic search; add semantic search only when speed is a hard constraint. The pattern of skepticism toward pre-computed, opaque retrieval — whether RAG over a wiki or vector search in an agent — is consistent across all Anthropic sources in this wiki.

## Key Sources

[[LLM Wiki Pattern]] (source summary), [[Building Agents with the Claude Agent SDK]]

## Related Concepts

- [[LLM Wiki Pattern]] — the alternative to RAG described here
- [[Knowledge Compounding]] — the core advantage of the wiki approach
- [[CLAUDE.md Schema]] — what makes the wiki approach disciplined rather than ad hoc
- [[Agentic Search]] — the agent-level analog: prefer file-system exploration over vector search
- [[Just-in-Time Context Retrieval]] — the broader JIT paradigm that both wiki and agentic search instantiate
