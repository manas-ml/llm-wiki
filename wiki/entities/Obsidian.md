---
type: entity
tags: [tool, markdown, knowledge-management, note-taking]
created: 2026-05-10
updated: 2026-05-10
sources: [wiki.md]
---

# Obsidian

**Type**: product / software
**Also known as**: Obsidian.md

## Overview

Obsidian is a markdown-based note-taking and knowledge management application. It renders wikilinks (`[[Page Name]]`), backlinks, and a graph view showing connections between pages — all without a build step, working directly on local markdown files. In the [[LLM Wiki Pattern]], Obsidian serves as the IDE: the human browses the wiki in Obsidian in real time while the LLM writes and updates pages.

## Key Facts

| Attribute | Value |
|-----------|-------|
| Type | Desktop app (Mac, Windows, Linux, mobile) |
| File format | Plain markdown |
| Key features | Wikilinks, backlinks, graph view, canvas, plugins |
| Storage | Local files (also supports sync) |

## Key Features Relevant to This Wiki

- **Graph view** — visualizes the link structure of the wiki; best way to spot orphan pages and hub pages
- **Backlinks panel** — shows every page that links to the current page
- **Wikilink resolution** — resolves `[[Page Name]]` by filename, not path, matching the wiki's convention
- **Web Clipper** — browser extension that converts articles to markdown for dropping into `raw/`
- **Dataview / Bases plugin** — queries over frontmatter fields (type, tags, updated, sources)
- **Marp plugin** — renders markdown as slide decks

## Appears In

[[LLM Wiki Pattern]] (source summary)

## Related

- [[LLM Wiki Pattern]] — Obsidian is the recommended viewer/browser for the wiki
- [[qmd]] — complementary search tool for when the wiki grows beyond index.md navigation
