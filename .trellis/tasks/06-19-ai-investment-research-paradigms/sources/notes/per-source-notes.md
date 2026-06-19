# Per-Source Notes: Agent 自动化投研原始材料逐条笔记

Access date: 2026-06-19

These notes cover every saved raw source record from the first Firecrawl collection batch. Duplicate records are retained when they came from different raw searches, but synthesis should de-duplicate by URL.

## Web Search Batch A: `search-agentic-ai-investment-tools-github.json`

### R001: Azure-Samples/Agentic-AI-Investment-Analysis-Sample

- URL: https://github.com/Azure-Samples/Agentic-AI-Investment-Analysis-Sample
- Source type: GitHub repository page
- Saved raw file: `sources/raw/search-agentic-ai-investment-tools-github.json`
- Saved form: scraped markdown in JSON
- Core content: Azure sample for an AI-powered investment analysis platform using specialized agents for financial, risk, market, and compliance perspectives. The captured README describes document processing, workflow visualization, SSE progress streaming, document/opportunity management, and Azure service dependencies.
- Data/workflow relevance: Demonstrates platform-level orchestration, document ingestion, investment opportunity objects, multi-agent fan-out, debate/aggregation, and live progress events.
- Tool/skill relevance: Document processing, multi-agent orchestration, streaming status, scenario chat, report generation.
- Limitations: Vendor-specific Azure stack. It is a sample, not evidence that the architecture is production-proven. Need code/license inspection before reuse.
- Longview implication: Useful blueprint for product modules and agent-role UI, but Longview should avoid locking the research model to Azure-specific primitives.
- Confidence: Medium.

### R002: Building an Open-Source Agentic AI Equity Research Tool

- URL: https://medium.datadriveninvestor.com/building-an-open-source-agentic-ai-equity-research-tool-172783ed6961
- Source type: Medium article
- Saved raw file: `sources/raw/search-agentic-ai-investment-tools-github.json`
- Saved form: scraped markdown in JSON
- Core content: Article about building an open-source agentic equity research tool. Firecrawl captured enough metadata and body text for initial review, but this is a secondary/practitioner article.
- Data/workflow relevance: Useful for practical pitfalls and implementation framing around equity research agents.
- Tool/skill relevance: Likely touches source discovery, data gathering, synthesis, and report generation.
- Limitations: Blog source; needs cross-check against code or primary implementation if it references one. Medium access can be partial or gated.
- Longview implication: Treat as practitioner color and workflow inspiration, not authoritative evidence.
- Confidence: Low to Medium.

### R003: GitHub Topics - investment-research

- URL: https://github.com/topics/investment-research
- Source type: GitHub topic page
- Saved raw file: `sources/raw/search-agentic-ai-investment-tools-github.json`
- Saved form: scraped markdown in JSON
- Core content: Topic page listing repositories related to investment research. Search description highlights an AI agent toolkit for SEC EDGAR filing data and multiple agent/framework integrations.
- Data/workflow relevance: Useful discovery surface for candidate repositories, especially SEC/EDGAR tooling.
- Tool/skill relevance: Repository discovery, filing analysis, LangChain/MCP/Gradio/Dify/smolagents integration clues.
- Limitations: Topic pages are discovery indexes, not primary documentation for any one tool. Must follow through to specific repositories before drawing conclusions.
- Longview implication: Use as a source-discovery input only.
- Confidence: Low.

### R004: Building A Multi-Agent Investment Platform: From Research to Trade Execution

- URL: https://medium.com/data-science-collective/building-a-multi-agent-investment-platform-from-research-to-trade-execution-7ca297595ce9
- Source type: Medium article
- Saved raw file: `sources/raw/search-agentic-ai-investment-tools-github.json`
- Saved form: scraped markdown in JSON
- Core content: Article frames a multi-agent investment system using Google ADK, A2A, MCP, tools/prompts/memory, and a portfolio manager orchestrator. Captured body indicates some content is member-gated.
- Data/workflow relevance: Useful for architecture vocabulary: tool boundaries, orchestration patterns, A2A/MCP communication, and portfolio-manager orchestration.
- Tool/skill relevance: Agent communication, tool registry, memory, portfolio orchestration.
- Limitations: Medium/member-gated; raw capture may not contain full article. Treat as partial source and do not rely on details not captured.
- Longview implication: Good for workflow taxonomy and questions to investigate, but not enough as primary support.
- Confidence: Low to Medium.

### R005: Reddit - Open-Source Agentic AI for Company Research

- URL: https://www.reddit.com/r/BusinessIntelligence/comments/1n0lh95/opensource_agentic_ai_for_company_research/
- Source type: Reddit discussion
- Saved raw file: `sources/raw/search-agentic-ai-investment-tools-github.json`
- Saved form: metadata only; markdown body length was 0
- Core content: Search result says a project called Mira automates company research using the OpenAI Agents SDK.
- Data/workflow relevance: Potential lead for company-research agent tooling.
- Tool/skill relevance: Company research automation and OpenAI Agents SDK.
- Limitations: No captured body; low evidentiary value until source repository or announcement is retrieved.
- Longview implication: Follow-up lead only.
- Confidence: Low.

## Web Search Batch B: `search-ai-equity-research-agent-workflow-blog.json`

### R006: Building an Open-Source Agentic AI Equity Research Tool

- URL: https://medium.datadriveninvestor.com/building-an-open-source-agentic-ai-equity-research-tool-172783ed6961
- Source type: Medium article
- Saved raw file: `sources/raw/search-ai-equity-research-agent-workflow-blog.json`
- Saved form: scraped markdown in JSON
- Core content: Duplicate of R002 from a second query.
- Data/workflow relevance: Same as R002.
- Tool/skill relevance: Same as R002.
- Limitations: Duplicate; use one source note in synthesis.
- Longview implication: Confirms this article appears across related queries, but does not increase evidence quality.
- Confidence: Low to Medium.

### R007: FinRobot GitHub

- URL: https://github.com/ai4finance-foundation/finrobot
- Source type: GitHub repository
- Saved raw file: `sources/raw/search-ai-equity-research-agent-workflow-blog.json`
- Saved form: scraped markdown in JSON
- Core content: Open-source financial analysis agent platform connected to the FinRobot paper. Search description says FinRobot Pro automates professional stock analysis with LLMs and AI agents.
- Data/workflow relevance: Strong source for equity research and valuation agent design.
- Tool/skill relevance: Data aggregation, concept reasoning, valuation, risk analysis, thesis/report generation.
- Limitations: Need inspect repo code, dependencies, examples, licenses, and live data adapters before deciding implementation fit.
- Longview implication: Strong candidate reference for Longview's analyst-role decomposition and report-generation flow.
- Confidence: High for relevance; medium for implementation maturity until deeper code review.

### R008: CFA Institute - Agentic AI for Finance

- URL: https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance
- Source type: Practitioner guide
- Saved raw file: `sources/raw/search-ai-equity-research-agent-workflow-blog.json`
- Saved form: scraped markdown in JSON
- Core content: Practical finance guide to agents, including screening agents, RAG, and multi-agent workflows.
- Data/workflow relevance: Good finance-domain framing for where agents fit into analyst workflows.
- Tool/skill relevance: Screening, RAG, multi-agent collaboration, workflow design.
- Limitations: Practitioner/educational source, not a benchmark or primary implementation.
- Longview implication: Useful for explaining product value to investment users and for designing non-technical workflow language.
- Confidence: High for framing.

### R009: AWS Bedrock Multi-Agent Investment Research Assistant

- URL: https://aws.amazon.com/blogs/machine-learning/part-3-building-an-ai-powered-assistant-for-investment-research-with-multi-agent-collaboration-in-amazon-bedrock-and-amazon-bedrock-data-automation/
- Source type: Vendor architecture blog
- Saved raw file: `sources/raw/search-ai-equity-research-agent-workflow-blog.json`
- Saved form: scraped markdown in JSON
- Core content: Walkthrough of a multi-agent investment research assistant using Amazon Bedrock multi-agent collaboration and Bedrock Data Automation.
- Data/workflow relevance: Concrete cloud architecture for document/data automation and multi-agent collaboration.
- Tool/skill relevance: Document automation, agent collaboration, cloud orchestration, report generation.
- Limitations: Vendor-specific; claims should be treated as AWS implementation guidance, not neutral comparison.
- Longview implication: Useful reference for a cloud-native prototype, especially if Longview later evaluates Bedrock.
- Confidence: Medium.

### R010: Reddit - Automated research agent for stock analysis

- URL: https://www.reddit.com/r/algotrading/comments/1l82has/ive_built_an_automated_research_agent_for_stock/
- Source type: Reddit discussion
- Saved raw file: `sources/raw/search-ai-equity-research-agent-workflow-blog.json`
- Saved form: metadata only; markdown body length was 0
- Core content: Search result claims an automated stock analysis agent that pulls data from multiple financial sources and cross-references information.
- Data/workflow relevance: Potential lead for source cross-checking workflow.
- Tool/skill relevance: Multi-source data pull, cross-reference, stock analysis.
- Limitations: No captured body; not suitable as supporting evidence without follow-up.
- Longview implication: Follow-up lead only.
- Confidence: Low.

## Paper Search Batch: `papers-llm-agents-financial-analysis.json`

### R011: MarketSenseAI 2.0

- ID: arxiv:2502.00415
- Source type: Paper search result
- Saved raw file: `sources/raw/papers-llm-agents-financial-analysis.json`
- Saved form: metadata and abstract
- Core content: Framework for stock analysis using LLM agents over financial news, historical prices, company fundamentals, macro environment, SEC filings, earnings calls, and institutional reports. Abstract claims RAG + agents and reports performance metrics.
- Data/workflow relevance: Broad multi-source investment analysis architecture.
- Tool/skill relevance: RAG, SEC filing processing, earnings call processing, macro report enrichment.
- Limitations: Only abstract saved. Performance claims require full paper and methodology review.
- Longview implication: Good candidate for multi-source research architecture, but Longview should validate carefully before using results.
- Confidence: Medium.

### R012: FinRobot paper

- ID: arxiv:2411.08804
- Source type: Paper search result
- Saved raw file: `sources/raw/papers-llm-agents-financial-analysis.json`
- Saved form: metadata and abstract
- Core content: Proposes FinRobot for equity research and valuation using multi-agent CoT: Data-CoT, Concept-CoT, and Thesis-CoT agents.
- Data/workflow relevance: Strong fit for analyst workflow decomposition and research report generation.
- Tool/skill relevance: Data aggregation, financial integration, analyst reasoning, investment thesis/report synthesis.
- Limitations: Only abstract saved. Need full paper and repository inspection.
- Longview implication: One of the strongest role-model references for Longview's research agent architecture.
- Confidence: High for conceptual relevance.

### R013: The New Quant survey

- ID: arxiv:2510.05533
- Source type: Paper search result
- Saved raw file: `sources/raw/papers-llm-agents-financial-analysis.json`
- Saved form: metadata and abstract
- Core content: Survey of LLMs in financial prediction and trading, covering sentiment/event extraction, numerical reasoning, multimodal understanding, RAG, time-series prompting, and agentic systems for research/backtesting/execution.
- Data/workflow relevance: Useful taxonomy for connecting unstructured signals to portfolio construction and evaluation.
- Tool/skill relevance: Signal extraction, retrieval-first prompting, tool-verified numerics, portfolio controls.
- Limitations: Firecrawl metadata should be verified; survey details require full paper.
- Longview implication: Good organizing taxonomy for report sections and future evaluation framework.
- Confidence: Medium.

### R014: Finance Agent Benchmark

- ID: arxiv:2508.00828
- Source type: Paper search result / benchmark
- Saved raw file: `sources/raw/papers-llm-agents-financial-analysis.json`
- Saved form: metadata and abstract
- Core content: Benchmark with 537 expert-authored real-world finance research questions using recent SEC filings; includes agentic harness with Google Search and EDGAR tools. Abstract reports strong limitations for current LLM agents.
- Data/workflow relevance: Crucial evaluation reference for filing-based research tasks.
- Tool/skill relevance: EDGAR access, web search, financial modeling, benchmark harness.
- Limitations: Need full benchmark inspection before citing metrics as final.
- Longview implication: Longview should build evaluation before trusting automation and should expect agents to underperform on complex finance tasks without strong tool discipline.
- Confidence: High.

### R015: FinSphere

- ID: arxiv:2501.12399
- Source type: Paper search result
- Saved raw file: `sources/raw/papers-llm-agents-financial-analysis.json`
- Saved form: metadata and abstract
- Core content: Stock analysis agent plus AnalyScore evaluation framework and Stocksis dataset. Uses real-time data feeds, quantitative tools, and instruction-tuned LLMs for stock analysis reports.
- Data/workflow relevance: Useful for real-time stock analysis and evaluation rubric design.
- Tool/skill relevance: Real-time data, quantitative tools, report evaluation.
- Limitations: Only abstract saved; claims require full paper verification.
- Longview implication: Evaluation framework and report-quality scoring may inform Longview quality gates.
- Confidence: Medium.

### R016: Multi-Agent Investment System for Chinese Public REITs

- ID: arxiv:2602.00082
- Source type: Paper search result
- Saved raw file: `sources/raw/papers-llm-agents-financial-analysis.json`
- Saved form: metadata and abstract
- Core content: Multi-agent trading framework for Chinese public REITs with announcement, event, price momentum, and market agents, followed by prediction and decision agents.
- Data/workflow relevance: Shows domain-specific agent roles and analysis-prediction-decision-execution loop.
- Tool/skill relevance: Announcement parsing, event analysis, price momentum, market analysis, prediction, risk control.
- Limitations: Trading-focused and market-specific. Needs full paper and leakage/backtest inspection.
- Longview implication: Useful as a specialized workflow example, not as a direct Longview deep-value model.
- Confidence: Medium.

### R017: Toward Expert Investment Teams

- ID: arxiv:2602.23330
- Source type: Paper search result
- Saved raw file: `sources/raw/papers-llm-agents-financial-analysis.json`
- Saved form: metadata and abstract
- Core content: Multi-agent LLM trading framework with fine-grained task decomposition, evaluated on Japanese stock data with prices, statements, news, and macro information.
- Data/workflow relevance: Highlights that fine-grained task decomposition may matter more than generic role imitation.
- Tool/skill relevance: Task decomposition, multi-source analysis, preference alignment, portfolio optimization.
- Limitations: Trading-focused; abstract only.
- Longview implication: Strong design principle: decompose real analyst tasks concretely rather than naming agents after job titles.
- Confidence: Medium.

### R018: P1GPT

- ID: arxiv:2510.23032
- Source type: Paper search result
- Saved raw file: `sources/raw/papers-llm-agents-financial-analysis.json`
- Saved form: metadata and abstract
- Core content: Layered multi-agent framework for multimodal financial information analysis, fusing technical, fundamental, and news insights with interpretable decision support.
- Data/workflow relevance: Supports a structured reasoning pipeline over role-play agents.
- Tool/skill relevance: Multimodal fusion, technical/fundamental/news analysis, causal rationale generation.
- Limitations: Full methodology and data leakage controls need verification.
- Longview implication: Useful for designing layered synthesis and explanation rather than loosely connected analysts.
- Confidence: Medium.

### R019: AFIB / SuperInvesting benchmark

- ID: arxiv:2603.08704
- Source type: Paper search result / benchmark
- Saved raw file: `sources/raw/papers-llm-agents-financial-analysis.json`
- Saved form: metadata and abstract
- Core content: Multi-dimensional financial intelligence benchmark across factual accuracy, analytical completeness, data recency, model consistency, and failure patterns.
- Data/workflow relevance: Useful for thinking about evaluation dimensions.
- Tool/skill relevance: Retrieval-oriented systems, factuality, completeness, recency, consistency.
- Limitations: Mentions evaluated systems and performance claims that require full paper validation.
- Longview implication: Longview should measure several dimensions, not just answer accuracy.
- Confidence: Medium.

### R020: Multimodal Gen-AI for Fundamental Investment Research

- ID: arxiv:2401.06164
- Source type: Paper search result / report
- Saved raw file: `sources/raw/papers-llm-agents-financial-analysis.json`
- Saved form: metadata and abstract
- Core content: Discusses automating summarization and investment idea generation over research reports, investment memos, market news, and time-series data using fine-tuning experiments.
- Data/workflow relevance: Relevant to fundamental research document overload and multimodal/market-data fusion.
- Tool/skill relevance: Summarization, idea generation, fine-tuning, market-news/time-series integration.
- Limitations: Abstract includes stock recommendation formatting, which Longview should treat carefully due to non-advice boundary.
- Longview implication: Useful for document-heavy fundamental research but less aligned with auditable agent workflow than filing-first RAG.
- Confidence: Medium.

## GitHub Research Batch: `github-investment-research-agent-workflow.json`

### R021: Apache Airflow SEC 10-K LlamaIndex DAG PR

- URL: https://github.com/apache/airflow/issues/67671
- Source type: GitHub PR/history
- Saved raw file: `sources/raw/github-investment-research-agent-workflow.json`
- Saved form: GitHub history result with snippet
- Core content: Describes two DAGs: weekly SEC 10-K indexing via EDGAR and on-demand analysis with analyst question decomposition, retrieval, structured report, usage limits, and HITL approval.
- Data/workflow relevance: Excellent workflow blueprint for filing-first research automation.
- Tool/skill relevance: EDGAR ingestion, section extraction, vector indexing, dynamic task mapping, structured synthesis, human approval.
- Limitations: Need inspect merged example code and exact Airflow APIs before implementation.
- Longview implication: Strong MVP candidate pattern.
- Confidence: High for workflow relevance.

### R022: MASFIN

- URL: https://github.com/mmontalvo9/masfin
- Source type: GitHub README
- Saved raw file: `sources/raw/github-investment-research-agent-workflow.json`
- Saved form: README snippet
- Core content: Multi-Agent System for Financial Forecasting using CrewAI. Five-stage sequential pipeline: Postmortem, Screening, Analysis, Timing, Portfolio. Data sources include Yahoo Finance and Finnhub API, with HITL validation.
- Data/workflow relevance: Concrete staged workflow for forecasting/portfolio decision support.
- Tool/skill relevance: Screening, quantitative indicators, news sentiment, portfolio allocation, weekly cycle.
- Limitations: Performance claims and live-market evaluation need independent review. It is trading/forecasting oriented.
- Longview implication: Good source for staged crew architecture; should not be copied as advice workflow.
- Confidence: Medium.

### R023: MOE-INVEST / guruagents

- URL: https://github.com/yejining99/guruagents
- Source type: GitHub README
- Saved raw file: `sources/raw/github-investment-research-agent-workflow.json`
- Saved form: README snippet
- Core content: Multi-agent investment system implementing several investment philosophies such as Graham, Piotroski, Greenblatt, Carlisle, and Driehaus, plus meta-agent reasoning and portfolio construction.
- Data/workflow relevance: Shows philosophy-specific agents and master/meta-agent synthesis.
- Tool/skill relevance: Fundamental factor screening, strategy-specific scoring, meta-agent aggregation, portfolio evaluation.
- Limitations: README appears partly Korean; code quality, maturity, and data assumptions need inspection.
- Longview implication: Useful for representing value-investing schools as evidence/scoring modules, but final decisions need human review.
- Confidence: Low to Medium.

### R024: AlphaAgent

- URL: https://github.com/rndmvariableq/alphaagent
- Source type: GitHub README / paper code
- Saved raw file: `sources/raw/github-investment-research-agent-workflow.json`
- Saved form: README snippet
- Core content: KDD 2025 paper code for LLM-driven alpha mining with Idea Agent, Factor Agent, and Eval Agent. Uses Qlib and Chinese stock data via baostock.
- Data/workflow relevance: Good example of hypothesis -> factor construction -> backtest/evaluation loop.
- Tool/skill relevance: Alpha hypothesis generation, factor construction, regularization, evaluation feedback.
- Limitations: Quant alpha-mining is different from Longview's structured fundamental research; requires code/full paper review.
- Longview implication: Keep as optional module for hypothesis testing, not the core research workflow.
- Confidence: Medium.

### R025: Apache Airflow SEC 10-K LangChain DAG PR

- URL: https://github.com/apache/airflow/issues/67727
- Source type: GitHub PR/history
- Saved raw file: `sources/raw/github-investment-research-agent-workflow.json`
- Saved form: GitHub history result with snippet
- Core content: LangChain counterpart to the LlamaIndex Airflow example: fetch SEC 10-K filings, build per-company FAISS indexes, decompose analyst questions, synthesize structured reports, HITL approval.
- Data/workflow relevance: Same filing-first workflow with alternative retrieval stack.
- Tool/skill relevance: EDGAR ingestion, FAISS indexing, LangChain retrieval, HITL.
- Limitations: Need inspect merged example code.
- Longview implication: Confirms the filing-first architecture can be implemented with multiple retrieval frameworks.
- Confidence: High for workflow relevance.

### R026: Financial Research Analyst Agent

- URL: https://github.com/gsaini/financial-research-analyst-agent
- Source type: GitHub repository search result
- Saved raw file: `sources/raw/github-investment-research-agent-workflow.json`
- Saved form: web result snippet only
- Core content: Search result describes an end-to-end AI solution for automating financial data analysis and insight generation.
- Data/workflow relevance: Potential reference implementation for financial analyst automation.
- Tool/skill relevance: Financial data analysis, insight generation.
- Limitations: Only snippet captured; must scrape/inspect repository before using as evidence.
- Longview implication: Follow-up candidate.
- Confidence: Low.

### R027: AI Hedge Fund

- URL: https://github.com/virattt/ai-hedge-fund
- Source type: GitHub README
- Saved raw file: `sources/raw/github-investment-research-agent-workflow.json`
- Saved form: README snippet
- Core content: Proof-of-concept AI-powered hedge fund with multiple named investment-style agents including valuation and value-investing personas.
- Data/workflow relevance: Shows role/persona-based investment-agent design.
- Tool/skill relevance: Valuation, Graham-style screening, multi-agent decision synthesis.
- Limitations: Educational proof-of-concept and explicitly not intended for real trading/investment. Must not be treated as production-ready.
- Longview implication: Useful cautionary comparison: persona agents are attractive but need evidence contracts and numeric validation.
- Confidence: Low to Medium.

### R028: GitHub Topics - investment-research

- URL: https://github.com/topics/investment-research
- Source type: GitHub topic page
- Saved raw file: `sources/raw/github-investment-research-agent-workflow.json`
- Saved form: web result snippet
- Core content: Discovery surface for investment research repositories. Snippet mentions evidence-tracked, refreshable investment theses across equities, earnings, macro, and portfolio review.
- Data/workflow relevance: Useful for finding additional repositories.
- Tool/skill relevance: Research workspace discovery.
- Limitations: Topic pages are not source evidence.
- Longview implication: Use as follow-up source discovery.
- Confidence: Low.

### R029: FinGAIA

- URL: https://github.com/sufe-aiflm-lab/fingaia
- Source type: GitHub README / benchmark
- Saved raw file: `sources/raw/github-investment-research-agent-workflow.json`
- Saved form: README snippet
- Core content: Chinese benchmark for financial AI agents. Captured content highlights real-world financial scenarios and required capabilities: web browsing, file processing, multimodal ability, code execution, and calculation.
- Data/workflow relevance: Strong benchmark reference for tool-use complexity in finance.
- Tool/skill relevance: Browser, file parse, multimodal, Python/code, calculation, multi-step reasoning.
- Limitations: Chinese financial benchmark; need inspect dataset and paper before applying to global equity research.
- Longview implication: Good basis for Longview's evaluation dimensions and tool-skill checklist.
- Confidence: Medium.

### R030: AWS Samples Financial Analysis Multi-Agent Collaboration

- URL: https://github.com/aws-samples/financial-analysis-multi-agent-collaboration
- Source type: GitHub repository search result
- Saved raw file: `sources/raw/github-investment-research-agent-workflow.json`
- Saved form: web result snippet only
- Core content: Search result suggests an investment advisory multi-agent system with agents specialized in financial data analysis, research, forecasting, and investment.
- Data/workflow relevance: Potential code sample related to AWS Bedrock article.
- Tool/skill relevance: Multi-agent collaboration, data analysis, research, forecasting.
- Limitations: Only snippet captured; needs scrape/inspect before use as evidence.
- Longview implication: Follow-up candidate, especially if evaluating AWS stack.
- Confidence: Low.

