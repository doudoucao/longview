# Research Report: Agent 自动化投资研究范式与工具

Access date: 2026-06-19

Status: first full report from the initial Firecrawl source batch. This report is process and product research for Longview. It is not investment advice and does not recommend buying, selling, or holding any security.

Primary evidence lives in:

- `sources/index.md`
- `sources/notes/per-source-notes.md`
- `sources/raw/*.json`
- `core-synthesis.md`

## 1. Executive Summary

当前先进的 agent 自动化投研范式不是“一个大模型直接给结论”，而是“数据获取 + 原始材料保存 + 工具调用 + 多角色推理 + 结构化审查 + 人工门禁”的可审计流水线。第一批材料里最有价值的方向有三类：

1. Filing-first RAG workflow：以 SEC/公告/财报为主证据，自动拉取、切分、索引、问答、生成报告，并加入 human-in-the-loop。Airflow 的 SEC 10-K DAG PR 和 Finance Agent Benchmark 都指向这个方向（R014, R021, R025）。
2. Equity research agent framework：FinRobot/FinSphere/MarketSenseAI 这类系统把研究拆成数据、概念推理、投资 thesis、报告生成和评价模块（R011, R012, R015）。
3. Multi-agent product platform：Azure/AWS/CFA/Medium 等材料展示了面向用户的多 agent 协作、文档自动化、workflow visualization、agent progress streaming 和 scenario chat（R001, R004, R008, R009, R030）。

对 Longview 来说，最稳妥的 MVP 不是“自动选股”或“AI hedge fund”，而是一个可审计的研究助理：输入 ticker + 研究问题，自动保存原始材料，生成结构化 notes，抽取证据，写出带引用的 thesis memo，再由 reviewer agent 和人工审核把关。

## 2. Scope and Method

### Scope

本报告覆盖：

- agent 自动化投研范式
- 数据获取方式
- agent tools / skills
- 工作流自动化
- 原始材料保存和 evidence chain
- Longview 可落地 MVP

本报告不覆盖：

- 具体证券投资建议
- 交易策略回测结论
- 任何“可直接实盘”的系统推荐
- 对监管合规的结论性法律判断

### Method

使用 Firecrawl 执行四组采集：

- web search + scrape：agentic AI investment tools / GitHub / multi-agent
- web search + scrape：AI equity research agent workflow / blog / open source
- paper search：LLM agents for financial analysis and investment research
- GitHub research：investment research agent workflow financial analysis

每条原始记录都在 `sources/notes/per-source-notes.md` 里形成 R001-R030 的逐条笔记。高价值来源在 `sources/index.md` 里用 S001-S013 汇总。

## 3. Source Map and Evidence Quality

### Source Categories

| Category | Representative sources | Evidence quality | Use in report |
| --- | --- | --- | --- |
| GitHub repositories / READMEs | Azure sample, FinRobot, MASFIN, AlphaAgent, AI Hedge Fund, FinGAIA | Medium, varies by repo maturity | Tooling patterns, architecture, skills, implementation leads |
| Papers / benchmarks | FinRobot, Finance Agent Benchmark, MarketSenseAI, FinSphere, FinGAIA-related | Medium to High for concepts; full paper still needed | Agent architecture, evaluation, warning signs |
| Practitioner / vendor guides | CFA Institute, AWS Bedrock, Medium articles | Medium for workflow framing; lower for neutral claims | Workflow language, cloud/product patterns |
| Discovery surfaces / snippets | GitHub topics, Reddit, snippet-only repo hits | Low | Follow-up leads only |

### Evidence Caveats

- Some sources are abstracts or snippets only; they should not support detailed implementation claims until full text/code is inspected.
- Vendor materials are useful for architecture but should not be treated as neutral comparisons.
- GitHub repos must be evaluated for license, activity, tests, examples, and reproducibility before adoption.
- Blog and Medium material can inspire workflow design but needs corroboration.

## 4. Paradigm Taxonomy

### 4.1 Filing-First Research Pipeline

This is the strongest Longview MVP candidate. The workflow starts from authoritative filings rather than market chatter:

1. Fetch filings via EDGAR or another regulatory source.
2. Extract relevant sections such as Risk Factors and MD&A.
3. Build company-specific or topic-specific indexes.
4. Decompose analyst questions into sub-questions.
5. Retrieve evidence per sub-question.
6. Generate structured report.
7. Gate with human approval.

Evidence: Airflow SEC 10-K examples explicitly describe weekly indexing DAGs and on-demand analysis DAGs with EDGAR, vector indexes, structured reports, and HITL approval (R021, R025). Finance Agent Benchmark also centers challenging tasks on recent SEC filings and EDGAR/search tools (R014).

Longview fit: High. It matches deep-value research because filings are authoritative, auditable, and relatively stable.

### 4.2 Equity Research Agent Framework

FinRobot-style systems decompose equity research into specialized reasoning agents:

- Data agent: gather and normalize data.
- Concept/reasoning agent: interpret company, sector, metrics, risks.
- Thesis/report agent: synthesize into a written research memo.

Evidence: FinRobot paper describes Data-CoT, Concept-CoT, and Thesis-CoT agents (R012). FinSphere and MarketSenseAI point to similar stock-analysis and multi-source financial analysis patterns (R011, R015).

Longview fit: High, if the agent outputs are evidence-bound and reviewed. Good model for thesis memo generation.

### 4.3 Multi-Agent Product Platform

Product-facing implementations add UI, document upload, progress visualization, chat, and workflow monitoring around multi-agent analysis.

Evidence: Azure sample includes investment opportunities, documents, specialized agents, workflow visualization, SSE progress streaming, and scenario chat (R001). AWS Bedrock blog describes a multi-agent investment research assistant using managed agent collaboration and data automation (R009).

Longview fit: Medium to High. Useful for product architecture, but vendor samples should not dictate core domain model.

### 4.4 Screening and Portfolio Crew Pipeline

MASFIN-like systems use staged crews for postmortem, screening, analysis, timing, and portfolio allocation.

Evidence: MASFIN README describes a five-stage sequential CrewAI pipeline using Yahoo Finance and Finnhub, with HITL validation (R022).

Longview fit: Medium. Useful for watchlist generation and monitoring, but allocation/trading outputs should remain out of scope until governance and evaluation are much stronger.

### 4.5 Alpha / Factor Mining Loop

AlphaAgent-like systems use agents for hypothesis generation, factor construction, evaluation, and feedback.

Evidence: AlphaAgent README describes Idea Agent, Factor Agent, and Eval Agent, using Qlib and market data (R024).

Longview fit: Medium. Useful as a separate research lab module, not the core community thesis workflow.

### 4.6 Benchmark-Driven Evaluation

Benchmarks expose the gap between demos and reliable finance agents.

Evidence: Finance Agent Benchmark reports hard real-world finance tasks and current limitations (R014). FinGAIA emphasizes web browsing, file processing, multimodal understanding, code execution, and calculation as required skills for financial agents (R029).

Longview fit: Very High. Longview needs its own evaluation harness before trusting agent outputs.

## 5. Data Acquisition Map

| Data category | Examples | Acquisition path | Structure | Automation difficulty | Longview recommendation |
| --- | --- | --- | --- | --- | --- |
| Regulatory filings | SEC EDGAR, 10-K, 10-Q, MD&A, Risk Factors | EDGAR APIs/download, filing parsers, Firecrawl for web pages | Semi-structured HTML/XBRL/text | Medium | Start here for MVP. Build filing-first evidence chain. |
| Company materials | IR pages, earnings presentations, transcripts | Firecrawl scrape/crawl, PDF parse, transcript APIs | Mixed PDFs/HTML/audio transcripts | Medium to High | Save metadata, links, excerpts, and parsed notes. |
| Market and fundamentals | Yahoo Finance, Finnhub, Qlib, baostock, commercial APIs | APIs/SDKs/downloads | Structured time series/tables | Medium | Abstract provider layer; avoid vendor lock-in. |
| News/events/blogs | News sites, blogs, newsletters, company announcements | Firecrawl search/scrape/crawl, news APIs | Unstructured | High | Use as supporting context, not sole evidence. |
| Papers | arXiv, SSRN, academic sites | Firecrawl research search-papers, parse PDFs when allowed | Abstract/PDF | Medium | Use for architecture and benchmark literature. |
| GitHub/repos | READMEs, issues, PRs, examples | Firecrawl research search-github, scrape repos/docs | Markdown/code/history | Medium | Inspect license/activity/tests before reuse. |
| Benchmarks/datasets | Finance Agent Benchmark, FinGAIA | Paper/repo/dataset download | Dataset + docs | Medium | Use to design Longview evaluation set. |

## 6. Agent Skills and Tool Inventory

| Skill | Input | Output | Candidate tools / references | Failure modes | Longview fit |
| --- | --- | --- | --- | --- | --- |
| Source discovery | Topic, ticker, question | Candidate source list | Firecrawl search, search-papers, search-github | SEO noise, duplicates, stale results | Core |
| Raw capture | URLs/files | Markdown/JSON/PDF parse/source notes | Firecrawl scrape, crawl, parse | Paywalls, copyright, JS rendering gaps | Core |
| Filing extraction | Filing URL / ticker / form type | MD&A, Risk Factors, tables | EDGAR tooling, Airflow examples | Section misidentification, filing format drift | Core |
| Document parsing | PDF/doc/html/xlsx | Text, tables, links | Firecrawl parse, document AI tools | Table loss, OCR errors, partial extraction | Core |
| Retrieval/indexing | Raw docs and notes | Company/topic index | LlamaIndex, LangChain, FAISS, vector DB | Stale index, wrong chunking, lost citations | Core |
| Question decomposition | Research question | Sub-questions and evidence plan | Airflow examples, agent planners | Missed sub-questions, bad routing | Core |
| Financial extraction | Filings/tables/API data | Metrics and normalized facts | Python, pandas, SEC parsers, APIs | Unit/period/currency errors | Core |
| Thesis synthesis | Evidence tables | Draft memo | FinRobot-style Thesis-CoT | Overconfidence, unsupported inference | Core with review |
| Reviewer/red-team | Draft + evidence | Issues, counterpoints, citation checks | Dedicated reviewer agent | Reviewer hallucination | Core with human gate |
| Monitoring | Watchlist and source set | Change alerts | Firecrawl monitor, API polling | Noise, missed events | Later |
| Portfolio/decision support | Thesis set, constraints | Scenario analysis | MASFIN/AlphaAgent patterns | Advice boundary, backtest overfit | Later / restricted |

## 7. Tool / Framework Comparison

| Tool / source | Category | Strengths | Weaknesses / risks | Longview fit | Evidence |
| --- | --- | --- | --- | --- | --- |
| FinRobot | Equity research agent framework | Clear Data-CoT / Concept-CoT / Thesis-CoT architecture; open-source signal | Need repo/code maturity review | High | R007, R012 |
| Finance Agent Benchmark | Benchmark | Real-world finance tasks, EDGAR/search harness, exposes limitations | Need full benchmark inspection | High | R014 |
| Airflow SEC 10-K DAG examples | Workflow reference | Production-shaped indexing + analysis + HITL DAGs | Need code/API inspection | High | R021, R025 |
| CFA Institute Agentic AI guide | Practitioner guide | Good finance workflow framing | Secondary source | High for product language | R008 |
| Azure investment analysis sample | Product sample | End-to-end multi-agent product, streaming, documents, scenario chat | Vendor-specific | Medium | R001 |
| AWS Bedrock assistant | Vendor architecture | Managed multi-agent and document automation pattern | Vendor-specific | Medium | R009, R030 |
| MarketSenseAI | Research framework | Broad multi-source RAG + agents | Abstract-level evidence here | Medium | R011 |
| FinSphere | Stock analysis agent | Evaluation framework plus real-time data/tool integration | Abstract-level evidence here | Medium | R015 |
| MASFIN | Crew pipeline | Decomposed stages and HITL summaries | Trading/forecasting bias, claims need validation | Medium | R022 |
| AlphaAgent | Alpha mining | Idea/Factor/Eval loop | Quant factor focus, not core fundamental memo | Medium | R024 |
| FinGAIA | Benchmark | Maps finance tasks to browser/file/multimodal/code/calculation skills | Chinese-domain specificity | High for skill taxonomy | R029 |
| AI Hedge Fund | Proof of concept | Persona-based agents, educational clarity | Not production/investment advice; weak evidence | Low to Medium | R027 |
| MOE-INVEST | Strategy-agent system | Value/factor strategy agents and meta-agent | Needs code/data review | Low to Medium | R023 |
| Medium multi-agent platform article | Practitioner article | ADK/A2A/MCP architecture vocabulary | Partial/member-gated | Low to Medium | R004 |

## 8. Automation Workflow Patterns

### Pattern A: Evidence-First Research Memo

Use for Longview's first product loop.

1. User enters ticker and research question.
2. Source discovery finds filings, IR pages, transcripts, news, and prior internal notes.
3. Raw capture saves source material and updates `sources/index.md`.
4. Extraction pulls facts, metrics, and relevant sections.
5. Retrieval indexes by company, period, document, and claim.
6. Planner decomposes the question.
7. Analyst agent drafts answers per sub-question.
8. Thesis agent assembles a memo.
9. Reviewer agent checks citations, math, stale data, and counterevidence.
10. Human approves or sends back for revision.

### Pattern B: Continuous Thesis Monitoring

Use after a thesis exists.

1. Define watchlist and key claims.
2. Monitor filings, IR pages, news, macro indicators, and price/fundamental data.
3. Detect source changes and material claim changes.
4. Produce update note, not an automatic recommendation.
5. Human decides whether thesis needs revision.

### Pattern C: Candidate Screening

Use to generate leads, not conclusions.

1. Run screens from fundamentals, valuation, events, or qualitative themes.
2. Use agents to explain why names entered the queue.
3. Store evidence and uncertainty.
4. Route to human or deeper filing-first research.

### Pattern D: Research Evaluation Harness

Use to evaluate the agents themselves.

1. Build tasks around known filings, tables, and questions.
2. Require source-backed answers.
3. Score factual accuracy, completeness, recency, math correctness, citation quality, and uncertainty handling.
4. Maintain regression tests before changing prompts/tools/models.

## 9. Source Quality and Claim Verification Workflow

Use this hierarchy:

1. Primary: filings, company IR, official docs, original papers, official repos, regulator materials.
2. Strong secondary: practitioner guides, industry reports, reputable analysis with transparent method.
3. Weak/discovery: blogs, Medium, Reddit, GitHub topics, snippets.

Verification process:

1. Identify every material claim.
2. Attach source ID and source type.
3. Mark claim as fact, estimate, opinion, inference, or recommendation.
4. Check freshness sensitivity.
5. For numbers, verify unit, currency, period, formula, and source table.
6. For tool capabilities, prefer official docs/repo/code over blog claims.
7. For investment implications, force reviewer/red-team and human approval.

## 10. Longview Target Operating Model

### Research Objects

- Company
- Source
- Claim
- Metric
- Thesis
- Evidence packet
- Review issue
- Watch trigger

### Agent Roles

- Source Librarian: finds and saves sources.
- Filing Analyst: extracts and normalizes filings.
- Data Analyst: computes metrics and checks tables.
- Business Analyst: interprets model, moat, risks, management.
- Thesis Writer: writes structured memo.
- Reviewer: checks citations, math, uncertainty, and counterarguments.
- Compliance/Safety Gate: enforces non-advice language and publication rules.

### Human Roles

- Research owner: sets question and scope.
- Reviewer/approver: accepts, rejects, or requests revision.
- Community moderator: decides what becomes publishable discussion content.

## 11. MVP Roadmap

### MVP 1: Filing-First Evidence Loop

- Input: ticker + research question.
- Data: SEC filings and company IR pages.
- Output: source index, extracted filing sections, evidence table, draft memo, reviewer notes.
- Success criteria: every claim links to saved raw material or source note.

### MVP 2: Research Memo Workflow

- Add thesis template, reviewer checklist, and human approval state.
- Add structured claim table and contradiction table.
- Add version history for thesis updates.

### MVP 3: Monitoring

- Add watchlists, source refresh, and change summaries.
- Monitor filings, news, IR pages, and key metrics.
- No automatic investment actions.

### MVP 4: Evaluation Harness

- Build a Longview benchmark using historical filings and known answers.
- Score factual accuracy, completeness, citation quality, math correctness, and uncertainty.
- Use benchmark before model/tool changes.

## 12. Risks and Controls

| Risk | Why it matters | Control |
| --- | --- | --- |
| Hallucinated citations | Can make reports look reliable when they are not | Require source IDs and raw/source-note links |
| Numeric errors | Finance tasks depend on units, periods, formulas | Use code/tool verification and reviewer checks |
| Data staleness | Market and filing data changes | Record access date and freshness sensitivity |
| Vendor lock-in | Azure/AWS samples may bias architecture | Keep provider abstraction |
| Copyright/paywall issues | Raw material storage can violate access limits | Save metadata/short notes when full content should not be stored |
| Over-automation | Users may treat output as advice | Non-advice language, human gate, and clear uncertainty |
| Benchmark gap | Demos do not prove reliability | Build Longview evaluation harness |

## 13. Recommended Next Research Steps

1. Scrape/inspect the full FinRobot repo and paper.
2. Inspect Airflow's merged SEC 10-K examples and code paths.
3. Read full Finance Agent Benchmark and FinGAIA papers/datasets.
4. Compare data providers: SEC EDGAR, Finnhub, Polygon, Alpha Vantage, FMP, Nasdaq Data Link, Tiingo, yfinance, akshare.
5. Build a small Longview evaluation set from 10-K questions.
6. Prototype source index + claim table + thesis memo before any UI work.

## 14. References

Use `sources/notes/per-source-notes.md` as the authoritative per-source note file. Key source IDs:

- R001: Azure-Samples/Agentic-AI-Investment-Analysis-Sample
- R007/R012: FinRobot repo and paper
- R008: CFA Institute agentic AI finance guide
- R009/R030: AWS Bedrock / AWS sample material
- R011: MarketSenseAI 2.0
- R014: Finance Agent Benchmark
- R015: FinSphere
- R021/R025: Airflow SEC 10-K DAG PRs
- R022: MASFIN
- R024: AlphaAgent
- R029: FinGAIA

