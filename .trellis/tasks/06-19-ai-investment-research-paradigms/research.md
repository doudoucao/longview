# Agent 自动化投资研究范式与工具深度调研报告

访问日期：2026-06-19

状态：第二轮深挖后的中文正式报告。本文是 Longview 的产品战略、研究运营和工程可落地性研究，不构成任何证券的买入、卖出、持有建议，也不提供个性化投资建议。

主要证据材料：

- 来源索引：`sources/index.md`
- 第一轮逐条笔记：`sources/notes/per-source-notes.md`
- 第二轮深挖逐篇中文笔记：`sources/notes/deep-dive-source-notes.md`
- 第一轮综合：`core-synthesis.md`
- 第二轮综合：`deep-dive-synthesis.md`
- 原始材料：`sources/raw/` 与 `sources/raw/deep-dive/`

## 1. 执行摘要

第二轮深挖后的核心判断是：金融领域先进的 agent 自动化投研，不应理解为“一个自治 agent 直接给投资结论”，而应理解为“可审计 workflow + 受控 agent skills + 结构化证据链 + 自动审查 + 人工门禁”。

对 Longview 来说，最稳健的起点不是自动选股、交易执行或组合建议，而是一个 filing-first 的结构化研究助理：输入 ticker 和研究问题，系统自动获取并保存监管披露、公司材料和相关来源，抽取章节和指标，建立公司级/主题级索引，分解研究问题，生成带来源和置信度的 thesis memo，再经过 reviewer agent 和人工审核。

最关键的证据组合如下：

| 证据来源 | 关键启发 | Longview 采用方式 |
| --- | --- | --- |
| FinRobot GitHub 与论文 | Data-CoT / Concept-CoT / Thesis-CoT 三层 agent research 模型 | 用作研究过程分层，不直接采用投资建议输出 |
| Airflow LangChain/LlamaIndex 10-K DAG | SEC EDGAR 抓取、章节抽取、公司级索引、动态 fan-out、结构化报告、HITL 审批 | 用作 MVP 工程 workflow 蓝图 |
| Finance Agent Benchmark | 真实金融研究任务中最佳模型仍显著不足，需 benchmark 和工具 harness | 用作质量门禁和评测集设计依据 |
| FinGAIA | 中文金融 agent 在复杂业务流程、多工具、多模态任务上仍明显落后专家 | 用作中文场景评测依据 |
| CFA Institute 指南 | 金融高风险场景更适合 workflow-first、guardrails、HITL | 用作产品和合规设计原则 |
| AWS/Azure 样例 | 多 agent 产品体验需要任务对象、文档对象、进度流、what-if chat | 用作 UI/平台形态参考，不绑定云厂商 |

## 2. 范围和方法

### 2.1 调研范围

本报告覆盖：

- Agent 自动化投研范式
- 数据获取方式和证据链
- 开源工具、论文、benchmark、云厂商样例和工程 workflow
- Agent skills / tooling 清单
- 自动化工作流设计
- Longview 的目标运营模型和 MVP 路线图
- 风险、合规和人工审核边界

本报告不覆盖：

- 任何具体证券的投资建议
- 实盘交易策略
- 组合配置建议
- 对监管合规的法律结论
- 对论文或开源项目投资表现的独立复现

### 2.2 方法

本任务使用 Firecrawl 进行两轮资料采集：

- 第一轮：广度发现，包括 web search + scrape、paper search、GitHub research。
- 第二轮：深挖全文级和代码级材料，包括 FinRobot、Finance Agent Benchmark、FinGAIA、MarketSenseAI、CFA 指南、AWS/Azure 样例、Airflow 10-K DAG、MASFIN、AlphaAgent、AI Hedge Fund。

每条重要来源都有 raw material、source index 和中文/结构化笔记。第二轮材料保存于 `sources/raw/deep-dive/`，逐篇中文笔记保存于 `sources/notes/deep-dive-source-notes.md`。

## 3. 来源地图和证据质量

| 来源类型 | 代表材料 | 证据质量 | 使用方式 |
| --- | --- | --- | --- |
| 论文 / benchmark | FinRobot、Finance Agent Benchmark、FinGAIA、MarketSenseAI | 高，适合支撑范式、评估和限制 | 用于架构分层、质量门禁、评测集设计 |
| 开源代码 / README | Airflow DAG、FinRobot、AlphaAgent、MASFIN、AI Hedge Fund | 中到高，取决于代码可运行性 | 用于 workflow、工具和实现形态参考 |
| 官方样例 | AWS Bedrock、Azure Agentic AI Investment Sample | 中到高，适合架构参考 | 用于产品体验和云架构参考，需标记 vendor claim |
| 专业协会指南 | CFA Institute | 高，适合 workflow 和风控原则 | 用于 workflow-first、guardrails、HITL 原则 |
| 博客 / 社区 / 搜索摘要 | Medium、Reddit、GitHub topic | 低到中 | 仅作为线索，不支撑关键结论 |

关键限制：

- 论文中的投资表现结果没有在本任务中复现，不能作为产品承诺。
- Vendor 示例展示的是可实现模式，不等于中立最佳实践。
- GitHub README 反映项目声称和文档状态，仍需代码审查和运行实验。
- 金融数据接口、模型能力和产品能力变化快，后续实现前必须重新验证。

## 4. Agent 自动化投研范式分类

### 4.1 Filing-first RAG 研究流水线

这是 Longview 第一版最适合采用的范式。

典型流程：

1. 从 SEC EDGAR 或本地监管披露源抓取 10-K/10-Q/公告。
2. 抽取 MD&A、Risk Factors、Business、Notes、财务表等章节。
3. 按公司、年份、材料类型和章节建立索引。
4. 把研究问题拆成可验证子问题。
5. 对每个子问题 fan-out 检索。
6. 用结构化 schema 生成报告。
7. 自动 reviewer 检查引用、数字、矛盾和未证实推断。
8. 人工审批后发布或返工。

代表证据：

- Airflow LangChain 10-K DAG 使用 SEC EDGAR live data、FAISS、Dynamic Task Mapping、UsageLimits、AnalysisReport 和 ApprovalOperator。
- Airflow LlamaIndex 10-K DAG 使用 LlamaIndexEmbeddingOperator / LlamaIndexRetrievalOperator，并要求 EDGAR User-Agent。
- Finance Agent Benchmark 也以 SEC filings、EDGAR search 和 web search 为核心工具环境。

Longview 适配度：高。它符合深度价值研究对权威来源、可追溯证据和审计轨迹的要求。

### 4.2 Equity Research Agent 分层

FinRobot 给出的三层模型是目前最清晰的 equity research agent 分层：

| 层级 | 职责 | Longview 改造 |
| --- | --- | --- |
| Data-CoT | 多源数据采集、结构化、指标抽取 | 保存 raw path、source id、访问日期、章节和指标 |
| Concept-CoT | 业务、竞争、财务、风险解释 | 所有判断绑定证据，标注事实/推断/观点 |
| Thesis-CoT | 生成研究 thesis 和报告 | 输出 thesis memo，不输出买卖建议 |

Longview 可采用这个分层作为研究报告 agent 的核心结构，但需要去掉或严格限制 buy/hold/sell 这类输出，把它替换成“证据支持的研究结论、风险、反方观点、待验证问题”。

### 4.3 Workflow-first 金融自动化

CFA Institute 指南明确指出，在高风险金融场景，若步骤可以预先写清楚，workflow 通常比高自治 agent 更可控、更可审计。适合 Longview 的模式包括：

- Prompt chaining：固定顺序生成财报摘要、风险表、初稿。
- Routing：按资产类别、公司地区、数据质量选择子流程。
- Parallelization：并行处理公司、同业、风险因素和新闻。
- Orchestrator-workers：中心编排器拆分任务并汇总。
- Evaluator-optimizer：初稿由 reviewer 循环改进。
- ReAct：仅用于低风险探索或明确工具边界下的信息检索。

Longview 应默认 workflow-first，agent 作为 workflow 中的可控节点，而不是让 agent 自由决定全部步骤。

### 4.4 多 agent 产品平台

AWS 和 Azure 样例显示，投研平台层面常见能力包括：

- Supervisor agent 编排多个子 agent。
- 文档上传和解析。
- 知识库/RAG。
- 股票价格或基本面数据工具。
- 进度流和状态可视化。
- what-if scenario chat。
- 最终报告生成。

这些能力对 Longview 的产品体验有价值，但必须建立在证据链之上。UI 不应只展示“agent 正在思考”，而应展示“agent 获取了哪些材料、使用了哪些工具、哪些结论还没有证据”。

### 4.5 Benchmark-first 质量控制

Finance Agent Benchmark 和 FinGAIA 都说明：当前金融 agent 在复杂任务上仍不能自治。

关键事实：

- Finance Agent Benchmark 包含 537 个专家金融研究问题，使用 EDGAR/search 等工具，最佳模型准确率仍低于 50%。
- FinGAIA 覆盖中文金融七大子领域和 407 个任务，最佳 agent 仍显著落后金融专家。

Longview 的结论：先做 benchmark，再谈自动化规模化。没有评测集的 agent 研究平台不可上线。

### 4.6 候选池、组合和 alpha mining

MASFIN、AlphaAgent、AI Hedge Fund 代表三类更激进的方向：

- MASFIN：CrewAI staged pipeline，覆盖 postmortem、screening、analysis、timing、portfolio。
- AlphaAgent：Idea Agent / Factor Agent / Eval Agent，用 Qlib 和 baostock 做 alpha mining。
- AI Hedge Fund：多个投资风格 agent 与 valuation/fundamentals/risk manager。

这些材料可作为后续实验，但不适合作为 Longview MVP 主线。它们更容易滑向投资建议、交易信号、过拟合和风格幻觉。

## 5. 如何拿数据

| 数据类别 | 代表来源 | 获取路径 | 更新频率 | 结构化难度 | 授权/限制 | Longview 适配 |
| --- | --- | --- | --- | --- | --- | --- |
| SEC/监管披露 | Airflow、Finance Agent Benchmark、FinRobot | EDGAR submissions/companyfacts/filing HTML/API | filing 发布时 | 中到高 | EDGAR User-Agent、格式差异 | 第一优先级 |
| 公司 IR / earnings | FinRobot、AWS/Azure | IR 网站、PDF、earnings call transcript、presentation | 季度/事件 | 高 | 版权、网页变化 | 高 |
| 市场行情 | MASFIN、AlphaAgent、AWS | Yahoo Finance、Finnhub、Qlib、baostock、stock data tools | 日内/日频 | 中 | 授权、延迟、复权 | 中，先做辅助 |
| 基本面 API | FinRobot | FMP、Financial Datasets、其他 provider | 日/季 | 中 | API key、额度、口径 | 高，但需 provider 抽象 |
| 新闻/事件 | AWS、MASFIN、CFA | web search、news API、Firecrawl search | 高频 | 高 | 噪声、重复、版权 | 中，用于监控和补充 |
| 宏观/行业报告 | MarketSenseAI | 央行、统计局、IMF、BIS、投行/机构报告 | 周/月/季 | 高 | 报告版权、观点偏差 | 高，适合作为背景库 |
| 论文/GitHub/博客 | 本任务所有 Firecrawl 材料 | Firecrawl research/search/scrape | 不定期 | 中 | demo 和 marketing 偏差 | 高，用于工具研究 |

Longview 数据层建议：

1. 所有外部研究采集默认使用 Firecrawl 或可复现命令保存 raw material。
2. 所有公司研究先建立 `source index`，再进入 synthesis。
3. 所有 provider 都通过抽象接口接入，避免 FMP、Finnhub、Yahoo 等供应商锁死研究逻辑。
4. 所有高时效来源必须标注访问日期、材料日期和 freshness sensitivity。
5. 所有版权受限材料只保存链接、元数据、短摘录和中文笔记。

## 6. Agent Skills / Tooling 清单

| Skill | 输入 | 输出 | 候选工具 | 失败模式 | Longview 门禁 |
| --- | --- | --- | --- | --- | --- |
| 来源发现 | ticker、研究问题、主题 | 候选来源列表 | Firecrawl search、research search-papers/search-github | 搜索噪声、重复、SEO 污染 | 必须保留 query 和结果文件 |
| 网页抓取 | URL | markdown/json | Firecrawl scrape | JS 渲染失败、paywall | 记录失败原因 |
| 文件解析 | PDF/DOCX/XLSX/HTML | markdown/结构化抽取 | Firecrawl parse、BDA、文档解析器 | 表格错位、图像遗漏 | 人工抽样校验 |
| Filing 获取 | ticker/CIK/form type | filing URL、HTML、metadata | SEC EDGAR、EdgarTools、Airflow 示例逻辑 | ticker/CIK 错配、User-Agent 问题 | 保存 CIK、accession、filing date |
| 章节抽取 | filing 文本 | MD&A、Risk Factors 等 | section parser、LLM 辅助 | 章节边界错 | 引用必须到章节 |
| 向量索引 | 文档 chunks | index / retrieval results | LlamaIndex、LangChain、FAISS | chunk 粒度不当、索引过期 | 索引版本化 |
| 问题分解 | 研究问题 | 子问题和工具计划 | LLMOperator、agent planner | 漏掉关键问题 | reviewer 检查覆盖率 |
| 数字计算 | 财报数据、公式 | 指标表、估值辅助 | Python、spreadsheet、calculator tool | 单位/期间错配 | 可复算 |
| Thesis 生成 | 证据表、指标、反方 | 初稿 | Thesis-CoT、报告模板 | 过度自信、引用漂移 | claim-level citation |
| 自动审查 | 初稿、source notes | 问题清单 | reviewer agent、rubric judge | reviewer 幻觉 | 只允许指出可核验证据问题 |
| 人工审批 | 初稿和审查记录 | 发布/返工 | HITL、ApprovalOperator | 形式化审批 | 审批记录入库 |
| 持续监控 | watchlist、source set | 变更提醒 | Firecrawl monitor、调度器 | 噪声、漏报 | 重要变更必须转 source note |

## 7. 工具和框架对比

| 名称 | 类别 | 核心能力 | 数据适配 | 弱点 | Longview 位置 |
| --- | --- | --- | --- | --- | --- |
| Firecrawl | 研究采集 | search/scrape/parse/crawl/research/interact/agent | 网页、PDF、GitHub、论文 | 需处理限流和访问限制 | 默认采集层 |
| Airflow | 工作流编排 | 定时 DAG、动态任务映射、HITL、XCom | SEC/索引/报告流水线 | 引入部署复杂度 | MVP 编排候选 |
| LlamaIndex | RAG | embedding/retrieval、索引持久化 | 公司级 filing index | 需调 chunk 和引用 | RAG 候选 |
| LangChain | RAG/工具调用 | FAISS、text splitter、工具生态 | 快速原型 | 抽象复杂、版本变化 | 原型候选 |
| FinRobot | 金融 agent 平台 | Data-CoT/Concept-CoT/Thesis-CoT、报告 | FMP、SEC、文档 | 成熟度需运行验证 | 研究分层参考 |
| Finance Agent Benchmark | Benchmark | 537 专家任务、EDGAR/search harness | SEC filings | 不是产品 | 质量门禁参考 |
| FinGAIA | 中文金融 benchmark | 407 中文金融任务、多工具能力 | 中文金融场景 | 数据公开范围需核验 | 中文评测参考 |
| MarketSenseAI | 多源 RAG/agent | 新闻、价格、基本面、SEC、宏观报告 | 多源融合 | 收益声明需复现 | 数据融合参考 |
| CFA 指南 | 方法论 | workflow vs agent、guardrails、HITL | 数据接口原则 | 非代码实现 | 运营原则 |
| AWS Bedrock Agents | 云 agent 平台 | supervisor/subagent、BDA、Knowledge Base | AWS 生态 | vendor lock-in | 云参考 |
| Azure Agent Framework 样例 | 产品样例 | 多 agent、SSE、what-if、文档对象 | Azure 生态 | demo 成熟度 | 产品体验参考 |
| MASFIN | CrewAI pipeline | 五阶段 crew、HITL、Yahoo/Finnhub | 市场/新闻 | 周期短、偏交易 | 后续实验 |
| AlphaAgent | Alpha mining | idea/factor/eval loop、Qlib | 量化数据 | 回测风险 | 研究实验室 |
| AI Hedge Fund | 多角色 PoC | 风格化 agent、backtester | Financial Datasets API | 风格幻觉、建议风险 | 反例和灵感 |

## 8. 自动化工作流模式

### 8.1 一次性公司研究

触发：用户提交 ticker + 研究问题。

产物链：

`source discovery -> raw capture -> source notes -> extraction -> retrieval -> thesis draft -> review -> human approval -> publish`

最小可行版本必须包含：

- raw material path
- source note
- claim type
- confidence
- review status

### 8.2 持续监控

触发：watchlist 中公司发布 10-K/10-Q、8-K、公告、IR 更新、重大新闻。

产物链：

`monitor -> change detection -> source note -> impact summary -> thesis update request`

关键点：持续监控的输出不应直接改变 thesis，而应生成“需要复核的变更事件”。

### 8.3 多公司比较

触发：用户提出跨公司问题，例如“比较 A/B/C 的云业务风险”。

产物链：

`per-company index -> LLM sub-question decomposition -> dynamic fan-out retrieval -> structured comparison -> reviewer`

Airflow 10-K DAG 是当前最清晰的工程样例。

### 8.4 深度研究专题

触发：行业、宏观、技术、政策专题。

产物链：

`query planning -> parallel search/scrape -> source notes -> evaluator loop -> synthesis -> report`

MarketSenseAI 的宏观 agent 和 CFA 的 deep research workflow 对此有启发。

## 9. 来源质量和声明验证流程

Longview 应对每条关键声明执行以下流程：

1. 标记声明类型：事实、估计、观点、推断、建议。
2. 绑定 source id 和 raw path。
3. 标记 freshness：低、中、高。
4. 检查是否有直接证据支撑。
5. 对数字进行单位、期间、币种和公式复核。
6. 检查是否存在反方证据。
7. reviewer 输出问题清单。
8. 人工审批决定发布、返工或降级为假设。

最低质量标准：

- 没有来源的内容不能写成事实。
- 单一 marketing/vendor claim 不能支撑“领先”“最佳”“生产成熟”。
- 回测或收益表现必须标为来源声称，除非 Longview 自己复现。
- 个股层面的结论必须是研究观点或假设，不得变成投资建议。

## 10. Longview 目标运营模型

### 10.1 核心对象

| 对象 | 说明 |
| --- | --- |
| Research Task | 一次研究任务，包含问题、公司、范围、状态 |
| Source | 外部来源，包含 URL、访问日期、类型、权限限制 |
| Raw Material | Firecrawl/EDGAR/parse 保存的原始材料 |
| Source Note | 中文结构化笔记 |
| Extract | 章节、表格、指标、事件、引文 |
| Claim | 报告中的可验证声明 |
| Review Finding | 自动或人工审查问题 |
| Thesis Memo | 带证据和置信度的研究备忘录 |
| Approval | 发布/返工/拒绝记录 |

### 10.2 Agent 分工

| Agent | 职责 |
| --- | --- |
| Research Planner | 拆解研究问题，定义来源策略 |
| Source Collector | 用 Firecrawl/EDGAR/API 保存 raw materials |
| Evidence Extractor | 抽取章节、指标、引文和事件 |
| Financial Calculator | 复算财务指标和简单估值辅助 |
| Concept Analyst | 解释业务、竞争、风险和财务质量 |
| Thesis Writer | 生成结构化 thesis memo |
| Citation Reviewer | 检查引用是否支撑声明 |
| Numeric Reviewer | 检查数字、单位、期间 |
| Red-team Reviewer | 搜索反方证据和弱假设 |
| Human Approver | 最终发布门禁 |

## 11. MVP 路线图

### 第一阶段：Filing-first Research Loop

目标：输入 ticker + question，生成带证据的中文研究备忘录草稿。

包含：

- SEC/监管披露抓取
- Firecrawl raw 保存
- source index 和 source notes
- MD&A / Risk Factors 抽取
- 公司级 RAG
- 问题分解
- 结构化报告
- reviewer checklist
- 人工审批

不包含：

- 自动买卖建议
- 自动组合配置
- 实盘交易
- 用户个性化适配

### 第二阶段：Longview Eval Set

目标：建立 30-50 个内部评测问题。

覆盖：

- 事实检索
- 财务数字复算
- 同业比较
- 风险因素总结
- 反方证据
- 中文报告质量
- 引用准确性

### 第三阶段：数据 provider 抽象

目标：接入 FMP/Finnhub/Yahoo/Financial Datasets/akshare/baostock 等候选数据源，但不让 provider 口径污染研究对象模型。

### 第四阶段：协作产品化

目标：让用户看到研究过程，而不仅是最终答案。

包含：

- 任务进度
- 来源列表
- 证据图谱
- reviewer findings
- 变更提醒
- 社区讨论入口

## 12. 风险和控制

| 风险 | 表现 | 控制 |
| --- | --- | --- |
| 幻觉引用 | 引用不能支撑结论 | claim-level citation review |
| 数字错误 | 单位、期间、币种、同比错配 | numeric reviewer + 可复算表 |
| 数据过期 | 旧 filing 或旧价格被当作最新 | freshness label |
| 版权问题 | 保存不该保存的全文 | 保存元数据、短摘录、笔记 |
| 过度自动化 | 用户误以为是投资建议 | 非投资建议边界 + 人工审批 |
| Vendor lock-in | 绑定 AWS/Azure/FMP | provider 抽象 |
| 回测幻觉 | 短期收益被包装成可靠策略 | 不把收益声明作为产品承诺 |
| 角色幻觉 | 名人 agent 输出权威化 | 所有观点绑定证据和规则 |

## 13. 后续研究任务

1. 市场数据 provider 深度对比：FMP、Finnhub、Polygon、Tiingo、Alpha Vantage、Nasdaq Data Link、Financial Datasets、yfinance、akshare、baostock、Qlib。
2. SEC/XBRL 工具专项：SEC submissions/companyfacts、EdgarTools、sec-api、XBRL parsers、Airflow DAG 可运行性。
3. FinRobot 可运行性审查：安装、API key、数据依赖、报告质量、引用能力、license。
4. Longview eval set：设计中文/英文混合 benchmark 和评分 rubric。
5. Firecrawl ingestion playbook：统一 search/scrape/parse/research/crawl/map/interact/agent 的保存规范。
6. Human review workflow：定义 reviewer 输出、人工审批状态和返工机制。

## 14. 参考来源

详见 `sources/index.md` 和 `sources/notes/deep-dive-source-notes.md`。关键来源包括：

- FinRobot GitHub：`sources/raw/deep-dive/finrobot-github.md`
- FinRobot paper：`sources/raw/deep-dive/paper-read-finrobot-workflow.json`
- Finance Agent Benchmark：`sources/raw/deep-dive/paper-read-finance-agent-benchmark.json`
- FinGAIA GitHub/论文：`sources/raw/deep-dive/fingaia-github.md`、`sources/raw/deep-dive/paper-read-fingaia.json`
- MarketSenseAI：`sources/raw/deep-dive/paper-read-marketsenseai.json`
- CFA Institute 指南：`sources/raw/deep-dive/cfa-agentic-ai-for-finance.md`
- AWS Bedrock 博客和样例：`sources/raw/deep-dive/aws-bedrock-investment-research-assistant.md`、`sources/raw/deep-dive/aws-bedrock-investment-research-agent-readme.md`
- Azure 样例：`sources/raw/deep-dive/azure-agentic-ai-investment-analysis-sample.md`
- Airflow SEC 10-K DAG：`sources/raw/deep-dive/github-search-airflow-sec-10k-dag.json`、`sources/raw/deep-dive/airflow-example-langchain-10k.md`、`sources/raw/deep-dive/airflow-example-llamaindex-10k.md`
- MASFIN：`sources/raw/deep-dive/masfin-github.md`
- AlphaAgent：`sources/raw/deep-dive/alphaagent-github.md`
- AI Hedge Fund：`sources/raw/deep-dive/ai-hedge-fund-github.md`
