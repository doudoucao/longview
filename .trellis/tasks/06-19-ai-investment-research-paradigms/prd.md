# Research AI Automated Investment Research Paradigms

## Goal

Research state-of-the-art AI automated investment research paradigms, frameworks, workflows, evidence standards, and implementation patterns for Longview, a deep value investing community and structured investment research platform.

The output should help define how Longview can use AI agents, data pipelines, source verification, and human review to produce rigorous investment research without presenting automated outputs as personalized investment advice.

## Confirmed Facts

- Repository positioning from `README.md`: Longview is a "deep value investing community / structured investment research platform" (`深度价值投资社区 / 结构化投研平台方案仓库`).
- The repository currently has Trellis scaffolding and no application source code, so this task should create research guidance and planning artifacts before implementation work.
- The task is research-heavy and time-sensitive: current AI agent patterns, investment research tooling, market data APIs, regulatory expectations, and public examples may change quickly.
- Financial research is high-stakes. Outputs must distinguish facts, estimates, opinions, and model-generated inferences.

## Requirements

- Map leading AI-enabled investment research paradigms:
  - agentic research workflows
  - retrieval-augmented research and knowledge bases
  - SEC/filing-driven fundamental analysis pipelines
  - market/news/event monitoring systems
  - multi-agent debate, reviewer, and red-team loops
  - human-in-the-loop research approval workflows
  - portfolio/risk-aware research orchestration
- Compare notable frameworks and reference implementations, including open-source agent frameworks, research orchestration patterns, data extraction pipelines, and investment research products where public information is available.
- Identify evidence standards Longview should require for AI-produced investment research:
  - source hierarchy
  - citation requirements
  - claim verification
  - freshness checks
  - uncertainty labeling
  - audit trail retention
- Define a practical research operating model for Longview:
  - research object model
  - source ingestion strategy
  - analyst/agent roles
  - quality gates
  - compliance and disclaimer boundaries
  - MVP workflow candidates
- Produce actionable recommendations, but do not produce specific buy/sell/hold recommendations for securities as part of this task.

## Constraints

- Use current sources. Because the topic is fast-moving, the research phase must browse or otherwise verify up-to-date information before making claims about "latest", "leading", or "state-of-the-art" approaches.
- Prefer primary sources when available: official documentation, public filings, regulator materials, research papers, reputable company materials, and original project repositories.
- Clearly label source type and confidence. Separate direct evidence from inference.
- Avoid presenting research as personalized investment advice.
- Preserve user trust by explaining data gaps, coverage limits, and assumptions.

## Acceptance Criteria

- [ ] `research.md` or equivalent final report explains the leading AI automated investment research paradigms and where each is strongest or weak.
- [ ] The report compares at least 8 relevant frameworks, products, or public reference implementations with citations.
- [ ] The report includes a source-quality rubric and a claim-verification workflow suitable for financial research.
- [ ] The report proposes a Longview-specific target operating model for automated investment research.
- [ ] The report includes an MVP roadmap with clear first experiments and non-goals.
- [ ] The report explicitly covers compliance and safety boundaries, including non-advice language and human review.
- [ ] Every material claim about current tools, products, laws, market structure, or model capability is sourced or explicitly marked as an inference.

## Out of Scope

- Building the full research platform.
- Producing investment recommendations for any specific security.
- Backtesting a trading strategy.
- Scraping paywalled or access-controlled data without permission.
- Making claims about regulatory compliance without citing current authoritative sources.

## Open Questions

- Should the first final report optimize for a product strategy audience, an engineering architecture audience, or an investment research operations audience?

## Notes

- Keep `prd.md` focused on requirements, constraints, and acceptance criteria.
- Lightweight tasks can remain PRD-only.
- For complex tasks, add `design.md` for technical design and `implement.md` for execution planning before `task.py start`.
