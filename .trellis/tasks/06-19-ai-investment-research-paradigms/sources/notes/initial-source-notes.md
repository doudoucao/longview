# Initial Source Notes

Access date: 2026-06-19

## S001: Azure Agentic AI Investment Analysis Sample

- URL: https://github.com/Azure-Samples/Agentic-AI-Investment-Analysis-Sample
- Source type: GitHub sample
- Saved raw file: `sources/raw/search-agentic-ai-investment-tools-github.json`
- Key content: Firecrawl captured the repository README and file listing. The search description frames it as an AI-powered investment analysis platform using multi-agent workflows for investment opportunity analysis.
- Relevance: Useful as a vendor-backed reference architecture for multi-agent investment analysis UX/backend separation, Azure deployment assumptions, and production-adjacent configuration.
- Limits: Need inspect repository code and license before adopting patterns. Treat Azure service choices as vendor-context, not neutral best practice.
- Confidence: Medium.

## S002/S003: FinRobot

- URLs:
  - https://github.com/ai4finance-foundation/finrobot
  - arxiv:2411.08804
- Source type: GitHub project + paper
- Saved raw files:
  - `sources/raw/search-ai-equity-research-agent-workflow-blog.json`
  - `sources/raw/papers-llm-agents-financial-analysis.json`
- Key content: The paper describes three specialized agents: Data-CoT Agent for aggregating diverse financial data, Concept-CoT Agent for analyst-style reasoning, and Thesis-CoT Agent for synthesizing an investment thesis and report. It emphasizes quantitative and qualitative analysis, valuation, risk assessment, and a dynamically updatable data pipeline.
- Relevance: Strong candidate model for Longview's research object model and agent role decomposition.
- Limits: Need verify repo maturity, examples, dependencies, data adapters, and whether reported outputs are reproducible.
- Confidence: High for conceptual architecture; medium for production readiness until code inspection.

## S004: CFA Institute Agentic AI for Finance

- URL: https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance
- Source type: practitioner guide
- Saved raw file: `sources/raw/search-ai-equity-research-agent-workflow-blog.json`
- Key content: Firecrawl captured a long practitioner guide around AI agents in finance, screening agents, RAG, and multi-agent workflows.
- Relevance: Useful for grounding Longview in finance practitioner expectations and non-engineering workflow language.
- Limits: Secondary/practitioner source. Use for framing and use cases, not as proof of technical capability.
- Confidence: High for workflow framing.

## S005: AWS Bedrock Investment Research Assistant

- URL: https://aws.amazon.com/blogs/machine-learning/part-3-building-an-ai-powered-assistant-for-investment-research-with-multi-agent-collaboration-in-amazon-bedrock-and-amazon-bedrock-data-automation/
- Source type: vendor architecture blog
- Saved raw file: `sources/raw/search-ai-equity-research-agent-workflow-blog.json`
- Key content: Describes building a multi-agent investment research assistant using Amazon Bedrock multi-agent collaboration and Bedrock Data Automation.
- Relevance: Useful for cloud-native orchestration, document automation, and multi-agent collaboration design.
- Limits: Vendor-specific and should be used as one reference architecture, not a neutral evaluation.
- Confidence: Medium.

## S007: Finance Agent Benchmark

- ID: arxiv:2508.00828
- Source type: paper / benchmark
- Saved raw file: `sources/raw/papers-llm-agents-financial-analysis.json`
- Key content: The abstract describes 537 expert-authored real-world finance research questions across nine categories, validated by experts from banks, hedge funds, and private equity firms. It mentions an agentic harness with Google Search and EDGAR database access, and reports substantial limitations in current AI capabilities.
- Relevance: Important benchmark and safety anchor. It suggests Longview should emphasize evaluation, human review, and realistic task design rather than assuming agent autonomy is solved.
- Limits: Need inspect full paper and benchmark details before using numeric claims in final report.
- Confidence: High for benchmark relevance.

## S009: Airflow SEC 10-K Financial Analysis DAGs

- URL: https://github.com/apache/airflow/issues/67671
- Source type: GitHub PR/history
- Saved raw file: `sources/raw/github-investment-research-agent-workflow.json`
- Key content: Describes two DAGs: a weekly indexing DAG fetching 10-K filings via EDGAR, extracting Risk Factors and MD&A, building vector indexes; and an on-demand analysis DAG decomposing analyst questions, retrieving company-specific evidence, synthesizing a structured report, and gating with human approval.
- Relevance: Strong workflow blueprint for Longview's first automatable research loop: source ingest -> section extraction -> indexing -> question decomposition -> retrieval -> structured synthesis -> HITL approval.
- Limits: Example is part of Airflow PR history; inspect merged code/examples before treating it as durable API guidance.
- Confidence: Medium.

## S010: MASFIN

- URL: https://github.com/mmontalvo9/masfin
- Source type: GitHub README
- Saved raw file: `sources/raw/github-investment-research-agent-workflow.json`
- Key content: Multi-stage pipeline with Postmortem, Screening, Analysis, Timing, and Portfolio crews. Data sources include Yahoo Finance and Finnhub API. The README claims weekly portfolios and HITL validation through summary agents.
- Relevance: Useful as an example of decomposed financial reasoning and forecasting. The explicit stages map well to skills/tool inventory.
- Limits: Reported performance needs independent verification; not necessarily aligned with Longview's deep-value orientation.
- Confidence: Medium.

## S011: AlphaAgent

- URL: https://github.com/rndmvariableq/alphaagent
- Source type: GitHub README / paper code
- Saved raw file: `sources/raw/github-investment-research-agent-workflow.json`
- Key content: Autonomous alpha mining framework with Idea Agent, Factor Agent, and Eval Agent. Data preparation mentions Qlib and Chinese stock data via baostock.
- Relevance: Useful for Longview's "hypothesis -> factor/model artifact -> evaluation -> feedback loop" automation pattern.
- Limits: More quant/alpha-mining oriented than fundamental deep-value research.
- Confidence: Medium.

## S013: FinGAIA

- URL: https://github.com/sufe-aiflm-lab/fingaia
- Source type: GitHub README / benchmark
- Saved raw file: `sources/raw/github-investment-research-agent-workflow.json`
- Key content: Benchmark for financial AI agents in real-world financial scenarios. The captured README emphasizes web browsing, file processing, multimodal ability, code, and calculation as required underlying skills.
- Relevance: Strong source for Longview's required agent skills and evaluation dimensions.
- Limits: Chinese financial domain benchmark; relevance to US/global equity research should be mapped carefully.
- Confidence: Medium.

