[Skip to main content](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#main-content)

notices - See details

Notices



Dismiss all

# Agentic AI For Finance: Workflows, Tips, and Case Studies

**By** [**Brian Pisaneschi, CFA**](https://www.linkedin.com/in/brianpisaneschi)

[Table of Contents](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#table-of-contents)

## Introduction

Artificial intelligence (AI) agents—once the domain of niche machine-learning teams—have now moved firmly into the investment mainstream. In Q4 2024, earnings call mentions of AI agents jumped 4x quarter-over-quarter, [according to CB Insights](https://www.cbinsights.com/research/ai-agent-trends-to-watch-2025/).

Venture capital trends reflect the same momentum. In early-stage hubs like Y Combinator—often a bellwether for the broader venture capital (VC) landscape—agentic AI startups now account for the majority of VC targeting the investment sector.1 Specifically, **Exhibit 1** shows that  over the period January 2024 to June 2025, 73% of the investment-related startups funded by Y Combinator were agentic AI related.

##### **Exhibit 1. Al Classification among Investment-Related Y Combinator Startups (Jan 2024–Jun 2025)**

![ai-agents_exhibit1.jpeg](https://cfainstitute.widen.net/content/jeulohxnlk/jpeg/ai-agents_exhibit1.jpeg?w=800&keep=c&crop=yes&color=cccccc&quality=80&u=bkkbnx)

So what is agentic AI? Is the hype leading to value? Many firms have internal proof-of-concept tools swirling around, but can these systems be trusted in production? Specifically, can investment professionals rely on them to make decisions, surface insights, or take other actions? We explore these questions here with practical examples.

This installment of [_The Automation Ahead_ series](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series) is a practical guide to Agentic AI in finance. It’s designed to give professionals a clear starting point, grounded in real use cases. You’ll find walkthroughs for the following:

- Building a fundamental screening agentic workflow
- Running a sustainability research agentic workflow
- Combining these into a portfolio construction workflow

Along the way, we break down key concepts, common pitfalls, and implementation tips—whether you’re using Python, no-code tools, or both.

We close with applied case studies that bring it all together, including a custom index built to client specifications. The goal is to show how these tools, even in their imperfect form, can bring greater nuance, customization, and control to modern investment workflows.

Before we get into the how, let’s start with the what: what exactly are AI agents?

## What are Agents?

At their core, AI agents are programs that can **independently** take actions on behalf of the user. As large language models (LLMs) have evolved, their ability to perform chain-of-thought (CoT) reasoning—where the model breaks problems down step by step—has enabled them to tackle increasingly complex tasks. When these models are trained to output data in formats compatible with traditional automation systems, they gain the ability to call external functions independently. This independence is a key distinction from user-defined LLM **workflows**.

_For a deeper understanding of how agentic AI and related technologies fit into broader investment workflows and decision systems, see_ [_AI in Asset Management_](https://rpc.cfainstitute.org/research/foundation/2025/ai-in-asset-management-book) _from CFA Institute Research Foundation._

### Workflows versus Agents

Workflows are **predefined**, step-by-step sequences in which a language model is asked to perform specific tasks—like extracting data, generating summaries, or making classifications—before moving on to the next step.

The distinction has led to some confusion between workflows and agents. In fact, due to the novelty of agentic AI systems, a single industry definition of AI agents does not yet exist. For example, Anthropic makes a clear distinction between _workflows_ as predefined code paths and _agents_ as highly autonomous systems2. Whereas [OpenAI](https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf) makes only a clear distinction between agents and **single-turn LLM** calls, like simple chatbots and sentiment classifiers, the latter not being agents.

**Exhibit 2** depicts the differences between LLM systems.

##### **Exhibit 2. LLM Systems**

![workflow_agents.drawio.svg](https://cfainstitute.widen.net/content/ueqqfb8iuh/svg/workflow_agents.drawio.svg?u=bkkbnx)

Because finance is a tightly regulated, high-stakes domain, workflow-style automations, which offer more control and predictability, are more likely to be adopted in practice than highly autonomous, low predictability agents. For that reason, this article follows Anthropic’s separation between workflows and agents, with an in-depth focus on workflow patterns relevant to finance use cases.

Despite the lack of a single industry definition for agents, consensus is growing around the core building blocks of agentic systems. In the next section, we break down these components and explain how they come together to form agentic AI systems.

### The Building Blocks of an Agent

Since AI models are trained on human-generated data, itʼs no surprise that AI agents are built with many of the same functional components we rely on to get things done. To complete a task, we need clear instructions. We use tools to extend our capabilities. We rely on memory to retain context, seek out new information when we donʼt have the answers, and operate within boundaries to avoid mistakes and manage risks. AI agents are no different. **Exhibit 3** shows how the building blocks of an agent work together.

##### **Exhibit 3. Agent Building Blocks**

![agent_components.drawio.svg](https://cfainstitute.widen.net/content/dkz3ntyw1k/svg/agent_components.drawio.svg?u=bkkbnx)

We start with the first building block Instructions.

#### Instructions

Building an agent starts with giving it the right context to act effectively. This is the role of prompt engineering. Well-crafted instructions define what the agent needs to know, what itʼs being asked to do, and how it should do it.3 The following fundamentals shown in **Exhibit 4** outline best practices for writing clear, actionable agent instructions.

##### Exhibit 4. Best Practices for Writing Agent Instructions

| Component | What to Include | Example (Fixed-Income Trade-Idea Agent) |
| --- | --- | --- |
| **Role** | Title + expertise | “Investment-grade Credit Strategist” |
| **Task / Objective** | Outcome & success metric | “Produce a 200-word long/short idea with target spread move and catalysts.” |
| **Constraints** | Forbidden actions, compliance rules, data limits | - Exclude tickers from \[…\]<br>- Cite source for each metric<br>- Abort if spread data > 24h old |
| **Guidance** | Tool/agent usage triggers, fallback order, edge-case handling | “Use BloombergAPI first; if error, switch to LSEGAPI . If both fail, return DATA\_STALE .ˮ |
| **Response Format** | Schema & style | JSON:\`{"ticker": "...", "direction": "long"} |

The table above offers a solid starting point for defining agent instructions—but iteration is essential. In practice, agents often behave in unexpected ways, requiring you to sharpen task definitions, adjust constraints, or clarify assumptions to steer them more effectively.

#### Tools

LLM tools are just functions you make available for the model to use. You define each tool—usually a name, a short description, and what inputs it needs. When executing a task, the model looks at the available tools. decides if a tool is useful, and then tells your code what inputs to pass. Then, your system runs the tool and sends the result back to the model. This “ [function calling](https://huggingface.co/docs/hugs/en/guides/function-calling)” can be used in several applications, as shown in **Exhibit 5**.

##### Exhibit 5. Tool Types in Finance

| Tool Category | What the Agent Can Do | Example Financial Use Case | Example Interfaces |
| --- | --- | --- | --- |
| **Data Retrieval** 4 | Fetch third-party data via API endpoints | “Get current P/E ratios for S&P 500” → Calls Bloomberg or FactSet | Bloomberg BQL, LSEG, FactSet |
| **Action Execution** 4 | Transform user intent into specific API actions | “Rebalance model portfolio to target weights” → Places trades via broker API | Order Management System, Execution Management System |
| **Computation Tasks** 4 | Run math or code to generate output | “Calculate expected return under CAPM” | Python tool, Anthropic/OpenAI code interpreter |
| **LLMs as Tools** | Use specialized models or LLMs for specific tasks | “Analyze tone of CEO remarks in Q1 call” → Fine-tuned sentiment LLM or classifier | Hugging Face models |

These tools help overcome many of the well-documented limitations of large language models. LLMs arenʼt designed for reliable computation,5  but connecting them to calculators gives them accurate mathematical capabilities. Their knowledge is frozen at the point of their last training cutoff,6  but linking them to application programming interfaces (APIs) and live data feeds restores real-time awareness.

Another major limitation is their inherent lack of information that wasn’t available in their training data.7 This leads directly to our next building block: Information Retrieval.

#### Information Retrieval

For agentic systems to generate accurate, grounded outputs, they must be able to retrieve high-quality, context-specific information on demand. This is where knowledge retrieval comes into play. **Exhibit 6** shows the two primary forms of agentic information retrieval, internal and external.

##### Exhibit 6. Internal and External Information Retrieval

| Retrieval Layer | Typical Sources & Method | Common Tools / APIs |
| --- | --- | --- |
| **Internal** | - Retrieval-Augmented Generation (RAG) - vector databases<br>- SQL agents hitting proprietary tables | - FAISS, chromadb<br>- SQL Agents |
| **External** | - Web / news search APIs<br>- “Operator” browser agents that crawl pages and scrape text | Google (SerpAPI), DuckDuckGo, OpenAI/Anthropic search tools |

Internal retrieval keeps the LLM grounded on your data. A retrieval-augmented generation (RAG) step can surface your internal documents or ground the LLM on more direct information than its general pretrained knowledge. For example, an SQL agent might translate a prompt like “show me the top 10 holdings by VaRˮ into a safe, structured query against your proprietary data warehouse. For a deep dive on RAG, check out the previous installment of the Automation Ahead series, “ [RAG for Finance: Automating Document Analysis with LLMs](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/retrieval-augmented-generation).”

External retrieval gives the LLM access to external knowledge sources like the internet. Many platforms are building deep research agents that perform an agentic knowledge retrieval search for a given search topic.

First, it generates multiple search queries that explore different angles of the topic. Then, it enters a loop: evaluating the results, deciding whether the information is sufficient, and issuing follow-up queries, if needed. This process continues until the agent determines it has enough context to address the research problem. At that point, it synthesizes the findings and generates a final output—typically in the form of a report or structured summary.

Another form of external knowledge retrieval involves “operatorˮ agents that emulate a userʼs behavior in a web browser. These agents use vision-based capabilities to navigate websites, take screenshots, and perform step-by-step actions. The process is iterative: The agent forms a plan, takes control of the browser, observes the screen, and decides the next move based on what it sees.

While promising, these agents are still highly experimental and very unstable. Their success depends on factors like page structure, bot detection, loading times, and how well they can interpret visual or interactive content. Often the task could be done much faster by a human.

#### Memory

Agent memory stores relevant information the system has encountered and may need to recall later—either for its own use or to pass along to another agent or subprocess. Agent memory is typically divided into two forms:

- **Short-term memory** holds immediate context like conversation history or the current process state. This allows information to persist across steps and is often represented as structured datasets, lists, or key-value pairs.
- **Long-term memory** captures persistent knowledge accumulated over time. This usually involves writing important data to a vector database, which can be queried through a RAG system to surface relevant context on demand.

In practice, most agents use a hybrid memory system that combines both short- and long-term memory, depending on the complexity and duration of the task at hand.

#### Guardrails

Guardrails act as the agentʼs defense system. They prevent misuse, limit risk, and ensure outputs stay aligned with user intent and institutional constraints. This can include blocking off-topic or malicious inputs, enforcing tool usage limits, or escalating actions to a human when thresholds are exceeded.

**Exhibit 7** presents a breakdown of the agent’s key guardrail layers that form its defense system.

##### Exhibit 7. Key Guardrail Layers

| Layer | What It Does | Example |
| --- | --- | --- |
| **System Prompt** | Establishes core behavioral rules (“cite every sourceˮ) | Included at the start of every LLM thread |
| **Input Filters** | Block or sanitize user prompts or LLM outputs that violate scope or policy | A relevance classifier rejects “tell me a joke” |
| **Tool-Call Gatekeepers** | Validate or throttle how the LLM calls external functions | Prevent trades larger than 5% of portfolio |
| **Output Checks** | Scan drafts for restricted terms, entities, or compliance risks | Block responses containing 'CONFIDENTIAL' labels |
| **Human-in-the-Loop** | Requires manual approval for high-impact or sensitive actions | A trader must approve before an order is executed |

Each layer plays a distinct role in securing agentic systems. It starts with a system prompt, which sets nonnegotiables up front—like always citing sources. Input filters screen out irrelevant or harmful queries before they reach the model, while output checks scan responses for red flags like the presence of bias, personally identifiable information, or brand-risk language.

Sitting between input and output, tool-call gatekeepers enforce hard constraints on how tools are used—for example, setting maximum trade sizes or limiting how often the system can call an external API (rate limiting).

And when real money or risk is involved, a human-in-the-loop layer gives the final say before execution.

You might wonder what separates guardrails from constraints defined in the instructions. In practice, the two arenʼt mutually exclusive. Constraints written into the system prompt serve as text-based guardrails:  They define expected behavior and limitations for the LLM. But guardrails also include programmatic controls, such as functions, filters, or workflow logic that actively monitor, validate, or block the modelʼs inputs and outputs.

A well-guarded agentic system puts guardrails into the entire workflow, adding more reliability, control, and compliance at every step.

The components above form the core infrastructure of an agentic system. But building true workflows requires more than assembling parts: It requires decomposing complex tasks into structured, controllable steps. In the next section, we explore common agentic workflow patterns and how they apply to investment use cases.

### Workflows in Finance

Since the key distinction between LLM calls, workflows, and agents is the level of autonomy involved, itʼs useful to break them down along that spectrum. Weʼll start with the most structured, least autonomous systems and work our way up to more complex, agentic architectures. Keep in mind that the following workflows fall under the umbrella of agentic AI. In the next section, “ [Agents in Finance](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#Agents-in-Finance),” we explore the more autonomous end of this spectrum—AI agents and how those systems apply to finance.

#### Prompt Chaining

Prompt chaining breaks a complex task into a **linear sequence** of LLM calls, where each step builds on the output of the previous one. The task is divided into sub-tasks, with each sub-task output feeding into the next. You can also insert validation steps to decide whether the chain should continue, branch, or stop. At any point in the chain, tools can be invoked to enhance accuracy or retrieve external data. **Exhibit 8** depicts the prompt-chaining process.

##### **Exhibit 8. Prompt Chaining**

![simple_prompt_chain.drawio.svg](https://cfainstitute.widen.net/content/ucb6ogc7cg/svg/simple_prompt_chain.drawio.svg?u=bkkbnx)

**When to use:**

- When you have a particular process flow for performing a task.
- When you want to boost the accuracy, clarity, and reliability of a task and are comfortable with longer process times.
- When you need heavy control over the process flow with regular checks and validations at each step

**Exhibit 9** shows examples of prompt chaining in action.

##### Exhibit 9. Financial Use Cases for Prompt Chaining

| Example Task | Chain Outline | Why a Chain Helps |
| --- | --- | --- |
| **Draft a credit memo** for a corporate bond | 1\. Retrieve latest financial statements → 2. Summarize key financials → 3. Generate risk table<br>→ 4\. Assemble memo template | Keeps narrative, tables, and risk flags in a fixed order while letting the model fill content |
| **Quarterly commentary** for a mutual fund | 1\. Pull holdings & returns → 2. Run attribution calc tool → 3. Write manager commentary → 4. Export to PDF | Each step calls a specialized tool (performance database, attribution engine, PDF renderer) |
| **K-factor screen** for small caps | 1\. Query fundamental API → 2. Calculate ratios → 3. Filter by thresholds → 4. Return ranked list → 5. Summarize results | Linear flow mirrors a traditional Excel workflow but is fully automated |

#### Routing

Whereas prompt chaining follows a fixed, linear flow, routing introduces a decision point after inspecting the user query, where it then chooses which sub-workflow or tool to run next. The routing process is depicted in **Exhibit 10**.

##### **Exhibit 10. Routing**

![router.drawio.svg](https://cfainstitute.widen.net/content/q3esy2sohx/svg/router.drawio.svg?u=bkkbnx)

**When to use:**

- When you have separate process flows that depend on the input
- When you want a single entry point to dynamically handle inputs
- If the model can accurately classify the right path

**Exhibit 11** shows example use cases for routing.

##### Exhibit 11. Financial Use Cases for Routing

| Example Task | Routing Decision | Downstream Paths | Benefit |
| --- | --- | --- | --- |
| **Multi-Asset Research Assistant** | Classify query as Equity, Credit, or Macro | - **Equity** → Financial statement parser → DCF tool<br>- **Credit** → spread monitor → covenant checker<br>- **Macro** → economic data fetch → scenario writer | Delivers asset-specific insight through one unified interface |
| **Environmental, Social, and Governance (ESG)  Disclosure Checker** | Detect issuer region from input ticker | - **U.S.** → SEC filings path<br>- **EU** → CSRD database path<br>- **APAC** → local exchange filings path | Applies the correct regulatory framework automatically |
| **Data-Provider Fallback** | Check API response quality / confidence level | - **Primary** → Bloomberg BQL<br>- **Secondary** → LSEG Eikon API<br>- **Tertiary** → in-house database | Maintains uptime and data integrity when a vendor feed is down |

#### Parallelization

The parallelization workflow runs independent subtasks at the same time, and then it joins their results. Highly repetitive subtasks are essentially fanned out, running in parallel to cut execution time. **Exhibit 12** depicts the parallelization process.

##### Exhibit 12. Parallel Execution

![parallelization_workflow.drawio.svg](https://cfainstitute.widen.net/content/5xrdqoy3vu/svg/parallelization_workflow.drawio.svg?u=bkkbnx)

**When to use:**

- Many homogeneous units (e.g., 50 search queries, 100 tickers, 50 filings)
- Latency-sensitive tasks
- Need more robust answers or focused attention on individual subtasks

**Exhibit 13** offers examples of parallelization in action.

##### Exhibit 13. Financial Use Cases for Parallel Execution

| Example Task | Parallel Plan | Parallel Units | Benefit |
| --- | --- | --- | --- |
| **Majority-Vote Classifier/Scorer** | Run multiple models/prompts in parallel; **join**: majority vote classification or average score | Models/prompts | More robust labels or scores (e.g., news sentiment, ESG scoring) vs. single-model variance |
| **Peer Benchmarking** | Compute ratios (EBITDA Margin, ND/EBITDA) in parallel; **join**: compare deltas | Portfolio constituent ratios | Fast, reproducible comps with consistent methodology |
| **Regime Stress Tests** | Run scenarios (expansion/inflation/stagflation/recession) simultaneously; **join**: compare deltas | Scenarios × models | Broader coverage without serial delay; clearer trade- offs |

#### Orchestrator—Workers

In an orchestrator-workers setup, a central orchestrator breaks a task into subtasks, routes each subtask to a specialized worker (such as a workflow or model), and then synthesizes the results into a final output. This process is shown in **Exhibit 14**.

##### Exhibit 14. Orchestrator—Workers

![orchestrator_worker.drawio.svg](https://cfainstitute.widen.net/content/sydazento9/svg/orchestrator_worker.drawio.svg?u=bkkbnx)

**When to use:**

- When the subtasks are dynamic and cannot be predefined up front
- When the same workers (e.g., calculate\_var, fetch\_bloomberg) can be used for several tasks

**Exhibit 15** offers examples of how orchestrator–workers can be used.

##### Exhibit 15. Financial Use Cases for Orchestrator-Workers

| Workflow | Orchestrator Logic | Key Workers | Outcome |
| --- | --- | --- | --- |
| **Automated Portfolio Rebalance** | 1\. Run risk check → 2. In parallel optimize equity & fixed-income sleeves → 3. Merge trades → 4. Execute via broker | RiskCheck, EquityOptimizer, BondLadderBuilder, TradeExecutor | Parallel processing shortens overnight batch; orchestrator keeps full audit log |
| **ESG Research Pipeline** | Branch by region → fetch disclosures → summarize via RAG → compliance check | RegionRouter, DisclosureFetcher, RAGSummarizer, ComplianceChecker | Region-specific data sources and policies handled automatically. |
| **Intraday Alert Engine** | Listen to newsfeed → classify headline → if price-moving run VaR impact & alert desk; else store for newsletter | NewsListener, LLMClassifier, VaRWorker, TraderNotifier, NewsletterAppender | Real-time escalation of material events with quantitative impact attached. |

_Note:_ The key workers listed in Exhibit 15 are for illustrative purposes only and represent hypothetical specialized components that might be implemented in such systems.

#### Evaluator-Optimizer

The evaluator-optimizer pattern creates a feedback loop: One model proposes a solution; another critiques it and prompts revisions. As shown in **Exhibit 16**, the loop continues until the output meets a quality bar, hits a confidence threshold, or reaches a set iteration limit.

##### Exhibit 16. Evaluator-Optimizer

![generator_evaluator.drawio.svg](https://cfainstitute.widen.net/content/lzhgawfuo7/svg/generator_evaluator.drawio.svg?u=bkkbnx)

**When to use:**

- When the information needed to complete the task is vast and cannot be wholistically created in a single go.
- When the output could be significantly improved from human feedback and an LLM demonstrates the ability to provide similar feedback.
- When trying to optimize a result or utility function through iteration.

**Exhibit 17** shows the process, including when the loop stops.

##### Exhibit 17. Financial Use Cases for Evaluator-Optimizer

| Use Ccase | Optimizer’s First Pass | Evaluator Check | Loop Stops When… |
| --- | --- | --- | --- |
| **Iterative Research Gatherer** | Generates search queries, retrieves docs, drafts research summary | Reviews whether information suffices to answer the question | Confidence that coverage is sufficient reaches 90% or 3 passes reached |
| **Factor-Tilt Optimizer** | Proposes new value/quality/momentum/size weights and rebalances portfolio | Verifies tracking-error ≤<br>1.5 %, turnover < 20 bps | All risk checks pass or 5 optimization rounds completed |

The workflow patterns above offer a foundation for building agentic systems, but theyʼre just the starting point. The flexibility of agentic AI allows for endless combinations, and real-world tasks often require custom stacking and adaptation of these patterns. Next, we explore more autonomous agent systems that push beyond predefined flows and begin to reason, plan, and act with greater independence.

### Agents in Finance

As reasoning models have advanced, LLMs have improved their ability to generate well-structured plans. Through techniques like chain-of-thought prompting, models are now better at thinking step by step—mapping out their reasoning before taking action. When this capability is paired with the agentic building blocks outlined earlier—tools, memory, retrieval, and guardrails—LLMs can move beyond passive responses and begin to actively execute on tasks.

This shift lays the foundation for one of the most popular agentic frameworks to date: the **ReAct** framework.

#### ReAct

The ReAct paradigm (short for **Reason + Act**) prompts a model to emit two outputs: a chain-of-thought explaining why it is doing something and a concrete action describing what tool to call next. This cyclical process is depicted in Exhibit 18.  In their [original 2022 paper](https://arxiv.org/abs/2210.03629), Yao, Zhao, Yu, Du, Shafran, Narasimhan, and Cao  demonstrated that this approach reduces hallucinations and enables a single LLM to plan, fetch data, validate results, and iterate until the task is complete.

##### **Exhibit 18. ReAct Cycle**

![react_pattern.drawio.svg](https://cfainstitute.widen.net/content/rdvwvbjft7/svg/react_pattern.drawio.svg?u=bkkbnx)

Here is an example of what the ReAct reasoning trace might look like:

> \> Question: Is AT&T’s current dividend payout sustainable over the next two years?
>
> Thought: I need free cash flow (FCF) and total dividends paid to assess coverage. First I'll get the Trailing 12-month(TTM) Free Cashflow for AT&T with ticker T
>
> Action: FinDataAPI.get\_metric("T", "FCF", "TTM")
>
> Observation: 27.4 B
>
> Thought: Now I’ll fetch dividends paid over the same period.
>
> Action: FinDataAPI.get\_metric("T", "DividendsPaid", "TTM")
>
> Observation: 14.8 B
>
> Thought: I’ll calculate the coverage ratio (FCF / DividendsPaid).
>
> Action: Calculator.calc("27.4 / 14.8")
>
> Observation: ≈ 1.85
>
> Thought: A ratio above 1.0 suggests the payout is currently covered. Let's project FCF forward  2 years under modest growth assumptions.
>
> Action: Calculator.calc("27.4 \* (1 + 0.02)^2")
>
> Observation: ≈ 28.5 B
>
> Thought: If dividends remain flat at ~14.8 B, coverage stays strong.
>
> Action: Calculator.calc("28.5 / 14.8")
>
> Observation: ≈ 1.93
>
> Thought: Coverage remains well above 1.5x, even under flat-to-modest growth.
>
> Final Answer: Based on trailing FCF of $27.4B and dividends of $14.8B (coverage ≈ 1.85×), plus forward estimates, AT&T’s dividend appears sustainable for at least the next two years. Key risks include rising capex or interest expenses.

This pattern is the foundation of many modern autonomous agents: a single LLM reasoning step by step, calling tools, and adjusting as it goes. Often though, the system can benefit from more than one specialized agent working in tandem—sharing information and coordinating toward a shared goal. That’s where multi-agent frameworks come in.

#### Multi-Agent Orchestration

A multi-agent system swaps a single, all-purpose model for a coordinated team of specialists—each with its own role, tools, and objectives. At the center sits an orchestrator responsible for routing tasks, managing handoffs, and consolidating results.

As **Exhibit 19** shows, the orchestrator assigns subtasks to the appropriate agents, passes data and outputs between them, and oversees tool usage across the system. Like a project lead, it runs its own ReAct-style reasoning loop—evaluating observations, determining next steps, and deciding when the overall task is complete.

##### **Exhibit 19. Multi-Agent Orchestration**

![multi-agent_orchestration.drawio.svg](https://cfainstitute.widen.net/content/qeqdse8nto/svg/multi-agent_orchestration.drawio.svg?u=bkkbnx)

**When multi-agent beats single-agent**

- Tasks break naturally into domain roles (data pull, modeling, narrative, compliance)
- Sub-tasks can run in parallel
- Different agents need different LLMs or tool sets (e.g., a cheap classifier vs. a reasoning-heavy model)
- You want clear ownership for audit—each agentʼs log shows who did what

We now transition to the Model Context Protocol (MCP), a widely used approach for enabling autonomous agents to coordinate, share context, and interact with tools or each other in a structured way.

#### Model Context Protocol

Model Context Protocol is emerging as the de facto plumbing layer for autonomous-agent systems. An MCP server sits in front of an existing API (market-data feed, CRM, SQL backend) and presents a uniform, LLM-friendly interface—so an agent can discover the endpoint, see the input schema, and call it safely without hard-coded function or custom prompts.

Originally proposed by Anthropic8 as a way to standardize “how LLMs talk to external toolsˮ, MCP is now supported—at least experimentally—by OpenAI, Google, and several open-source stacks. The attraction is speed: Once an API is wrapped in MCP, any compliant agent framework can consume it with near-zero code. No-code orchestration platforms, such as n8n and Zapier, have already added MCP connectors, instantly exposing their large catalogs of SaaS integrations to tool-using LLMs.

### When to Use Agents versus Workflows?

Before choosing a build pattern, ask a single question: “Can I write the exact sequence of steps up front?” If the answer is _yes_, a traditional workflow—prompt chaining, routing, or an orchestrator–worker pattern—delivers speed, predictability, and a simple audit trail.

If the answer is _no_, if the task can deal with variability in outputs, and if it lies in a lower risk camp, consider using an agent. But do not underestimate the higher costs of LLM inference, as the looping can become quite costly, especially with higher cost reasoning models.

**Exhibit 20** shows examples of how to choose between build patterns.

##### Exhibit 20. When to Use Agents vs. Workflows

| Pattern to Pick | Typical Task Profile | What You Optimize for | Task Riskiness |
| --- | --- | --- | --- |
| **Workflow** | Known steps, repeatable logic, low ambiguity | Speed, determinism, auditability | High |
| **Agent** | Steps are unclear or evolve | Flexibility, exploration | Low |

Todayʼs agents are the least capable weʼll ever use. They will undoubtedly evolve to handle higher-risk tasks with greater reliability. Still, the need for transparent, deterministic systems means predefined workflows will likely remain essential for production use cases for the foreseeable future in finance.

## Conclusion

Agentic AI is on track to influence every part of modern organizations. Given the high-risk nature of many investment workflows, we have a responsibility to build systems that are transparent, controllable, and aligned with institutional standards. Now is the time to experiment, test boundaries, and learn—while the stakes are still low enough to do so safely.

This article outlined the foundational building blocks of agentic AI systems, explored practical workflow patterns, and offered a path toward turning early prototypes into more reliable, auditable tools. In the case studies that follow, we offer more in-depth examples of how you can build valuable agentic AI systems even in their imperfect state.

We also encourage those interested in building these systems to follow along with the code on the [RPC Labs GitHub page](https://github.com/CFA-Institute-RPC/The-Automation-Ahead) and we welcome contributions for implementations of finance-specific agentic AI implementation.

### Case Study 1: Fundamental Assessment Agentic Workflow

This agentic workflow is designed to assess whether current economic conditions are favorable for a specific company based on fundamental financial criteria. It introduces an economic regime-aware screening process with human-in-the-loop inputs, as shown in **Exhibit A1**. This use case demonstrates how reasoning-capable LLM agents can blend macroeconomic awareness, financial data retrieval, and conditional logic to generate informed assessments.

##### **Exhibit A1. Economic Regime-Based Fundamental Assessment Router Workflow**

![economic_regime_fundamentals.drawio.svg](https://cfainstitute.widen.net/content/cdlnqfkvnt/svg/economic_regime_fundamentals.drawio.svg?u=bkkbnx)

### Workflow Summary

This use case follows a combined **Promt-Chaining** and **Router workflow pattern** with regime-specific branching. It can be broken down into the following steps:

1. **Get Ticker**
2. **Pull Financial Data**
3. **Route to Economic Regime**
4. **Calculate Regime-Specific Metrics**
5. **Provide Data Validation Commentary**
6. **Generate Scored Assessment**

Each step can be more or less autonomous, depending on user preference. **Click the steps to dive in.**

- [Step 1: Get Ticker](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-2349443464)
- [Step 2: Pull Financial Data](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-1010350550)
- [Step 3: Route to Economic Regime](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-1977282117)
- [Step 4: Calculate Regime-Specific Metrics](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-1736708336)
- [Step 5: Provide Data Validation Commentary](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-446932838)
- [Step 6: Generate Scored Assessment](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-1376932270)

[Step 1: Get Ticker](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-2349443464)

The workflow begins by the user inputting the ticker for analysis.

[Step 2: Pull Financial Data](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-1010350550)

The system fetches comprehensive financial data (annual/quarterly income statements and balance sheets) for the target company.

[Step 3: Route to Economic Regime](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-1977282117)

The user selects from four predefined economic regimes:

- **0 = Expansionary:** Focus on growth and leverage metrics
- **1 = Inflationary:** Focus on pricing power and inventory efficiency
- **2 = Stagflationary:** Focus on margin stability and low leverage
- **3 = Recession:** Focus on liquidity and defensive positioning

**Example:** The user selects **2 = Stagflationary** environment.

[Step 4: Calculate Regime-Specific Metrics](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-1736708336)

Based on the selected regime, the workflow branches to calculate specific financial metrics. For stagflation, the system calculates the metrics shown in **Exhibit A2**.

**Exhibit A2. Stagflationary Environment Metrics**

| Metric | Favorable | Neutral | Unfavorable | Rationale |
| --- | --- | --- | --- | --- |
| **EBITDA Margin Volatility (stdev)** | < 4% | 4% – 6% | \> 6% | Operational stability matters when growth slows and costs rise |
| **Gross Margin Trend (YoY, bps)** | \> +50 | 0 to +50 | < 0 | Ability to hold or expand gross margin indicates pricing power |
| **Net Debt / EBITDA** | < 1.0x | 1.0x–2.0x | \> 2.0x | Low leverage mitigates refinancing and spread-widening risk |

These thresholds can be stored for different economic regimes for the agent to retrieve during evaluation and based on the confirmed regime.

[Step 5: Provide Data Validation Commentary](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-446932838)

The system validates calculated metrics against predefined outlier bounds for the given metrics to identify potential data anomalies.

**Example Prompt:**

```python
Copy

prompt = f"""Evaluate the following financial metrics for accuracy, completeness, and nuance.
{metrics)}
Look at the outlier table:
{OUTLIER_TABLE}
-Comment on any metrics that fall in the extreme upper and lower bounds.

-Determine if the metrics are reasonable based on the input data or if there are odd/missing items that are affecting their calculations.
Input data:
Annual Income Statement:{ann_inc} Quarterly Income Statement:{qtr_inc} Annual Balance Sheet:{ann_bs} Quarterly Balance Sheet:{qtr_bs}

-Lastly, comment on the nuance that may be affecting these metrics that would be important to determine their value.
For example, if gross margin volatility is in the extreme upper bound but the volatility is mostly coming from the upside
or extremely high values of gross margin then provide this nuance in the commentary."""
```

The resulting metric outlier bounds are then shown in **Exhibit A3**.

##### Exhibit A3. Metric Outlier Bounds

| Metric | Q1 | Q3 | IQR | Extreme Lower Bound (Q1 −<br>1.5×IQR) | Extreme Upper Bound (Q3 +<br>1.5×IQR) | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| **EBITDA Margin  Volatility - Annual (std)** | 1.0% | 5.0% | 4.0% | −5.0% (floor 0) | 11.0% | Excessive volatility suggests instability; near-zero (if seen) might prompt sanity check |
| **Gross Margin Trend (YoY, bps)** | -100 | +100 | 200 bps | −400 bps | +400 bps | Large swings could reflect mix, pricing, or recognition timing anomalies |
| **Net Debt / EBITDA** | 1.0x | 4.0x | 3.0x | −3.5x | 8.5x | High leverage or large net cash (negative) should be validated for definitions and normalization |

**Sample Output (condensed for readability)**

```plaintext
Copy

Let's evaluate each of the three key metrics provided — **EBITDA Margin Volatility**, **Gross Margin Trend (YoY, bps)**, and **Net Debt / EBITDA** — for accuracy, completeness, and nuance, referencing the input data and the outlier table bounds.

## 1. EBITDA Margin Volatility

- **Reported Metric:** 0.0085 (or 0.85%)
- **Outlier Table Bounds:**

- Extreme Lower Bound: 0% (floor)
- Extreme Upper Bound: 11.0%

### Data Review:
- Annual EBITDA Margins:
- 2021: 14.93%
- 2022: 13.06%
- 2023: 14.48%
- 2024: 13.56%

### Assessment:
- Volatility is **below Q1**, reflecting **very stable margins**.
- Fluctuations are modest (~13–15%) and consistent.
- No data issues observed.
- **Nuance:**
- Low volatility indicates operational consistency.

## 2. Gross Margin Trend (YoY, bps)

- **Reported Metric:** –45.33 bps
- **Outlier Table Bounds:**
- Extreme Lower Bound: -400 bps
- Extreme Upper Bound: +400 bps

### Data Review:
- Gross Margins:
- 2021: 21.24%
- 2022: 19.39%
- 2023: 18.55%
- 2024: 19.88%
- YoY Changes:
- 2022: –1.85%
- 2023: –0.84%
- 2024: +1.34%
- Quarterly trend avg: –16.63 bps

### Assessment:
- The trend is **within bounds**, showing a moderate overall decline.
- Quarterly margins range from 21.33% (Q2) to 19.31% (Q4) in 2024.
- **Nuance:**
- Trend reflects earlier declines partially offset in 2024.
- Cost pressures and revenue mix shifts likely drivers.

## 3. Net Debt / EBITDA

- **Reported Metric (2024):** 3.30x
- **Outlier Table Bounds:**
- Extreme Lower Bound: 0
- Extreme Upper Bound: 8.5x ###

Data Review:
- Net Debt:
- 2021: 5.44B → 2024: 6.11B
- EBITDA:
- 2021: 1.92B → 2024: 1.85B
- Ratio:
- 2021: 2.83x → 2024: 3.30x

### Assessment:
- **Within bounds**, but trending upward.
- Increase driven by declining EBITDA, not rising debt.
- **Nuance:**
- EBITDA decline tied to unusual items and lower ops income.
- Debt stable, but total debt grew slightly.
- Cash is low (~588M) relative to 7.19B total debt.
- Interest expense (~348M) is high; coverage should be monitored. ##

Additional Observations and Nuances

### Unusual Items and Their Impact
- Recurring restructuring and M&A charges (e.g., –97M in 2024) affect EBITDA and margins.
- Normalized EBITDA is slightly higher; more reflective of core performance.

### Seasonality and Quarterly Fluctuations
- Quarterly EBITDA and gross margins show variation.
- Q2 2024 margins notably stronger.
- Indicates some seasonality or timing effects.

# **Final Conclusion**

- **All three metrics are reasonable and aligned with the data.**
- **No outliers or red flags.**
- Unusual items introduce some noise, so **normalized metrics are more informative**.
- **EBITDA margin volatility is low**, but plausible.
- **Gross margin trend shows moderate decline**, with some recent improvement.
- **Net debt / EBITDA is moderate**, but upward trend and liquidity limits warrant monitoring.
```

[Step 6: Generate Scored Assessment](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-1376932270)

The system uses the calculated metrics and then cross-references this against the screening criteria for the regime. Based on the outcome of the metric assessment and the data validation commentary, the LLM generates a score and an explanation for the score.

**Sample Output**

```plaintext
Copy

Explanation:
The evaluation of the three key financial metrics against the criteria for a Stagflationary regime shows the following:

1. EBITDA Margin Volatility is very low at 0.85%, below the first quartile bound of 1%, indicating stable operational profitability with minimal variability. This is consistent with the relatively stable EBITDA margins over the past four years. While low volatility is plausible, it should be sanity checked to ensure no smoothing accounting adjustments mask true variability.

2. Gross Margin Trend (YoY) is negative at −45 bps, within the acceptable bounds (−100 to +100 bps). The trend reflects a moderate decline in gross margin from 2021 to 2023, with some recovery in 2024. This suggests operational challenges and cost pressures typical in a stagflationary environment, but the decline is not extreme.

3. Net Debt / EBITDA ratio is 3.30x, within the interquartile range but approaching the upper quartile bound of 4.0x. The increase from 2.83x in 2021 to 3.30x in 2024 is driven by stable net debt and declining EBITDA in 2024. This moderate leverage increase warrants monitoring, especially given limited liquidity buffers and significant interest expenses.

Additional nuances include the impact of unusual items and restructuring charges that affect EBITDA and margins, suggesting normalized metrics should be prioritized. Quarterly fluctuations and seasonality also influence margin trends.

Overall, the metrics are reasonable, consistent with the data, and reflect characteristics expected in a stagflationary regime: stable but low margin volatility, moderate margin pressure, and increasing leverage. No extreme outliers or data errors are evident.
Score: 85
```

For a code implementation of this example, follow the tutorial available [here](https://github.com/CFA-Institute-RPC/The-Automation-Ahead/tree/main/Agents/regime_screening_workflow).

### Case Study 2: Deep Research Sustainability Screening Workflow

This agent uses a hybrid **Parallelization** and **Evaluator–Optimizer** workflow pattern to perform deep research on a company’s adoption of innovative sustainable technologies. It enables investment teams to assess sustainability leadership using dynamic, internet-scale data.

#### Objective

The user inputs a prompt like the following:

> Assess and score Company X’s adoption of innovative sustainable technology. Provide a score from 0–100 based on the following five dimensions. The five evaluation criteria include:
>
> 1. **Key Technologies Adopted** – What sustainable technologies has the company implemented?
> 2. **Innovation Leadership** – Are there any cutting-edge initiatives, R&D efforts, or partnerships?
> 3. **Measurable Impact** – Are there quantifiable environmental or business outcomes?
> 4. **Future Strategy** – What are the documented future commitments or roadmaps?
> 5. **Industry Position** – How does the company compare to its competitors?

#### Workflow Structure

The workflow structure for this case is shown in **Exhibit A4**.

##### **Exhibit A4. Deep Research Sustainability Screening Agentic Workflow**

![sustainability_screening.drawio.svg](https://cfainstitute.widen.net/content/qhsmrq1bsa/svg/sustainability_screening.drawio.svg?u=bkkbnx)

- [1\. Query Planning](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-734931860)
- [2\. Worker Execution (Parallel Search and Extraction)](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-1090430047)
- [3\. Evaluation Loop (Evaluator–Optimizer)](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-3530175621)
- [4\. Synthesis and Scoring](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-4282741810)
- [Example Output (Excerpt)](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-2119482239)

[1\. Query Planning](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-734931860)

The first LLM receives the user’s prompt and generates targeted search queries for each of the five evaluation criteria—for example:

- "Company X sustainable technology adoption 2025"
- "Company X sustainability R&D initiatives"
- "Company X environmental impact from sustainability programs"
- "Company X climate strategy roadmap"
- "Company X sustainability leadership vs. peers 2025"

[2\. Worker Execution (Parallel Search and Extraction)](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-1090430047)

Each query is sent to a search agent that:

- Conducts an internet search and
- Extracts and summarizes relevant content

This is done in parallel to reduce latency.

[3\. Evaluation Loop (Evaluator–Optimizer)](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-3530175621)

The results are sent to an evaluator which evaluates the quality and completeness of the results:

- If coverage is incomplete, it generates follow-up queries.
- This loop repeats until the system reaches a predefined confidence threshold that enough information has been gathered to score the company meaningfully.

[4\. Synthesis and Scoring](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-4282741810)

Once sufficient information is gathered, the results are compiled into a report:

- Assigns sub-scores for each evaluation criterion
- Justifies each score with extracted evidence
- Aggregates to a final score between 0–100
- Outputs a final narrative explaining the rating

[Example Output (Excerpt)](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-2119482239)

> Final Score: 82/100
>
> Company X has demonstrated strong adoption of innovative sustainable technologies. Notable highlights include:
>
> - Adoption of carbon capture solutions in two facilities (Key Tech Score: 16/20)
> - Participation in a joint R&D lab with a major university on green hydrogen (Innovation Leadership: 18/20)
> - A reported 12% reduction in operational emissions since 2022 (Impact: 15/20)
> - A public commitment to net-zero Scope 1 and 2 emissions by 2035 (Strategy: 17/20)
> - Sustainability rankings place Company X in the top quartile of its sector (Position: 16/20)

[Notes](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#notes)

This kind of workflow is highly extensible. Users could add custom weights to the scoring dimensions or adjust scoring rubrics depending on strategy mandates (e.g., thematic ESG funds, transition-focused portfolios, etc.). For a code implementation of this example, follow the tutorial [here.](https://github.com/CFA-Institute-RPC/The-Automation-Ahead/tree/main/Agents/sustainability_research_workflow)

In the next case, we connect this workflow into a broader portfolio construction use case that builds and tailors custom indexes using the workflows we introduced above.

### Case Study 3: Agentic AI-Augmented Portfolio Construction for Custom Dividend Strategy

This case study shows how traditional portfolio construction techniques can be augmented with agentic systems to produce highly customized, client-specific portfolios at scale. The agent-enhanced process combines traditional fundamental factor models, agentic sustainability scoring, and agentic fundamental screening to create a bespoke equity portfolio.

#### Task Scope

A wealth manager is asked to build a custom, separately managed account (SMA) for a high-net-worth client. The client requests the following:

- A portfolio of US companies drawn from the S&P 500
- A dividend yield 20% higher than the S&P 500
- A beta between 0.8 and 1.2
- Sustainability considerations—adoption of innovative sustainable technology
- Benchmark: S&P Global Dividend Aristocrats Index

- [Step 1: Define the Universe and Extract Data](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-3042780025)
- [Step 2: Construct Dividend Factors](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-2469224591)
- [Step 3: Run Agentic AI-Based Sustainability Screen](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-2577250370)
- [Step 4: Run Regime-Based Fundamental Assessment](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-2875933584)
- [Step 5: Final Composite Ranking](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-129070516)
- [Step 6: Portfolio Optimization](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-2976689673)
- [Outcome](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-3138560699)

[Step 1: Define the Universe and Extract Data](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-3042780025)

The base universe is the **S&P 500**. Data are extracted for each company:

- **Dividend Yield**
- **Payout Ratio** (as a proxy for dividend sustainability)
- **3-Year Dividend Growth Compound Annual Growth Rate (CAGR)**
- **Price History** (for return calculation)
- **Sector Classification**

[Step 2: Construct Dividend Factors](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-2469224591)

We use a decile spread methodology to construct three dividend factors, comprising dividend yield, payout ratio, and dividend growth rate (3-year CAGR). The methodology includes taking the returns of companies in the top decile of the given factor and subtracting the returns of companies in the bottom decile of the factor.

Each factor is regressed on the returns of each company, creating beta values representing the exposure to each factor.

Given we want to rank high for each factor, we use a rank methodology to score each company for a given factor (i.e., the highest factor is ranked 1, and the lowest factor is ranked 100).

These scores are summed to create a **composite dividend factor score** (see **Exhibit A5**), and the top 100 companies are selected (see **Exhibit A6**).

##### Exhibit A5. Example Factor Table

| Company | Dividend Yield Beta | Payout Ratio Beta | Dividend Growth Beta |
| --- | --- | --- | --- |
| Company C | 0.91 | 0.79 | 0.88 |
| Company D | 0.76 | 0.91 | 0.92 |
| Company A | 0.82 | 0.63 | 0.77 |
| Company B | 0.67 | 0.85 | 0.69 |
| Company E | 0.89 | 0.73 | 0.65 |

##### Exhibit A6. Example Composite Rank

| Company | Dividend Yield Rank | Payout Ratio Rank | Dividend Growth Rank | Composite Score |
| --- | --- | --- | --- | --- |
| Company C | 1 | 3 | 2 | 6 |
| Company D | 4 | 1 | 1 | 6 |
| Company A | 3 | 5 | 3 | 11 |
| Company B | 5 | 2 | 4 | 11 |
| Company E | 2 | 4 | 5 | 11 |

[Step 3: Run Agentic AI-Based Sustainability Screen](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-2577250370)

Each of the top 100 companies is evaluated by the **Deep Research Sustainability Workflow**, which scores them on a 0–100 scale based on the following factors:

- Adoption of sustainable technology
- Innovation leadership
- Measurable impact
- Future strategy
- Industry position

The scores are normalized and added as a fourth factor in the composite rank.

[Step 4: Run Regime-Based Fundamental Assessment](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-2875933584)

Next, the **Economic Regime Workflow** is activated to evaluate how well each company is positioned for the current macroeconomic environment (e.g., stagflation, inflation, recession, expansion).

These scores are added as a fifth factor.

[Step 5: Final Composite Ranking](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-129070516)

All five signals are combined into a **final composite rank:**

- Dividend factor scores
- Sustainability screen score
- Economic regime alignment score

The top 50 companies by composite rank are selected for portfolio construction. **Exhibit A7** shows an example of a final composite rank matrix.

##### Exhibit A7. Example Final Composite Rank Matrix

| Company | Dividend Yield Rank | Payout Ratio Rank | Dividend Growth Rank | Sustainability Rank | Regime Fit Rank | Composite Score |
| --- | --- | --- | --- | --- | --- | --- |
| Company C | 1 | 3 | 2 | 2 | 1 | 9 |
| Company D | 4 | 1 | 1 | 3 | 2 | 11 |
| Company A | 3 | 5 | 3 | 1 | 3 | 15 |
| Company E | 2 | 4 | 5 | 4 | 5 | 20 |
| Company B | 5 | 2 | 4 | 5 | 4 | 20 |

[Step 6: Portfolio Optimization](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-2976689673)

A traditional optimizer is used to generate the final portfolio. Constraints include the following:

- **Objective:** Maximize dividend yield
- **Beta Target:** Between 0.8 and 1.2
- **Sector Deviation:** ±5% of S&P 500 sector weights
- **Position Limits:** Max 5% per position

[Outcome](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#4257225834-3138560699)

The resulting portfolio is:

- aligned to the client's income objective and constraints and
- tailored using interpretable sustainability, macroeconomic and fundamental filters.

This approach highlights how agentic systems can replace rigid filters with dynamic assessments. Rather than relying on hard cutoffs, agentic AI-generated scores allow for ranking-based filtering, which is far more adaptable to large universes or when clients request customization across multiple dimensions.

For a code implementation of this example, follow the tutorial available [here](https://github.com/CFA-Institute-RPC/The-Automation-Ahead/blob/main/Agents/Agent-Augmented%20Portfolio%20Construction%20-%20Case%20Study.ipynb).

### Challenges and Tips

#### Choosing the Right Models

Choosing the right models starts with aligning their capabilities to the task. High-context reasoning—like building a cash flow model or interpreting legal covenants—warrants a capable reasoning model like OpenAIʼs [o-series](https://openai.com/index/introducing-o3-and-o4-mini/) models. But for routine steps like routing, classification, or simple extraction, non-reasoning models or even fine-tuned small language models (SMLs) can handle the job efficiently, saving on both cost and latency.

In mixed-agent workflows, you can assign models strategically: Use heavyweight models for synthesis and decision making, and delegate repetitive or narrow tasks to lighter models. Context length also plays a role.

Processing a 10-K filing or board presentation may require a 100k-token window or a RAG layer, while a simple routing prompt wonʼt.

#### Output Variability

Output variability is an inherent feature of stochastic text generation. One of the simplest ways to reduce it is by lowering the temperature parameter, which controls the degree of randomness in model outputs. This can improve determinism in sections where consistency is important. However, variability can still appear even at a temperature of zero. In fact, OpenAI has removed the temperature setting entirely for its reasoning models. When the task requires a high degree of consistency between runs—such as with scoring agents— it is best to either use a non-reasoning model with temperature set to zero or apply a majority-vote approach to approximate limiting behavior.

Another source of variability arises when a model incorporates live internet data, since the retrieved context can change between runs. A Retrieval-Augmented Generation (RAG) pipeline offers a more controlled alternative: instead of letting the model fetch dynamic web content, it is anchored to curated documents or structured datasets, which reduces both randomness and hallucinations.

#### Debugging and Evaluation

As you stack tools and agents, complexity can escalate quickly—especially in multi-agent systems. Default to simplicity. Start by testing each component in isolation. Confirm the SQL agent generates safe queries, the risk model returns coherent outputs, and the PDF renderer handles edge cases—all before wiring them together.

Instrument your workflows with logging at every step: agent inputs and outputs, tool calls, API responses, and edge-case branches. Without detailed logs, diagnosing failures becomes guesswork.

Simplicity, paired with robust observability, is your best defense against fragile agentic systems.

Taken together, these practices move a prototype closer to something reliable, auditable, and resilient—the foundations needed to turn promising proof of concepts into real, measurable value.

[Footnotes](https://rpc.cfainstitute.org/research/the-automation-ahead-content-series/agentic-ai-for-finance#footnotes)

\[1\] See the Y Combinator Startup Directory: [https://www.ycombinator.com/companies](https://www.ycombinator.com/companies).

\[2\] Anthropic, “Building Effective Agents” (19 December 2024): [https://www.anthropic.com/engineering/building-effective-agents](https://www.anthropic.com/engineering/building-effective-agents).

\[3\] Microsoft Copilot Studio, “Instructive Guidance for Generative Orchestration” (8 July 2025): [https://learn.microsoft.com/en-us/microsoft-copilot-studio/guidance/generative-mode-guidance](https://learn.microsoft.com/en-us/microsoft-copilot-studio/guidance/generative-mode-guidance).

\[4\] Hugging Face: [https://huggingface.co/docs/hugs/en/guides/function-calling](https://huggingface.co/docs/hugs/en/guides/function-calling).

\[5\] A. Patel, S. Bhattamishra, and N. Goyal, “Are NLP Models Really Able to Solve Simple Math Word Problems?” ACL Anthology (June 2021): [https://aclanthology.org/2021.naacl-main.168/](https://aclanthology.org/2021.naacl-main.168/).

\[6\] M. Komeili, K. Shuster, and J. Weston, “Internet-Augmented Dialogue Generation,” ACL Anthology (May 2022): [https://aclanthology.org/2022.acl-long.579/](https://aclanthology.org/2022.acl-long.579/).

\[7\] J. Maynez, S. Narayan, B. Bohnet, and R. McDonald, “On Faithfulness and Factuality in Abstractive Summarization,” arXiv (2 May 2020): [https://arxiv.org/abs/2005.00661](https://arxiv.org/abs/2005.00661).

\[8\] Anthropic, “Introducing the Model Context Protocol” (25 November 2024): [https://www.anthropic.com/news/model-context-protocol](https://www.anthropic.com/news/model-context-protocol).