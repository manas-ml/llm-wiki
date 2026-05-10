---
type: concept
tags: [llm, wiki, knowledge-management, architecture]
created: 2026-05-10
updated: 2026-05-10
sources: [wiki.md]
---

# LLM Wiki Pattern

**Domain**: knowledge management, LLM tooling
**Also called**: LLM-maintained wiki, persistent knowledge base

## Definition

A pattern for building and maintaining a personal knowledge base in which an LLM incrementally writes and updates a structured wiki of markdown files from curated source documents. Unlike [[RAG vs Wiki|RAG]], knowledge is compiled once at ingest time and kept current — the wiki is a persistent, compounding artifact, not a retrieval index.

## How It Works

Three layers:

| Layer | Owner | Role |
|-------|-------|------|
| `raw/` | Human | Immutable source documents — articles, papers, transcripts. The source of truth. |
| `wiki/` | LLM | Generated markdown pages — summaries, entity pages, concept pages, syntheses. |
| `CLAUDE.md` | Co-owned | [[CLAUDE.md Schema]] — tells the LLM how the wiki is structured and what workflows to follow. |

Three operations:

**Ingest** — Human drops a source in `raw/`, tells the LLM to process it. LLM reads, extracts key information, writes/updates wiki pages (typically 10–15 pages per source), updates index, appends to log.

**Query** — Human asks a question. LLM reads `index.md`, drills into relevant pages, synthesizes a cited answer. Good answers are filed back as synthesis pages — explorations compound just like ingested sources.

**Lint** — Periodic health check. LLM scans for: orphan pages, broken wikilinks, stale claims, missing cross-references, frontmatter gaps, contradictions between pages.

Two navigation primitives:
- `wiki/index.md` — content catalog organized by page type. LLM reads first on every query.
- `wiki/log.md` — append-only chronological record, grep-parseable (`## [YYYY-MM-DD]` prefix).

## Relevance to This Wiki

This page describes the system this wiki itself runs on. The pattern is self-referential: this wiki is an instance of the LLM Wiki Pattern, and `wiki.md` is the meta-specification source.

## Key Sources

[[LLM Wiki Pattern]] (source summary)

## Related Concepts

- [[RAG vs Wiki]] — the contrast that motivates this pattern
- [[Knowledge Compounding]] — the core value proposition
- [[CLAUDE.md Schema]] — the configuration layer that makes it work
- [[Memex]] — the 1945 precursor vision by [[Vannevar Bush]]
