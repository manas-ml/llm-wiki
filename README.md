# llm-wiki

A personal knowledge base maintained by Claude. You drop in sources (PDFs, articles, URLs); Claude reads them, writes structured wiki pages, cross-links concepts, flags contradictions, and answers questions against the compiled knowledge.


---

## How it works

| Role | Responsibility |
|------|---------------|
| You | Add raw sources to `raw/`, give Claude commands |
| Claude | Reads sources, writes/updates wiki pages, maintains the index and log |

Claude never modifies `raw/` and you never write to `wiki/`. The separation makes Claude's transformations fully auditable.

**Three operations:**

- **INGEST** — `ingest <filename>` or `ingest <url>`: Claude reads the source, writes a summary page, creates/updates entity and concept pages, updates the index, appends to the log.
- **QUERY** — `query: <question>` or just ask a question: Claude reads relevant wiki pages and synthesizes an answer. Optionally files the answer as a synthesis page.
- **LINT** — `lint`: Claude audits for orphan pages, broken wikilinks, missing index entries, stale claims, frontmatter gaps.

---

## Directory structure

```
llm-wiki/
├── raw/               READ-ONLY. Source documents (PDFs, saved articles).
│   └── assets/        Images referenced by raw sources.
├── wiki/              LLM-written. Do not edit directly.
│   ├── index.md       Master catalog of all pages. Read this to orient yourself.
│   ├── log.md         Append-only operation log. Grep-parseable.
│   ├── sources/       One summary page per ingested source.
│   ├── entities/      Named things: people, orgs, products, tools.
│   ├── concepts/      Ideas, frameworks, methodologies, terms.
│   └── syntheses/     Filed query answers and cross-cutting analyses.
├── CLAUDE.md          Operating instructions for Claude. The schema lives here.
├── Architecture.md    Design decisions reference.
├── Plan.md            Phased roadmap.
└── .mcp.json          MCP server config (qmd search integration).
```

---

## Getting started

### 1. Open in Claude Code

```bash
cd llm-wiki
claude
```

Claude will run the Session Start Protocol automatically: read `CLAUDE.md`, read `wiki/index.md`, read recent log entries, then report the wiki's current state.

### 2. Add a source

```bash
# Copy a PDF into raw/
cp ~/Downloads/some-paper.pdf raw/

# Then in Claude Code:
ingest some-paper.pdf

# Or ingest a URL directly:
ingest https://example.com/article
```

### 3. Query the wiki

```
query: what are the tradeoffs between hierarchical and collaborative multi-agent systems?
```

### 4. Recommended viewer

[Obsidian](https://obsidian.md) — open the `wiki/` folder as a vault. Wikilinks, backlinks, and the graph view work out of the box.

---

## qmd search integration

[qmd](https://github.com/tobi/qmd) is a local hybrid search engine for markdown files. It combines BM25 keyword search, vector semantic search, and LLM re-ranking — all on-device with no cloud dependencies. At wiki scale (50+ sources), it becomes the primary candidate discovery mechanism during QUERY operations.

qmd is already wired into this wiki via `.mcp.json`. Once installed, Claude Code gets four native tools: `qmd:query`, `qmd:get`, `qmd:multi_get`, `qmd:status`.

### Installation

```bash
npm install -g @tobilu/qmd
```

### Index the wiki

```bash
qmd collection add /path/to/llm-wiki/wiki --name wiki
```

### Generate embeddings (optional)

BM25 keyword search works immediately after indexing. For semantic search and LLM re-ranking, run:

```bash
qmd embed
```

This downloads ~2.1GB of GGUF models on first run (cached at `~/.cache/qmd/` afterward). The three models:

| Model | Size | Purpose |
|-------|------|---------|
| EmbeddingGemma (300M) | ~300MB | Vector embeddings |
| Qwen3-Reranker (0.6B) | ~640MB | Re-ranking by relevance |
| Qwen3-Query-Expansion (1.7B) | ~1.1GB | Query expansion |

Models load on-demand per query and unload when done — no persistent background process.

### Verify

```bash
qmd query "agent coordination patterns" --collection wiki --json
```

You should see ranked wiki pages with relevance scores. Then start a new Claude Code session in `llm-wiki/` — `qmd:query` will be available as a native tool.

### Keep the index current

After each ingest, Claude runs `qmd update --collection wiki` automatically (INGEST Step 7 in `CLAUDE.md`). To update manually:

```bash
qmd update --collection wiki
```

### Fallback behavior

If qmd is not installed, Claude falls back to `wiki/index.md` alone for candidate page discovery. The wiki remains fully functional — qmd is an enhancement, not a dependency.

---

## Current state

See `wiki/index.md` for the full catalog. Key stats are in the header (total pages, total sources, last updated date).

See `wiki/log.md` for the operation history. Quick parse:

```bash
# Last 10 operations
grep "^## \[" wiki/log.md | tail -10

# All ingests
grep "^## \[.*ingest" wiki/log.md

# All queries
grep "^## \[.*query" wiki/log.md
```
