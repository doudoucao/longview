# Core Synthesis: Agent 自动化投研初步提炼

状态：第一轮 Firecrawl 原始材料的核心提炼；第二轮深挖后的新增综合见 `deep-dive-synthesis.md`。

访问日期：2026-06-19

本文件不是最终报告，而是从 `sources/raw/`、`sources/index.md` 和第一轮逐条笔记中提炼早期模式。第二轮全文级材料、代码级材料和论文正文检索已经单独写入 `deep-dive-synthesis.md`，最终中文报告见 `research.md`。

## 1. 核心判断

先进的 agent 自动化投研不是一个单一 agent 问答系统，而是一个可审计的研究流水线。高价值模式反复出现：

- 数据获取先行：先把 SEC/公告/财报/新闻/行情/宏观/论文/GitHub 等来源接入并保存，再做综合判断。
- 角色拆分：Data agent、Concept/Reasoning agent、Thesis/Report agent、Reviewer/Red-team、Human approver 比单一 analyst agent 更可控。
- RAG + 工具调用是基础设施：仅靠模型记忆不够，需要检索、解析、表格/代码计算、估值工具、引用管理。
- 工作流必须有中间产物：原始材料、source note、结构化抽取、证据表、草稿、review 记录应分层保存。
- 自动化不能绕开人工审核：Finance Agent Benchmark、FinGAIA 等材料都提示当前 agent 在复杂金融任务上仍有限制，高风险输出必须 human-in-the-loop。

## 2. 初步范式地图

| 范式 | 代表来源 | 数据获取 | Agent/Skill 结构 | 适合 Longview 的启发 |
| --- | --- | --- | --- | --- |
| Equity research report agent | FinRobot, FinSphere | 公司数据、财务指标、实时数据、研究上下文 | Data-CoT / Concept-CoT / Thesis-CoT 或类似三段式 | 可作为 Longview 自动生成研究初稿的基本角色模型。 |
| Filing-first RAG workflow | Airflow SEC 10-K DAG PR, Finance Agent Benchmark | EDGAR/10-K/MD&A/Risk Factors | ingest -> section extraction -> vector index -> analyst question decomposition -> synthesis -> approval | 适合 Longview 的第一批 MVP，因为证据清晰、可审计、合规风险较低。 |
| Multi-agent investment platform | Azure sample, AWS Bedrock blog, Medium platform article | 产品/平台接入的数据源不一，常含文档和外部 API | 多 agent 协作、任务编排、前后端平台化 | 可学习架构边界，但需警惕 vendor lock-in 和 demo 质量。 |
| Screening/portfolio crew pipeline | MASFIN | Yahoo Finance、Finnhub、新闻情绪 | Postmortem / Screening / Analysis / Timing / Portfolio crews | 对 Longview 可作为“监控与候选池生成”模块，而不是直接投资建议。 |
| Alpha/factor mining loop | AlphaAgent | Qlib、baostock、市场数据 | Idea Agent / Factor Agent / Eval Agent | 可作为研究假设生成与量化验证模块，和深度价值研究分开边界。 |
| Financial agent benchmark/evaluation | Finance Agent Benchmark, FinGAIA | EDGAR、搜索、多文件、多模态、代码计算 | agent harness + tool use + task taxonomy | Longview 需要先定义 evaluation harness，否则自动化质量无法控制。 |

## 3. 数据获取层的初步结论

Longview 的自动化投研数据层应按可信度和可自动化程度分层：

| 数据层 | 例子 | 获取方式 | 自动化难点 | 初步建议 |
| --- | --- | --- | --- | --- |
| 监管披露 / 财报 | SEC EDGAR、10-K、10-Q、公告、MD&A、Risk Factors | API、下载、解析、section extraction | HTML/PDF/XBRL 格式差异、章节切分、版本更新 | MVP 从 EDGAR filing-first workflow 开始。 |
| 公司官方材料 | IR、earnings call、presentation | 网站抓取、PDF parse、transcript API | 网站结构变化、版权和访问限制 | 只保存元数据、短摘录和解析摘要，保留链接。 |
| 市场/基本面数据 | Yahoo Finance、Finnhub、Qlib、baostock、其他 API | API/SDK/下载 | 授权、质量、复权、更新频率 | 作为可替换 provider，不把供应商耦合进研究逻辑。 |
| 新闻/事件/博客 | 新闻、技术博客、CFA/AWS/Medium | Firecrawl search/scrape/crawl | 噪声、重复、营销表述、版权限制 | 用于发现模式和事件，不单独支撑关键结论。 |
| 论文/GitHub | arXiv、GitHub README/issues/PRs | Firecrawl research search-papers/search-github | 代码质量差异、未复现、未来/错误元数据 | 作为框架和 benchmark 来源，需二次核验。 |

## 4. Agent Skills 初步清单

| Skill | 输入 | 输出 | 代表来源 | 失败模式 |
| --- | --- | --- | --- | --- |
| Source discovery | 研究问题、ticker、主题 | 候选来源列表 | Firecrawl search, CFA/AWS/GitHub results | 搜索结果重复、SEO/营销污染。 |
| Raw capture | URL、本地文件、站点范围 | markdown/json/PDF parse/source note | Firecrawl scrape/parse/crawl | 版权限制、paywall、页面渲染失败。 |
| Filing section extraction | filing URL、ticker、表单类型 | MD&A/Risk Factors/notes 等结构化章节 | Airflow SEC 10-K DAG PR | 章节识别错误、不同公司格式差异。 |
| Retrieval/indexing | 原始材料、source notes | per-company/per-topic index | Airflow DAG, RAG patterns | 过期索引、引用漂移、chunk 粒度错误。 |
| Question decomposition | 研究问题 | 子问题、数据需求、工具计划 | Airflow PR, multi-agent workflows | 子问题过粗或漏掉关键证据。 |
| Financial extraction/modeling | 财报/市场数据 | 指标、估值表、同业比较 | FinRobot, FinSphere, MASFIN | 数字幻觉、单位/币种/期间错配。 |
| Thesis synthesis | 证据表、模型结果、反方意见 | 投研初稿/投资 thesis | FinRobot Thesis-CoT | 过度自信、未标注推断。 |
| Review/red-team | 初稿、证据、假设 | 问题清单、修订建议、风险标注 | Benchmarks/HITL patterns | reviewer 同样幻觉，缺少真实证据核查。 |
| Human approval | 草稿和审查记录 | 发布/驳回/返工决定 | Airflow HITL, MASFIN HITL | 审核标准不明确、审批流形式化。 |
| Continuous monitoring | thesis/watchlist/source set | 更新提醒、变更摘要 | Agentic workflows, Firecrawl monitor candidate | 噪声高、重要变化漏报。 |

## 5. 对 Longview MVP 的初步建议

优先做一个 filing-first 的可审计研究 loop，而不是直接做“自动选股”：

1. 输入：ticker + 研究问题。
2. Source discovery：自动发现 SEC filings、IR 页面、最近 earnings call、关键新闻。
3. Raw capture：用 Firecrawl 保存 raw markdown/json，并写 `sources/index.md`。
4. Structured extraction：抽取 MD&A、Risk Factors、财务表、management commentary。
5. RAG/index：按公司和材料类型建索引。
6. Decompose：把研究问题拆成可验证子问题。
7. Draft：生成 thesis memo，但所有结论必须带来源和置信度。
8. Review：自动 reviewer 检查引用、数字、反方证据和风险。
9. Human gate：人工确认后才能进入发布或社区讨论。

这个 MVP 与当前材料最一致：FinRobot 给出 equity research agent 角色模型，Airflow SEC 10-K DAG 给出可审计 workflow，Finance Agent Benchmark 和 FinGAIA 提醒必须评估工具使用与复杂任务能力。

## 6. 下一步研究缺口

- 需要补充官方 API / 数据提供商对比：SEC EDGAR、Finnhub、Polygon、Alpha Vantage、Financial Modeling Prep、Nasdaq Data Link、Tiingo、yfinance、akshare 等。
- 需要逐个检查 GitHub 项目的 license、活跃度、代码结构和可运行性。
- 需要阅读关键论文全文，而不是只用 abstract：FinRobot、Finance Agent Benchmark、MarketSenseAI、FinSphere、FinGAIA、AlphaAgent。
- 需要建立 Longview 自己的 evaluation set：至少覆盖事实检索、财务计算、同业比较、反方证据、报告生成、审稿纠错。
- 需要明确 raw material 保存策略的版权边界：哪些可以保存全文，哪些只保存短摘录、链接和 source note。

## 7. 当前材料引用

原始文件路径和来源元数据见 `sources/index.md`。第一轮逐条中文笔记见 `sources/notes/per-source-notes.md`；第二轮深挖中文笔记见 `sources/notes/deep-dive-source-notes.md`。
