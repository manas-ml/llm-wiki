---
type: entity
tags: [tool, search, markdown, llm, cli]
created: 2026-05-10
updated: 2026-05-10
sources: [wiki.md]
---

# qmd

**Type**: product / software tool
**Also known as**: tobi/qmd

## Overview

qmd is a local search engine for markdown files with hybrid BM25/vector search and LLM re-ranking, all running on-device. It is designed to index and search markdown knowledge bases. In the [[LLM Wiki Pattern]], it becomes relevant when the wiki grows beyond the scale where `wiki/index.md` alone is sufficient for the LLM to navigate (~100+ sources, ~hundreds of pages).

## Key Facts

| Attribute | Value |
|-----------|-------|
| Type | CLI tool + MCP server |
| Search | Hybrid BM25 + vector, LLM re-ranking |
| Runs | On-device (no cloud required) |
| Source | github.com/tobi/qmd |
| Integration | CLI (shell out) or MCP server (native tool) |

## Role in This Wiki

Optional Phase 5 tooling. Currently not needed — the wiki has 1 source and ~15 pages, well within index.md navigation. Relevant when the wiki reaches 50–200+ sources.

## Appears In

[[LLM Wiki Pattern]] (source summary)

## Related

- [[Obsidian]] — the viewer; qmd is the search complement
- [[LLM Wiki Pattern]] — the system it supports at scale
