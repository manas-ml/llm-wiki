---
type: source
tags: [llm, wiki, knowledge-management, rag, architecture]
created: 2026-05-10
updated: 2026-05-10
sources: [wiki.md]
---

# LLM Wiki Pattern

**File**: `wiki.md`
**Type**: idea document / pattern specification
**Author(s)**: unknown (personal project pattern)
**Date**: 2026
**Domain**: knowledge management, LLM tooling

## Summary

A pattern for building personal knowledge bases using LLMs, distinct from RAG. Instead of retrieving from raw documents at query time, the LLM incrementally builds and maintains a persistent wiki — a structured, interlinked collection of markdown files. Knowledge is compiled once at ingest time and kept current, not re-derived on every query. The document is intentionally abstract; the specifics are meant to be instantiated collaboratively with an LLM agent.

## Key Claims

- [[RAG vs Wiki]] — RAG re-derives knowledge on every query with no accumulation; the wiki compiles it once and the result compounds with every source added
- The wiki is a **persistent, compounding artifact**: cross-references already built, contradictions already flagged, synthesis already reflects everything ingested
- The human's job is curation, sourcing, and asking good questions; the LLM's job is all the bookkeeping (summarizing, cross-referencing, filing, consistency maintenance)
- [[CLAUDE.md Schema]] is the key configuration file — it transforms the LLM from a generic chatbot into a disciplined wiki maintainer
- Good query answers should be filed back into the wiki as synthesis pages — explorations compound just like ingested sources
- The pattern is related in spirit to [[Vannevar Bush]]'s [[Memex]] (1945): private, actively curated, with associative trails as valuable as the documents themselves. The LLM solves the part Bush couldn't — who does the maintenance

## Key Entities

[[Vannevar Bush]], [[Obsidian]], [[qmd]], [[Tolkien Gateway]]

## Key Concepts

[[LLM Wiki Pattern]], [[RAG vs Wiki]], [[Knowledge Compounding]], [[CLAUDE.md Schema]], [[Memex]]

## Notable Quotes

> "The wiki is a persistent, compounding artifact. The cross-references are already there. The contradictions have already been flagged. The synthesis already reflects everything you've read."

> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase."

> "The tedious part of maintaining a knowledge base is not the reading or the thinking — it's the bookkeeping. Humans abandon wikis because the maintenance burden grows faster than the value. LLMs don't get bored."

## Contradicts or Challenges

None identified. (Note: this source describes the system this wiki itself implements — it is both a source and the meta-specification.)

## Questions Raised

- At what scale does index.md navigation break down and embedding-based search become necessary?
- How should schema (CLAUDE.md) evolve as the domain matures — what triggers a schema revision?
- How do multi-user or team wikis handle conflicting LLM edits to the same page?
