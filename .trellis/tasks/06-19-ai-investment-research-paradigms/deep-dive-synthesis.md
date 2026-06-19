# 深挖综合：Agent 自动化投研范式与 Longview 落地判断

访问日期：2026-06-19

本综合基于第二轮 Firecrawl 深挖材料，原始材料见 `sources/raw/deep-dive/`，逐篇中文笔记见 `sources/notes/deep-dive-source-notes.md`。本文件是从证据到最终报告之间的中间综合，不替代原始材料和 source notes。

## 1. 核心结论

第二轮深挖后，判断比第一轮更明确：

1. 金融场景的先进 agent 自动化投研不是“自治 agent 直接给投资结论”，而是“可审计 workflow + 受控 agent skill + 人工门禁”。
2. 目前最适合 Longview MVP 的路线是 filing-first RAG：从 SEC/公告/财报等高可信来源开始，先解决材料摄取、章节抽取、公司级索引、证据引用、结构化报告和人工审批。
3. FinRobot 的 Data-CoT / Concept-CoT / Thesis-CoT 给出研究分层；Airflow 10-K DAG 给出工程分层；Finance Agent Benchmark 和 FinGAIA 给出质量评估分层。
4. AWS/Azure 样例说明产品体验需要文档对象、任务对象、agent 进度、what-if chat 和多 agent 协作，但这些界面能力必须服务于证据链，而不是制造“自动化很强”的错觉。
5. MASFIN、AlphaAgent、AI Hedge Fund 可以作为后续实验参考，但不应成为 Longview 第一版主线，因为它们更接近交易、组合或风格化信号生成，合规和信任门槛更高。

## 2. 先进范式的稳定结构

| 层级 | 最强证据来源 | 稳定模式 | Longview 应采用的版本 |
| --- | --- | --- | --- |
| 数据层 | FinRobot、MarketSenseAI、Airflow DAG | 多源摄取、原始材料保存、章节/语义切分、向量索引 | 先做 filings/IR/earnings call 的保守数据层，再扩展新闻和宏观 |
| 推理层 | FinRobot、CFA 指南、AWS/Azure 样例 | Data agent、reasoning agent、thesis/report agent、reviewer | Data-CoT -> Concept-CoT -> Thesis-CoT -> Reviewer |
| 编排层 | Airflow LangChain/LlamaIndex 10-K DAG、AWS Bedrock | 定时索引 + 按需分析 + 动态 fan-out + 结构化输出 | 研究任务状态机，保留每一步输入、输出、来源和失败 |
| 评估层 | Finance Agent Benchmark、FinGAIA | 专家问题、rubric、工具 harness、分项评分、矛盾检查 | Longview 自建小型 benchmark，覆盖中文/英文、财务计算、引用和反方证据 |
| 产品层 | Azure 样例、AWS 样例 | 文档上传、机会/公司对象、进度流、what-if chat | 先把研究过程可视化，再开放社区协作 |

## 3. 数据获取和证据链

第二轮材料显示，成熟投研 agent 的数据层至少要区分五类：

| 数据类别 | 代表来源 | 获取方式 | 自动化重点 | 风险 |
| --- | --- | --- | --- | --- |
| 监管披露 | Airflow DAG、Finance Agent Benchmark、FinRobot | SEC EDGAR API、公司 CIK/ticker 映射、10-K/10-Q 下载 | User-Agent、章节抽取、版本化索引、引用到 filing/section | 章节切分错误、数字期间错配 |
| 公司材料 | FinRobot、AWS/Azure 样例 | IR 页面、earnings call、presentation、PDF/DOC/DOCX | 文档解析、摘要、图表/表格抽取 | 版权、付费墙、格式变化 |
| 市场和基本面 | FinRobot、MASFIN、AWS 样例、AlphaAgent | FMP、Yahoo Finance、Finnhub、Financial Datasets、Qlib、baostock | provider 抽象、复权、频率、币种和单位标准化 | 授权、延迟、供应商不一致 |
| 宏观和行业 | MarketSenseAI、CFA 指南 | 央行、统计局、IMF、BIS、投行/机构报告 | 元数据抽取、长文档清洗、语义 chunk、HyDE/查询扩展 | 来源混杂、观点偏差 |
| 技术和研究材料 | GitHub、arXiv、CFA/AWS/Azure 文档 | Firecrawl search/scrape/research、GitHub README/PR/code | 保存 raw、source notes、版本和访问日期 | demo 夸大、论文未复现 |

Longview 的第一原则应是：先保存来源，再做结构化笔记；先能追溯证据，再生成 thesis。

## 4. 工作流模板

### 4.1 Filing-first MVP

1. 用户输入：ticker + 研究问题。
2. Source discovery：解析 ticker/CIK，定位最新 10-K/10-Q、IR 页面和 earnings materials。
3. Raw capture：用 Firecrawl 和 EDGAR 工具保存 markdown/json/元数据。
4. Section extraction：抽取 MD&A、Risk Factors、business、notes、财务表。
5. Indexing：按公司、材料、年份、章节建立索引。
6. Decompose：把研究问题拆成公司/章节/指标级子问题。
7. Retrieval：每个子问题独立检索，保留 chunk、score、来源。
8. Structured synthesis：用 schema 生成 executive summary、company findings、key risks、open questions。
9. Validation：检查引用、数字、期间、矛盾、未证实推断。
10. Human approval：人工审批后才能发布或进入社区讨论。

### 4.2 研究报告 agent 分层

| Agent | 输入 | 输出 | 工具 | 必须留痕 |
| --- | --- | --- | --- | --- |
| Data-CoT | ticker、filing URLs、IR URLs | 原始材料、章节、指标表 | Firecrawl、EDGAR、parse、表格抽取 | raw path、source id、访问日期 |
| Concept-CoT | 章节、指标、同业/行业材料 | 业务解释、风险解释、同业比较 | RAG、计算、行业知识库 | 每个判断对应来源 |
| Thesis-CoT | 证据表、概念分析、反方证据 | thesis memo 初稿 | 报告模板、引用工具 | 声明类型和置信度 |
| Reviewer | 初稿、证据表 | 问题清单、修订建议 | 引用核查、数字复算、反例搜索 | 发现和修订记录 |
| Human Approver | 草稿、review 记录 | 发布/退回/补充研究 | 人工判断 | 审批人、时间、决定 |

## 5. 质量门禁

Finance Agent Benchmark 和 FinGAIA 都显示当前 agent 在金融任务上远未达到自治标准。Longview 需要上线前门禁：

| 门禁 | 检查内容 | 最低要求 |
| --- | --- | --- |
| 来源门禁 | 是否有原始材料和 source note | 关键结论必须链接到 source id |
| 引用门禁 | 引用是否支持具体句子 | 不允许“弱引用”支撑强结论 |
| 数字门禁 | 单位、币种、期间、同比/环比 | 关键数字必须可复算 |
| 时效门禁 | filing、价格、新闻、宏观数据日期 | 高时效内容必须标访问日期 |
| 推断门禁 | 事实/估计/观点/推断/建议是否分开 | 未证实推断不能写成事实 |
| 反方门禁 | 是否检索反面证据和主要风险 | thesis 必须包含反方观点 |
| 人工门禁 | 是否经过 reviewer 和人工确认 | 未通过不得发布 |

## 6. 工具和框架取舍

| 工具/框架 | 第二轮证据 | 适合 Longview 的位置 |
| --- | --- | --- |
| Firecrawl | 当前任务已用于 search/scrape/research/raw 保存 | 默认外部研究采集和原始材料保存工具 |
| Airflow | 10-K 示例展示 scheduled indexing、on-demand analysis、HITL | 长流程编排候选，适合 filings pipeline |
| LlamaIndex | Airflow LlamaIndex DAG 有专用 embedding/retrieval operator | RAG 索引和检索候选 |
| LangChain | Airflow LangChain DAG 使用 FAISS 和 plain task | 快速原型候选 |
| Bedrock Agents | AWS 样例提供 supervisor/subagent/cloud 工具组合 | 企业云部署参考，不作为默认绑定 |
| Microsoft Agent Framework | Azure 样例提供多 agent + SSE + what-if chat | 产品体验和 workflow 可视化参考 |
| CrewAI | MASFIN 阶段化 crew pipeline | 研究实验参考，不作为第一版主线 |
| Qlib | AlphaAgent 依赖 Qlib 回测 | 后续因子/量化实验模块 |

## 7. Longview 第一批实验

1. 建立 `filing-first-research-loop` 任务：输入 ticker + question，保存 SEC 10-K raw、章节和 source notes。
2. 建立 `research-eval-mini-benchmark`：设计 30-50 个 Longview 风格问题，覆盖事实检索、数字计算、风险总结、反方证据、中文报告。
3. 建立 `source-claim-schema`：为报告每条关键声明保存 source id、claim type、confidence、freshness、review status。
4. 建立 `firecrawl-ingestion-playbook`：定义 search/scrape/parse/research/map/crawl 的命名规范、保存位置和失败记录。
5. 建立 `human-review-workflow`：规定 reviewer agent 和人工审批输出格式。

## 8. 明确非目标

- 不做自动买入/卖出/持有建议。
- 不做直接面向用户的组合配置。
- 不把短期回测收益作为产品承诺。
- 不把名人投资风格 agent 当作证据来源。
- 不绕过付费墙、登录限制或授权限制保存全文。

## 9. 仍需补充

- 市场数据 provider 深度对比：FMP、Finnhub、Polygon、Tiingo、Alpha Vantage、Nasdaq Data Link、yfinance、akshare、baostock、Qlib。
- SEC/XBRL 专门工具评估：companyfacts、submissions、XBRL parser、EdgarTools、sec-api 等。
- 论文/仓库可运行性验证：FinRobot、AlphaAgent、MASFIN、Finance Agent Benchmark harness。
- 中文投资研究语料和术语表：面向 A/H 股、公告、年报、交易所问询函。
