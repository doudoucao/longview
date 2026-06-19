# 深挖来源笔记：Agent 自动化投研第二轮材料

访问日期：2026-06-19

本文件记录第二轮 Firecrawl 深挖材料。目标不是复述原文，而是把每个高价值来源转写成可用于 Longview 产品、研究运营和工程设计的中文笔记。原始抓取保存在 `sources/raw/deep-dive/`。

## D001：FinRobot GitHub 仓库

- URL：https://github.com/AI4Finance-Foundation/FinRobot
- 原始材料：`sources/raw/deep-dive/finrobot-github.md`
- 来源类型：开源仓库 README / 项目页面
- 关键内容：FinRobot 把金融分析 agent 拆成数据源、agent、functional 工具和报告生成模块。仓库 README 明确展示了本地部署 equity research assistant、FMP API 数据获取、3 年预测、DCF 估值、同业比较、15+ 图表、HTML/PDF 报告、FastAPI Web 界面、任务监控和用户认证。
- 数据获取方式：FMP API、SEC API、结构化财务报表、潜在第三方接口、文档和网页数据。
- 自动化工作流：先取财务数据，再做预测/估值/同业比较，然后由 8 个专门 agent 生成投资 thesis、风险评估、估值概览等报告章节。
- 可复用 skill：财务数据拉取、报表标准化、估值辅助、同业比较、图表生成、报告生成、任务监控。
- 限制：README 中部分能力是项目声称，需要实际运行和代码审查确认。使用 FMP、SEC API 等外部服务会带来授权、额度和数据质量问题。项目声明不构成金融建议。
- Longview 启发：FinRobot 是“自动生成研究初稿”的高价值参考，但 Longview 应把它拆成证据层、计算层、叙事层和审查层，而不是直接复制其投资建议式输出。
- 置信度：高，作为架构和功能参考；中，作为可运行成熟度判断。

## D002：FinRobot 论文

- ID：arXiv:2411.08804
- 原始材料：`sources/raw/deep-dive/paper-inspect-finrobot.json`、`sources/raw/deep-dive/paper-read-finrobot-workflow.json`
- 来源类型：论文元数据与正文检索
- 关键内容：论文提出面向 equity research and valuation 的多 agent CoT 架构，包含 Data-CoT、Concept-CoT、Thesis-CoT 三层。Data-CoT 负责 SEC filings、earnings call transcripts、corporate releases、alternative data 等采集与处理；Concept-CoT 负责竞争分析、情绪评估、财务模型和 analyst-style inquiry；Thesis-CoT 将证据综合为研究报告、财务预测、风险评估和估值叙事。
- 数据获取方式：SEC 10-K/10-Q、公司网站、季度公告、投资者演示、sell-side 研究、Google Trends、行业研究、数据库、PDF/DOC/DOCX/JPEG/PNG、搜索引擎和多源网站。
- 自动化工作流：数据处理层 -> 金融概念层 -> 研究报告模板层，强调从结构化指标和非结构化材料生成符合 sell-side report 模板的输出。
- 可复用 skill：多源摄取、文件解析、指标计算、竞争分析、风险识别、叙事生成、格式检查、PDF/图表输出。
- 限制：论文将“buy/hold/sell”等输出作为示例，Longview 不能直接采纳为面向用户的投资建议。论文中的效果对比和质量声称需要复现实验或第三方 benchmark 验证。
- Longview 启发：三层 CoT 比“角色扮演若干投资大师”更适合作为深度价值研究的结构骨架：先证据，再概念，再 thesis。
- 置信度：高，作为范式来源；中，作为效果声明。

## D003：Finance Agent Benchmark 论文

- ID：arXiv:2508.00828
- 原始材料：`sources/raw/deep-dive/paper-inspect-finance-agent-benchmark.json`、`sources/raw/deep-dive/paper-read-finance-agent-benchmark.json`
- 来源类型：论文元数据与正文检索
- 关键内容：该 benchmark 包含 537 个由金融专家撰写和验证的真实金融研究问题，覆盖九类任务，从信息检索到复杂财务建模。agent harness 提供 Google Search、EDGAR search、HTML parsing、document retrieval 等工具。论文报告最佳模型 OpenAI o3 准确率为 46.8%，平均每题成本约 3.79 美元，说明当前 agent 在高风险金融研究中仍不能自治部署。
- 数据获取方式：SEC EDGAR、Google Search、HTML 页面解析、工具内 key-value 存储、LLM 检索处理。
- 自动化工作流：专家问题 -> rubric -> agent tool-use evaluation -> LLM-as-judge 分项评分和 contradiction check。
- 可复用 skill：EDGAR 搜索、网页搜索、HTML 清洗、文档存储、基于 rubric 的分项评分、矛盾检测、错误处理、指数退避重试。
- 限制：论文为 benchmark 设计和基线评测，不是投研产品实现。LLM-as-judge 仍可能带来评估偏差。
- Longview 启发：Longview 必须建立自己的评测集，且必须把复杂财务建模、跨文档检索、数字一致性和引用准确性作为上线门槛。
- 置信度：高，作为风险和评估标准来源。

## D004：FinGAIA GitHub 仓库

- URL：https://github.com/SUFE-AIFLM-Lab/FinGAIA
- 原始材料：`sources/raw/deep-dive/fingaia-github.md`
- 来源类型：开源 benchmark 仓库 README
- 关键内容：FinGAIA 是面向金融场景的中文端到端 agent benchmark，包含 407 道专家指导下人工编写任务，覆盖证券、基金、银行、保险、期货、信托、资产管理七个子领域，以及基础业务分析、资产决策支持、战略风控管理三个层级。README 报告最佳 agent ChatGPT DeepResearch 总体准确率 48.9%，仍落后金融专家 35 个百分点以上。
- 数据获取方式：仓库提供 GitHub 材料、arXiv 论文和 Hugging Face 数据集链接。
- 自动化工作流：用于评估 agent 的网页浏览、文件处理、多模态、代码、计算和金融专业推理能力。
- 可复用 skill：中文金融任务评测、多工具协作评测、跨系统协同评测、复杂场景分层评分。
- 限制：README 中的评测结果需要结合论文和数据集细节复核；部分数据公开范围可能有限。
- Longview 启发：Longview 如果面向中文深度价值社区，应把中文金融语境、术语偏差、业务流程理解和多文件处理纳入评测，而不是只测英文 SEC 问答。
- 置信度：高，作为中文金融 agent benchmark 来源。

## D005：FinGAIA 论文

- ID：arXiv:2507.17186
- 原始材料：`sources/raw/deep-dive/paper-inspect-fingaia.json`、`sources/raw/deep-dive/paper-read-fingaia.json`
- 来源类型：论文元数据与正文检索
- 关键内容：论文强调现有通用 agent benchmark 缺少金融领域的专业性、数据动态性和合规约束。FinGAIA 从行业知识、工具使用和任务复杂度三个维度评估 agent，任务覆盖从信息检索、多模态文档分析、Python 计算到多工具多步骤决策。论文指出常见失败模式包括跨模态对齐不足、金融术语偏差、业务流程感知障碍等。
- 数据获取方式：真实金融业务数据、专家设计任务、多层级场景标注。
- 自动化工作流：从基础业务理解到资产决策支持再到战略风控管理，逐级提高对金融理解、工具协作和推理深度的要求。
- 可复用 skill：多模态文件处理、代码计算、业务流程推理、领域术语校验、跨工具任务执行评估。
- 限制：论文承认对时间序列、实时市场波动、快速变化宏观指标的评估仍不充分；主要评估 zero-shot，不完全覆盖少样本适配。
- Longview 启发：Longview 的质量门禁不能只看“答案像不像”，还要单独评估工具使用、金融术语、流程意识、实时性和多文件处理。
- 置信度：高，作为中文金融 agent 评估来源。

## D006：MarketSenseAI 2.0 论文

- ID：arXiv:2502.00415
- 原始材料：`sources/raw/deep-dive/paper-inspect-marketsenseai.json`、`sources/raw/deep-dive/paper-read-marketsenseai.json`
- 来源类型：论文元数据与正文检索
- 关键内容：MarketSenseAI 2.0 用 RAG + LLM agents 做股票分析，处理金融新闻、历史价格、公司基本面、SEC filings、earnings calls、宏观环境和机构报告。其宏观 agent 包含数据注入、元数据抽取、内容清洗、摘要、语义切分、向量索引、查询扩展和 HyDE 检索。
- 数据获取方式：新闻、价格、基本面、季度报表、SEC filings、earnings call transcripts、央行/统计局/IMF/BIS/投行报告。
- 自动化工作流：多源数据注入 -> 清洗与摘要 -> 语义 chunk -> 向量库 -> 查询扩展/HyDE -> 宏观和个股分析 -> 月度信号生成。
- 可复用 skill：宏观报告摄取、长文档分块摘要、语义索引、HyDE 查询扩展、多源定性/定量融合。
- 限制：论文报告的投资表现需要独立复现，且可能受回测设计、样本选择、交易成本和数据可得性影响。Longview 不应把收益表现作为产品承诺。
- Longview 启发：其“文档摄取和检索层”比“选股收益结果”更值得借鉴，尤其适合做宏观和行业背景知识库。
- 置信度：高，作为数据融合架构来源；低到中，作为投资表现声明。

## D007：CFA Institute《Agentic AI for Finance》

- URL：https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance
- 原始材料：`sources/raw/deep-dive/cfa-agentic-ai-for-finance.md`
- 来源类型：专业协会实践指南
- 关键内容：CFA 指南明确区分 workflow 与 agent：金融高风险场景更适合可控、可审计的 workflow，而不是完全自治 agent。文章总结 prompt chaining、routing、parallelization、orchestrator-workers、evaluator-optimizer、ReAct、多 agent orchestration、MCP 等模式，并强调 guardrails、tool-call gatekeepers、output checks 和 human-in-the-loop。
- 数据获取方式：内部 RAG、SQL agent、外部 web/news search、browser operator、Bloomberg/LSEG/FactSet 等数据接口示例。
- 自动化工作流：基本面筛选、可持续研究 deep research、组合构建增强等案例。
- 可复用 skill：工具选择、数据源 fallback、约束提示、异常数据检查、并行搜索、评估循环、组合约束检查。
- 限制：文章包含行业趋势和案例指导，不是中立 benchmark；部分采用外部引用和趋势数据，需按发布日期理解。
- Longview 启发：Longview 应默认采用“工作流优先、agent 作为可控子步骤”的架构原则。
- 置信度：高，作为金融 workflow 指南。

## D008：AWS Bedrock 多 agent 投研博客

- URL：https://aws.amazon.com/blogs/machine-learning/part-3-building-an-ai-powered-assistant-for-investment-research-with-multi-agent-collaboration-in-amazon-bedrock-and-amazon-bedrock-data-automation/
- 原始材料：`sources/raw/deep-dive/aws-bedrock-investment-research-assistant.md`
- 来源类型：云厂商架构博客
- 关键内容：AWS 示例由 supervisor agent 编排 quantitative analysis agent、news agent、smart summarizer agent。数据包括结构化价格数据、非结构化 SEC filings、分析师报告、earnings calls 和 presentations。BDA 用于多模态数据处理，Bedrock Knowledge Bases/OpenSearch/S3/Lambda 支撑 RAG 和工具调用。
- 数据获取方式：Bedrock Data Automation、S3、OpenSearch Serverless、Lambda 工具、知识库、网页搜索和股票数据工具。
- 自动化工作流：用户问题 -> supervisor 分解 -> subagents 并行/串行执行 -> 聚合 subagent 输出 -> LLM 生成研究洞察。
- 可复用 skill：supervisor 编排、子 agent 专业化、文档自动化、知识库检索、价格数据工具、组合优化工具、摘要。
- 限制：厂商材料会强调自家平台能力；示例输出中包含组合优化和 allocation，应在 Longview 中限制为研究工具，不输出个性化建议。
- Longview 启发：适合参考企业级部署组件，但 Longview 的核心研究对象模型不应绑定 AWS。
- 置信度：中到高，作为 AWS 架构参考。

## D009：AWS Bedrock 投研样例仓库 README

- URL：https://github.com/awslabs/amazon-bedrock-agent-samples/blob/main/examples/multi_agent_collaboration/investment_research_agent/README.md
- 原始材料：`sources/raw/deep-dive/aws-bedrock-investment-research-agent-readme.md`
- 来源类型：官方样例仓库 README
- 关键内容：仓库 README 确认 investment research supervisor agent 有三个 collaborator：News agent、Quantitative Analysis Data agent、Smart Summarizer agent。它还要求部署 web search、stock data lookup、portfolio optimization 工具，并提示 IAM policy 权限较宽。
- 数据获取方式：AWS shared web_search、stock_data CloudFormation stack、Bedrock、S3、Lambda 等。
- 自动化工作流：以股票 ticker 为入口，结合最新新闻和近期股价，由 supervisor 调用子 agent。
- 可复用 skill：Web search、stock data lookup、portfolio optimization、权限隔离检查。
- 限制：示例权限策略较宽，需要最小权限改造；portfolio optimization 不应直接变成 Longview 的用户建议。
- Longview 启发：如果未来做云部署，权限设计和工具边界应早于 agent 编排设计。
- 置信度：高，作为样例实现来源。

## D010：Azure Agentic AI Investment Analysis Sample

- URL：https://github.com/Azure-Samples/Agentic-AI-Investment-Analysis-Sample
- 原始材料：`sources/raw/deep-dive/azure-agentic-ai-investment-analysis-sample.md`
- 来源类型：官方样例仓库 README
- 关键内容：Azure 样例是面向 investment opportunity 的多 agent 平台，包含 Financial Analyst、Risk Analyst、Market Analyst、Compliance Analyst，支持文档上传处理、投资机会管理、工作流可视化、SSE 实时进度、agent debate、what-if chat、Cosmos DB、Blob Storage、Azure OpenAI 和 Microsoft Agent Framework。
- 数据获取方式：用户上传文档、Azure Blob、Cosmos DB、Azure OpenAI、可能的外部市场和尽调资料。
- 自动化工作流：创建 investment opportunity -> 上传文档 -> 文档处理 -> 并行专门 agent 分析 -> agent debate -> summary report -> what-if scenario chat。
- 可复用 skill：文档处理、工作流状态流、agent 辩论、合规视角、场景分析、前端进度展示。
- 限制：偏 demo/sample；更像“投资机会分析平台”，不直接等同于上市公司深度价值研究。
- Longview 启发：产品层面值得借鉴其任务对象、文档对象、分析对象和实时进度；研究层面需加入更严格来源索引和引用校验。
- 置信度：高，作为样例架构来源。

## D011：Airflow SEC 10-K DAG GitHub history

- URL：https://github.com/apache/airflow/issues/67671、https://github.com/apache/airflow/issues/67727
- 原始材料：`sources/raw/deep-dive/github-search-airflow-sec-10k-dag.json`
- 来源类型：GitHub PR / issue history 搜索
- 关键内容：Airflow common.ai 示例展示了“生产形态”的 SEC 10-K 研究管线：索引 DAG 周期性抓取公司 10-K，抽取 Risk Factors 和 MD&A，构建每家公司向量索引；分析 DAG 由人工输入 tickers 和比较问题，LLM 运行时分解子问题，fan-out 检索，合成 Pydantic 结构化报告，并通过 ApprovalOperator 做人工审核。
- 数据获取方式：SEC EDGAR public API、LlamaIndex 或 LangChain、FAISS、Dynamic Task Mapping、Airflow HITL。
- 自动化工作流：weekly indexing DAG + on-demand analysis DAG。
- 可复用 skill：定时摄取、章节抽取、公司级索引、问题分解、fan-out 检索、结构化输出、成本/用量限制、人工审批。
- 限制：示例 DAG 仍是示例，不等于完整生产系统；需要处理 SEC user-agent、失败重试、索引版本、章节抽取准确性和数据更新。
- Longview 启发：这是当前最适合作为 Longview MVP 技术蓝图的来源之一。
- 置信度：高，作为工程 workflow 参考。

## D012：Airflow LangChain 10-K 示例 DAG

- URL：https://github.com/apache/airflow/blob/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_langchain_10k.py
- 原始材料：`sources/raw/deep-dive/airflow-example-langchain-10k.md`
- 来源类型：开源代码文件抓取
- 关键内容：LangChain 示例使用 SEC EDGAR live data，先抓 10-K，再用 RecursiveCharacterTextSplitter + FAISS 建索引。分析 DAG 用 HITLEntryOperator 收集 tickers 和问题，LLM 分解子问题，Dynamic Task Mapping 并行检索，LLMOperator 用 UsageLimits 和 AnalysisReport schema 合成结构化报告，最后 ApprovalOperator 人工审核。
- 数据获取方式：SEC EDGAR public API；LangChainHook；FAISS；Airflow XCom。
- 自动化工作流：decompose -> fan-out retrieval -> collect -> synthesize -> approve。
- 可复用 skill：LangChain 向量索引、结构化输出、人工输入和审批、运行时动态 fan-out。
- 限制：示例中 LangChain 部分用普通 `@task` 包装，没有专门 Airflow operator；生产环境需补索引持久化、审计日志和引用粒度。
- Longview 启发：适合第一版 filing-first MVP，尤其是“人工输入研究问题 + 自动检索 + 人工审批”的闭环。
- 置信度：高，作为代码级 workflow 证据。

## D013：Airflow LlamaIndex 10-K 示例 DAG

- URL：https://github.com/apache/airflow/blob/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_llamaindex_10k.py
- 原始材料：`sources/raw/deep-dive/airflow-example-llamaindex-10k.md`
- 来源类型：开源代码文件抓取
- 关键内容：LlamaIndex 示例与 LangChain 示例共享 DAG 形状，但使用 LlamaIndexEmbeddingOperator 和 LlamaIndexRetrievalOperator。它明确要求 EDGAR User-Agent，提到每家公司一个 persisted index，并把 UsageLimits 和 AnalysisReport schema 暴露在 XCom 里，增强可审计性。
- 数据获取方式：SEC EDGAR public API、LlamaIndex embedding/retrieval、对象存储路径或本地索引。
- 自动化工作流：fetch_filings -> build_index -> analyst_input -> decompose_question -> retrieval_kwargs -> retrieve -> collect_results -> synthesize_report -> format_report -> ApprovalOperator。
- 可复用 skill：LlamaIndex 专用索引/检索 operator、公司级索引、结构化 schema、用量控制、人工审批。
- 限制：仍需验证章节切分、引用精度、索引刷新策略和 XCom 中结构化对象的长期兼容。
- Longview 启发：如果 Longview 选择 Airflow 作为长流程编排层，LlamaIndex 版本比 LangChain 版本更接近“可复用 operator”思路。
- 置信度：高，作为代码级 workflow 证据。

## D014：MASFIN

- URL：https://github.com/mmontalvo9/MASFIN
- 原始材料：`sources/raw/deep-dive/masfin-github.md`
- 来源类型：开源仓库 README
- 关键内容：MASFIN 使用 CrewAI 建立五阶段顺序管线：Postmortem、Screening、Analysis、Timing、Portfolio。数据源包括 Yahoo Finance 和 Finnhub API。每个 crew 有 Summary Agent，并在阶段之间进行 HITL validation。
- 数据获取方式：Yahoo Finance 市场数据、Finnhub 新闻情绪。
- 自动化工作流：失败模式分析 -> 候选筛选 -> 指标分析 -> 入场时机 -> 组合权重；每周重复。
- 可复用 skill：候选池生成、新闻情绪、风险/收益指标计算、阶段摘要、HITL 交接。
- 限制：README 报告 8 周 live-market period 的收益表现，但样本周期很短，且没有置信区间或统计检验。Portfolio 输出不适合直接进入 Longview MVP。
- Longview 启发：可借鉴“阶段化 crew + 每阶段摘要 + 人工确认”，但应把它降级为 watchlist/monitoring，而不是交易建议。
- 置信度：中。

## D015：AlphaAgent

- URL：https://github.com/RndmVariableQ/AlphaAgent
- 原始材料：`sources/raw/deep-dive/alphaagent-github.md`
- 来源类型：开源仓库 README / KDD 2025 paper code
- 关键内容：AlphaAgent 用 Idea Agent、Factor Agent、Eval Agent 三个 agent 挖掘可解释且抗衰减的 alpha factor。它依赖 Qlib backtesting framework 和 baostock 中国股票数据，支持本地 backtest、factor CSV、UI 监控。
- 数据获取方式：Qlib、baostock、中国股票数据、用户自定义 backtest 配置。
- 自动化工作流：市场假设 -> factor 构造 -> backtest/evaluation -> feedback loop -> 因子迭代。
- 可复用 skill：假设生成、因子表达式生成、回测、重复因子/过拟合控制、实验监控。
- 限制：偏量化 alpha mining，不是深度价值研究主线；需要严格防止数据泄漏、过拟合和回测偏差。
- Longview 启发：适合作为后续“研究假设实验室”模块，不应放在第一版研究平台核心。
- 置信度：中到高，作为 alpha mining workflow 来源。

## D016：AI Hedge Fund

- URL：https://github.com/virattt/ai-hedge-fund
- 原始材料：`sources/raw/deep-dive/ai-hedge-fund-github.md`
- 来源类型：开源仓库 README
- 关键内容：AI Hedge Fund 采用多个投资风格 agent，包括 Graham、Buffett、Munger、Damodaran、Taleb、Burry 等角色，以及 Valuation、Sentiment、Fundamentals、Technicals、Risk Manager。项目需要 LLM API key 和 Financial Datasets API key，支持 CLI、Web app 和 backtester。
- 数据获取方式：Financial Datasets API、LLM provider APIs、可能的模型 provider 配置。
- 自动化工作流：多个风格化 analyst agent 给出信号，风险管理设置 position limits，backtester 评估结果。
- 可复用 skill：风格化观点生成、估值/基本面/情绪/技术面信号、风险经理角色、回测 UI。
- 限制：角色扮演的投资大师 agent 易产生权威感和风格幻觉；对 Longview 的证据标准价值低于 filing-first 或 benchmark 来源。仓库自身声明非金融建议。
- Longview 启发：可以作为“反例”提醒 Longview 不要用名人角色替代可审计证据；可保留多视角审稿，但每个视角必须绑定证据和检验规则。
- 置信度：中，作为模式和风险案例。

## 二轮深挖综合判断

- 当前最可落地的范式是“可审计 workflow + 受控 agent”，不是完全自治 agent。
- Longview MVP 应先做 filing-first RAG：SEC/公告/财报摄取、章节抽取、公司级索引、问题分解、结构化报告、自动验证、人工审批。
- FinRobot 的 Data-CoT / Concept-CoT / Thesis-CoT 可作为研究过程分层；Airflow 的 10-K DAG 可作为工程流程分层；Finance Agent Benchmark 和 FinGAIA 应作为质量门禁分层。
- AWS/Azure 样例说明产品体验应包括任务对象、文档对象、agent 进度、可视化、what-if chat，但这些 UI 能力必须服务于证据链，而不是掩盖不确定性。
- MASFIN、AlphaAgent、AI Hedge Fund 有启发，但应放在后续实验层，避免第一版产品直接走向组合建议或交易信号。
