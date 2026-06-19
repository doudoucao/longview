<!-- TRELLIS:START -->
# Trellis Instructions

These instructions are for AI assistants working in this project.

This project is managed by Trellis. The working knowledge you need lives under `.trellis/`:

- `.trellis/workflow.md` — development phases, when to create tasks, skill routing
- `.trellis/spec/` — package- and layer-scoped coding guidelines (read before writing code in a given layer)
- `.trellis/workspace/` — per-developer journals and session traces
- `.trellis/tasks/` — active and archived tasks (PRDs, research, jsonl context)

If a Trellis command is available on your platform (e.g. `/trellis:finish-work`, `/trellis:continue`), prefer it over manual steps. Not every platform exposes every command.

If you're using Codex or another agent-capable tool, additional project-scoped helpers may live in:
- `.agents/skills/` — reusable Trellis skills
- `.codex/agents/` — optional custom subagents

Managed by Trellis. Edits outside this block are preserved; edits inside may be overwritten by a future `trellis update`.

<!-- TRELLIS:END -->

# Project Instructions

Longview is a deep value investing community and structured investment research platform (`深度价值投资社区 / 结构化投研平台方案仓库`). Treat this repository as a research and product strategy workspace until application code exists.

## Research Work

- For investment research, AI automation research, market data, regulations, product capabilities, pricing, and anything described as latest or state-of-the-art, verify with current sources before answering.
- Use Firecrawl as the default tool for external research acquisition: web search, page scrape, local file parse, site crawl/map, arXiv/GitHub research, and browser interaction when needed.
- Relevant Firecrawl commands include `firecrawl search`, `firecrawl scrape`, `firecrawl parse`, `firecrawl crawl`, `firecrawl map`, `firecrawl research search-papers`, `firecrawl research search-github`, `firecrawl interact`, and `firecrawl agent`.
- Save important Firecrawl outputs under the active task's `sources/` directory and update the source index. Use `/tmp` only for smoke tests.
- Prefer primary sources: official docs, filings, regulator materials, research papers, release notes, and source repositories.
- Separate facts, estimates, opinions, inferences, and recommendations.
- Do not present personalized investment advice, buy/sell/hold instructions, or unsupported price targets.
- Use `.trellis/spec/research/` as the default project guidance for research tasks.
- Never commit `.env`, `FIRECRAWL_API_KEY`, or uncurated `.firecrawl/` cache.

## Trellis Usage

- Create Trellis tasks for non-trivial research, product, or implementation work.
- Complex research tasks should have `prd.md`, `design.md`, and `implement.md` before execution.
- Keep research outputs auditable: sources, dates, assumptions, confidence, and unresolved questions should be visible.
