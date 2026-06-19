# Raw Material Preservation

## Purpose

Research tasks must preserve intermediate raw materials so another analyst or agent can reconstruct the work. A final report without source notes, raw links, and retrieval context is not sufficient for Longview investment research.

## Directory Convention

For research tasks, prefer this structure under the active task directory:

```text
core-synthesis.md
sources/
  index.md
  raw/
  notes/
  extracts/
```

- `sources/index.md`: source inventory and audit trail.
- `sources/raw/`: saved raw materials when allowed, such as markdown exports, PDFs, screenshots, or cloned metadata snapshots.
- `sources/notes/`: structured source notes written by the agent.
- `sources/extracts/`: structured extractions, tables, schemas, or normalized snippets.
- `core-synthesis.md`: standalone extraction of core findings and reusable patterns from the raw source set.

Do not save secrets, credentials, private account data, or unauthorized paywalled content.

## Source Index Fields

Each source index entry should include:

- id
- title
- URL or local path
- author / publisher
- publication date if available
- access date
- source type: paper, blog, GitHub, docs, filing, API docs, product page, report, dataset, or other
- saved form: markdown, PDF, screenshot, excerpt, metadata-only, link-only, clone, or generated note
- why it matters
- key claims or capabilities
- limitations and access restrictions
- confidence
- related final-report section

## Raw Content Rules

- Save original content when permission and format allow.
- For copyrighted or access-controlled sources, prefer metadata, short excerpts within fair-use limits, source notes, and links instead of full copies.
- For GitHub repositories, save URL, commit/tag when available, license, README summary, relevant file paths, and examples inspected. Clone only when necessary and permitted.
- For papers, save DOI/arXiv/SSRN URL, abstract summary, key methods, and citation metadata. Save PDFs only when access rights allow.
- For blogs and docs, save URL, access date, key excerpts, and a markdown summary.

## Minimum Before Synthesis

Before writing synthesis, the agent should have:

- a source index with all important sources
- source notes for high-value sources
- explicit labels for sources that could not be saved
- enough metadata for another analyst to retrieve or verify each source

After that, write a separate `core-synthesis.md` before drafting the final report. It should summarize core patterns, early conclusions, tables worth preserving, and remaining research gaps while linking back to `sources/index.md`.

## Review Checklist

- Can another analyst find every source used in the final report?
- Are raw materials or notes separated from synthesis?
- Does `core-synthesis.md` exist separately from raw notes and the final report?
- Are access, copyright, and paywall limits documented?
- Are GitHub repositories pinned to a commit, release, or access date where possible?
- Are papers and blogs clearly labeled as evidence, opinion, or inspiration?
