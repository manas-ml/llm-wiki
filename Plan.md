# Implementation Plan

Phased approach — start simple, improve with each phase. Each phase has a concrete verification step.

---

## Phase 1: Bootstrap ✓

**Goal**: Skeleton in place. Claude can orient itself in a new session.

Files created:
- `CLAUDE.md` — schema and operating instructions
- `Architecture.md` — design decisions
- `Plan.md` — this file
- `wiki/index.md` — empty content catalog
- `wiki/log.md` — empty activity log
- `raw/` — source document directory
- `raw/assets/` — image asset directory
- `wiki/entities/`, `wiki/concepts/`, `wiki/sources/`, `wiki/syntheses/` — page directories

**Verification**: Start a new Claude Code session in `llm-wiki/`. Claude should self-execute the session start protocol and reply:
> "Ready. Wiki has 0 pages across 0 sources. No prior activity."

---

## Phase 2: First Ingest

**Goal**: One real source processed end-to-end. Validates templates and conventions.

Steps:
1. Find a real source (article, paper, transcript, PDF) relevant to your domain.
2. Save it as a markdown file in `raw/` (use Obsidian Web Clipper, `wget`, or paste directly).
3. Tell Claude: `ingest [filename]`
4. Claude reads the source, discusses 3–5 key takeaways, then:
   - Writes `wiki/sources/<Title>.md`
   - Creates entity and concept pages
   - Updates `wiki/index.md`
   - Appends to `wiki/log.md`
5. After the ingest, read the output and refine `CLAUDE.md` if anything was off.

**Verification**:
```bash
grep "^## \[" wiki/log.md | wc -l        # → 1
find wiki/ -name "*.md" | grep -v index | grep -v log | wc -l  # → ≥ 2 new pages
```

---

## Phase 3: Query Workflow

**Goal**: Ask a cross-source question, get a cited answer, file it.

Steps:
1. Ingest 2–3 more sources (repeat Phase 2).
2. Ask a question that requires synthesizing across sources:
   - `query: [your question]` or just ask naturally
3. Claude reads `index.md`, identifies relevant pages, synthesizes an answer with `[[wikilinks]]`.
4. Claude asks: "Should I file this as a wiki page?" — say yes.
5. Claude creates `wiki/syntheses/<Title>.md` and updates `index.md` and `log.md`.

**Verification**:
```bash
grep "^## \[.*query" wiki/log.md         # shows query log entry
find wiki/syntheses/ -name "*.md"        # lists filed synthesis page
```

---

## Phase 4: Lint

**Goal**: Health-check the wiki. Find orphans, gaps, contradictions.

Steps:
1. After 5+ sources ingested, tell Claude: `lint`
2. Claude runs the 6-item checklist (defined in CLAUDE.md) and reports findings.
3. Fix at least one issue from each category found.
4. Verify the log has a lint entry.

**Verification**:
```bash
grep "^## \[.*lint" wiki/log.md          # shows lint entry with findings
```

---

## Phase 5: Tooling (Optional)

Add as the wiki grows. Implement whichever you need, in any order.

### 5a: Git version control
```bash
cd llm-wiki
git init
echo "raw/assets/" >> .gitignore
git add -A
git commit -m "Initial wiki scaffold"
```
After each session: `git add -A && git commit -m "Session YYYY-MM-DD: [brief summary]"`

### 5b: Grep search script

Create `tools/search.sh`:
```bash
#!/bin/bash
# Usage: ./tools/search.sh "query term"
grep -rli "$1" wiki/ --include="*.md"
```
```bash
chmod +x tools/search.sh
```
Tell Claude: "Use `bash tools/search.sh \"term\"` when index.md doesn't identify relevant pages clearly."

### 5c: qmd search engine

Install [qmd](https://github.com/tobi/qmd), then add to `CLAUDE.md`:
- CLI: `qmd search "query" --dir wiki/`
- MCP: configure in `.claude/settings.json` as an MCP server

### 5d: Dataview / Obsidian Bases queries

If viewing in Obsidian, add query blocks to `wiki/index.md`:
````
```dataview
TABLE updated, tags FROM "wiki/entities" SORT updated DESC
```
````
Requires Dataview community plugin, or use native Obsidian Bases for simpler queries.

### 5e: Marp slide decks

Install Obsidian Marp plugin. Tell Claude: "Convert [[Synthesis Title]] into a Marp slide deck." Claude adds `marp: true` frontmatter and `---` section breaks.

---

## Evolving CLAUDE.md

After each phase, review `CLAUDE.md` and refine it:
- Did Claude follow the ingest steps correctly? If not, clarify the instructions.
- Did it create the right page types? Adjust the taxonomy if needed.
- Did the templates produce pages you want to read? Modify the templates.

`CLAUDE.md` is a living document. The more precisely it describes what you want, the more consistently Claude delivers it.
