# [Topic] Wiki — Schema & Conventions

This directory is a persistent, compounding knowledge artifact on a topic of your choice. It follows the LLM-maintained wiki pattern: raw sources come in, the LLM synthesizes and maintains wiki pages, and the index + log stay current automatically.

---

## Directory Layout

```
<topic>/
├── CLAUDE.md          ← this file; schema & conventions
├── index.md           ← catalog of all wiki pages (LLM-maintained)
├── log.md             ← append-only ingest/query history
├── raw/               ← immutable source documents (never modified)
│   └── ...
└── wiki/              ← LLM-generated markdown pages
    └── ...
```

---

## Roles

**Human:** curates sources, directs analysis, asks questions, decides what matters.  
**LLM:** ingests sources, writes/updates wiki pages, maintains index and log, flags contradictions.

---

## Workflows

### Ingest

Triggered when the user adds a file to `raw/` or pastes a URL/text.

1. Read the source.
2. Discuss key takeaways with the user if unclear.
3. Write or update a summary page in `wiki/`.
4. Update `index.md` (add or revise the entry for the new/updated page).
5. Revise up to ~15 existing wiki pages that intersect with the new source.
6. Append an entry to `log.md`: `## [YYYY-MM-DD] ingest | <Source Title>`.

### Query

Triggered when the user asks a question.

1. Read `index.md` to locate relevant pages.
2. Read those pages.
3. Synthesize an answer with citations (wiki page names).
4. If the answer is non-trivial and novel, create a new wiki page for it.
5. Append to `log.md`: `## [YYYY-MM-DD] query | <Question Summary>`.

### Lint

Triggered periodically or on request (`/lint`).

1. Scan all wiki pages for: contradictions, stale claims, orphaned pages, missing cross-references, data gaps.
2. Report findings and proposed fixes.
3. Propose new questions or sources to investigate.

---

## Wiki Page Conventions

- One concept, entity, or synthesis per file.
- Filename: `wiki/<slug>.md` using lowercase kebab-case.
- Each page has a `# Title` H1, a one-paragraph summary at the top, then sections as needed.
- Cross-references use markdown links: `[concept](concept.md)`.
- Claim provenance: cite source filenames from `raw/` inline, e.g. `(src: raw/article.pdf)`.

## index.md Conventions

Group entries by category. Each entry is one line:

```
- [Page Title](wiki/slug.md) — one-line summary · sources: N · updated: YYYY-MM-DD
```

## log.md Conventions

Append-only. Most recent entry at the bottom. Format:

```
## [YYYY-MM-DD] <action> | <title>
<one or two sentence description of what was done>
```

Actions: `ingest`, `query`, `lint`, `schema`.
