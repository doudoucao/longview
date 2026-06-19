# Design: AI Automated Investment Research Paradigms

## Boundary

This task designs and documents a research foundation for Longview. It does not build product code. The deliverables are planning artifacts, research standards, and a final synthesized report that can inform later product and engineering tasks.

## Research Architecture

Use a layered research model:

1. Evidence layer: primary sources, regulator materials, public filings, technical docs, papers, product pages, credible reporting, and open-source repositories.
2. Extraction layer: structured notes for each source, including source type, date, author/publisher, relevance, claims, and limitations.
3. Synthesis layer: comparison matrices, pattern taxonomy, operating model, and MVP recommendations.
4. Review layer: contradiction checks, freshness checks, citation coverage, compliance/safety review, and human judgment.

## Research Objects

Represent findings around stable objects:

- Paradigm: a repeatable approach such as agentic research, RAG knowledge base, filing extraction, monitoring, debate/reviewer loops, or human approval workflows.
- Framework/Product: a concrete implementation, tool, platform, or publicly documented system.
- Capability: data ingestion, retrieval, extraction, reasoning, valuation support, monitoring, audit, or workflow orchestration.
- Evidence Standard: a rule for whether a claim is acceptable in financial research.
- Longview Recommendation: a product or operating model proposal derived from evidence.

## Evidence Contract

Every material claim should carry:

- source URL or local source path
- publication or access date when available
- claim type: fact, estimate, opinion, inference, or recommendation
- confidence: high, medium, or low
- freshness sensitivity: low, medium, or high

If a claim cannot be verified, it must be labeled as an inference or excluded from the final report.

## Data Flow

1. Collect sources across AI automation, financial research workflows, filings, regulatory guidance, and market data tooling.
2. Normalize notes into a comparable format.
3. Build taxonomy and comparison tables.
4. Draft Longview operating model.
5. Run review pass for citations, uncertainty, and compliance boundaries.

## Tradeoffs

- Breadth versus depth: start broad enough to see the landscape, then go deep only on patterns relevant to Longview.
- Product strategy versus engineering detail: keep both, but separate them so later tasks can branch cleanly.
- Automation ambition versus trust: favor auditable workflows over fully autonomous claims or recommendations.

## Compliance and Safety

This task may discuss financial research systems but must not present personalized investment advice. Recommendations should target Longview's research process, product architecture, and operating model, not individual investor actions.
