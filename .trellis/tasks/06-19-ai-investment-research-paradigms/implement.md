# Implementation Plan

## Checklist

- [ ] Confirm final report audience and depth preference with the user.
- [ ] Gather current sources from primary documentation, public filings/regulators, research papers, technical blogs, GitHub repositories, reputable market/industry analysis, product docs, and relevant open-source implementations.
- [ ] Create `sources/index.md` and preserve intermediate raw materials or source notes under the task's `sources/` directory.
- [ ] Create structured source notes with source type, access date, saved form, claims, data acquisition method, related agent skill/tool, limitations, and confidence.
- [ ] Create `core-synthesis.md` as a standalone extraction of core patterns, early conclusions, and remaining research gaps from the saved raw materials.
- [ ] Build a taxonomy of agent automated investment research paradigms.
- [ ] Build a data acquisition map covering filings, market data, news/events, papers/blogs/GitHub, APIs, scraping/download paths, licensing limits, freshness, and automation difficulty.
- [ ] Build an agent skills/tooling inventory covering search, scrape/crawl, parse, extract, normalize, validate, model, compare, cite, review, red-team, monitor, and report generation.
- [ ] Compare at least 12 tools, skills, frameworks, products, or public reference implementations.
- [ ] Define Longview's evidence rubric and claim-verification workflow.
- [ ] Propose a Longview operating model and MVP roadmap.
- [ ] Review for citation coverage, raw-material coverage, stale claims, compliance boundaries, and unmarked inference.

## Validation

- Every material "current/latest/leading" claim has a recent source or is downgraded to a dated observation.
- Every framework/product comparison cites an original or authoritative source where possible.
- Every important source has a source-index entry and either a saved raw artifact, source note, or explicit reason raw content was not saved.
- `core-synthesis.md` exists separately from `sources/index.md` and references the saved source set.
- The final report includes data acquisition, skills/tooling, and workflow automation tables.
- The final report separates product recommendations from investment advice.
- The report includes explicit limitations and unresolved questions.

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
