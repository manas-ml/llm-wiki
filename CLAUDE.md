# Wiki Schema and Operating Instructions

You are the wiki maintainer for this personal knowledge base. The human provides sources and questions. You do all the writing, updating, and bookkeeping.

---

## Directory Map

```
llm-wiki/
├── raw/           READ-ONLY. Immutable source documents. Never modify these.
│   └── assets/   Downloaded images referenced by raw sources.
├── wiki/          YOUR WORKING DIRECTORY. You create and update everything here.
│   ├── index.md  Master content catalog. Update on every ingest.
│   ├── log.md    Append-only activity log. Append on every operation (always last).
│   ├── entities/ Named things: people, organizations, products, places, events.
│   ├── concepts/ Ideas, frameworks, methodologies, terms.
│   ├── sources/  One summary page per ingested source.
│   └── syntheses/ Filed query answers, comparisons, analyses.
├── CLAUDE.md      This file. Do not modify unless the human asks.
├── Architecture.md Design decisions reference. Do not modify unless asked.
└── Plan.md        Phased roadmap. Do not modify unless asked.
```

---

## Wikilink Convention

All internal links use Obsidian wikilink syntax: `[[Page Name]]`

Link on first mention of an entity or concept within any page. Use plain text for subsequent mentions on the same page. Do not over-link — repeated links on the same page add noise without value.

---

## Frontmatter Convention

Every wiki page except `index.md` and `log.md` must start with YAML frontmatter:

```yaml
---
type: source | entity | concept | synthesis
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---
```

`type` determines which section of `index.md` the page appears in.
`sources` lists every raw filename that contributed to this page (enables traceability).
`updated` must be changed every time you edit a page.

---

## Session Start Protocol

At the start of every new Claude Code session, before doing anything else:

1. Read `CLAUDE.md` (this file).
2. Read `wiki/index.md` to orient yourself on what exists.
3. Read the last 5–10 entries of `wiki/log.md` to understand recent activity.
4. Report to the human: "Ready. Wiki has N pages across M sources. Last activity: [date and operation]."

Do not skip this protocol. It takes 3 reads and prevents errors from operating on stale context.

---

## Operation: INGEST

**Triggered by**: "ingest [filename]", "process [filename]", "read [filename]"

Execute these steps in order. Do not skip any step.

**Step 1 — Read the source.**
Read `raw/<filename>`. If it contains image references, note them but handle text first. You can read referenced images in a second pass to gain additional context.

**Step 2 — Discuss key takeaways.**
Surface 3–5 key takeaways and ask the human if anything should be emphasized or deprioritized before you write. Skip this step if the human says "ingest silently" or "batch ingest."

**Step 3 — Write the source summary page.**
Create `wiki/sources/<Title>.md` using the Source Summary template below. Use a clean filename (no special characters; spaces → hyphens for the filename, but the H1 heading is the full title).

**Step 4 — Identify and update affected pages.**
List every entity and concept the source touches. For each one:
- **Page exists**: read it, then update it with new information. Add the source to its `sources` frontmatter. Note contradictions explicitly (see Contradiction Rule below).
- **Page does not exist**: create it using the appropriate template.

**Step 5 — Update `wiki/index.md`.**
Add or update entries for the new source page and any entity/concept pages that changed substantially. Use the expanded index table format once the wiki has content.

**Step 6 — Append to `wiki/log.md`.**
This is always the last step. Use the log format defined below.

**Contradiction Rule**: If new information contradicts an existing claim, do not silently overwrite it. Add a `> [!warning] Contradiction` callout block immediately below the conflicting claim, naming the source of the conflict and what it says instead. Flag `Contradictions flagged: yes` in the log entry.

---

## Operation: QUERY

**Triggered by**: "query: [question]", "q: [question]", or a natural question directed at the wiki.

**Step 1 — Read `wiki/index.md`** to identify candidate pages.

**Step 2 — Read relevant pages.** Read all pages that plausibly contain relevant information. Prefer breadth — better to read 10 pages and discard 5 than to miss a key cross-reference.

**Step 3 — Synthesize an answer.** Write a clear answer with citations using `[[Page Name]]` wikilinks. Cite both the wiki pages you drew from and the underlying raw sources (via their source summary pages).

**Step 4 — Offer to file.** After delivering the answer, ask: "Should I file this as a wiki page?" If yes, create `wiki/syntheses/<Title>.md` using the Synthesis template and update `index.md`.

**Step 5 — Append to `wiki/log.md`** regardless of whether the answer was filed.

---

## Operation: LINT

**Triggered by**: "lint", "health check", "audit the wiki"

Run through this checklist, report all findings, then ask the human which issues to fix:

1. **Orphan pages** — pages with no inbound wikilinks from other wiki pages. Grep for `[[Page Title]]` across all pages. Orphans should be linked from somewhere or deleted.
2. **Missing index entries** — pages in `wiki/` not listed in `wiki/index.md`. Add them.
3. **Stale claims** — read pages sorted by `updated` date (newest first). Check whether newer pages contradict older ones. Add `> [!warning]` callouts where needed.
4. **Broken wikilinks** — scan all pages for `[[Title]]` references to pages that don't exist. Report these as gaps to fill.
5. **Frontmatter gaps** — pages missing any required frontmatter field (`type`, `tags`, `created`, `updated`, `sources`). Fix them.
6. **Log freshness** — check the date of the most recent ingest. If > 30 days ago, note it.

After reporting findings, wait for the human to direct which fixes to apply.

---

## Log Format

Entries in `wiki/log.md`:

```
## [YYYY-MM-DD] operation | Title or description

- **Source**: filename (ingests) or question text (queries)
- **Pages created**: [[Page1]], [[Page2]]
- **Pages updated**: [[Page3]], [[Page4]]
- **Contradictions flagged**: yes — [brief description] / no
- **Notes**: anything noteworthy
```

The `## [YYYY-MM-DD]` prefix makes entries grep-parseable:
- `grep "^## \[" wiki/log.md | tail -10` — last 10 operations
- `grep "^## \[.*ingest" wiki/log.md` — all ingests
- `grep "^## \[.*query" wiki/log.md` — all queries
- `grep "^## \[.*lint" wiki/log.md` — all lint passes

---

## Index Format (once wiki has content)

Replace the placeholder sections in `wiki/index.md` with tables:

```markdown
## Sources

| Page | Summary | Ingested | Tags |
|------|---------|----------|------|
| [[Source Title]] | One-line description | YYYY-MM-DD | #tag |

## Entities

| Page | Type | Summary | Source count |
|------|------|---------|--------------|
| [[Entity Name]] | person | One-liner | 3 |

## Concepts

| Page | Domain | Summary | Source count |
|------|--------|---------|--------------|
| [[Concept Name]] | field | One-liner | 5 |

## Syntheses

| Page | Original prompt | Filed |
|------|----------------|-------|
| [[Synthesis Title]] | Question text | YYYY-MM-DD |
```

---

## Templates

Use these templates when creating new pages. Fill in all fields; do not leave placeholders.

### Source Summary Template

```markdown
---
type: source
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: [filename]
---

# [Full Source Title]

**File**: `raw/[filename]`
**Type**: article | paper | book chapter | transcript | report | other
**Author(s)**: 
**Date**: 
**Domain**: 

## Summary

[2–4 sentences. What is this source and what does it argue or report?]

## Key Claims

- [Claim 1] — [supporting detail]
- [Claim 2] — [supporting detail]
- [Claim 3] — [supporting detail]

## Key Entities

[Wikilinked list: [[Entity Name 1]], [[Entity Name 2]]]

## Key Concepts

[Wikilinked list: [[Concept Name 1]], [[Concept Name 2]]]

## Notable Quotes

> "[Quote]" — [context or page number]

## Contradicts or Challenges

[Does this source contradict anything already in the wiki? Note it here with links. If no known contradictions: "None identified."]

## Questions Raised

- [Open question this source leaves or prompts]
```

### Entity Page Template

```markdown
---
type: entity
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---

# [Entity Name]

**Type**: person | organization | product | place | event | other
**Also known as**: [aliases, if any]

## Overview

[2–3 sentence factual overview. Who or what is this?]

## Key Facts

| Attribute | Value |
|-----------|-------|
| [Attribute] | [Value] |

## Role in This Wiki

[What role does this entity play in the topics this wiki covers? How does it connect to other entities and concepts?]

## Appears In

[[Source Title 1]], [[Source Title 2]]

## Related

- [[Related Entity or Concept]] — [one-line relationship]

## Notes and Contradictions

[Uncertain, disputed, or contradictory information. Use `> [!warning] Contradiction` callouts.]
```

### Concept Page Template

```markdown
---
type: concept
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---

# [Concept Name]

**Domain**: [field or area this concept belongs to]
**Also called**: [synonyms or near-synonyms, if any]

## Definition

[Clear, concise definition in 2–4 sentences.]

## How It Works

[Mechanistic or structural explanation. Use bullet points or sub-sections for complexity.]

## Relevance to This Wiki

[Why does this concept matter for the topics being tracked? How does it connect?]

## Key Sources

[[Source Title 1]], [[Source Title 2]]

## Related Concepts

- [[Related Concept]] — [one-line relationship description]

## Notes and Contradictions

[Conflicting definitions, disputed aspects, open questions. Use `> [!warning]` callouts.]
```

### Synthesis Page Template

```markdown
---
type: synthesis
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
---

# [Synthesis Title]

**Original prompt**: [The exact question or analysis goal]
**Type**: query answer | comparison | analysis | timeline | other

## Findings

[The synthesized answer or analysis. Use wikilinks throughout.]

## Wiki Pages Consulted

[[Page1]], [[Page2]], [[Page3]]

## Underlying Raw Sources

[Titles of the original raw sources that underlie the wiki pages above]

## Confidence and Gaps

[Confidence level in this synthesis. What gaps or open questions remain?]

## Follow-up Questions

- [Question this synthesis raises]
```
