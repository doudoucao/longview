# Source Index: Agent 自动化投研初始材料

Access date: 2026-06-19

This index tracks the first Firecrawl collection batch for the `ai-investment-research-paradigms` task. Raw captures are stored under `sources/raw/`; this file records what was collected, why it matters, and how it should be used in synthesis.

## Collection Commands

```bash
firecrawl search "agentic AI investment research tools GitHub multi-agent" \
  --limit 5 \
  --scrape \
  --scrape-formats markdown \
  --json \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/search-agentic-ai-investment-tools-github.json

firecrawl search "AI equity research agent workflow blog open source" \
  --limit 5 \
  --scrape \
  --scrape-formats markdown \
  --json \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/search-ai-equity-research-agent-workflow-blog.json

firecrawl research search-papers "large language model agents for financial analysis investment research" \
  --limit 10 \
  --json \
  --pretty \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/papers-llm-agents-financial-analysis.json

firecrawl research search-github "investment research agent workflow financial analysis" \
  --limit 10 \
  --json \
  --pretty \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/github-investment-research-agent-workflow.json
```

## Raw Files

| File | Saved form | Contents | Notes |
| --- | --- | --- | --- |
| `sources/raw/search-agentic-ai-investment-tools-github.json` | Firecrawl search JSON with scraped markdown | GitHub and web results for agentic AI investment research tools | Includes Azure sample, Medium articles, GitHub topics, and a Reddit result with no markdown body. |
| `sources/raw/search-ai-equity-research-agent-workflow-blog.json` | Firecrawl search JSON with scraped markdown | Equity research agent workflows, open-source tools, practitioner guides | Includes FinRobot GitHub, CFA Institute guide, AWS Bedrock investment research assistant blog. |
| `sources/raw/papers-llm-agents-financial-analysis.json` | Firecrawl research paper JSON | arXiv-style paper search results for LLM agents in financial analysis | Includes MarketSenseAI 2.0, FinRobot, Finance Agent Benchmark, FinSphere, FinGAIA-adjacent topics. |
| `sources/raw/github-investment-research-agent-workflow.json` | Firecrawl GitHub research JSON | GitHub README/history results for investment research agent workflows | Includes Airflow SEC 10-K DAG PR, MASFIN, AlphaAgent, AI Hedge Fund, FinGAIA. |

## High-Value Sources

| ID | Title | URL / ID | Type | Saved raw material | Why it matters | Confidence |
| --- | --- | --- | --- | --- | --- | --- |
| S001 | Azure-Samples/Agentic-AI-Investment-Analysis-Sample | https://github.com/Azure-Samples/Agentic-AI-Investment-Analysis-Sample | GitHub sample | `search-agentic-ai-investment-tools-github.json` | Direct reference implementation for multi-agent investment opportunity analysis. Useful for architecture and Azure stack patterns. | Medium |
| S002 | FinRobot GitHub | https://github.com/ai4finance-foundation/finrobot | GitHub project | `search-ai-equity-research-agent-workflow-blog.json` | Open-source financial analysis agent platform connected to an arXiv paper; useful for Data-CoT / Concept-CoT / Thesis-CoT patterns. | High |
| S003 | FinRobot paper | arxiv:2411.08804 | Paper | `papers-llm-agents-financial-analysis.json` | Defines an equity research and valuation agent framework with specialized agents and dynamic data pipeline claims. | High |
| S004 | CFA Institute: Agentic AI for Finance | https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance | Practitioner guide | `search-ai-equity-research-agent-workflow-blog.json` | Practitioner-oriented guide to screening agents, RAG, and multi-agent finance workflows. | High |
| S005 | AWS: multi-agent investment research assistant | https://aws.amazon.com/blogs/machine-learning/part-3-building-an-ai-powered-assistant-for-investment-research-with-multi-agent-collaboration-in-amazon-bedrock-and-amazon-bedrock-data-automation/ | Vendor architecture blog | `search-ai-equity-research-agent-workflow-blog.json` | Concrete cloud workflow for multi-agent investment research using Bedrock and document/data automation. Treat capability claims as vendor claims. | Medium |
| S006 | MarketSenseAI 2.0 | arxiv:2502.00415 | Paper | `papers-llm-agents-financial-analysis.json` | RAG + LLM agent framework processing news, prices, fundamentals, SEC filings, earnings calls, macro and institutional reports. | Medium |
| S007 | Finance Agent Benchmark | arxiv:2508.00828 | Paper / benchmark | `papers-llm-agents-financial-analysis.json` | Important counterweight: reports difficult real-world finance research tasks, EDGAR/search tools, and limitations of current agents. | High |
| S008 | FinSphere | arxiv:2501.12399 | Paper | `papers-llm-agents-financial-analysis.json` | Stock analysis agent with real-time data feeds, quantitative tools, instruction-tuned model, and evaluation framework. | Medium |
| S009 | Airflow SEC 10-K financial analysis DAG PR | https://github.com/apache/airflow/issues/67671 | GitHub PR/history | `github-investment-research-agent-workflow.json` | Production-shaped workflow: fetch SEC 10-K filings, extract Risk Factors/MD&A, build vector indexes, decompose analyst question, synthesize structured report, human approval. | Medium |
| S010 | MASFIN | https://github.com/mmontalvo9/masfin | GitHub README | `github-investment-research-agent-workflow.json` | Multi-agent financial forecasting pipeline with staged crews, Yahoo Finance + Finnhub data, HITL validation. | Medium |
| S011 | AlphaAgent | https://github.com/rndmvariableq/alphaagent | GitHub README / paper code | `github-investment-research-agent-workflow.json` | LLM-driven alpha mining with Idea, Factor, and Eval agents; useful for alpha discovery workflow design. | Medium |
| S012 | AI Hedge Fund | https://github.com/virattt/ai-hedge-fund | GitHub README | `github-investment-research-agent-workflow.json` | Popular proof-of-concept with role-based investment agents; useful as an example and as a source of patterns to critique. | Low |
| S013 | FinGAIA | https://github.com/sufe-aiflm-lab/fingaia | GitHub README / benchmark | `github-investment-research-agent-workflow.json` | Benchmark for financial AI agents, emphasizing web browsing, file processing, multimodal, code, and calculation skills. | Medium |

## Complete Raw Record Coverage

Every raw record from the first Firecrawl batch has a note in `sources/notes/per-source-notes.md`.

| Raw record range | Source file | Count | Note coverage |
| --- | --- | ---: | --- |
| R001-R005 | `sources/raw/search-agentic-ai-investment-tools-github.json` | 5 | Complete |
| R006-R010 | `sources/raw/search-ai-equity-research-agent-workflow-blog.json` | 5 | Complete |
| R011-R020 | `sources/raw/papers-llm-agents-financial-analysis.json` | 10 | Complete |
| R021-R030 | `sources/raw/github-investment-research-agent-workflow.json` | 10 | Complete |

## Synthesis Artifacts

| File | Purpose |
| --- | --- |
| `sources/notes/per-source-notes.md` | One note for every raw source record, including duplicates, empty-body records, and snippet-only results. |
| `sources/notes/initial-source-notes.md` | Earlier focused notes for the first high-value subset. Kept as historical scratch notes. |
| `core-synthesis.md` | Standalone extraction of core patterns and early Longview implications. |
| `research.md` | Comprehensive first report based on the saved source set. |

## Limitations

- This is an initial collection batch, not an exhaustive literature review.
- Search results include duplicates and marketing/vendor claims. Later synthesis must verify high-impact claims against primary sources.
- Reddit results were discovered but not retained as core sources because scraped markdown was unavailable in this run and the evidence value is lower.
- Later research should verify paper availability and metadata before treating paper-search entries as authoritative.
