# ICLR 2026 second pass + open-core/BSL route — 2026-09-27

Two independent literature tasks for the Cambrian academic project, run in one
session. Every identifier below was **read off a fetched page**, never recalled:
OpenReview IDs and titles come from ICLR's own virtual-site JSON dump plus an
HTTP-200 + `<title>` read-back of each poster page; DOIs come from a
Crossref `/works/{doi}` title-match; arXiv IDs from `id_list=` title-match.
Nothing is UNVERIFIED unless explicitly labelled.

Verification ledger:

| Source | Method | Notes |
|---|---|---|
| `iclr.cc/static/virtual/data/iclr-2026-orals-posters.json` | HTTP 200, 15.0 MB, 5 691 accepted papers | 1:1 ID overlap with abstracts file |
| `iclr.cc/static/virtual/data/iclr-2026-abstracts.json` | HTTP 200, 8.1 MB, 5 691 abstracts | `{event_id: abstract_string}` |
| `iclr.cc/virtual/2026/poster/<id>` | HTTP 200 + `<title>` read-back for all 8 | title matched the JSON record exactly |
| `api.crossref.org/works/{doi}` | title-match, 18 DOIs fetched | venue/volume/pages from the Crossref record |
| `api.openalex.org/works/doi:{doi}` | abstract reconstruction from inverted index | used only where Crossref carries no abstract |
| `export.arxiv.org/api/query?id_list=` | title-match, 3 queries, 20–25 s apart | no 429 hit |
| `sciencedirect.com` | **403** — unreachable | Research Policy placement confirmed via the *issue TOC* instead |

---

## Task A — ICLR 2026 second pass (max 8 new papers)

Filter ran locally over 5 691 title+abstract pairs with the narrow keywords
`orchestrat*`, `dispatch`, `router/routing`, `ledger`, `agentic retrieval`,
`process reward / process-level`, `memory`, `consolidat*`, `placebo`,
`negative control`, `ablation`, `evaluat*`, `benchmark methodolog*`,
`reproduc*`, `confound*`. 2 341 papers matched; after removing the 13 ICLR
papers already recorded in `notes/citation-confirm-2026-09-26.md`
(`10009684, 10009904, 10009754, 10010282, 10007039, 10010781, 10006679,
10006866, 10008216, 10006492, 10010451, 10010130, 10009249`) and
title-colliding with the MDPI survey / `LITERATURE_REVIEW.md` §1–4 entries, the
eight below are the new ones with a direct bearing on Cambrian.

All eight are **Accept (Poster)**. Every row: OpenReview ID, virtual URL,
verified title, and one line on Cambrian relevance.

### A1. Learning to Orchestrate Agents in Natural Language with the Conductor
- **Authors:** Stefan Nielsen, Edoardo Cetin, Peter Schwendeman, Qi Sun, Jinglue Xu, Yujin Tang
- **OpenReview:** `U23A2BUKYt` · **Event ID:** `10009267`
- **Virtual:** https://iclr.cc/virtual/2026/poster/10009267
- **Venue:** ICLR 2026 (Poster)
- **Cambrian:** An RL-trained 7B *conductor* learns both agent-to-agent
  communication topologies and role placement over a randomized pool of worker
  LLMs — the learned, LLM-side counterpart to Cambrian's capability-typed
  dispatch. This is the sharpest available contrast for the Zero-Hardcode Rule
  (routing must live in the Awareness layer, never in Go `if/else`) and for
  ADR-0100: same problem, cost profile driven by LLM calls rather than a
  zero-RPC merit rank.

### A2. GraphPlanner: Graph Memory-Augmented Agentic Routing for Multi-Agent LLMs
- **Authors:** Tao Feng, Haozhen Zhang, Zijie Lei, Peixuan Han, Jiaxuan You
- **OpenReview:** `ZdGB7MNQDT` · **Event ID:** `10008792`
- **Virtual:** https://iclr.cc/virtual/2026/poster/10008792
- **Venue:** ICLR 2026 (Poster)
- **Cambrian:** Formulates agentic routing as an MDP where each step picks
  *both* the LLM backbone *and* the role (Planner / Executor / Summarizer),
  conditioned on a heterogeneous graph memory over queries, agents and
  responses. The single best ICLR-2026 paper spanning the substrate × dispatch
  intersection: it is the "dispatch decision as a memory-conditioned state"
  formulation Cambrian's Gatekeeper answers with a deterministic merit rank.

### A3. DiSRouter: Distributed Self-Routing for LLM Selections
- **Authors:** Hang Zheng, Hongshen Xu, Yongkai Lin, Shuai Fan, Lu Chen, Kai Yu
- **OpenReview:** `KDcwXKr0NU` · **Event ID:** `10010146`
- **Virtual:** https://iclr.cc/virtual/2026/poster/10010146
- **Venue:** ICLR 2026 (Poster)
- **Cambrian:** Replaces the centralized external router with per-agent
  *self-awareness* — each LLM decides for itself whether to answer or forward.
  The cleanest published statement of the alternative Cambrian rejects: the
  routing decision is taken by the model's own competence estimate rather than
  by an out-of-band kernel signal. Useful as the "no external gate" arm in a
  dispatch ablation.

### A4. RouterArena: An Open Platform for Comprehensive Comparison of LLM Routers
- **Authors:** Yifan (Louie) Lu, Rixin Liu, Jiayi Yuan, Xingqi Cui, Shenrun Zhang, Hongyi Liu
- **OpenReview:** `9HsaIi4ngF` · **Event ID:** `10011126`
- **Virtual:** https://iclr.cc/virtual/2026/poster/10011126
- **Venue:** ICLR 2026 (Poster)
- **Cambrian:** Documents that router evaluation is currently fragmented —
  "inconsistent metrics, making it difficult to systematically assess progress" —
  then supplies a standard dataset, per-domain difficulty levels, a metric suite
  and an automated leaderboard. This is the external-harness candidate to pair
  with `OrchestrationBench` (already in the review, OpenReview `10009754`) when
  scoring Cambrian's dispatch decisions, and the external authority for the
  review's claim that dispatch metrics need standardising.

### A5. TRAJECT-Bench: A Trajectory-Aware Benchmark for Evaluating Agentic Tool Use
- **Authors:** Pengfei He, Zhenwei Dai, Bing He, Hui Liu, Xianfeng Tang
- **OpenReview:** `TZWnWvsQ0X` · **Event ID:** `10009304`
- **Virtual:** https://iclr.cc/virtual/2026/poster/10009304
- **Venue:** ICLR 2026 (Poster)
- **Cambrian:** States the exact gap `METHODOLOGY.md` already flags — existing
  work "largely focus[es] on the final answers yet overlook the detailed tool
  usage trajectory" — and answers it with per-dimension trajectory diagnostics:
  tool selection, argument correctness, and dependency/order satisfaction. The
  concrete metric menu for splitting `METHODOLOGY.md`'s per-row artifacts into
  scored sub-dimensions rather than one aggregate.

### A6. Process-Level Trajectory Evaluation for Environment Configuration in Software Engineering Agents
- **Authors:** Jiayi Kuang, Yinghui Li, Xin Zhang, Yangning Li, di yin, Xing Sun
- **OpenReview:** `Q8qgloDKUO` · **Event ID:** `10009617`
- **Virtual:** https://iclr.cc/virtual/2026/poster/10009617
- **Venue:** ICLR 2026 (Poster) · topic `Applications->Everything Else`
- **Cambrian:** The bluntest published statement of the breakdown-over-aggregates
  rule: "Existing benchmarks assess only end-to-end build/test success, obscuring
  where and why agents succeed or fail." EnConda-bench scores setup-planning,
  error diagnosis, feedback-driven repair and execution *separately*. Cite this
  as the methodology authority for why `failure_kind` must be a first-class
  field, not a post-hoc annotation.

### A7. REMem: Reasoning with Episodic Memory in Language Agent
- **Authors:** Yiheng Shu, Padmaja Jonnalagedda, Xiang Gao, Bernal Jimenez Gutierrez, Weijian Qi
- **OpenReview:** `fugnQxbvMm` · **Event ID:** `10008195`
- **Virtual:** https://iclr.cc/virtual/2026/poster/10008195
- **Venue:** ICLR 2026 (Poster)
- **Cambrian:** Argues language-agent memory "remains mainly semantic" and
  supplies a two-phase design — offline indexing into a hybrid memory graph of
  time-aware gists and facts, then an *agentic retriever* for iterative
  retrieval over that graph. This is the direct prior art for the
  episodic → procedural consolidation step in topic 10, and the specific
  architectural contrast to `benchmarking/`'s existing `locomo` suite
  (one-shot recall rather than graph-structured episodic recollection).

### A8. Your Agent May Misevolve: Emergent Risks in Self-evolving LLM Agents
- **Authors:** Shuai Shao, Qihan Ren, Dongrui Liu, Chen Qian, Boyi Wei
- **OpenReview:** `Fd1jgQQW28` · **Event ID:** `10010580`
- **Virtual:** https://iclr.cc/virtual/2026/poster/10010580
- **Venue:** ICLR 2026 (Poster) · topic `Social Aspects->Fairness, Equity, Justice and Safety`
- **Cambrian:** Evaluates *misevolution* along four pathways — model, memory,
  tool, workflow — and finds safety-alignment degradation **after memory
  accumulation**, plus vulnerabilities introduced by tool creation and reuse,
  even on top-tier LLMs. The risk register for any self-modifying substrate
  surface, and a direct threat to the memory-consolidation path: accumulating
  experience is not automatically safe accumulation.

### Task A negative results (worth recording, they close search debt)

- **`placebo` — 0 hits. `negative control` — 0 hits. `sham` — 0 hits.** Across
  all 5 691 ICLR 2026 titles *and* abstracts. The word-count-matched sham-policy
  placebo design in Zhang et al. (`arXiv:2609.20474`, already in
  `LITERATURE_REVIEW.md` §1) has **no ICLR 2026 counterpart**. ICLR 2026 has no
  comparable negative-control methodology to cite; do not spend another pass
  looking for one there.
- **`ledger` — 0 title hits.** The task-ledger / progress-ledger line
  (Magentic-One, `arXiv:2411.04468`, already in the review) has no ICLR 2026
  successor under that name. Routing-trace accounting at ICLR 2026 goes under
  "routing" (A1–A4), not "ledger".
- **`dispatch` — 2 title hits, both false positives** (ride-sharing order
  dispatch, `10007021` / `10007020`). Agent-granularity dispatch is not an
  ICLR 2026 title-level term; it is filed under *routing*. Anyone re-running
  this filter should search `rout*`, not `dispatch`.
- **Title-level `consolidat*` (5 hits) is entirely ML-continual-learning and
  KV-cache consolidation** (`10008228`, `10010555`, `10009096`, `10011081`,
  `10007077`) — none is agent memory consolidation. Agentic memory
  consolidation in ICLR 2026 is filed under *memory* (A7) or *workflow*
  synthesis, not *consolidation*.
- `construct validity` — 0 hits. `internally valid` — 0 hits. Neither term is
  used in ICLR 2026 agent-evaluation abstracts, so a Cambrian methods section
  using them will be citing an outside literature (SE venues), not ICLR.

### Task A reserve candidates (IDs verified, not written up)

Fetched and title-matched, held back to respect the max-8 cap. All ICLR 2026
(Poster) unless noted; all `https://iclr.cc/virtual/2026/poster/<id>` → HTTP 200.

| Event ID | OpenReview | Title | Why held back |
|---|---|---|---|
| `10009049` | `WV7xIboTDK` | Reliable Weak-to-Strong Monitoring of LLM Agents | **Oral** — strongest monitoring paper here; kept out only to fit the cap. Escalate to first choice if a security satellite is drafted next. |
| `10011762` | `2H03gm4Rq6` | Towards Self-Evolving Agent Benchmarks: Validatable Agent Trajectory via Test-Time Exploration (TRACE) | Benchmark-generation method; overlaps A5/A6 territory |
| `10011614` | `3lm8lWYxiq` | The Illusion of Diminishing Returns: Measuring Long Horizon Execution in LLMs | Long-horizon measurement, not dispatch |
| `10007190` | `r0BFucF2dH` | Meta-Router: Bridging Gold-standard and Preference-based Evaluations in LLM Routing | Causal-inference framing of router-supervision bias — the closest thing to a negative-control treatment in routing |
| `10011673` | `34v7DVz2l0` | FlowSearcher: Synthesizing Memory-Guided Agentic Workflows for Web Information Seeking | Workflow consolidation + orchestration; overlaps A2 |
| `10010510` | `GGrLeik2qo` | MaRS: Memory-Adaptive Routing for Reliable Capacity Expansion | Continual-learning router, not LLM/agent routing |
| `10009097` | `Vr5f3kRvLD` | IDER: IDempotent Experience Replay for Reliable Continual Learning | Replay reliability; CL not agents |

---

## Task B — open-core / BSL route for `LITERATURE_REVIEW.md` §5 (max 8 items)

§5 was empty (`🔍 Open-core/BSL licensing studies … still open`). The search
route was Crossref venue-scoped (`query.bibliographic` × `query.container-title`
across 17 target venues, 170 requests, ~1.1 s apart) plus arXiv title-match.
201 in-venue hits were triaged down to the 8 that can actually carry an
open-core case study. Every DOI below came back from a
`api.crossref.org/works/{doi}` fetch whose `title` field matches the row.

**Headline finding for §5:** there is *no* peer-reviewed study of the Business
Source License, Fair Source, or open-core licensing *as such* in the SE or
economics literature. What exists is (a) license-usage measurement, (b) the
Redis/SSPL re-licensing event study, (c) fork-sustainability case studies,
(d) business-model and commercial-participation SLRs, and (e) firm-level
economics of OSS companies. BSL/Fair Source is covered only by one
workshop-submitted preprint and by industry primary sources (Sentry's Fair
Source announcement, `blog.sentry.io/sentry-is-now-fair-source`; the
Open Core Ventures `OCVSAL` license text, `github.com/OpenCoreVentures/open-core-ventures-source-available-license`).
**Consequence for the paper:** ADR-0057 cannot cite a peer-reviewed BSL
literature, because there is not one. It must be framed as an *extension* of the
license-usage measurement and fork-sustainability literatures, with the
open-core rationale argued rather than cited.

### B1. Are cloud providers exploiting open-source? An exploratory study of Redis license change
- **Authors:** Fanyu Han, Shengyu Zhao, Xiaoya Xia, Wei Wang
- **DOI:** `10.1016/j.infsof.2025.107951` · https://doi.org/10.1016/j.infsof.2025.107951
- **Venue:** *Information and Software Technology* **190** (2026), art. 107951 · Crossref title-match + OpenAlex
- **Fact-checked detail (from the reconstructed abstract):** on **20 March 2024**
  Redis Inc. moved Redis from BSD to dual licensing under **RSALv2 + SSPLv1**
  — *not* BSL; BSL came later, in May 2024. Method: CHAOSS community-health
  metrics + collaboration data, contributors split into company-driven,
  community-driven and communication-driven. Result: contributor participation
  and influence fell during the change window and core contributors moved to
  the **Valkey** fork.
- **Framing for ADR-0057:** this is the *event study* the open-core argument
  needs. It supplies the outcome variable a re-licensing case study measures —
  contributor attrition and fork migration — and it is the empirical basis for
  stating that a permissive-OSS → source-available move is not costless for
  the project even when the commercial rationale is sound. Cambrian's case
  study should be positioned as the *prevented* version of this event (a
  separate premium module instead of re-licensing the core), which is exactly
  the design ADR-0057 encodes.

### B2. Sustainability of Open Source software communities beyond a fork: How and why has the LibreOffice project evolved?
- **Authors:** Jonas Gamalielsson, B. Lundell
- **DOI:** `10.1016/j.jss.2013.11.1077` · https://doi.org/10.1016/j.jss.2013.11.1077
- **Venue:** *Journal of Systems and Software* **89** (2014), pp. 128–145 · Crossref title-match + Semantic Scholar
- **Framing for ADR-0057:** "First comprehensive analysis of Open Source
  projects involving a fork" (S2 abstract). BSL's entire design premise is
  "fork me and it becomes open" — so the fork-outcome literature is the
  *consequence-side* evidence BSL does not itself provide. LibreOffice
  outliving OpenOffice.org is the counter-case that a re-licensing project
  optimists cite and critics must engage.

### B3. The financial implications of open innovation: Evidence from stock market reactions to public companies' open source software releases
- **Authors:** Wei Yang
- **DOI:** `10.1016/j.respol.2026.105500` · https://doi.org/10.1016/j.respol.2026.105500
- **Venue:** *Research Policy* **55**(7) (2026), art. 105500 · Crossref title-match; volume/issue independently confirmed from the ScienceDirect *issue TOC* (Vol 55, Issue 7, September 2026) after `sciencedirect.com` returned **403**
- **Framing for ADR-0057:** the *market-value* leg of the open-core case. If
  public firms' OSS releases move stock prices, then the OSS half of an
  open-core structure is an investable asset, not charity — which is the
  economic justification for ADR-0057's decision to keep `core/` genuinely
  open and put only the premium layer behind a paywall. **Abstract not
  retrieved** (Crossref, S2 and OpenAlex all return null; ScienceDirect 403) —
  the *direction* of the stock-market effect is **UNVERIFIED**; cite the paper
  for the existence and venue of the evidence, not for its sign, until the PDF
  is read.

### B4. A Large-Scale Empirical Study of Open Source License Usage: Practices and Challenges
- **Authors:** Jiaqi Wu, Ling-Feng Bao, Xiao-Hu Yang, Xin Xia, Xing Hu
- **DOI:** `10.1145/3643991.3644900` · https://doi.org/10.1145/3643991.3644900
- **Venue:** MSR 2024 (21st International Conference on Mining Software
  Repositories), pp. 595–606 · Crossref title-match + Semantic Scholar
- **Framing for ADR-0057:** the *measurement instrument* precedent. Analyses
  license usage at scale across **five package-management platforms** and
  argues package-manager data beats GitHub data because it reflects mature
  rather than personal/class projects. The methodological lesson for the
  Cambrian case study: a licensing claim about an infrastructure project should
  be evidenced from consumption surfaces (module proxy, container registry,
  package index), not from a GitHub licence-file census.

### B5. A business model for commercial open source software: A systematic literature review
- **Authors:** Shahrivar Shahrivar, Sima Elahi, Amir Hassanzadeh, Gholamreza Montazer
- **DOI:** `10.1016/j.infsof.2018.06.018` · https://doi.org/10.1016/j.infsof.2018.06.018
- **Venue:** *Information and Software Technology* **103** (2018), pp. 202–214 · Crossref title-match + Semantic Scholar
- **Framing for ADR-0057:** the *taxonomy* leg. It separates Commercial OSS
  from community OSS and makes the point that the literature has usually
  studied the **revenue model** (one component) and called it the business
  model. That distinction is load-bearing for ADR-0057: the open-core boundary
  is a *licensing and module-structure* decision, not a pricing decision, and
  conflating the two is the most common error in open-core commentary.

### B6. Systematic Literature Review of Commercial Participation in Open Source Software
- **Authors:** Xuetao Li, Yuxia Zhang, Cailean Osborne, Minghui Zhou, Zhi Gang Jin, Hui Liu
- **DOI:** `10.1145/3690632` · https://doi.org/10.1145/3690632
- **Open-access preprint:** arXiv:`2405.16880` (v1, 2024-05-27) — arXiv
  title-match confirms same author list; use the arXiv ID if the ACM paywall bites
- **Venue:** *ACM Transactions on Software Engineering and Methodology* **34**(2) (2025), art. 1, pp. 1–31 · Crossref title-match; abstract from the Crossref record
- **Framing for ADR-0057:** the current *state of the field*. 105 papers
  organized into participation motivation / contribution model / impact on OSS
  development. Use it to position the premium-layer contribution model as one
  recognized entry in a taxonomy rather than a novel arrangement — which is
  the honest framing and is also the strongest defence of ADR-0057.

### B7. Resolving governance disputes in communities: A study of software license decisions
- **Authors:** Vivianna Fang He, Phanish Puranam, Yash Raj Shrestha, Georg von Krogh
- **DOI:** `10.1002/smj.3181` · https://doi.org/10.1002/smj.3181
- **Venue:** *Strategic Management Journal* **41**(10) (2020), pp. 1837–1868 · Crossref title-match; abstract from the Crossref record
- **Framing for ADR-0057:** the *mechanism* theory. GitHub data on discussions
  around **initiation and change of software licenses**; resolution is more
  likely in larger discussion groups and in projects *without* a licence, and
  the causal mechanism is reflective agency (switching disputes from bargaining
  to problem solving) plus preference alignment over attributes. This is the
  best available theory of *why* a licence change is a governance event — i.e.
  why moving Cambrian's `core/` licence, or adding a licence to a previously
  un-licensed repository, is a project-governance decision and not a packaging
  detail. von Krogh co-authors the open-source-community boundary-spanning work
  (`s0048-7333(03)00053-2`, *Research Policy* 2003), so the lineage is coherent.

### B8. Open innovation and within-industry diversification in small and medium enterprises: The case of open source software firms
- **Authors:** Massimo G. Colombo, Evila Piva, Cristina Rossi-Lamastra
- **DOI:** `10.1016/j.respol.2013.08.015` · https://doi.org/10.1016/j.respol.2013.08.015
- **Venue:** *Research Policy* **43**(5) (2014), pp. 891–902 · Crossref title-match
- **Framing for ADR-0057:** the *firm-economics* leg. OSS firms' within-industry
  diversification is the empirical result that an OSS-based firm is a genuine
  business rather than a loss leader — the premise that makes a premium module
  a defensible revenue line and a separate Go module (`app.Options` per
  ADR-0057 D13) a rational structure rather than an artificial split.
  Abstract elided by the publisher (S2 reports it as withheld); title-level
  claim only.

### Task B — BSL/Fair Source specific item (non-peer-reviewed; flagged)

**Open Source at a Crossroads: The Future of Licensing Driven by Monetization**
- **Authors:** Raula Gaikovina Kula, Brittany Anne Reid, Christoph Treude
- **arXiv:** `2503.02817` (v2) · https://arxiv.org/abs/2503.02817
  — arXiv `id_list` title-match; v2 confirmed
- **Venue claim:** arXiv comment reads **"Submitted to SE 2030 Software
  Engineering Roadmap Workshop"** — i.e. a *workshop submission*, not an
  accepted, peer-reviewed SE-venue paper. Do not cite it as ICSE/FSE/MSR.
- **Content (from arXiv abstract):** reviews three cases where OSS licences
  were modified to allow monetization, argues open source licensing is at a
  crossroads, and closes with a research agenda of research questions.
- **Framing for ADR-0057:** the closest thing to a BSL-adjacent academic
  treatment that exists, and the natural place to find the monetization-driven
  re-licensing cases. Cite as a workshop-submitted preprint, and use its
  research-agenda section as the gap statement for a *peer-reviewed* open-core
  case study — which, on this search, does not yet exist.

### Task B verified reserves (not written up; identifiers confirmed)

| Identifier | Title | Venue |
|---|---|---|
| `10.1016/j.respol.2009.06.004` | When does community participation enhance the performance of open source software companies? | *Research Policy* 38(8) 2009, pp. 1288–1299 (Stam) |
| `10.1016/j.respol.2008.11.004` | Monetary donations to an open source software platform | *Research Policy* 38(2) 2009, pp. 404–414 (Krishnamurthy, Tripathi) |
| `10.1016/j.respol.2023.104846` | Open source software and global entrepreneurship | *Research Policy* 52(9) 2023, art. 104846 (Wright, Nagle, Greenstein) |
| `10.1145/3524842.3528491` | A large-scale dataset of (open source) license text variants | MSR 2022, pp. 757–761 (Zacchiroli) |
| `10.1016/j.infsof.2019.01.001` | The current state of software license renewals in the I.T. industry | *Inf. Softw. Technol.* 108 (2019), pp. 139–152 — mechanism evidence for BSL's auto-renewal change date |
| `10.1145/3774748.3793628` | Catching License Changes Before They Cost: Leveraging LLMs to Analyze License Evolution in Open Source Software | ICSE 2026, pp. 389–399 — the 2026 SE-venue peer for B4 |
| `10.1007/s10664-018-9614-9` | Investigating whether and how software developers understand open source software licensing | *Empirical Software Engineering* 24(1) 2018, pp. 211–239 |
| `10.1093/jiplp/jpm163` | Managing commercial risks in open source software licensing | *J. Intellectual Property Law & Practice* 2(11) 2007, pp. 770–776 (Determann, Pixley, Shapiro) — the legal-scholarship leg |
| `10.9785/cri-2015-0102` | Managing Open Source Software in the Corporate Environment | *Computer Law Review International* 16(1) 2015, pp. 1–7 (Schöttle, Steger) |
| `10.1002/smj.3712` | Blood diamonds? Responses of open-source software developers to the Facebook–Cambridge Analytica scandal | *Strategic Management Journal* 46(7) 2025, pp. 1790–1827 (Zheng, Wang) — DiD on developer response to a governance failure |

---

## Search debt after this session

1. `LITERATURE_REVIEW.md` §5 can be written from B1–B8 + the arXiv preprint.
   Suggested section title: "Open-core licensing and the OSS business model"
   rather than "open-core/BSL", because the BSL half has no peer-reviewed
   literature.
2. **Still open:** the *sign* of the Research Policy 2026 stock-market effect
   (B3). Needs the PDF; no open-access copy found. Also open: the BSL-specific
   retail/adoption dataset — no academic study measures BSL adoption rates, so
   any such claim in the paper must be sourced to industry data (e.g. Fair
   Source's own adoption page) and labelled as such, not cited.
3. **Still open:** anything from Fair Source's own governance record as a
   *primary* source. `fairuse.io` and the Sentry announcement were reachable
   only as blog text; no academic treatment exists to cite instead.
4. ICLR 2026 is now **exhausted** for the orchestr* / rout* / memory /
   trajectory-methodology axes (two passes, 5 691 papers, local filter).
   Further ICLR coverage would need a different keyword family (e.g.
   `verifier`, `rubric`, `judge`), which is a separate task.
