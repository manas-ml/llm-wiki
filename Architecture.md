# Architecture

Design decisions behind this wiki system. Reference for the human — operating instructions are in CLAUDE.md.

---

## Directory Philosophy

| Directory | Owner | Rule |
|-----------|-------|------|
| `raw/` | Human | Immutable. Sources go in, nothing changes. LLM reads only. |
| `raw/assets/` | Human | Downloaded images referenced by raw sources. |
| `wiki/` | LLM | Entirely LLM-written. Human reads, not writes. |
| `CLAUDE.md` | Co-owned | Human and LLM evolve together as the domain matures. |

`raw/` is immutable so the LLM's transformations are fully auditable — you can always diff the source against what the LLM claimed it said.

---

## Page Type Taxonomy

| Type | Directory | Purpose |
|------|-----------|---------|
| `source` | `wiki/sources/` | One page per ingested document. What the source says, not what we think of it. |
| `entity` | `wiki/entities/` | A named, discrete thing: person, org, product, place, event. |
| `concept` | `wiki/concepts/` | An idea, framework, methodology, or term. |
| `synthesis` | `wiki/syntheses/` | A filed query answer, comparison, or cross-cutting analysis. |

---

## Cross-Reference Strategy

Wikilinks (`[[Page Name]]`) are the connective tissue. Rules:
- Link entity and concept names **on first mention** within any page.
- Use plain text for subsequent mentions on the same page.
- Source pages link to every entity and concept they introduce.
- Synthesis pages link back to every wiki page they drew from.

This keeps the graph meaningful without cluttering every sentence.

---

## Contradiction Handling

Contradictions are expected as the wiki grows. Protocol:
1. **Never silently overwrite** a claim.
2. Add a `> [!warning] Contradiction` callout below the conflicting claim, naming the source of the conflict.
3. Flag it in the log entry (`Contradictions flagged: yes`).
4. During lint passes, surface unresolved contradictions for human review.

This preserves epistemic history and makes uncertainty visible.

---

## Index vs. Log

**`wiki/index.md`** — content-oriented. A catalog organized by page type (sources, entities, concepts, syntheses). The LLM reads it first on every query to identify candidate pages. Updated on every ingest.

**`wiki/log.md`** — chronological. Append-only record of every operation (ingest, query, lint). Parseable with grep: `grep "^## \[" wiki/log.md | tail -10`. Helps the LLM understand recent activity across sessions.

---

## Frontmatter

Every wiki page (except `index.md` and `log.md`) carries YAML frontmatter:

```yaml
---
type: entity | concept | source | synthesis
tags: [tag1, tag2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: [Source Title 1, Source Title 2]
---
```

`updated` must be kept current — change it on every edit. `sources` lists every raw document that contributed to this page, enabling traceability.

---

## Scaling Path

| Scale | Navigation approach |
|-------|-------------------|
| < 50 sources, < 200 pages | `wiki/index.md` is sufficient |
| 50–200 sources, 200–1000 pages | Add `tools/search.sh` (grep-based) or qmd CLI |
| > 200 sources | qmd MCP server for proper hybrid search |

[qmd](https://github.com/tobi/qmd) provides hybrid BM25/vector search with LLM re-ranking, all on-device. It exposes both a CLI (LLM shells out) and an MCP server (LLM uses it as a native tool).

---

## Git

The `llm-wiki/` directory should be a git repo. This gives:
- Version history on all wiki pages
- Ability to diff what changed after each ingest
- Branching for experimental wiki reorganizations

Recommended `.gitignore`:
```
raw/assets/
```
(Images can be large; keep them local but untracked unless you want them versioned.)

---

## Why Not RAG

RAG re-derives knowledge from scratch on every query. This system compiles knowledge once (at ingest time) and keeps it current. The cross-references are already there. The contradictions are already flagged. The synthesis already reflects everything ingested. Queries become fast reads against structured wiki pages rather than fuzzy retrieval from raw chunks.

The tradeoff: the LLM must do real work at ingest time (touching 10–15 pages per source). This is fine because ingest is interactive and relatively infrequent. The payoff compounds with every source added.
