# AI Automation Framework Standards

## Evaluation Dimensions

When comparing AI automation frameworks, products, or reference implementations, evaluate:

- orchestration model: single agent, multi-agent, graph workflow, queue, pipeline, or human-in-the-loop
- research workflow coverage: question decomposition, source discovery, raw material capture, extraction, synthesis, review, publication, and monitoring
- retrieval and memory: RAG, document stores, source freshness, citation support, and audit logs
- data acquisition: filings, market data APIs, news/event feeds, web/blog search, PDF parsing, GitHub repository ingestion, academic paper search, and local file parsing
- tool use: browser, filings, market data, code execution, database queries, structured extraction, web scraping, document parsing, and alerting
- reusable skills: search, scrape, crawl, parse, extract, normalize, validate, model, compare, monitor, cite, review, and red-team
- reliability controls: retries, validation, schema enforcement, guardrails, review steps, and testability
- observability: traces, logs, evaluation harnesses, cost tracking, and provenance
- security and permissions: secrets, data access, sandboxing, and least privilege
- extensibility: APIs, plugins, connectors, and local deployment options
- fit for investment research: filing analysis, valuation support, news monitoring, debate/review workflows, and report generation

## Comparison Rules

Do not rank frameworks only by popularity. Tie evaluation to Longview's needs:

- high-trust source handling
- explicit raw-material preservation
- clear data acquisition paths
- repeatable research workflows
- human review before publication
- transparent citations
- ability to separate evidence from inference
- support for long-running research tasks

## Evidence for Capability Claims

Capability claims should be backed by docs, code, demos, release notes, or hands-on tests. Marketing claims are acceptable only when labeled as vendor claims.

For GitHub projects, inspect README, examples, release notes, license, recent commit activity, issues/discussions when relevant, and integration surface. Treat star count as popularity context, not proof of capability.

## Data and Skill Mapping

For each candidate framework or tool, capture:

- what data it can fetch or ingest
- which connectors or APIs it supports
- whether it can preserve raw content and provenance
- what agent skills it makes easier to implement
- what still requires custom code
- failure modes: stale data, hallucinated citations, rate limits, schema drift, paywalls, and brittle scraping

## Recommended Output

Use comparison tables with:

- name
- category
- source link
- core idea
- strengths
- weaknesses
- evidence quality
- data acquisition fit
- useful agent skills
- automation workflow fit
- Longview fit
- follow-up experiment
