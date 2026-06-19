# Research Output Standards

## Final Report Structure

Research reports should include:

1. Executive summary
2. Scope and method
3. Source map, raw-material index, and evidence quality notes
4. Paradigm taxonomy
5. Data acquisition map
6. Agent skills and tool inventory
7. Automation workflow patterns
8. Framework/product comparison
9. Longview implications
10. Recommended operating model
11. MVP roadmap
12. Risks, compliance boundaries, and non-goals
13. Open questions and follow-up experiments
14. References

## Required Task Artifacts

For substantial research tasks, maintain separate artifacts instead of mixing everything into one report:

- `sources/index.md`: raw-material inventory, commands used, source metadata, access dates, saved forms, and limitations.
- `sources/raw/`: Firecrawl or other raw captures when allowed.
- `sources/notes/`: source-level notes and evidence annotations.
- `sources/extracts/`: structured extractions, normalized tables, parsed documents, or intermediate datasets.
- `core-synthesis.md`: a standalone synthesis of the core findings, patterns, early conclusions, and remaining research gaps.
- `research.md`: final report, written only after the raw material index and core synthesis are coherent enough to support it.

The `core-synthesis.md` file must not replace raw materials or source notes. It is the bridge between saved evidence and the final report.

## Notes Structure

Use structured notes for important sources:

- source title
- URL or local path
- publisher/author
- publication or access date
- source type
- saved raw-material path or reason not saved
- key claims
- relevant data acquisition method, if any
- relevant agent skill/tool, if any
- limitations
- relevance to Longview
- confidence

## Required Research Tables

For agent automation investment research tasks, include:

- Data source table: source category, access method, freshness, structure, licensing constraints, automation difficulty, Longview fit.
- Tool/skill table: skill name, input, output, dependencies, failure modes, candidate tools/frameworks, Longview fit.
- Workflow table: trigger, agent roles, tools used, intermediate artifacts, review gate, final output, automation risk.
- Framework/product table: name, category, source, capabilities, data acquisition fit, raw-material support, weaknesses, follow-up experiment.

## Writing Standards

- Separate facts from interpretation.
- Avoid vague claims such as "best", "most advanced", or "industry-leading" unless the ranking basis is defined.
- Preserve raw-material links and source notes before writing high-level synthesis.
- Write a separate `core-synthesis.md` for core pattern extraction before drafting the final `research.md`.
- Use tables for comparisons and concise prose for synthesis.
- State assumptions explicitly.
- Preserve uncertainty instead of smoothing it away.

## Language Requirements

- If the user requests Chinese research output, write all formal research reports, source notes, intermediate notes, synthesis files, and task-facing summaries in Chinese.
- Preserve original source titles, paper IDs, repository names, commands, URLs, and technical identifiers in their original language when translation would reduce traceability.
- Do not leave English scratch notes as the only formal intermediate artifact. If an English capture or draft exists, add or replace it with a Chinese source note before treating the task as complete.
- User-facing final summaries for PRs or task updates should follow the user's requested language unless the user explicitly asks otherwise.

## Review Checklist

Before considering a report done:

- Every material claim has support.
- Intermediate raw materials or source notes are saved and indexed.
- `core-synthesis.md` exists and points back to the relevant source index or source notes.
- Current claims were checked against current sources.
- Contradictions or weak evidence are visible.
- Recommendations target Longview's product/research process, not a reader's personal portfolio.
- The report can be used to create follow-up Trellis tasks.
- The language of formal notes and reports matches the user's explicit language preference.
