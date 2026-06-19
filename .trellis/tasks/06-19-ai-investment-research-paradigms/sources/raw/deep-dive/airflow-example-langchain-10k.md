[Skip to content](https://github.com/apache/airflow/blob/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_langchain_10k.py#start-of-content)

You signed in with another tab or window. [Reload](https://github.com/apache/airflow/blob/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_langchain_10k.py) to refresh your session.You signed out in another tab or window. [Reload](https://github.com/apache/airflow/blob/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_langchain_10k.py) to refresh your session.You switched accounts on another tab or window. [Reload](https://github.com/apache/airflow/blob/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_langchain_10k.py) to refresh your session.Dismiss alert

{{ message }}

[apache](https://github.com/apache)/ **[airflow](https://github.com/apache/airflow)** Public

- [Notifications](https://github.com/login?return_to=%2Fapache%2Fairflow) You must be signed in to change notification settings
- [Fork\\
17.3k](https://github.com/login?return_to=%2Fapache%2Fairflow)
- [Star\\
45.9k](https://github.com/login?return_to=%2Fapache%2Fairflow)


## Collapse file tree

## Files

main

Search this repository(forward slash)` forward slash/`

/

# example\_langchain\_10k.py

Copy path

Blame

More file actions

Blame

More file actions

## Latest commit

[![vikramkoka](https://avatars.githubusercontent.com/u/25178089?v=4&size=40)](https://github.com/vikramkoka)[vikramkoka](https://github.com/apache/airflow/commits?author=vikramkoka)

[Fix XCom deserialization of Pydantic models in LangChain 10-K example (](https://github.com/apache/airflow/commit/1a1b131fd587c3182c8bda84e3470dcb8169066e) […](https://github.com/apache/airflow/pull/67930)

Open commit detailsfailure

2 weeks agoJun 2, 2026

[1a1b131](https://github.com/apache/airflow/commit/1a1b131fd587c3182c8bda84e3470dcb8169066e) · 2 weeks agoJun 2, 2026

## History

[History](https://github.com/apache/airflow/commits/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_langchain_10k.py)

Open commit details

[View commit history for this file.](https://github.com/apache/airflow/commits/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_langchain_10k.py) History

614 lines (495 loc) · 23.2 KB

·

/

# example\_langchain\_10k.py

Copy path

Top

## File metadata and controls

- Code

- Blame


614 lines (495 loc) · 23.2 KB

·

[Raw](https://github.com/apache/airflow/raw/refs/heads/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_langchain_10k.py)

Copy raw file

Download raw file

Open symbols panel

Edit and raw actions

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

36

37

38

39

40

41

42

43

44

45

46

47

48

49

50

51

52

53

54

55

56

57

58

59

60

61

62

63

64

65

66

67

68

69

70

71

72

73

74

75

76

77

78

79

80

81

82

83

84

85

86

87

88

89

90

91

92

93

94

95

96

97

98

99

100

101

102

103

104

105

106

107

108

109

110

111

112

113

114

115

116

117

118

119

120

121

122

123

124

125

126

127

128

129

130

131

132

133

134

135

136

137

138

139

140

141

142

143

144

145

146

147

148

149

150

541

542

543

544

545

546

547

548

549

550

551

552

553

554

555

556

557

558

559

560

561

562

563

564

565

566

567

568

569

570

571

572

573

574

575

576

577

578

579

580

581

582

583

584

585

586

587

588

589

590

591

592

593

594

595

596

597

598

599

600

601

602

603

604

605

606

607

608

609

610

611

612

613

614

\# Licensed to the Apache Software Foundation (ASF) under one

\# or more contributor license agreements. See the NOTICE file

\# distributed with this work for additional information

\# regarding copyright ownership. The ASF licenses this file

\# to you under the Apache License, Version 2.0 (the

\# "License"); you may not use this file except in compliance

\# with the License. You may obtain a copy of the License at

#

\# http://www.apache.org/licenses/LICENSE-2.0

#

\# Unless required by applicable law or agreed to in writing,

\# software distributed under the License is distributed on an

\# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY

\# KIND, either express or implied. See the License for the

\# specific language governing permissions and limitations

\# under the License.

"""

SEC 10-K financial analysis -- LangChain RAG with live SEC EDGAR data.

Two production-shaped Dags that demonstrate a multi-company financial

research pipeline using real SEC 10-K filings fetched from the EDGAR

public API: one fetches and indexes filings on a schedule, the other

decomposes a comparison question at runtime and fans out retrieval via

Dynamic Task Mapping.

Filings are fetched from the SEC EDGAR public API (free, no auth

required) using stock ticker symbols. Any US publicly-traded company

is supported -- configure via the \`\`tickers\`\` Dag parameter.

This is the LangChain counterpart to \`\`example\_llamaindex\_10k.py\`\`.

Both share the same DAG shape (decompose -> fan-out retrieval -> collect

-\> synthesize -> approve) and the same live SEC EDGAR data source,

demonstrating that the framework choice is a swappable implementation

detail while Airflow provides the orchestration.

\`\`example\_langchain\_10k\_index\`\` (weekly schedule):

.. code-block:: text

fetch\_filings (@task, live from SEC EDGAR)

 -\> build\_index (@task, mapped x N companies)

Uses LangChainHook + RecursiveCharacterTextSplitter + FAISS

\`\`example\_langchain\_10k\_analysis\`\` (manual trigger):

.. code-block:: text

analyst\_question (HITLEntryOperator)

 -\> get\_question (@task)

 -\> get\_tickers (@task)

 -\> decompose\_question (@task.llm, structured output)

 -\> extract\_sub\_questions (@task)

 -\> build\_retrieval\_kwargs (@task)

 -\> retrieve (@task, mapped x N sub-questions)

 -\> collect\_results (@task)

 -\> synthesize\_report (LLMOperator, UsageLimits + structured output)

 -\> format\_report (@task, readable text for reviewer)

 -\> review\_report (ApprovalOperator)

\*\*What this makes visible that a notebook hides:\*\*

\\* The LLM decides how many sub-questions to create -- N is unknown at

parse time, determined at runtime via Dynamic Task Mapping.

\\* Each retrieval is an independent task instance: if one company's index

is unavailable, only that instance retries.

\\* The synthesis step's token budget (\`\`UsageLimits\`\`) and output schema

(\`\`AnalysisReport\`\`) are auditable in XCom.

\\* An analyst reviews the report before it reaches the investment committee.

\*\*LangChain-specific components used:\*\*

\\* \`\`LangChainHook\`\` -- vendor-agnostic model dispatch via \`\`init\_embeddings\`\`

\\* \`\`RecursiveCharacterTextSplitter\`\` -- character-based chunking

\\* \`\`FAISS\`\` -- in-process vector store (no external server)

\*\*Pattern difference from the LlamaIndex counterpart:\*\*

The LlamaIndex example uses \`\`LlamaIndexEmbeddingOperator\`\` and

\`\`LlamaIndexRetrievalOperator\`\` (dedicated operators). Here, indexing

and retrieval are plain \`\`@task\`\` functions that call LangChain's FAISS

and RecursiveCharacterTextSplitter directly via \`\`LangChainHook\`\`,

because LangChain does not yet have dedicated Airflow operators. The

DAG shape is identical; the operator-vs-task distinction is the only

structural difference.

Before running:

1\. Install LangChain packages::

pip install langchain langchain-openai langchain-text-splitters \\\

langchain-community faiss-cpu

2\. Create an LLM connection \`\`pydanticai\_default\`\` for synthesis and

decomposition, and \`\`langchain\_default\`\` for embedding and retrieval.

3\. Update \`\`EDGAR\_USER\_AGENT\`\` with your name and email (SEC requires a

descriptive User-Agent header on all EDGAR API requests).

4\. Run \`\`example\_langchain\_10k\_index\`\` once to fetch filings and build

the sample indexes.

5\. Trigger \`\`example\_langchain\_10k\_analysis\`\` to run the query pipeline.

"""

from \_\_future\_\_ importannotations

importjson

importos

importre

importurllib.request

fromdatetimeimporttimedelta

fromtypingimportAny

frompydanticimportBaseModel

frompydantic\_ai.usageimportUsageLimits

fromairflow.providers.common.ai.operators.llmimportLLMOperator

fromairflow.providers.common.compat.sdkimportdag, task

fromairflow.providers.standard.operators.hitlimportApprovalOperator, HITLEntryOperator

fromairflow.sdkimportParam

\# ---------------------------------------------------------------------------

\# Configuration

\# ---------------------------------------------------------------------------

LLM\_CONN\_ID="pydanticai\_default"

LANGCHAIN\_CONN\_ID="langchain\_default"

EMBEDDING\_MODEL="openai:text-embedding-3-small"

INDEX\_BASE\_DIR="/opt/airflow/data/indexes/10k\_langchain"

DEFAULT\_TICKERS="AAPL,MSFT,UBER,LYFT,AMZN"

\# SEC EDGAR requires a descriptive User-Agent header with a contact email

\# on every request. Update this before running in production.

EDGAR\_USER\_AGENT="Apache Airflow Example dev@airflow.apache.org"

\# ---------------------------------------------------------------------------

\# Structured output models

\# ---------------------------------------------------------------------------

\# \[START 10k\_langchain\_structured\_output\]

classSubQuestion(BaseModel):

"""One sub-question targeting a specific company."""

sub\_question: str

ticker: str

classDecomposedQuestion(BaseModel):

"""LLM-produced decomposition of the analyst's question."""

sub\_questions: list\[SubQuestion\]

\# Step 6: Synthesize a structured comparison report.

\# UsageLimits caps the token spend; output\_type=AnalysisReport

\# enforces the Pydantic schema on the LLM response.

\# ------------------------------------------------------------------

\# \[START 10k\_langchain\_synthesis\]

synthesize=LLMOperator(

task\_id="synthesize\_report",

llm\_conn\_id=LLM\_CONN\_ID,

system\_prompt=SYNTHESIS\_SYSTEM\_PROMPT,

prompt="""\

Synthesize a cross-company financial comparison from these retrieval results.

Cite specific data points and scores.

{{ ti.xcom\_pull(task\_ids='collect\_results') }}""",

output\_type=AnalysisReport,

serialize\_output=True,

usage\_limits=UsageLimits(

request\_limit=10,

input\_tokens\_limit=50\_000,

output\_tokens\_limit=16\_000,

),

)

\# \[END 10k\_langchain\_synthesis\]

collected>>synthesize

\# ------------------------------------------------------------------

\# Step 7: Format the structured report into readable text for the

\# human reviewer. The LLM produced a dict (via output\_type=

\# AnalysisReport with serialize\_output); this task renders it as clean prose.

\# ------------------------------------------------------------------

@task

defformat\_report(report: dict) ->str:

lines= \[f"# Executive Summary\\n\\n{report\['executive\_summary'\]}"\]

ifreport.get("company\_findings"):

lines.append("\\n\# Company Findings")

forfindinginreport\["company\_findings"\]:

company=finding.get("company") orfinding.get("ticker", "Unknown")

lines.append(f"\\n\## {company}")

forkey, valueinfinding.items():

ifkeynotin ("company", "ticker"):

lines.append(f"- \*\*{key}\*\*: {value}")

ifreport.get("key\_risks"):

lines.append("\\n\# Key Risks")

forriskinreport\["key\_risks"\]:

lines.append(f"- {risk}")

ifreport.get("recommendations"):

lines.append("\\n\# Recommendations")

forrecinreport\["recommendations"\]:

lines.append(f"- {rec}")

return"\\n".join(lines)

review\_body=format\_report(synthesize.output)

\# ------------------------------------------------------------------

\# Step 8: Analyst reviews the report before it reaches the

\# investment committee.

\# ------------------------------------------------------------------

\# \[START 10k\_langchain\_hitl\_approval\]

ApprovalOperator(

task\_id="review\_report",

subject="Review 10-K comparison report before sharing",

body=review\_body,

response\_timeout=timedelta(hours=24),

)

\# \[END 10k\_langchain\_hitl\_approval\]

\# \[END example\_langchain\_10k\_analysis\]

example\_langchain\_10k\_analysis()

You can’t perform that action at this time.