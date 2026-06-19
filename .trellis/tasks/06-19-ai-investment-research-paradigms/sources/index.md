# 来源索引：Agent 自动化投研材料

访问日期：2026-06-19

本索引记录 `ai-investment-research-paradigms` 任务的 Firecrawl 采集材料、原始保存路径、来源价值、限制和综合使用方式。第一轮原始材料保存在 `sources/raw/`，第二轮深挖材料保存在 `sources/raw/deep-dive/`。

## 第一轮采集命令

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

## 第一轮 Raw Files

| 文件 | 保存形式 | 内容 | 备注 |
| --- | --- | --- | --- |
| `sources/raw/search-agentic-ai-investment-tools-github.json` | Firecrawl search JSON，包含 scraped markdown | agentic AI investment research tools 相关 GitHub 和网页结果 | 包含 Azure 样例、Medium 文章、GitHub topics 和一个未抓到正文的 Reddit 结果。 |
| `sources/raw/search-ai-equity-research-agent-workflow-blog.json` | Firecrawl search JSON，包含 scraped markdown | equity research agent workflow、开源工具和实践指南 | 包含 FinRobot GitHub、CFA Institute 指南、AWS Bedrock 投研助手博客。 |
| `sources/raw/papers-llm-agents-financial-analysis.json` | Firecrawl research paper JSON | LLM agents in financial analysis 相关论文搜索结果 | 包含 MarketSenseAI 2.0、FinRobot、Finance Agent Benchmark、FinSphere、FinGAIA 相关材料。 |
| `sources/raw/github-investment-research-agent-workflow.json` | Firecrawl GitHub research JSON | investment research agent workflow 相关 GitHub README/history 结果 | 包含 Airflow SEC 10-K DAG PR、MASFIN、AlphaAgent、AI Hedge Fund、FinGAIA。 |

## 第一轮高价值来源

| ID | 标题 | URL / ID | 类型 | 保存材料 | 关键价值 | 置信度 |
| --- | --- | --- | --- | --- | --- | --- |
| S001 | Azure-Samples/Agentic-AI-Investment-Analysis-Sample | https://github.com/Azure-Samples/Agentic-AI-Investment-Analysis-Sample | GitHub sample | `search-agentic-ai-investment-tools-github.json` | 多 agent 投资机会分析参考实现，适合观察 Azure 架构和产品对象。 | 中 |
| S002 | FinRobot GitHub | https://github.com/ai4finance-foundation/finrobot | GitHub project | `search-ai-equity-research-agent-workflow-blog.json` | 开源金融分析 agent 平台，连接 arXiv 论文，适合研究 Data-CoT / Concept-CoT / Thesis-CoT。 | 高 |
| S003 | FinRobot paper | arxiv:2411.08804 | Paper | `papers-llm-agents-financial-analysis.json` | 定义 equity research and valuation agent framework。 | 高 |
| S004 | CFA Institute: Agentic AI for Finance | https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance | Practitioner guide | `search-ai-equity-research-agent-workflow-blog.json` | 金融 workflow、RAG、多 agent、guardrails 和 HITL 的实践指南。 | 高 |
| S005 | AWS: multi-agent investment research assistant | https://aws.amazon.com/blogs/machine-learning/part-3-building-an-ai-powered-assistant-for-investment-research-with-multi-agent-collaboration-in-amazon-bedrock-and-amazon-bedrock-data-automation/ | Vendor architecture blog | `search-ai-equity-research-agent-workflow-blog.json` | Bedrock 多 agent 投研助手和数据自动化云架构参考；能力声明应按厂商材料处理。 | 中 |
| S006 | MarketSenseAI 2.0 | arxiv:2502.00415 | Paper | `papers-llm-agents-financial-analysis.json` | RAG + LLM agent 处理新闻、价格、基本面、SEC filings、earnings calls、宏观和机构报告。 | 中 |
| S007 | Finance Agent Benchmark | arxiv:2508.00828 | Paper / benchmark | `papers-llm-agents-financial-analysis.json` | 真实金融研究任务 benchmark，强调 EDGAR/search 工具和当前 agent 限制。 | 高 |
| S008 | FinSphere | arxiv:2501.12399 | Paper | `papers-llm-agents-financial-analysis.json` | 股票分析 agent、实时数据、量化工具和评估框架线索。 | 中 |
| S009 | Airflow SEC 10-K financial analysis DAG PR | https://github.com/apache/airflow/issues/67671 | GitHub PR/history | `github-investment-research-agent-workflow.json` | 抓取 SEC 10-K、抽取 Risk Factors/MD&A、建索引、分解问题、结构化报告、人工审批的工程 workflow。 | 中 |
| S010 | MASFIN | https://github.com/mmontalvo9/masfin | GitHub README | `github-investment-research-agent-workflow.json` | 阶段式多 agent 金融预测 pipeline，使用 Yahoo Finance、Finnhub 和 HITL validation。 | 中 |
| S011 | AlphaAgent | https://github.com/rndmvariableq/alphaagent | GitHub README / paper code | `github-investment-research-agent-workflow.json` | Idea / Factor / Eval agent 的 alpha mining workflow。 | 中 |
| S012 | AI Hedge Fund | https://github.com/virattt/ai-hedge-fund | GitHub README | `github-investment-research-agent-workflow.json` | 多角色投资 agent PoC，可作为模式观察和风险批判对象。 | 低 |
| S013 | FinGAIA | https://github.com/sufe-aiflm-lab/fingaia | GitHub README / benchmark | `github-investment-research-agent-workflow.json` | 中文金融 agent benchmark，强调网页浏览、文件处理、多模态、代码和计算。 | 中 |

## 第一轮原始记录覆盖

第一轮 Firecrawl 的每条原始记录都已在 `sources/notes/per-source-notes.md` 中形成中文笔记。

| 原始记录范围 | 来源文件 | 数量 | 笔记覆盖 |
| --- | --- | ---: | --- |
| R001-R005 | `sources/raw/search-agentic-ai-investment-tools-github.json` | 5 | 完成 |
| R006-R010 | `sources/raw/search-ai-equity-research-agent-workflow-blog.json` | 5 | 完成 |
| R011-R020 | `sources/raw/papers-llm-agents-financial-analysis.json` | 10 | 完成 |
| R021-R030 | `sources/raw/github-investment-research-agent-workflow.json` | 10 | 完成 |

## Synthesis Artifacts

| 文件 | 用途 |
| --- | --- |
| `sources/notes/per-source-notes.md` | 第一轮每条原始记录的中文笔记，包含重复记录、空正文记录和仅摘要记录。 |
| `sources/notes/deep-dive-source-notes.md` | 第二轮深挖逐篇中文笔记，覆盖 FinRobot、Finance Agent Benchmark、FinGAIA、MarketSenseAI、CFA、AWS/Azure、Airflow、MASFIN、AlphaAgent、AI Hedge Fund 等高价值来源。 |
| `sources/notes/initial-source-notes.md` | 第一轮早期高价值子集草稿笔记，保留为历史 scratch notes。正式笔记以中文 `per-source-notes.md` 和 `deep-dive-source-notes.md` 为准。 |
| `core-synthesis.md` | 第一轮核心模式和 Longview 初步启发。 |
| `deep-dive-synthesis.md` | 第二轮深挖后的中文综合，连接原始材料、逐篇笔记和最终报告。 |
| `research.md` | 第二轮深挖后的中文正式报告。 |

## 第二轮深挖材料

第二轮深挖聚焦全文级材料、论文正文检索、开源仓库 README、工程样例代码和 GitHub PR 历史。所有正式新增笔记和综合均使用中文撰写。

### 第二轮采集命令

```bash
firecrawl scrape "https://github.com/ai4finance-foundation/FinRobot" \
  --only-main-content \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/finrobot-github.md

firecrawl scrape "https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance" \
  --only-main-content \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/cfa-agentic-ai-for-finance.md

firecrawl scrape "https://aws.amazon.com/blogs/machine-learning/part-3-building-an-ai-powered-assistant-for-investment-research-with-multi-agent-collaboration-in-amazon-bedrock-and-amazon-bedrock-data-automation/" \
  --only-main-content \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/aws-bedrock-investment-research-assistant.md

firecrawl scrape "https://github.com/awslabs/amazon-bedrock-agent-samples/blob/main/examples/multi_agent_collaboration/investment_research_agent/README.md" \
  --only-main-content \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/aws-bedrock-investment-research-agent-readme.md

firecrawl scrape "https://github.com/Azure-Samples/Agentic-AI-Investment-Analysis-Sample" \
  --only-main-content \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/azure-agentic-ai-investment-analysis-sample.md

firecrawl scrape "https://github.com/sufe-aiflm-lab/FinGAIA" \
  --only-main-content \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/fingaia-github.md

firecrawl scrape "https://github.com/virattt/ai-hedge-fund" \
  --only-main-content \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/ai-hedge-fund-github.md

firecrawl scrape "https://github.com/rndmvariableq/alphaagent" \
  --only-main-content \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/alphaagent-github.md

firecrawl scrape "https://github.com/mmontalvo9/masfin" \
  --only-main-content \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/masfin-github.md

firecrawl research search-github "Apache Airflow SEC 10-K financial analysis DAG LlamaIndex LangChain" \
  --limit 10 \
  --json \
  --pretty \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/github-search-airflow-sec-10k-dag.json

firecrawl scrape "https://github.com/apache/airflow/blob/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_langchain_10k.py" \
  --only-main-content \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/airflow-example-langchain-10k.md

firecrawl scrape "https://github.com/apache/airflow/blob/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_llamaindex_10k.py" \
  --only-main-content \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/airflow-example-llamaindex-10k.md

firecrawl research inspect-paper arxiv:2411.08804 --json --pretty \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/paper-inspect-finrobot.json

firecrawl research read-paper arxiv:2411.08804 \
  --question "What agent architecture, data sources, reasoning workflow, evaluation setup, and limitations does the paper describe for equity research and valuation?" \
  --limit 8 \
  --json \
  --pretty \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/paper-read-finrobot-workflow.json

firecrawl research inspect-paper arxiv:2508.00828 --json --pretty \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/paper-inspect-finance-agent-benchmark.json

firecrawl research read-paper arxiv:2508.00828 \
  --question "What tasks, tools, evaluation protocol, agent limitations, and lessons for automated financial research are reported?" \
  --limit 8 \
  --json \
  --pretty \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/paper-read-finance-agent-benchmark.json

firecrawl research inspect-paper arxiv:2507.17186 --json --pretty \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/paper-inspect-fingaia.json

firecrawl research read-paper arxiv:2507.17186 \
  --question "What benchmark tasks, financial domains, agent capabilities, evaluation results, and limitations does FinGAIA report?" \
  --limit 8 \
  --json \
  --pretty \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/paper-read-fingaia.json

firecrawl research inspect-paper arxiv:2502.00415 --json --pretty \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/paper-inspect-marketsenseai.json

firecrawl research read-paper arxiv:2502.00415 \
  --question "What data sources, retrieval architecture, agent workflow, performance claims, and limitations are described for stock analysis?" \
  --limit 8 \
  --json \
  --pretty \
  -o .trellis/tasks/06-19-ai-investment-research-paradigms/sources/raw/deep-dive/paper-read-marketsenseai.json
```

### 第二轮 Raw Files

| 文件 | 保存形式 | 内容 | 用途 |
| --- | --- | --- | --- |
| `sources/raw/deep-dive/finrobot-github.md` | Firecrawl markdown | FinRobot README / GitHub 页面 | equity research agent 架构和工具清单 |
| `sources/raw/deep-dive/paper-inspect-finrobot.json` | Firecrawl Research JSON | FinRobot 论文元数据 | 论文引用和日期 |
| `sources/raw/deep-dive/paper-read-finrobot-workflow.json` | Firecrawl Research JSON | FinRobot 正文相关段落 | Data-CoT / Concept-CoT / Thesis-CoT 验证 |
| `sources/raw/deep-dive/paper-inspect-finance-agent-benchmark.json` | Firecrawl Research JSON | Finance Agent Benchmark 元数据 | benchmark 引用 |
| `sources/raw/deep-dive/paper-read-finance-agent-benchmark.json` | Firecrawl Research JSON | Benchmark 正文相关段落 | 任务、工具、评估和 agent 限制 |
| `sources/raw/deep-dive/fingaia-github.md` | Firecrawl markdown | FinGAIA README | 中文金融 agent benchmark |
| `sources/raw/deep-dive/paper-inspect-fingaia.json` | Firecrawl Research JSON | FinGAIA 论文元数据 | 论文引用 |
| `sources/raw/deep-dive/paper-read-fingaia.json` | Firecrawl Research JSON | FinGAIA 正文相关段落 | 中文金融任务、能力和失败模式 |
| `sources/raw/deep-dive/paper-inspect-marketsenseai.json` | Firecrawl Research JSON | MarketSenseAI 元数据 | RAG/多源数据论文引用 |
| `sources/raw/deep-dive/paper-read-marketsenseai.json` | Firecrawl Research JSON | MarketSenseAI 正文相关段落 | 宏观 agent、HyDE、数据融合 |
| `sources/raw/deep-dive/cfa-agentic-ai-for-finance.md` | Firecrawl markdown | CFA Institute 指南 | workflow vs agent、guardrails、HITL |
| `sources/raw/deep-dive/aws-bedrock-investment-research-assistant.md` | Firecrawl markdown | AWS Bedrock 投研博客 | supervisor/subagent 云架构 |
| `sources/raw/deep-dive/aws-bedrock-investment-research-agent-readme.md` | Firecrawl markdown | AWS 样例仓库 README | 工具栈、权限和部署要求 |
| `sources/raw/deep-dive/azure-agentic-ai-investment-analysis-sample.md` | Firecrawl markdown | Azure 投资分析样例 | 产品对象、SSE、what-if、agent debate |
| `sources/raw/deep-dive/github-search-airflow-sec-10k-dag.json` | Firecrawl Research JSON | Airflow SEC 10-K PR/issue/code搜索 | 工程 workflow、HITL、PR 历史 |
| `sources/raw/deep-dive/airflow-example-langchain-10k.md` | Firecrawl markdown | Airflow LangChain 10-K DAG | code-level filing-first workflow |
| `sources/raw/deep-dive/airflow-example-llamaindex-10k.md` | Firecrawl markdown | Airflow LlamaIndex 10-K DAG | code-level indexing/retrieval workflow |
| `sources/raw/deep-dive/masfin-github.md` | Firecrawl markdown | MASFIN README | staged crew pipeline |
| `sources/raw/deep-dive/alphaagent-github.md` | Firecrawl markdown | AlphaAgent README | alpha mining loop |
| `sources/raw/deep-dive/ai-hedge-fund-github.md` | Firecrawl markdown | AI Hedge Fund README | 多风格 agent 模式及风险案例 |

### 第二轮高价值来源

| ID | 标题 | URL / ID | 类型 | 保存材料 | 关键价值 | 置信度 |
| --- | --- | --- | --- | --- | --- | --- |
| D001 | FinRobot GitHub | https://github.com/AI4Finance-Foundation/FinRobot | GitHub | `deep-dive/finrobot-github.md` | equity research assistant、FMP/SEC、DCF、报告生成 | 高 |
| D002 | FinRobot paper | arxiv:2411.08804 | 论文 | `paper-inspect-finrobot.json`、`paper-read-finrobot-workflow.json` | Data-CoT / Concept-CoT / Thesis-CoT | 高 |
| D003 | Finance Agent Benchmark | arxiv:2508.00828 | 论文/benchmark | `paper-inspect-finance-agent-benchmark.json`、`paper-read-finance-agent-benchmark.json` | EDGAR/search harness、537 题、agent 限制 | 高 |
| D004 | FinGAIA GitHub | https://github.com/SUFE-AIFLM-Lab/FinGAIA | GitHub/benchmark | `fingaia-github.md` | 中文金融 agent benchmark | 高 |
| D005 | FinGAIA paper | arxiv:2507.17186 | 论文 | `paper-inspect-fingaia.json`、`paper-read-fingaia.json` | 七大金融子领域、407 题、失败模式 | 高 |
| D006 | MarketSenseAI 2.0 | arxiv:2502.00415 | 论文 | `paper-inspect-marketsenseai.json`、`paper-read-marketsenseai.json` | RAG + agent 多源数据融合 | 中-高 |
| D007 | CFA Agentic AI for Finance | CFA Institute URL | 专业指南 | `cfa-agentic-ai-for-finance.md` | finance workflow patterns、guardrails、HITL | 高 |
| D008 | AWS Bedrock assistant blog | AWS URL | 厂商架构 | `aws-bedrock-investment-research-assistant.md` | supervisor + 三个 subagent + BDA/RAG | 中-高 |
| D009 | AWS sample README | GitHub URL | 官方样例 | `aws-bedrock-investment-research-agent-readme.md` | 工具部署、web search、stock data、权限 | 高 |
| D010 | Azure investment sample | GitHub URL | 官方样例 | `azure-agentic-ai-investment-analysis-sample.md` | 文档、机会对象、SSE、what-if、agent debate | 高 |
| D011 | Airflow SEC 10-K PR history | GitHub issues/PR | GitHub history | `github-search-airflow-sec-10k-dag.json` | weekly indexing + on-demand analysis + HITL | 高 |
| D012 | Airflow LangChain 10-K DAG | GitHub code | 代码 | `airflow-example-langchain-10k.md` | FAISS + Dynamic Task Mapping + Approval | 高 |
| D013 | Airflow LlamaIndex 10-K DAG | GitHub code | 代码 | `airflow-example-llamaindex-10k.md` | LlamaIndex operators + structured output | 高 |
| D014 | MASFIN | GitHub URL | GitHub | `masfin-github.md` | staged CrewAI pipeline + HITL | 中 |
| D015 | AlphaAgent | GitHub URL | GitHub / paper code | `alphaagent-github.md` | Idea/Factor/Eval alpha mining loop | 中-高 |
| D016 | AI Hedge Fund | GitHub URL | GitHub | `ai-hedge-fund-github.md` | 多风格 agent 和风险案例 | 中 |

## 限制

- 第一轮搜索材料不是穷尽式文献综述；第二轮已对高价值来源做深挖，但仍未覆盖全部数据 provider、SEC/XBRL 工具和可运行性验证。
- 搜索结果包含重复记录和 marketing/vendor claim；高影响声明必须回到论文、官方文档、代码或可复现实验验证。
- Reddit 结果仅作为线索，因为本次没有抓到正文，证据价值较低。
- paper-search 条目的元数据和日期在用于正式引用前仍应二次核验。
- 第二轮论文检索曾触发 Firecrawl Research 429 限流，已改为串行加间隔补抓。未抓取完整 PDF，仅保存论文元数据和正文相关段落检索结果。
- 第二轮抓取保存的是研究审计材料和中文笔记；不应把 vendor/sample/demo 的能力声明当作生产成熟度结论。
