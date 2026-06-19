# 逐条来源笔记：Agent 自动化投研第一轮原始材料

访问日期：2026-06-19

本文件覆盖第一轮 Firecrawl 采集的全部原始记录 R001-R030。重复记录会保留，因为它们来自不同 query；最终综合时应按 URL 去重。

## Web Search Batch A：`search-agentic-ai-investment-tools-github.json`

### R001：Azure-Samples/Agentic-AI-Investment-Analysis-Sample

- URL：https://github.com/Azure-Samples/Agentic-AI-Investment-Analysis-Sample
- 来源类型：GitHub 仓库
- 保存材料：`sources/raw/search-agentic-ai-investment-tools-github.json`
- 核心内容：Azure 样例展示多 agent 投资机会分析平台，包括 Financial、Risk、Market、Compliance 等专业 agent，支持文档处理、机会对象管理、SSE 进度流、workflow 可视化、what-if chat 和报告生成。
- 数据/工作流价值：适合观察产品级多 agent 投研平台的对象模型和 UI/后端边界。
- 限制：官方样例不等于生产成熟系统；Azure 依赖较强。
- Longview 启发：可借鉴文档对象、研究任务对象、进度可视化和 agent debate，但核心证据链需要 Longview 自建。
- 置信度：中。

### R002：Building an Open-Source Agentic AI Equity Research Tool

- URL：https://medium.datadriveninvestor.com/building-an-open-source-agentic-ai-equity-research-tool-172783ed6961
- 来源类型：Medium 实践文章
- 保存材料：`sources/raw/search-agentic-ai-investment-tools-github.json`
- 核心内容：文章讨论开源 agentic equity research tool 的构建思路。
- 数据/工作流价值：可作为实践线索，帮助发现数据采集、综合和报告生成中的工程问题。
- 限制：博客证据等级低，可能有访问限制；不能作为关键能力声明的唯一来源。
- Longview 启发：作为线索，不作为架构定论。
- 置信度：低到中。

### R003：GitHub Topics - investment-research

- URL：https://github.com/topics/investment-research
- 来源类型：GitHub topic 索引页
- 保存材料：`sources/raw/search-agentic-ai-investment-tools-github.json`
- 核心内容：列出 investment research 相关仓库，包含 SEC EDGAR、agent toolkit、LangChain/MCP 等线索。
- 数据/工作流价值：用于发现候选仓库。
- 限制：topic 页不是单个工具的权威文档。
- Longview 启发：仅作为后续抓取入口。
- 置信度：低。

### R004：Building A Multi-Agent Investment Platform

- URL：https://medium.com/data-science-collective/building-a-multi-agent-investment-platform-from-research-to-trade-execution-7ca297595ce9
- 来源类型：Medium 实践文章
- 保存材料：`sources/raw/search-agentic-ai-investment-tools-github.json`
- 核心内容：文章讨论多 agent 投资平台，涉及 Google ADK、A2A、MCP、tools、prompts、memory 和 portfolio manager orchestrator。
- 数据/工作流价值：提供 agent 通信、工具注册和组合经理式编排的概念线索。
- 限制：可能有会员墙；不能支撑生产能力声明。
- Longview 启发：可用于术语和问题清单，不作为核心证据。
- 置信度：低到中。

### R005：Reddit - Open-Source Agentic AI for Company Research

- URL：https://www.reddit.com/r/BusinessIntelligence/comments/1n0lh95/opensource_agentic_ai_for_company_research/
- 来源类型：Reddit 讨论
- 保存材料：`sources/raw/search-agentic-ai-investment-tools-github.json`
- 核心内容：搜索结果提到 Mira 使用 OpenAI Agents SDK 自动化公司研究，但本次抓取没有正文。
- 数据/工作流价值：潜在线索。
- 限制：无正文，证据价值低。
- Longview 启发：只有在找到项目仓库或官方说明后才值得使用。
- 置信度：低。

## Web Search Batch B：`search-ai-equity-research-agent-workflow-blog.json`

### R006：Building an Open-Source Agentic AI Equity Research Tool

- URL：https://medium.datadriveninvestor.com/building-an-open-source-agentic-ai-equity-research-tool-172783ed6961
- 来源类型：Medium 实践文章
- 保存材料：`sources/raw/search-ai-equity-research-agent-workflow-blog.json`
- 核心内容：与 R002 重复，来自第二个 query。
- 数据/工作流价值：说明该文章在相关 query 中重复出现。
- 限制：重复记录，不增加证据强度。
- Longview 启发：去重后使用。
- 置信度：低到中。

### R007：FinRobot GitHub

- URL：https://github.com/ai4finance-foundation/finrobot
- 来源类型：GitHub 仓库
- 保存材料：`sources/raw/search-ai-equity-research-agent-workflow-blog.json`
- 核心内容：FinRobot 是开源金融分析 agent 平台，和论文相互对应，包含 equity research、valuation、Data-CoT / Concept-CoT / Thesis-CoT 等线索。
- 数据/工作流价值：强相关，是研究 agent 角色分层的重要来源。
- 限制：第一轮只保存搜索抓取，第二轮已补全文级抓取和论文正文检索。
- Longview 启发：作为研究报告 agent 架构核心参考。
- 置信度：高。

### R008：CFA Institute - Agentic AI for Finance

- URL：https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance
- 来源类型：专业协会实践指南
- 保存材料：`sources/raw/search-ai-equity-research-agent-workflow-blog.json`
- 核心内容：指南覆盖金融 agent 的 workflow、tools、RAG、多 agent、guardrails 和 human-in-the-loop。
- 数据/工作流价值：适合支撑 workflow-first 和金融风控原则。
- 限制：不是 benchmark，也不是代码实现。
- Longview 启发：Longview 应优先做可审计 workflow，而不是高自治 agent。
- 置信度：高。

### R009：AWS Bedrock Multi-Agent Investment Research Assistant

- URL：https://aws.amazon.com/blogs/machine-learning/part-3-building-an-ai-powered-assistant-for-investment-research-with-multi-agent-collaboration-in-amazon-bedrock-and-amazon-bedrock-data-automation/
- 来源类型：云厂商架构博客
- 保存材料：`sources/raw/search-ai-equity-research-agent-workflow-blog.json`
- 核心内容：AWS 展示 supervisor agent 编排 quantitative、news、smart summarizer 等子 agent，并结合 Bedrock Data Automation、Knowledge Bases、S3、OpenSearch 和 Lambda。
- 数据/工作流价值：提供企业云架构和多模态数据自动化参考。
- 限制：厂商材料，需标注 vendor claim。
- Longview 启发：可借鉴 supervisor/subagent 和工具边界，不应绑定 AWS。
- 置信度：中。

### R010：Reddit - Automated research agent for stock analysis

- URL：https://www.reddit.com/r/algotrading/comments/1l82has/ive_built_an_automated_research_agent_for_stock/
- 来源类型：Reddit 讨论
- 保存材料：`sources/raw/search-ai-equity-research-agent-workflow-blog.json`
- 核心内容：搜索结果声称有自动化股票分析 agent，可拉取多源金融数据并交叉验证。
- 数据/工作流价值：潜在线索。
- 限制：本次无正文抓取；论坛材料不适合支撑关键结论。
- Longview 启发：后续只有找到代码或官方说明才值得深入。
- 置信度：低。

## Paper Search Batch：`papers-llm-agents-financial-analysis.json`

### R011：MarketSenseAI 2.0

- ID：arXiv:2502.00415
- 来源类型：论文搜索结果
- 保存材料：`sources/raw/papers-llm-agents-financial-analysis.json`
- 核心内容：RAG + LLM agent 框架，用于处理新闻、价格、基本面、宏观、SEC filings、earnings calls 和机构报告。
- 数据/工作流价值：强于“多源数据融合”和宏观知识库设计。
- 限制：第一轮仅摘要；第二轮已补论文正文检索。
- Longview 启发：借鉴数据摄取和检索结构，不直接采纳收益表现声明。
- 置信度：中。

### R012：FinRobot paper

- ID：arXiv:2411.08804
- 来源类型：论文搜索结果
- 保存材料：`sources/raw/papers-llm-agents-financial-analysis.json`
- 核心内容：提出 Data-CoT、Concept-CoT、Thesis-CoT 三层 agent 架构，用于 equity research and valuation。
- 数据/工作流价值：最直接的 equity research agent 范式来源之一。
- 限制：第一轮仅摘要；第二轮已补论文正文检索。
- Longview 启发：作为研究报告 agent 分层模型。
- 置信度：高。

### R013：The New Quant survey

- ID：arXiv:2510.05533
- 来源类型：论文搜索结果
- 保存材料：`sources/raw/papers-llm-agents-financial-analysis.json`
- 核心内容：LLM 在金融预测与交易中的 survey，覆盖情绪/事件抽取、数值推理、多模态、RAG、时间序列 prompting 和 agentic systems。
- 数据/工作流价值：用于建立分类框架。
- 限制：仅摘要；日期和元数据需二次核验。
- Longview 启发：作为后续文献综述线索。
- 置信度：中。

### R014：Finance Agent Benchmark

- ID：arXiv:2508.00828
- 来源类型：论文/benchmark
- 保存材料：`sources/raw/papers-llm-agents-financial-analysis.json`
- 核心内容：537 个真实金融研究问题，围绕 SEC filings、EDGAR/search 工具和 agent harness，揭示当前 LLM agent 在复杂金融任务上的限制。
- 数据/工作流价值：Longview 质量门禁的关键来源。
- 限制：第一轮仅摘要；第二轮已补正文检索。
- Longview 启发：必须先建立评测集，再扩大自动化。
- 置信度：高。

### R015：FinSphere

- ID：arXiv:2501.12399
- 来源类型：论文搜索结果
- 保存材料：`sources/raw/papers-llm-agents-financial-analysis.json`
- 核心内容：股票分析 agent、AnalyScore 评估框架和 Stocksis 数据集，强调实时数据、量化工具和报告评估。
- 数据/工作流价值：可作为报告质量评估和实时数据工具线索。
- 限制：仅摘要，尚未全文深挖。
- Longview 启发：后续可作为 evaluation rubric 参考。
- 置信度：中。

### R016：Multi-Agent Investment System for Chinese Public REITs

- ID：arXiv:2602.00082
- 来源类型：论文搜索结果
- 保存材料：`sources/raw/papers-llm-agents-financial-analysis.json`
- 核心内容：面向中国公募 REITs 的多 agent 交易框架，包含公告、事件、价格动量、市场、预测和决策 agent。
- 数据/工作流价值：展示垂直资产类别的 agent 分工。
- 限制：交易导向，且仅摘要；日期需核验。
- Longview 启发：可作为领域特化 workflow 线索，不适合作为 MVP 主线。
- 置信度：中。

### R017：Toward Expert Investment Teams

- ID：arXiv:2602.23330
- 来源类型：论文搜索结果
- 保存材料：`sources/raw/papers-llm-agents-financial-analysis.json`
- 核心内容：多 agent LLM trading framework，强调细粒度任务分解，并使用日本股票数据、价格、财报、新闻和宏观信息。
- 数据/工作流价值：支持“具体任务分解优先于泛泛角色命名”的设计原则。
- 限制：交易导向，且仅摘要；日期需核验。
- Longview 启发：Longview 应拆真实分析步骤，而不是只定义 analyst 角色名。
- 置信度：中。

### R018：P1GPT

- ID：arXiv:2510.23032
- 来源类型：论文搜索结果
- 保存材料：`sources/raw/papers-llm-agents-financial-analysis.json`
- 核心内容：多层多 agent 框架，融合技术面、基本面和新闻见解，生成可解释决策支持。
- 数据/工作流价值：提供多层 synthesis 和解释结构线索。
- 限制：仅摘要；需验证方法和数据泄漏控制。
- Longview 启发：作为 layered synthesis 的后续研究线索。
- 置信度：中。

### R019：AFIB / SuperInvesting benchmark

- ID：arXiv:2603.08704
- 来源类型：论文/benchmark 搜索结果
- 保存材料：`sources/raw/papers-llm-agents-financial-analysis.json`
- 核心内容：多维金融智能 benchmark，覆盖事实准确性、分析完整性、数据时效性、一致性和失败模式。
- 数据/工作流价值：提供评估维度线索。
- 限制：仅摘要；日期和内容需核验。
- Longview 启发：Longview evaluation 不应只测答案对错，还要测完整性、时效性和一致性。
- 置信度：中。

### R020：Multimodal Gen-AI for Fundamental Investment Research

- ID：第一轮 paper search 记录
- 来源类型：论文搜索结果
- 保存材料：`sources/raw/papers-llm-agents-financial-analysis.json`
- 核心内容：与多模态生成式 AI 和基本面投研相关。
- 数据/工作流价值：作为多模态投研线索。
- 限制：第一轮材料不足以支撑细节结论。
- Longview 启发：后续若做 PDF、图表、presentation、earnings call 多模态摄取，可再深挖。
- 置信度：低到中。

## GitHub Research Batch：`github-investment-research-agent-workflow.json`

### R021：Apache Airflow SEC 10-K LlamaIndex DAG PR

- URL：https://github.com/apache/airflow/issues/67671
- 来源类型：GitHub PR/history
- 保存材料：`sources/raw/github-investment-research-agent-workflow.json`
- 核心内容：展示 SEC 10-K financial analysis DAG：weekly indexing、EDGAR 抓取、Risk Factors/MD&A 抽取、LlamaIndex 建索引、on-demand analysis、HITL approval。
- 数据/工作流价值：当前最接近 Longview MVP 的工程 workflow。
- 限制：第一轮为 PR 历史；第二轮已补代码文件。
- Longview 启发：可作为 filing-first MVP 蓝图。
- 置信度：高。

### R022：MASFIN

- URL：https://github.com/mmontalvo9/masfin
- 来源类型：GitHub README
- 保存材料：`sources/raw/github-investment-research-agent-workflow.json`
- 核心内容：CrewAI 五阶段金融预测和组合 pipeline，使用 Yahoo Finance 和 Finnhub，包含 HITL validation。
- 数据/工作流价值：提供 staged crew 和阶段摘要设计。
- 限制：偏交易和组合，评估周期短。
- Longview 启发：可用于 watchlist/monitoring 实验，不作为 MVP 主线。
- 置信度：中。

### R023：MOE-INVEST / guruagents

- URL：第一轮 GitHub research 记录
- 来源类型：GitHub README / 搜索结果
- 保存材料：`sources/raw/github-investment-research-agent-workflow.json`
- 核心内容：与投资 guru agents 或 mixture-of-experts 投资 agent 相关。
- 数据/工作流价值：角色化 agent 线索。
- 限制：第一轮材料不足；需单独抓仓库。
- Longview 启发：谨慎看待“投资大师角色”模式，不能替代证据链。
- 置信度：低。

### R024：AlphaAgent

- URL：https://github.com/rndmvariableq/alphaagent
- 来源类型：GitHub README / paper code
- 保存材料：`sources/raw/github-investment-research-agent-workflow.json`
- 核心内容：Idea Agent、Factor Agent、Eval Agent 挖掘 alpha factors，结合 Qlib 和 baostock。
- 数据/工作流价值：适合研究假设生成和因子实验。
- 限制：偏量化和回测；需防数据泄漏和过拟合。
- Longview 启发：作为后续实验室模块，不进 MVP 主线。
- 置信度：中到高。

### R025：Apache Airflow SEC 10-K LangChain DAG PR

- URL：https://github.com/apache/airflow/issues/67727
- 来源类型：GitHub PR/history
- 保存材料：`sources/raw/github-investment-research-agent-workflow.json`
- 核心内容：LangChain 版本的 10-K DAG，使用 EDGAR、FAISS、LLM 子问题分解、fan-out retrieval、AnalysisReport 和 HITL approval。
- 数据/工作流价值：与 R021 互为对照，说明相同 workflow 可替换 RAG 框架。
- 限制：示例代码仍需生产化处理。
- Longview 启发：Longview 的 workflow 应抽象，不绑定 LangChain 或 LlamaIndex。
- 置信度：高。

### R026：Financial Research Analyst Agent

- URL：第一轮 GitHub research 记录
- 来源类型：GitHub README / 搜索结果
- 保存材料：`sources/raw/github-investment-research-agent-workflow.json`
- 核心内容：与 financial research analyst agent 相关的仓库线索。
- 数据/工作流价值：潜在参考实现。
- 限制：第一轮材料不足，未进入第二轮重点。
- Longview 启发：后续可视需要深挖。
- 置信度：低到中。

### R027：AI Hedge Fund

- URL：https://github.com/virattt/ai-hedge-fund
- 来源类型：GitHub README
- 保存材料：`sources/raw/github-investment-research-agent-workflow.json`
- 核心内容：多个投资风格 agent、fundamentals/valuation/sentiment/technical/risk manager，支持 API key、CLI/web app/backtester。
- 数据/工作流价值：展示多视角 agent 和风险 manager 角色。
- 限制：风格化 agent 易产生权威感和幻觉；偏建议/交易。
- Longview 启发：可作为反例和灵感，不能替代证据链。
- 置信度：中。

### R028：GitHub Topics - investment-research

- URL：https://github.com/topics/investment-research
- 来源类型：GitHub topic 索引
- 保存材料：`sources/raw/github-investment-research-agent-workflow.json`
- 核心内容：与 R003 类似，列出 investment research 相关仓库。
- 数据/工作流价值：来源发现。
- 限制：索引页不能支撑具体能力。
- Longview 启发：用于后续仓库发现。
- 置信度：低。

### R029：FinGAIA

- URL：https://github.com/sufe-aiflm-lab/fingaia
- 来源类型：GitHub README / benchmark
- 保存材料：`sources/raw/github-investment-research-agent-workflow.json`
- 核心内容：中文金融 agent benchmark，覆盖多子领域、多层级任务、工具协作和复杂金融业务场景。
- 数据/工作流价值：中文金融 agent 评测的重要来源。
- 限制：第一轮为 GitHub 搜索记录；第二轮已补 README 和论文正文检索。
- Longview 启发：Longview 中文报告和中文金融语境评测必须纳入。
- 置信度：高。

### R030：AWS Samples Financial Analysis Multi-Agent Collaboration

- URL：第一轮 GitHub research 记录 / AWS sample 相关
- 来源类型：GitHub README / 官方样例
- 保存材料：`sources/raw/github-investment-research-agent-workflow.json`
- 核心内容：AWS Bedrock investment research assistant 样例仓库，包含 supervisor agent、news agent、quantitative analysis agent、smart summarizer agent，以及 web search、stock data、portfolio optimization 工具。
- 数据/工作流价值：云端多 agent 工具编排参考。
- 限制：厂商样例和宽权限部署需谨慎；portfolio optimization 不适合 Longview MVP。
- Longview 启发：可借鉴子 agent 分工和工具部署，但必须加入证据链和人工门禁。
- 置信度：中到高。

## 覆盖校验

- R001-R005：5 条，已覆盖。
- R006-R010：5 条，已覆盖。
- R011-R020：10 条，已覆盖。
- R021-R030：10 条，已覆盖。

合计：30 条原始记录，全部有中文笔记。
