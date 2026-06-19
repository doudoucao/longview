# Implementation Plan

## Checklist

- [x] Confirm final report audience and depth preference with the user: reports and intermediate notes should be written in Chinese, with deeper source-level research.
- [x] Gather current sources from primary documentation, public filings/regulators, research papers, technical blogs, GitHub repositories, reputable market/industry analysis, product docs, and relevant open-source implementations.
- [x] Create `sources/index.md` and preserve intermediate raw materials or source notes under the task's `sources/` directory.
- [x] Create structured source notes with source type, access date, saved form, claims, data acquisition method, related agent skill/tool, limitations, and confidence.
- [x] Create `core-synthesis.md` as a standalone extraction of core patterns, early conclusions, and remaining research gaps from the saved raw materials.
- [x] Build a taxonomy of agent automated investment research paradigms.
- [x] Build a data acquisition map covering filings, market data, news/events, papers/blogs/GitHub, APIs, scraping/download paths, licensing limits, freshness, and automation difficulty.
- [x] Build an agent skills/tooling inventory covering search, scrape/crawl, parse, extract, normalize, validate, model, compare, cite, review, red-team, monitor, and report generation.
- [x] Compare at least 12 tools, skills, frameworks, products, or public reference implementations.
- [x] Define Longview's evidence rubric and claim-verification workflow.
- [x] Propose a Longview operating model and MVP roadmap.
- [x] Review for citation coverage, raw-material coverage, stale claims, compliance boundaries, and unmarked inference.
- [x] Run a second deep-dive collection pass over high-value papers, GitHub repositories, official samples, and code-level workflow examples.
- [x] Save second-wave raw materials under `sources/raw/deep-dive/`.
- [x] Write Chinese deep-dive notes for every second-wave high-value source in `sources/notes/deep-dive-source-notes.md`.
- [x] Rewrite first-wave per-source notes in Chinese so formal intermediate notes are not left as English-only artifacts.
- [x] Create `deep-dive-synthesis.md` as the second-wave bridge between raw materials and the final report.
- [x] Rewrite `research.md` as a Chinese formal deep-dive report incorporating first- and second-wave evidence.
- [x] Update `sources/index.md` with second-wave commands, raw files, high-value sources, artifacts, limitations, and note paths.

## Validation

- Every material "current/latest/leading" claim has a recent source or is downgraded to a dated observation.
- Every framework/product comparison cites an original or authoritative source where possible.
- Every important source has a source-index entry and either a saved raw artifact, source note, or explicit reason raw content was not saved.
- `core-synthesis.md` exists separately from `sources/index.md` and references the saved source set.
- The final report includes data acquisition, skills/tooling, and workflow automation tables.
- The final report separates product recommendations from investment advice.
- The report includes explicit limitations and unresolved questions.
- Formal reports and formal intermediate notes for this task are in Chinese.
- Every second-wave high-value source has a Chinese note and a saved raw artifact or explicit saved search/read output.
- Deep-dive raw files are indexed and linked from `sources/index.md`.

## Risky Areas

- AI tooling and market data products change quickly; verify versions and product claims during the research pass.
- Financial and regulatory claims require current authoritative sources.
- Product marketing pages may overstate capabilities; prefer docs, demos, filings, papers, code, and independent evidence when possible.
- Raw content may be subject to copyright, paywall, robots, or account-access restrictions; preserve metadata and notes when full capture is inappropriate.

## Rollback

This task primarily creates research artifacts. If the direction proves too broad, split into child tasks:

- AI research automation frameworks
- financial data and filing pipelines
- Longview operating model
- compliance and evidence standards
