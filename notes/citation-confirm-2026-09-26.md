# Citation confirmation + 2026 venue scan — 2026-09-26

Closes `LITERATURE_REVIEW.md` (v0.2) search-debt items **2** and **3**.

- **Task 1** — the three identifiers left 🔍 in §1/§2 and in the
  `% UNVERIFIED — confirm before citing` block of `bib/references.bib`.
- **Task 2** — scan of EMNLP 2026 Industry, CIKM 2026, ICLR 2026, FSE 2026,
  ICSE 2026 for agent-systems papers across four topics: multi-agent
  orchestration/routing, GraphRAG evaluation, agent memory benchmarks, agent
  access-control/security.

All three Task-1 items **resolved**. No `UNVERIFIED` remains from Task 1.
Task 2 returned 10 fully-identified papers plus an also-noted set.

## Verification protocol

Same discipline as `notes/snowball-2026-09-26.md`. Every identifier below was
read off a fetched page; nothing is recalled or inferred.

| Tag | Route | Used for |
|---|---|---|
| `arXiv-API` | `export.arxiv.org/api/query`, returned `<title>` string-compared against the target title | Task 1, CIKM entry |
| `arXiv-abs` | `arxiv.org/abs/{id}` HTML, `<title>` + Comments + Subjects read directly | venue-comment checks |
| `ACL-BibTeX` | `aclanthology.org/{id}.bib`, the venue's own exported record | LoCoMo |
| `venue-page` | conference's own accepted-paper page, parsed | ICSE 2026, FSE 2026, CIKM 2026, EMNLP 2026 |
| `ICLR-data` | `iclr.cc/static/virtual/data/iclr-2026-orals-posters.json` + `…-abstracts.json`, the virtual site's own accepted-paper dump | all 8 ICLR entries |
| `URL-200` | fetched, HTTP 200, title read back | all 13 ICLR virtual URLs |

`UNVERIFIED` marks a claim that could not be confirmed from any fetched page.

### Access notes (routes that failed — do not retry blind)

- `dl.acm.org` → **HTTP 403** to this environment. DOIs read off conference
  pages are reported as DOIs, but their ACM landing pages could not be opened
  here. The DOI *string* came from the conference page, not from ACM.
- `dblp.org/search/publ/api` → **anti-bot interstitial** ("Making sure you're
  not a bot!"). DBLP is unusable from here without a browser.
- `api2.openreview.net/notes` → **HTTP 403 ChallengeRequiredError**. The
  `openreview.net/forum?id=…` HTML pages are behind a "Verifying your browser"
  wall. OpenReview is therefore **not** a usable verification route; use
  `iclr.cc/virtual/…` (below) instead, which is fetchable and authoritative.
- `export.arxiv.org` begins returning **HTTP 429** after ~8–10 queries in a
  session. Space queries ~20–45 s apart. A silent `(no entries)` in the helper
  means *rate-limited*, not *no such paper* — this bit me twice and nearly
  produced two false negatives.
- `aclanthology.org/volumes/2026.emnlp*` → **404**. EMNLP 2026 proceedings are
  not on ACL Anthology yet.
- FSE 2026 is co-located: the main conference is in **Montreal**; the
  `fse.iacr.org/2026/json/papers.json` file is the *co-located IACR Fast
  Software Encryption* volume (57 symmetric-crypto papers, zero agent papers).
  Do not mistake it for the FSE main program. Use `conf.researchr.org`.

---

# Task 1 — the three pending identifiers

## 1. "How Do Agent Harnesses Create Value?" — **CONFIRMED**

| Field | Value |
|---|---|
| arXiv ID | **arXiv:2609.20474** (`arXiv-API` + `arXiv-abs`) |
| Full title | *How Do Agent Harnesses Create Value? Planning Information and Release Control in Stateful LLM Agents* |
| Authors | Yukun Zhang, Kemu Xu, Yishen Chen |
| Submitted | 2026-09-17 |
| Subjects | cs.AI (only) |
| Comments | none |

The `LITERATURE_REVIEW.md` short title is a truncation of the real one. Two
reasons this matters for Cambrian:

- It is **not** a harness-design ablation like `fan2026harness`. Its primary
  comparison is prewritten task-specific plans (*Fixed*) against word-count-matched
  *sham* policy text, over 265 trials in two Retail experiments plus an Airline
  pilot on **τ²-bench**. It isolates the contribution of guidance *content* —
  a placebo/negative-control design Cambrian's own dispatch ablations should copy.
- **"Release control"** is the agent-side name for the thing Cambrian enforces in
  the kernel. Worth reading before drafting the dispatch-satellite related work;
  the τ²-bench tie-in also links it to the `tau2` probes already wired locally.

Recommended bib key: `zhang2026harnessvalue`. The existing
`fan2026harness` key is a *different* paper — do not merge them.

## 2. "SoL-Pi: Recursively Scaling Auto-Research Loops" — **CONFIRMED**

| Field | Value |
|---|---|
| arXiv ID | **arXiv:2609.20519** (`arXiv-API` + `arXiv-abs`) |
| Full title | *SoL-Pi: Recursively Scaling Auto-Research Loops for Efficient Agent Harness* |
| Authors (first 8) | Haozhe Liu, Tian Ye, Sensen Gao, Qihang Cao, Yitong Li, Mingchen Zhuge, Duomin Wang, Ruihua Zhang |
| Submitted | 2026-09-17 |
| Comments | 15 pages, 8 figures, 4 tables; code + project page linked |

"An RSI-inspired approach at the harness layer, scaling auto-research loops";
token efficiency under long unattended trajectories. Same submission day as
`2609.20474`. Cite as `liu2026solpi`. Venue: none claimed — do not attach one.

## 3. LoCoMo — **CONFIRMED**, and `references.bib` has a bug

| Field | Value |
|---|---|
| arXiv ID | **arXiv:2402.17753** (`arXiv-API` + `arXiv-abs`) |
| ACL Anthology | `2024.acl-long.747` |
| **DOI** | **10.18653/v1/2024.acl-long.747** (`ACL-BibTeX`) |
| Pages | **13851–13870** (`ACL-BibTeX`) |
| Venue | ACL 2024 (62nd Annual Meeting, Volume 1: Long Papers), Bangkok, Thailand |
| Project page | `snap-research.github.io/locomo` (from arXiv comment) |

**Bug in `references.bib`.** `maharana2024locomo` records the fifth author as
`Fang, Yi`. The ACL Anthology BibTeX and the arXiv author list both say
**`Fang, Yuwei`**. Fix to `Fang, Yuwei` and add `pages = {13851--13870}` and
`doi = {10.18653/v1/2024.acl-long.747}`.

The existing review note "50 dialogues" is not in the abstract; the abstract says
300 turns and ~9K tokens per conversation, over up to 35 sessions. Leave the
"50 dialogues" figure alone unless re-checked against the paper body — it is not
confirmable from the abstract.

---

# Task 2 — 2026 venue scan

## Venue availability first (this determined what was findable)

| Venue | Status on 2026-09-26 | Route used |
|---|---|---|
| **ICLR 2026** | ✅ **Fully published.** 5,691 accepted papers, complete titles + authors + OpenReview IDs in a public JSON dump. Best-covered venue by an order of magnitude. | `ICLR-data` |
| **ICSE 2026** | ✅ Full program public (321 research-track acceptances). Per-row DOIs / preprints where authors supplied them. | `venue-page` |
| **FSE 2026** | ✅ Program public. **Research track is a near-dead end; the Industry track is the productive one** (39 agent-related papers). | `venue-page` |
| **CIKM 2026** | ⚠️ Official accepted-papers page is literally the string `TBD`. Conference is 7–11 Nov 2026, not yet held. Papers were recovered via arXiv comments instead. | arXiv comments + institutional repository |
| **EMNLP 2026** | ⚠️ Conference is **25–27 Oct 2026 — next week**. `2026.emnlp.org/program/ind_papers/` renders the title and nav but the paper list is an empty placeholder; no ACL Anthology volume yet. Only self-declared arXiv comments are available. | arXiv comments |

Consequence for the review: the `TRACE … (CIKM 2026)` entry in
`LITERATURE_REVIEW.md` §2 and in `bib/references.bib` (`trace2026`,
`note={CIKM 2026}`) **cannot be confirmed** — see UNVERIFIED below. The
`RAFT … (EMNLP 2026 Industry)` entry **is** confirmed. A third already-cited
paper turns out to be EMNLP 2026 *Findings*, not Industry.

## The ten papers

Ordered by relevance to Cambrian. Every row has a fetched identifier.

| # | Paper | Venue | Verified identifier | Relevance to Cambrian (one line) |
|---|---|---|---|---|
| 1 | **AgentBound: Securing Execution Boundaries of AI Agents** | FSE 2026, Research Papers (Montreal) | DOI `10.1145/3808103`; `conf.researchr.org/details/fse-2026/fse-2026-research-papers/14/AgentBound-Securing-Execution-Boundaries-of-AI-Agents` (`venue-page`, `URL-200`) | The single closest published analogue to ADR-0085 chokepoints: enforcement lives at the *execution boundary*, not in the agent's own reasoning — exactly "model proposes, kernel disposes". Make this the security satellite's lead citation. |
| 2 | **AgentSpec: Customizable Runtime Enforcement for Safe and Reliable LLM Agents** | ICSE 2026, Research Track | DOI `10.1145/3744916.3764546`; arXiv:`2503.18666` (`venue-page`, `arXiv-abs`) | Customizable runtime enforcement DSL for agent actions; a second independent arrival at kernel-side policy over prompt-side policy, and a citable SE-venue peer for the same claim. |
| 3 | **Breaking and Fixing Defenses Against Control Flow Hijacking in Multi-Agent Systems** | ICLR 2026 (Poster) | OpenReview `10009684`; `iclr.cc/virtual/2026/poster/10009684` (`ICLR-data`, `URL-200`) — Jha, Triedman, Wagle, Shmatikov | Attacks **the orchestration mechanism itself** to make MAS execute unsafe actions and exfiltrate data. This is the dispatch-layer threat model; it is the sharpest available complement to `mcpthreats2025` (protocol-level) and belongs in the ADR-0043 threat taxonomy. |
| 4 | **ST-WebAgentBench: A Benchmark for Evaluating Safety and Trustworthiness in Web Agents** | ICLR 2026 (Poster) | OpenReview `10009904`; `iclr.cc/virtual/2026/poster/10009904` (`ICLR-data`, `URL-200`) — Levy, Wiesel, Marreed, Oved | Scores whether agents finish tasks *safely and enterprise-trustworthily*, not just successfully — the trust-axis complement to `smyth2026overclaim` and to the `gaia-tool-use` suite. |
| 5 | **OrchestrationBench: LLM-Driven Agentic Planning and Tool Use in Multi-Domain Scenarios** | ICLR 2026 (Poster) | OpenReview `10009754`; `iclr.cc/virtual/2026/poster/10009754` (`ICLR-data`, `URL-200`) — Ahn, Lee, Wang, Park | A dedicated benchmark for multi-step agentic planning + tool use across domains; the most plausible external harness for scoring Cambrian's dispatch decisions, complementing the locally-wired `orchestration` dataset. |
| 6 | **Stronger-MAS: Multi-Agent Reinforcement Learning for Collaborative LLMs** | ICLR 2026 (Poster) | OpenReview `10010282`; `iclr.cc/virtual/2026/poster/10010282` (`ICLR-data`, `URL-200`) — Zhao, Hu, Wang, Hou | States outright that MAS strengthens task-specialized performance via **role-based orchestration** — the closest ICLR-2026 sibling to the MDPI survey's role-based allocation strategy, i.e. the learned counterpart to Cambrian's capability-typed dispatch. |
| 7 | **AMemGym: Interactive Memory Benchmarking for Assistants in Long-Horizon Conversations** | ICLR 2026 (Poster) | OpenReview `10007039`; `iclr.cc/virtual/2026/poster/10007039` (`ICLR-data`, `URL-200`) — Jiayang, Ru, Qiu, Li | Argues existing memory benchmarks are static and offline, and supplies an **interactive** alternative; the natural successor-generation counterpart to LoCoMo/LongMemEval for topic 10. |
| 8 | **Evaluating Memory in LLM Agents via Incremental Multi-Turn Interactions** (MemoryAgentBench) | ICLR 2026 (Poster) | OpenReview `10010781`; `iclr.cc/virtual/2026/poster/10010781`; proceedings PDF `proceedings.iclr.cc/paper_files/paper/2026/file/fd1eff9dd295df50a41f2521942fa31d-Paper-Conference.pdf` (`ICLR-data`, `URL-200`) — Hu, Wang, McAuley | Memory measured as *incremental multi-turn* interaction rather than one-shot recall; a 2026 peer to ARE/Gaia2 in evaluation design, and the memory-side sibling the review was missing. |
| 9 | **Predicting Partial Answer Quality and Utility in Agentic Retrieval-Augmented Generation** | **CIKM 2026** (full paper; Rome, 7–11 Nov 2026) | arXiv:`2609.16453` (`arXiv-abs` — comment reads "this paper has been accepted by CIKM'26 as a full paper"); independently confirmed by Glasgow Enlighten `eprints.gla.ac.uk/392557` ("35th ACM International Conference on Information and Knowledge Management (CIKM 2026) … Accepted for Publication") — Tian, Ganguly, Macdonald | **In-trajectory probing**: forces an intermediate answer after each retrieve–reason step to measure how the answer state evolves. That is Cambrian's row-trace methodology, independently arrived at, at CIKM — cite it in `METHODOLOGY.md` as prior art for trajectory-level rather than final-answer scoring. |
| 10 | **Demystifying Deep Search: A Holistic Evaluation with Hint-free Multi-Hop Questions and Factorised Metrics** | ICLR 2026 (Poster) | OpenReview `10006679`; `iclr.cc/virtual/2026/poster/10006679` (`ICLR-data`, `URL-200`) — Song, Renhang, Wang, Jiang | Attacks the two methodological leaks in current multi-hop RAG/web-agent eval (reasoning paths leaked in the question; end-to-end metrics only) and replaces them with hint-free questions and factorised metrics — the concrete recipe for the `graphrag2026survey` gap the review already flagged. |

Coverage check against the four requested topics: orchestration/routing =
#3, #5, #6 · GraphRAG evaluation = #9, #10 · agent memory benchmarks = #7, #8 ·
agent access-control/security = #1, #2, #3, #4. Venues covered: ICLR 2026 (8),
CIKM 2026 (1), FSE 2026 (1), ICSE 2026 (1). EMNLP 2026 contributed no new
entry for the reasons above.

## Also verified, held in reserve

All identifiers fetched. Not in the top ten, but each maps to a live Cambrian
need and can be promoted without further verification.

| Paper | Venue | Verified identifier | Use |
|---|---|---|---|
| MCPMark: A Benchmark for Stress-Testing Realistic and Comprehensive MCP Use | ICLR 2026 | OpenReview `10006866`; `iclr.cc/virtual/2026/poster/10006866` | MCP is Cambrian's kernel transport; this is the adversarial-MCP-surface benchmark to pair with `mcpthreats2025`. High value. |
| MCP-Bench: Benchmarking Tool-Using LLM Agents with Complex Real-World Tasks via MCP Servers | ICLR 2026 | OpenReview `10008216`; `iclr.cc/virtual/2026/poster/10008216` | Second, independent MCP benchmark — good for showing the surface is not single-benchmark-dependent. |
| The Tool Decathlon: Benchmarking Language Agents for Diverse, Realistic, and Long-Horizon Task Execution | ICLR 2026 | OpenReview `10006492`; `iclr.cc/virtual/2026/poster/10006492` | Long-horizon heterogeneous task execution; candidate external suite. |
| HiPRAG: Hierarchical Process Rewards for Efficient Agentic RAG | ICLR 2026 | OpenReview `10010451`; `iclr.cc/virtual/2026/poster/10010451` | Process-level reward on agentic retrieval — a per-step scoring scheme to compare against row traces. |
| ResiliBench: Evaluating Agentic Workflow Adaptation in Stochastic Environments | ICLR 2026 | OpenReview `10010130`; `iclr.cc/virtual/2026/poster/10010130` | Degrades *instruction quality* and *tool reliability* on purpose; the robustness axis for orchestration claims. |
| Automated Stateful Specialization for Adaptive Agent Systems | ICLR 2026 | OpenReview `10009249`; `iclr.cc/virtual/2026/poster/10009249` | Argues static workflows cannot accumulate agent-level expertise — a direct challenge to static role tables, worth engaging. |
| TAAF: A Trace Abstraction and Analysis Framework Synergizing Knowledge Graphs and LLMs | ICSE 2026, Research Track | arXiv:`2601.02632` (`arXiv-abs`, title confirmed) | Trace abstraction over a KG + LLM; the KG-side analogue of Cambrian's routing traces. |
| Beyond Final Code: A Process-Oriented Error Analysis of Software Development Agents in Real-World GitHub Scenarios | ICSE 2026, Research Track | arXiv:`2503.12374` (`arXiv-abs`, title confirmed) | Process-oriented (not final-artifact) error analysis; peer support for trajectory-level failure kinds. |
| DepRadar: Agentic Coordination for Context-Aware Defect Impact Analysis in Deep Learning Libraries | ICSE 2026, Research Track | arXiv:`2601.09440` (`arXiv-abs`, title confirmed) | Agentic *coordination* as the studied object — orchestration as a first-class SE concern. |
| SMARLA: A Safety Monitoring Approach for Deep Reinforcement Learning Agents | FSE 2026, Industry | `conf.researchr.org/track/fse-2026/fse-2026-industry-papers` (`venue-page`); **no DOI/preprint published on the row** | Runtime safety monitoring for agents — pairs with #1/#2. Venue-verified, identifier pending. |
| Reducing Cost of LLM Agents with Trajectory Reduction | FSE 2026, Industry | arXiv:`2509.23586` (`arXiv-abs`, title confirmed) | Trajectory-level cost reduction; relevant to the cost arm of any routing ablation. |
| MemoryArena: Benchmarking Agent Memory in Interdependent Multi-Session Agentic Tasks | **ICML 2026** (not a requested venue) | arXiv:`2602.16313`, per the project's own citation block at `memoryarena.github.io` | Interdependent multi-session memory — the strongest 2026 memory benchmark found anywhere in this scan. Venue outside scope; flagged because the authors' own page is the only source read, so treat the ICML claim as author-asserted. |

## UNVERIFIED — do not cite until confirmed

- **`trace2026` / "TRACE: Accountable Agentic Retrieval for Source Discovery in
  Digital Archives" is NOT confirmed as CIKM 2026.** The arXiv record
  (`arXiv:2609.19897`, title confirmed) carries **no Comments field at all** — no
  venue claim of any kind. CIKM 2026's official accepted-papers page is `TBD`,
  so there is no official list to check it against. The arXiv ID itself is
  confirmed; only the `note={CIKM 2026}` in `bib/references.bib` and the
  "✅ … (CIKM 2026)" in `LITERATURE_REVIEW.md` §2 are unsupported. Strip the
  venue note or re-check after 7 Nov 2026. Given the subject matter it is
  *plausibly* CIKM, but plausibility is not evidence.
- **"Evaluating Privilege Usage of Agents on Real-World Tools"** (FSE 2026,
  Industry) is on the official program and is squarely on-topic for
  access-control — the best-titled match found for that sub-topic outside
  AgentBound. But its row carries **no DOI, no preprint, no details link**. Only
  the title and venue are confirmed. No identifier exists to cite.
- **EMNLP 2026 Industry Track**: no complete list exists yet (conference is
  25–27 Oct 2026). The only Industry-track item confirmable is `raft2026`
  below. A systematic Industry-track pass is **deferred**, not closed.
- **CIKM 2026**: only the one paper in #9 was recovered. Given the accepted
  list is `TBD`, treat CIKM 2026 coverage as **partial** and re-run after
  7 Nov 2026.
- **FSE 2026 research track** was screened (420 rows, 104 keyword-matching) and
  yielded **one** multi-agent paper, "Hydra-Reviewer", with **no identifier
  published**. FSE 2026's agent-systems content is in the Industry track. Do
  not spend another pass on the research track.

## Corrections to existing entries (all verified)

1. **`maharana2024locomo` author bug** — `Fang, Yi` → **`Fang, Yuwei`**; add
   `pages={13851--13870}`, `doi={10.18653/v1/2024.acl-long.747}`, and
   `eprint={2402.17753}`. See Task 1 §3.
2. **`raft2026` venue claim is correct.** `arXiv:2609.20754` Comments field reads
   "Accepted to the EMNLP 2026 Industry Track" (`arXiv-abs`). Keep as-is.
3. **`progrouter2026` is EMNLP 2026 *Findings*, not Industry.** `arXiv:2608.25992`
   Comments reads "Accepted in Findings of the Association for Computational
   Linguistics: EMNLP 2026" (`arXiv-abs`). The review lists it under §1 without
   a venue claim, which is fine — but do not let it be counted as Industry-track
   coverage.
4. **`trace2026` venue claim is unsupported** — see UNVERIFIED above.
5. Two new `🔍` entries can now be **promoted to ✅**: the Zhang harness-value
   paper and SoL-Pi. Neither is in `references.bib` yet — add
   `zhang2026harnessvalue` and `liu2026solpi` (both `@misc`, `archivePrefix={arXiv}`).

## Still open

- `LITERATURE_REVIEW.md` search-debt item 1 (snowballing) was closed
  separately in `notes/snowball-2026-09-26.md`; item 2 is closed by Task 1
  above; item 3 is **partially** closed — ICLR/ICSE/FSE are done, CIKM and EMNLP
  Industry are blocked on the venues publishing their lists.
- With items 1–3 addressed, `LITERATURE_REVIEW.md` can be promoted to **v0.3**,
  with the CIKM/EMNLP Industry rows annotated as pending-venue rather than
  marked complete.
- §5 (open-core / BSL licensing studies) remains open. This scan found nothing:
  none of the four venues publishes open-source-licensing or BSL research, and
  it is orthogonal to the agent-systems literature. It needs its own route
  (MSR/SEIP/ICSE-SEIP, or a legal scholarship index).
- ICLR 2026 is so productive (5,691 papers) that a single keyword pass almost
  certainly missed relevant work. A second, narrower pass over the
  `ICLR-data` JSON is cheap — it is one fetch and local filtering, no
  rate-limited API involved.
