---
type: entity
tags: [tool, search, markdown, llm, cli, mcp]
created: 2026-05-10
updated: 2026-05-10
sources: [wiki.md, https://claude.com/blog/building-agents-with-skills-equipping-agents-for-specialized-work]
---

# qmd

**Type**: product / software tool
**Also known as**: tobi/qmd

## Overview

qmd is a local hybrid search engine for markdown files combining BM25 keyword search, vector semantic search, and LLM re-ranking — all running on-device with no cloud dependencies. It is integrated into this wiki as the search layer for candidate page discovery during QUERY operations, supplementing the `wiki/index.md` catalog. Models run via llama.cpp using locally cached GGUF files; no separate server process is required.

## Key Facts

| Attribute | Value |
|-----------|-------|
| Type | CLI tool + MCP server |
| Search modes | BM25 keyword (`qmd search`), vector semantic (`qmd vsearch`), hybrid + re-ranking (`qmd query`) |
| Runs | On-device via llama.cpp / GGUF models |
| Model cache | `~/.cache/qmd/` (~2.1GB for full hybrid mode) |
| Source | github.com/tobi/qmd |
| Install | `npm install -g @tobilu/qmd` |
| Wiki collection | `wiki` (indexes `llm-wiki/wiki/`) |

## Installation and Setup

```bash
# Install
npm install -g @tobilu/qmd

# Index the wiki directory
qmd collection add /Users/manasnarkar/Documents/llm-wiki/wiki --name wiki

# Generate embeddings for semantic search (downloads ~2.1GB of GGUF models on first run)
qmd embed

# Verify
qmd query "agent coordination patterns" --collection wiki --json
```

BM25 keyword search (`qmd search`) works immediately after `qmd collection add` with no model download. Semantic + re-ranking requires `qmd embed` to complete first.

## Local Models

| Model | Size | Purpose |
|-------|------|---------|
| EmbeddingGemma (300M) | ~300MB | Vector embeddings |
| Qwen3-Reranker (0.6B) | ~640MB | Re-ranking by relevance |
| Qwen3-Query-Expansion (1.7B) | ~1.1GB | Query expansion |

Models are loaded on-demand per query and unloaded when done. No persistent background process at rest.

## MCP Integration

qmd is configured as an MCP server for this wiki via `.mcp.json` at the project root:

```json
{
  "mcpServers": {
    "qmd": {
      "command": "qmd",
      "args": ["mcp"]
    }
  }
}
```

This gives Claude Code four native tools: `qmd:query`, `qmd:get`, `qmd:multi_get`, `qmd:status`.

## Key CLI Commands

| Command | Purpose |
|---------|---------|
| `qmd collection add <path> --name <id>` | Index a directory |
| `qmd search "<terms>" --collection wiki` | BM25 keyword search |
| `qmd query "<terms>" --collection wiki --json` | Hybrid search with re-ranking |
| `qmd update --collection wiki` | Re-index after file changes |
| `qmd embed` | Generate/update vector embeddings |

## Role in This Wiki

Integrated as Phase 5d tooling. The [[CLAUDE.md Schema]] QUERY operation calls `qmd:query` in Step 1 to surface semantically relevant candidate pages alongside the index.md catalog scan. INGEST Step 7 runs `qmd update` after each new source to keep the index current. At the current scale (64 pages / 9 sources), qmd supplements index.md navigation; at 100+ sources it becomes the primary discovery mechanism.

## Appears In

[[LLM Wiki Pattern]] (source summary), [[Building Agents with Skills: Equipping Agents for Specialized Work]]

## Related

- [[Obsidian]] — the viewer; qmd is the search complement
- [[LLM Wiki Pattern]] — the system it supports at scale
- [[Agentic Search]] — qmd implements the agentic search principle for this wiki
- [[Just-in-Time Context Retrieval]] — qmd enables JIT candidate discovery at scale
- [[CLAUDE.md Schema]] — the schema that governs how qmd is used in operations
