[Skip to content](https://github.com/apache/airflow/blob/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_llamaindex_10k.py#start-of-content)

You signed in with another tab or window. [Reload](https://github.com/apache/airflow/blob/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_llamaindex_10k.py) to refresh your session.You signed out in another tab or window. [Reload](https://github.com/apache/airflow/blob/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_llamaindex_10k.py) to refresh your session.You switched accounts on another tab or window. [Reload](https://github.com/apache/airflow/blob/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_llamaindex_10k.py) to refresh your session.Dismiss alert

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

# example\_llamaindex\_10k.py

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

[History](https://github.com/apache/airflow/commits/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_llamaindex_10k.py)

Open commit details

[View commit history for this file.](https://github.com/apache/airflow/commits/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_llamaindex_10k.py) History

551 lines (450 loc) · 20.6 KB

·

/

# example\_llamaindex\_10k.py

Copy path

Top

## File metadata and controls

- Code

- Blame


551 lines (450 loc) · 20.6 KB

·

[Raw](https://github.com/apache/airflow/raw/refs/heads/main/providers/common/ai/src/airflow/providers/common/ai/example_dags/example_llamaindex_10k.py)

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

151

152

153

154

155

156

157

158

159

160

161

162

163

164

165

166

167

168

169

170

171

172

173

174

175

176

177

178

179

180

181

182

183

184

185

186

187

188

189

190

191

192

193

194

195

196

197

198

199

200

201

202

203

204

205

206

207

208

209

210

211

212

213

214

215

216

217

218

219

220

221

222

223

224

225

226

227

228

229

230

231

232

233

234

235

236

237

238

239

240

241

242

243

244

245

246

247

248

249

250

251

252

253

254

255

256

257

258

259

260

261

262

263

264

265

266

267

268

269

270

271

272

273

274

275

276

277

278

279

280

281

282

283

284

285

286

287

288

289

290

291

292

293

294

295

296

297

298

299

300

301

302

303

304

305

306

307

308

309

310

311

312

313

314

315

316

317

318

319

320

321

322

323

324

325

326

327

328

329

330

331

332

333

334

335

336

337

338

339

340

341

342

343

344

345

346

347

348

349

350

351

352

353

354

355

356

357

358

359

360

361

362

363

364

365

366

367

368

369

370

371

372

373

374

375

376

377

378

379

380

381

382

383

384

385

386

387

388

389

390

391

392

393

394

395

396

397

398

399

400

401

402

403

404

405

406

407

408

409

410

411

412

413

414

415

416

417

418

419

420

421

422

423

424

425

426

427

428

429

430

431

432

433

434

435

436

437

438

439

440

441

442

443

444

445

446

447

448

449

450

451

452

453

454

455

456

457

458

459

460

461

462

463

464

465

466

467

468

469

470

471

472

473

474

475

476

477

478

479

480

481

482

483

484

485

486

487

488

489

490

491

492

493

494

495

496

497

498

499

500

501

502

503

504

505

506

507

508

509

510

511

512

513

514

515

516

517

518

519

520

521

522

523

524

525

526

527

528

529

530

531

532

533

534

535

536

537

538

539

540

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

SEC 10-K financial analysis -- LlamaIndex RAG with live SEC EDGAR data.

Two production-shaped Dags that demonstrate a multi-company financial

research pipeline using real SEC 10-K filings fetched from the EDGAR

public API: one fetches and indexes filings on a schedule, the other

decomposes a comparison question at runtime and fans out retrieval via

Dynamic Task Mapping.

Filings are fetched from the SEC EDGAR public API (free, no auth

required) using stock ticker symbols. Any US publicly-traded company

is supported -- configure via the \`\`tickers\`\` Dag parameter.

\`\`example\_llamaindex\_10k\_index\`\` (weekly schedule):

.. code-block:: text

fetch\_filings (@task, live from SEC EDGAR)

 -\> build\_index (LlamaIndexEmbeddingOperator, mapped x N companies)

\`\`example\_llamaindex\_10k\_analysis\`\` (manual trigger):

.. code-block:: text

analyst\_question (HITLEntryOperator)

 -\> get\_question (@task)

 -\> get\_tickers (@task)

 -\> decompose\_question (@task.llm, structured output)

 -\> extract\_sub\_questions (@task)

 -\> build\_retrieval\_kwargs (@task)

 -\> retrieve (LlamaIndexRetrievalOperator x N, DTM)

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

Before running:

1\. Create an LLM connection \`\`pydanticai\_default\`\` for synthesis and

decomposition, and \`\`llamaindex\_default\`\` for embedding and retrieval.

2\. Update \`\`EDGAR\_USER\_AGENT\`\` with your name and email (SEC requires a

descriptive User-Agent header on all EDGAR API requests).

3\. Run \`\`example\_llamaindex\_10k\_index\`\` once to fetch filings and build

the vector indexes.

4\. Trigger \`\`example\_llamaindex\_10k\_analysis\`\` to run the query pipeline.

"""

from \_\_future\_\_ importannotations

importjson

importre

importurllib.request

fromdatetimeimporttimedelta

fromtypingimportAny

frompydanticimportBaseModel

frompydantic\_ai.usageimportUsageLimits

fromairflow.providers.common.ai.operators.llamaindex\_embeddingimportLlamaIndexEmbeddingOperator

fromairflow.providers.common.ai.operators.llamaindex\_retrievalimportLlamaIndexRetrievalOperator

fromairflow.providers.common.ai.operators.llmimportLLMOperator

fromairflow.providers.common.compat.sdkimportdag, task

fromairflow.providers.standard.operators.hitlimportApprovalOperator, HITLEntryOperator

fromairflow.sdkimportParam

\# ---------------------------------------------------------------------------

\# Configuration

\# ---------------------------------------------------------------------------

LLM\_CONN\_ID="pydanticai\_default"

LLAMAINDEX\_CONN\_ID="llamaindex\_default"

INDEX\_BASE\_DIR="/opt/airflow/data/indexes/10k"

DEFAULT\_TICKERS="AAPL,MSFT,UBER,LYFT,AMZN"

\# SEC EDGAR requires a descriptive User-Agent header with a contact email

\# on every request. Update this before running in production.

EDGAR\_USER\_AGENT="Apache Airflow Example dev@airflow.apache.org"

\# ---------------------------------------------------------------------------

\# Structured output models

\# ---------------------------------------------------------------------------

\# \[START 10k\_structured\_output\]

classSubQuestion(BaseModel):

"""One sub-question targeting a specific company."""

sub\_question: str

ticker: str

classDecomposedQuestion(BaseModel):

"""LLM-produced decomposition of the analyst's question."""

sub\_questions: list\[SubQuestion\]

classAnalysisReport(BaseModel):

"""Structured financial comparison report."""

executive\_summary: str

company\_findings: list\[dict\]

key\_risks: list\[str\]

recommendations: list\[str\]

\# \[END 10k\_structured\_output\]

\# ---------------------------------------------------------------------------

\# SEC EDGAR helpers

\# ---------------------------------------------------------------------------

def\_edgar\_get\_json(url: str) ->Any:

"""GET a JSON endpoint from SEC EDGAR (public, no auth)."""

req=urllib.request.Request(

url,

headers={"User-Agent": EDGAR\_USER\_AGENT, "Accept": "application/json"},

)

withurllib.request.urlopen(req, timeout=30) asresp:

returnjson.loads(resp.read())

def\_edgar\_get\_text(url: str) ->str:

"""GET an HTML/text document from SEC EDGAR."""

req=urllib.request.Request(url, headers={"User-Agent": EDGAR\_USER\_AGENT})

withurllib.request.urlopen(req, timeout=60) asresp:

returnresp.read().decode("utf-8", errors="replace")

def\_strip\_html\_tags(html: str) ->str:

"""Remove HTML tags, returning plain text."""

text=re.sub(r"<\[^>\]+>", " ", html)

returnre.sub(r"\\s+", " ", text).strip()

def\_resolve\_ticker(ticker: str) ->tuple\[int, str\]:

"""Look up a company's CIK number and name from its stock ticker.

Returns (cik, company\_name).

"""

data=\_edgar\_get\_json("https://www.sec.gov/files/company\_tickers.json")

forentryindata.values():

ifentry\["ticker"\].upper() ==ticker.upper():

returnint(entry\["cik\_str"\]), entry\["title"\]

raiseValueError(f"Ticker {ticker!r} not found in SEC EDGAR. Check the ticker symbol and try again.")

def\_find\_latest\_10k(cik: int) ->tuple\[str, str\]:

"""Find the latest 10-K filing URL and date for a given CIK.

Returns (document\_url, filing\_date).

"""

padded=str(cik).zfill(10)

submissions=\_edgar\_get\_json(f"https://data.sec.gov/submissions/CIK{padded}.json")

recent=submissions\["filings"\]\["recent"\]

fori, forminenumerate(recent\["form"\]):

ifform=="10-K":

accession=recent\["accessionNumber"\]\[i\].replace("-", "")

primary\_doc=recent\["primaryDocument"\]\[i\]

filing\_date=recent\["filingDate"\]\[i\]

url=f"https://www.sec.gov/Archives/edgar/data/{cik}/{accession}/{primary\_doc}"

returnurl, filing\_date

raiseValueError(f"No 10-K filing found for CIK {padded}")

def\_extract\_filing\_sections(full\_text: str, ticker: str, company\_name: str, filing\_date: str) ->list\[dict\]:

"""Split 10-K text into section-aware documents for embedding.

Attempts to extract Item 1A (Risk Factors) and Item 7 (MD&A).

Falls back to the first 40 000 characters if section markers

are not found.

"""

metadata\_base= {"ticker": ticker, "company": company\_name, "filing\_date": filing\_date}

sections= \[\]

section\_patterns= \[\
\
("risk\_factors", r"(?i)(item\\s+1a\\b.{0,40}risk\\s+factors.\*?)(?=\\bitem\\s+1b\\b\|\\bitem\\s+2\\b)"),\
\
("mda", r"(?i)(item\\s+7\\b.{0,60}management.\*?)(?=\\bitem\\s+7a\\b\|\\bitem\\s+8\\b)"),\
\
\]

forsection\_name, patterninsection\_patterns:

matches=list(re.finditer(pattern, full\_text, re.DOTALL))

ifmatches:

best=max(matches, key=lambdam: len(m.group(1)))

section\_text=best.group(1).strip()\[:20\_000\]

iflen(section\_text) >200:

sections.append(

{

"text": section\_text,

"metadata": {\*\*metadata\_base, "section": section\_name},

}

)

ifnotsections:

sections.append(

{

"text": full\_text\[:40\_000\],

"metadata": {\*\*metadata\_base, "section": "full\_filing"},

}

)

returnsections

\# ---------------------------------------------------------------------------

\# System prompts

\# ---------------------------------------------------------------------------

DECOMPOSE\_SYSTEM\_PROMPT="""\

You are a financial research assistant. Given a comparison question and a \

list of companies (identified by stock ticker), decompose it into specific \

sub-questions, one per company. Each sub-question should target information \

that would be found in the company's 10-K filing (Risk Factors or MD&A \

sections). Use the exact ticker symbol in the \`\`ticker\`\` field."""

SYNTHESIS\_SYSTEM\_PROMPT="""\

You are a senior financial analyst. Given retrieval results from multiple \

companies' 10-K filings, synthesize a structured comparison report. \

Cite specific data points from the source text. Be precise about which \

company each finding relates to."""

DEFAULT\_QUESTION= (

"Compare the risk factors and revenue trends across these companies. "

"Which company faces the most concentrated risk and which shows the "

"strongest growth trajectory?"

)

\# =========================================================================

\# DAG 1: Fetch and index filings (scheduled)

\# =========================================================================

\# \[START example\_llamaindex\_10k\_index\]

@dag(

schedule="@weekly",

catchup=False,

params={

"tickers": Param(

DEFAULT\_TICKERS,

type="string",

description="Comma-separated stock tickers to fetch and index (e.g. AAPL,MSFT,UBER)",

),

},

tags=\["example", "llamaindex", "10k"\],

)

defexample\_llamaindex\_10k\_index():

"""

Fetch 10-K filings from SEC EDGAR and build per-company vector indexes.

Runs weekly to refresh indexes when new filings arrive. Each company

gets its own persisted index via Dynamic Task Mapping.

Task graph::

fetch\_filings (@task, live from SEC EDGAR)

 -\> build\_index (LlamaIndexEmbeddingOperator x N companies)

"""

\# \[START 10k\_index\_dtm\]

@task

deffetch\_filings(params: dict) ->list\[dict\]:

tickers= \[t.strip().upper() fortinparams\["tickers"\].split(",") ift.strip()\]

result= \[\]

fortickerintickers:

cik, company\_name=\_resolve\_ticker(ticker)

doc\_url, filing\_date=\_find\_latest\_10k(cik)

html=\_edgar\_get\_text(doc\_url)

plain\_text=\_strip\_html\_tags(html)

documents=\_extract\_filing\_sections(plain\_text, ticker, company\_name, filing\_date)

result.append(

{

"documents": documents,

"persist\_dir": f"{INDEX\_BASE\_DIR}/{ticker.lower()}",

}

)

returnresult

LlamaIndexEmbeddingOperator.partial(

task\_id="build\_index",

embed\_model="text-embedding-3-small",

llm\_conn\_id=LLAMAINDEX\_CONN\_ID,

chunk\_size=512,

chunk\_overlap=50,

).expand\_kwargs(fetch\_filings())

\# \[END 10k\_index\_dtm\]

\# \[END example\_llamaindex\_10k\_index\]

example\_llamaindex\_10k\_index()

\# =========================================================================

\# DAG 2: Analyst query pipeline (on-demand)

\# =========================================================================

\# \[START example\_llamaindex\_10k\_analysis\]

@dag(

schedule=None,

catchup=False,

params={

"tickers": Param(

DEFAULT\_TICKERS,

type="string",

description="Comma-separated stock tickers (must match indexed companies)",

),

},

tags=\["example", "llamaindex", "10k"\],

)

defexample\_llamaindex\_10k\_analysis():

"""

Multi-company financial comparison via LLM-driven sub-question decomposition.

An analyst submits a comparison question. The LLM decomposes it into

company-specific sub-questions (N decided at runtime), each sub-question

retrieves from the appropriate company's vector index in parallel via

Dynamic Task Mapping, and the results are synthesized into a structured

report for human review.

Task graph::

analyst\_input (HITLEntryOperator, tickers + question)

 -\> get\_question (@task)

 -\> get\_tickers (@task)

 -\> decompose\_question (@task.llm, structured output)

 -\> extract\_sub\_questions (@task)

 -\> build\_retrieval\_kwargs (@task)

 -\> retrieve (LlamaIndexRetrievalOperator x N, DTM)

 -\> collect\_results (@task)

 -\> synthesize\_report (LLMOperator, UsageLimits + AnalysisReport)

 -\> format\_report (@task, readable text for reviewer)

 -\> review\_report (ApprovalOperator)

"""

\# ------------------------------------------------------------------

\# Step 1: Analyst submits the comparison question via HITL.

\# ------------------------------------------------------------------

\# \[START 10k\_hitl\_entry\]

analyst\_input=HITLEntryOperator(

task\_id="analyst\_input",

subject="Enter a 10-K comparison question",

params={

"tickers": Param(

DEFAULT\_TICKERS,

type="string",

description="Comma-separated stock tickers to compare (must match indexed companies)",

),

"question": Param(

DEFAULT\_QUESTION,

type="string",

description="Financial comparison question across the selected companies",

),

},

response\_timeout=timedelta(hours=1),

)

\# \[END 10k\_hitl\_entry\]

@task

defget\_question(hitl\_response: dict) ->str:

returnhitl\_response\["params\_input"\]\["question"\]

question=get\_question(analyst\_input.output)

@task

defget\_tickers(hitl\_response: dict) ->str:

returnhitl\_response\["params\_input"\]\["tickers"\]

tickers=get\_tickers(analyst\_input.output)

\# ------------------------------------------------------------------

\# Step 2: LLM decomposes the question into company-specific

\# sub-questions. N is decided by the LLM at runtime -- this is the

\# dynamic adaptation that a static Dag cannot express.

\# ------------------------------------------------------------------

\# \[START 10k\_decompose\]

@task.llm(

llm\_conn\_id=LLM\_CONN\_ID,

system\_prompt=DECOMPOSE\_SYSTEM\_PROMPT,

output\_type=DecomposedQuestion,

\# Push the structured output to XCom as a dict so the example runs on

\# every supported Airflow version (the model-instance form needs 3.3+).

serialize\_output=True,

)

defdecompose\_question(question: str, tickers: str) ->str:

return (

f"Decompose this question into company-specific sub-questions.\\n"

f"Available companies (by ticker): {tickers}\\n\\n"

f"Question: {question}"

)

decomposed=decompose\_question(question, tickers)

\# \[END 10k\_decompose\]

@task

defextract\_sub\_questions(decomposed: dict) ->list\[dict\]:

returndecomposed\["sub\_questions"\]

sub\_questions=extract\_sub\_questions(decomposed)

\# ------------------------------------------------------------------

\# Step 3: Map sub-questions to LlamaIndexRetrievalOperator kwargs.

\# Each sub-question targets a specific company's pre-built index.

\# ------------------------------------------------------------------

@task

defbuild\_retrieval\_kwargs(sub\_questions: list\[dict\]) ->list\[dict\]:

return \[\
\
{\
\
"query": sq\["sub\_question"\],\
\
"index\_persist\_dir": f"{INDEX\_BASE\_DIR}/{sq\['ticker'\].lower()}",\
\
}\
\
forsqinsub\_questions\
\
\]

retrieval\_kwargs=build\_retrieval\_kwargs(sub\_questions)

\# ------------------------------------------------------------------

\# Step 4: Retrieve relevant chunks for each sub-question.

\# Dynamic Task Mapping fans out one LlamaIndexRetrievalOperator

\# per sub-question, each targeting the company's vector index.

\# ------------------------------------------------------------------

\# \[START 10k\_dtm\_retrieval\]

retrieval\_results=LlamaIndexRetrievalOperator.partial(

task\_id="retrieve",

embed\_model="text-embedding-3-small",

llm\_conn\_id=LLAMAINDEX\_CONN\_ID,

top\_k=5,

).expand\_kwargs(retrieval\_kwargs)

\# \[END 10k\_dtm\_retrieval\]

\# ------------------------------------------------------------------

\# Step 5: Collect all retrieval results into a single context.

\# Mapped outputs preserve input order, so zip with sub\_questions

\# re-associates each result with its company.

\# ------------------------------------------------------------------

@task

defcollect\_results(sub\_questions: list\[dict\], results: list\[dict\]) ->str:

sections= \[\]

forsq, rinzip(sub\_questions, results):

chunks\_text="\\n".join(

f" \[{i+1}\] (score {c.get('score') or0.0:.2f}) {c\['text'\]}"

fori, cinenumerate(r\["chunks"\])

)

sections.append(f"## {sq\['ticker'\]} \-\- {sq\['sub\_question'\]}\\n{chunks\_text}")

return"\\n\\n".join(sections)

collected=collect\_results(sub\_questions, retrieval\_results.output)

\# ------------------------------------------------------------------

\# Step 6: Synthesize a structured comparison report.

\# UsageLimits caps the token spend; output\_type=AnalysisReport

\# enforces the Pydantic schema on the LLM response.

\# ------------------------------------------------------------------

\# \[START 10k\_synthesis\]

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

\# \[END 10k\_synthesis\]

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

\# \[START 10k\_hitl\_approval\]

ApprovalOperator(

task\_id="review\_report",

subject="Review 10-K comparison report before sharing",

body=review\_body,

response\_timeout=timedelta(hours=24),

)

\# \[END 10k\_hitl\_approval\]

\# \[END example\_llamaindex\_10k\_analysis\]

example\_llamaindex\_10k\_analysis()

You can’t perform that action at this time.