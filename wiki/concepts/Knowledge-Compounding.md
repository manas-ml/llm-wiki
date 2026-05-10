---
type: concept
tags: [knowledge-management, llm, wiki, compounding]
created: 2026-05-10
updated: 2026-05-10
sources: [wiki.md]
---

# Knowledge Compounding

**Domain**: knowledge management, personal epistemology
**Also called**: compounding knowledge base, accumulative synthesis

## Definition

Knowledge compounding is the property of the [[LLM Wiki Pattern]] whereby each new source and each filed query answer makes the wiki incrementally richer — cross-references denser, synthesis deeper, contradictions more resolved — rather than resetting to zero. The value of the wiki grows faster than the number of pages because connections between pages multiply.

## How It Works

Compounding happens through three mechanisms:

**1. Ingest compounding**
Each new source doesn't just add its own summary page. It also updates every existing entity and concept page it touches. A page about [[AI Agent]] that started from one source gets richer with each subsequent source that references the concept. By source 10, the page reflects a synthesis no single source contains.

**2. Query compounding**
When a query answer is filed as a synthesis page, it becomes part of the wiki. Future queries can draw on it. A comparison or analysis done once doesn't disappear into chat history — it becomes a permanent, citable resource that future syntheses build on.

**3. Contradiction resolution compounding**
When a new source contradicts an old claim, the contradiction is flagged visibly (`> [!warning]`). Over time, lint passes resolve contradictions — either updating the claim or noting the genuine disagreement. The wiki's epistemic quality improves with each resolution.

### The compounding contrast with RAG
[[RAG vs Wiki|RAG]] is stateless: every query re-derives from raw chunks with no memory of past queries or insights. The wiki accumulates: past ingests, past queries, and past lint passes all leave permanent improvements. The gap between them widens with every session.

## Relevance to This Wiki

Knowledge compounding is *why* this system is worth maintaining. The effort per source decreases as the wiki grows (the LLM has more context to integrate into) while the value per query increases (more compiled knowledge to draw from).

## Key Sources

[[LLM Wiki Pattern]] (source summary)

## Related Concepts

- [[LLM Wiki Pattern]] — the system that enables compounding
- [[RAG vs Wiki]] — compounding is the key advantage over RAG
- [[CLAUDE.md Schema]] — the schema ensures compounding happens consistently (lint, cross-referencing, contradiction tracking)
