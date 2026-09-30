# ICLR 2026 third pass — verifier / rubric / judge family — 2026-09-27

New keyword family over the ICLR 2026 accepted-paper dump, opened because
`notes/iclr2-opencore-2026-09-27.md` closed the `orchestrat*` / `rout*` /
`memory` / `trajectory` axes and explicitly named `verifier`, `rubric`,
`judge` as the next family.

Every identifier below was **read off a fetched file or page**, never recalled.
All 40 poster URLs fetched this session resolve HTTP 200 with a matching
`<title>`, and every OpenReview note ID is parsed out of the dump rather than
recalled. The single UNVERIFIED item is flagged inline (V1's benchmark size);
nothing else is.

Verification ledger:

| Source | Method | Notes |
|---|---|---|
| `iclr.cc/static/virtual/data/iclr-2026-orals-posters.json` | HTTP 200, 15 031 089 B, 5 691 records | Re-fetched this session; record shape = `{id, name, authors, decision, topic, virtualsite_url, paper_url}` |
| `iclr.cc/static/virtual/data/iclr-2026-abstracts.json` | HTTP 200, 8 118 594 B, 5 691 entries | Asserted set-equal to the orals-posters ID set before filtering |
| `iclr.cc/virtual/2026/poster/<id>` | HTTP 200 + `<title>` read-back | **40 URLs fetched, 40 / 40 returned HTTP 200 with a `<title>` matching the dump's `name`** (whitespace-normalised, punctuation-stripped fallback). Scope: the 8 reported papers, the 21 reserves, 6 duplicate-twin IDs, and 5 further candidates. Event IDs quoted *only* as negative-result examples (e.g. `10008842`, `10006872`) are dump-sourced and deliberately carry no URL. |
| `openreview.net` | **not used** | Browser-walled, as recorded in the previous session |

`OpenReview:` in each row is the note ID parsed out of the dump's
`paper_url` field (`https://openreview.net/forum?id=<ID>`), not a guessed
string. All eight are **Accept (Poster)** except V1, which is **Accept (Oral)**.

---

## Method

Local filter over all 5 691 title+abstract pairs, case-insensitive, with the
**35** event IDs already recorded in `LITERATURE_REVIEW.md` and the two prior
notes files excluded up front (the brief said "~21"; the actual recorded set is
larger — 13 from `citation-confirm-2026-09-26.md`, 8 written up + 7 reserves
+ 7 recorded-as-false-positive/negative-result from
`iclr2-opencore-2026-09-27.md`).

| Label | Pattern |
|---|---|
| `verifier` | `\bverif(?:y\|ies\|ied\|ying\|ier\|iers\|iable\|ication\|ications\|ed)\b` |
| `rubric` | `\brubrics?\b` |
| `judge` | `\bjudg(?:e\|es\|ing\|ed)\b\|\bjudg?ments?\b` |
| `proc-reward` | `\bprocess[-\s](?:reward\|verification\|supervision)\|\bprocess level\b\|\bPRMs?\b` |
| `critic` | `\bcritics?\b\|\bcritique\w*\b` |
| `audit` | `\baudit(?:s\|ed\|ing\|or\|ors\|able)\b` |

**Raw yield over the 5 656-paper non-excluded pool: 672 papers matched**
(125 title-level, 547 abstract-only). Triage to 8 was a three-stage funnel:
title-level keyword hit → title carries an eval frame (`benchmark*` /
`evaluat*` / `assess*` / `audit*` / `leaderboard` / `metric*` / `reliab*` /
`judg*`) **and** an agent frame (`agent*` / `trajector*` / `tool-use` /
`tool-integrated` / `deep research` / `web` / `code` / `software` /
`multi-agent` / `multi-turn` / `search` / `reason*`) → abstract read for
actual methodological content. The middle funnel stage yielded **89
candidates**; every one of their abstracts was read before the final 8 were
chosen. Every claim in the relevance lines below is taken from the **JSON
abstract string**, not inferred from the title.

**Two regex lessons, both learned the hard way here:**

- `\bcritic\w*` matches **"critical"** — 5 of the 125 title-level hits in a
  first pass were pure false positives on that spelling alone (`10009183`
  critical periods, `10011299` critical attention scaling, `10010238` critical
  weights, `10011329` critical instances, `10008048` critical-token). A sixth
  hit, `10011887` "mini-critics", is a *real* critic paper the sloppy pattern
  could not distinguish from those five. Use `\bcritics?\b|\bcritique\w*\b`.
- `\bjudges?\b` is **too narrow** — it silently drops "judging" and
  "judgment". Measured: the narrow pattern matched 550 papers, the correctly
  stemmed one 672, so `\bjudges?\b` throws away **18% of recall**. Stem the
  alternation explicitly.

---

## The eight

### V1. WebDevJudge: Evaluating (M)LLMs as Critiques for Web Development Quality
- **Authors:** Chunyang Li, Yilun Zheng, Xinting Huang, Tianqing Fang, Jiahao Xu, Lihui Chen, Yangqiu Song, Winston Hu
- **OpenReview:** `CCSPm6V5EF` · **Event ID:** `10010879` (duplicate twin `10010880`, `2026-Oral--24064-6e20e674`)
- **Virtual:** https://iclr.cc/virtual/2026/poster/10010879 → **HTTP 200, title matched**
- **Venue:** ICLR 2026 (**Oral**) · topic `Applications->Everything Else`
- **Cambrian:** The only paper here that tests whether an LLM judge can verify
  **agent-produced artifacts** rather than answers, using human preference
  labels over paired web implementations with structured, query-grounded
  rubrics as ground truth. It reports the three named judge failures — failure
  to recognise **functional equivalence**, failure to verify **task
  feasibility**, and residual bias — with a significant judge-vs-human gap. It
  also separates static-snapshot from live *interactive* judge evaluation,
  which is the axis `benchmarking/`'s opaque `BenchmarkTransport` sits on.
  Cite this whenever a Cambrian methods section proposes an LLM judge to score
  agent output. **The abstract states no benchmark size — treat the label count
  as UNVERIFIED from the sources read here.**

### V2. Noisy but Valid: Robust Statistical Evaluation of LLMs with Imperfect Judges
- **Authors:** Chen Feng, Minghe Shen, Ananth Balashankar, Carsten Gerner-Beuerle, Miguel Rodrigues
- **OpenReview:** `hEhxreaLdU` · **Event ID:** `10008079`
- **Virtual:** https://iclr.cc/virtual/2026/poster/10008079 → **HTTP 200, title matched**
- **Venue:** ICLR 2026 (Poster) · topic `General Machine Learning->Everything Else`
- **Cambrian:** The single most directly usable paper for treating a judge as a
  **noisy measurement instrument** rather than an oracle: it estimates the
  judge's TPR/FPR from a *small* human-labelled calibration set, applies a
  variance-corrected threshold to a *large* judge-labelled set, and **provably
  keeps finite-sample Type-I error under control despite calibration
  uncertainty**. It positions itself against Prediction-Powered Inference as a
  *diagnostic* rather than a black-box estimator, and quantifies an "Oracle
  Gap" — the cost of not knowing the judge's parameters. This is the citation
  that lets a Cambrian paper state a confidence interval around a judge-scored
  metric instead of reporting a bare number.

### V3. TrustJudge: Inconsistencies of LLM-as-a-Judge and How to Alleviate Them
- **Authors:** Yidong Wang, Yunze Song, Tingyuan Zhu, Xuanwang Zhang, Zhuohao Yu, Hao Chen, Chiyu Song, Qiufeng Wang, Zhen Wu, Xin-Yu Dai, Yue Zhang, Cunxiang Wang, Wei Ye, Shikun Zhang
- **OpenReview:** `4uPyOCeN6U` · **Event ID:** `10011516`
- **Virtual:** https://iclr.cc/virtual/2026/poster/10011516 → **HTTP 200, title matched**
- **Venue:** ICLR 2026 (Poster) · topic `None`
- **Cambrian:** Supplies the **named, measured** judge failure taxonomy to
  cite: Score-Comparison Inconsistency (lower-rated response beats a
  higher-scored one under pairwise comparison) at **23.32%**, and Pairwise
  Transitivity Inconsistency (circular `A>B>C>A` chains, `A=B=C≠A`
  contradictions) at **15.22%**, both attributed to information loss in
  discrete rating scales plus ambiguous tie handling; its fixes cut them to
  14.89% and 4.40%. Any Cambrian rubric that mixes absolute scores with
  pairwise comparisons should be checked against these two rates.

### V4. ResearchRubrics: A Benchmark of Prompts and Rubrics For Evaluating Deep Research Agents
- **Authors:** Manasi Sharma, Chen Bo Calvin Zhang, Chaithanya Bandi, Clinton Wang, Ankit Aich, Huy Nghiem, Tahseen Rabbani, Ye Htet, Brian Jang, Sumana Basu, Aishwarya Balwani, Denis Peskoff, Marcos Ayestaran, Sean Hendryx, Bradley Kenstler, Bing Liu
- **OpenReview:** `ErnvfmSX0P` · **Event ID:** `10010639`
- **Virtual:** https://iclr.cc/virtual/2026/poster/10010639 → **HTTP 200, title matched**
- **Venue:** ICLR 2026 (Poster) · topic `Deep Learning->Everything Else`
- **Cambrian:** The **rubric-calibration reference artifact** — 2 800+ hours of
  human labour producing 2 500+ expert-written fine-grained rubrics over
  domain-diverse agent prompts, plus a three-axis complexity taxonomy
  (conceptual breadth, logical nesting, exploration) and explicit
  human/model rubric-adherence protocols. Headline number worth carrying: even
  Gemini's and OpenAI's deep-research systems reach **<68% average rubric
  compliance**, driven by missed implicit context and weak reasoning over
  retrieved material — i.e. rubric non-compliance is informative about
  *where* an agent fails, which is exactly the `failure_kind` split Cambrian
  wants. Prompts, rubrics and eval code are released.

### V5. Robust Reward Modeling via Causal Rubrics
- **Authors:** Pragya Srivastava, Harman Singh, Rahul Madhavan, Gandharv Patil, Sravanti Addepalli, Arun Suggala, Rengarajan Aravamudhan, Soumya Sharma, Anirban Laha, Aravindan Raghuveer, Karthikeyan Shanmugam, Doina Precup
- **OpenReview:** `oP99JQiDYp` · **Event ID:** `10007414`
- **Virtual:** https://iclr.cc/virtual/2026/poster/10007414 → **HTTP 200, title matched**
- **Venue:** ICLR 2026 (Poster) · topic `Reinforcement Learning->Batch/Offline`
- **Cambrian:** The method for **calibrating a rubric so it is not a proxy**.
  CROME queries an oracle LLM for rubrics causally relevant to a prompt, then
  builds *Causal Augmentations* (pairs differing only along a named causal
  attribute) and *Neutral Augmentations* (tie-label pairs differing only along
  spurious attributes) — enforcing sensitivity to the former and invariance to
  the latter, and doing so **without prior knowledge of the spurious
  factors**. That is the concrete recipe for defending a judge rubric against
  the length/formatting/verbosity artefacts the abstract names as the standard
  reward-hacking failure; up to +5.3% average on RewardBench, with gains up to
  7.1% (reasoning) and 12.4% (safety).

### V6. Rubrics as Rewards: Reinforcement Learning Beyond Verifiable Domains
- **Authors:** Anisha Gunjal, Anthony Wang, Elaine Lau, Vaskar Nath, Yunzhong He, Bing Liu, Sean Hendryx
- **OpenReview:** `c1bTcrDmt4` · **Event ID:** `10008552`
- **Virtual:** https://iclr.cc/virtual/2026/poster/10008552 → **HTTP 200, title matched**
- **Venue:** ICLR 2026 (Poster) · topic `Reinforcement Learning->Deep RL`
- **Cambrian:** The bridge paper between the two families in this pass —
  it treats **instance-specific rubrics as reward signals** for on-policy RL and
  compares aggregation strategies against direct Likert LLM-judge rewards
  (up to **+31% on HealthBench, +7% on GPQA-Diamond**). Two findings matter for
  evaluation design: rubric-based reward is markedly better-aligned at **small
  judge scales**, and it **reduces performance variance across judge scales** —
  i.e. rubric aggregation is what makes a judge cheap without making it
  unreliable, which is the cost model Cambrian's harness needs.

### V7. Foundational Automatic Evaluators: Scaling Multi-Task Generative Evaluator Training for Reasoning-Centric Domains
- **Authors:** Austin Xu, Xuan-Phi Nguyen, Yilun Zhou, Chien-Sheng Wu, Caiming Xiong, Shafiq Joty
- **OpenReview:** `89Ei7PVpNl` · **Event ID:** `10011226`
- **Virtual:** https://iclr.cc/virtual/2026/poster/10011226 → **HTTP 200, title matched**
- **Venue:** ICLR 2026 (Poster) · topic `Deep Learning->Generative Models and Autoencoders`
- **Cambrian:** The strongest *verifier-as-reward-signal* infrastructure result
  here: 2.5 M samples across five evaluation task types — pairwise, **step-level**,
  reference-free and reference-based verification, single rating — producing FARE
  8B/20B (MoE) evaluators by plain iterative rejection-sampling SFT, no RL.
  Two numbers are directly usable: used **as verifiers in RL training** FARE
  improves the downstream model by **up to 14.1% over string-matching
  verifiers**, and as inference-time rerankers FARE-20B is near-oracle on MATH.
  The step-level/verification task split is the strongest published argument
  that a Cambrian row-trace scorer should be a trained evaluator, not a prompt.

### V8. Talk, Evaluate, Diagnose: User-aware Agent Evaluation with Automated Error Analysis
- **Authors:** Penny Chong, Harshavardhan Abichandani, Jiyuan SHEN, Atin Ghosh, Min Moe, Yifan Mai, Daniel Dahlmeier
- **OpenReview:** `fHsVNklKOc` · **Event ID:** `10008248`
- **Virtual:** https://iclr.cc/virtual/2026/poster/10008248 → **HTTP 200, title matched**
- **Venue:** ICLR 2026 (Poster) · topic `Applications->Language, Speech and Dialog`
- **Cambrian:** The closest agent-evaluation methodology paper in this pass,
  and the one that names Cambrian's exact problem: prior work each rolls its
  own success check (database lookups, regex match), which is precisely what
  makes a unified harness hard. TED (i) represents **subgoals as natural-language
  grading notes** scored by LLM-as-a-judge, (ii) adds turn-efficiency and
  intermediate-progress metrics, and (iii) ships an **automated error-analysis
  tool that analyses inconsistencies of the judge *and* the agents** — a
  judge-diagnosis loop, not just agent diagnosis. Feeding the identified
  remedies back into agent design moved its own metrics by 8–10%.

---

## Reserve candidates (IDs verified, not written up — held to respect the max-8 cap)

Every row below: `iclr.cc/virtual/2026/poster/<id>` → **HTTP 200 with
title matched**. Promote any of these without re-verification.

| Event ID | OpenReview | Title | Why held back |
|---|---|---|---|
| `10011292` | `2026-Oral--7668-4a64471a` | FRABench and UFEval: Unified Fine-grained Evaluation with Task and Aspect Generalization | **Oral**; 112-aspect taxonomy, 60.4k pairwise / 325k labels, unified `MLLM-as-a-Judge` that **generalises to unseen aspects**. Strongest rubric-calibration candidate after V4/V5. |
| `10011656` | `3Gy5mmyuxn` | ROC-n-reroll: How verifier imperfection affects test-time scaling | Proves Best-of-N / rejection-sampling accuracy is **exactly characterised by the verifier's ROC curve**, and that low-compute behaviour cannot predict high-compute accuracy. The theory a methods section needs before trusting a verifier-gated metric. |
| `10009965` | `MFDkLbcydi` | Taming Imperfect Process Verifiers: A Sampling Perspective on Backtracking | Names **error amplification** — small verifier errors cause catastrophic decoding failures — and gives provably more robust VGB. The process-reward-noise counterpart to V7. |
| `10011026` | `AXNRILww9c` | Incentivizing Agentic Reasoning in LLM Judges via Tool-Integrated RL (TIR-Judge) | Trains judges with a **Python executor** so they can verify constraints and compute, not just read text; +6.4% pointwise / +7.7% pairwise, 8B listwise ≈ Claude-Opus-4. Execution-based verification *for the judge*. |
| `10011573` | `4BjGVZ7Bxn` | AutoLibra: Agent Metric Induction from Open-Ended Human Feedback | Turns free-text human feedback into trajectory metrics, with **coverage** and **redundancy** meta-metrics to score the induced metric set. The automated alternative to hand-designing `failure_kind` values. |
| `10007188` | `r0L9GwlnzP` | Do LLM Agents Know How to Ground, Recover, and Assess? (SeekBench) | First **process-level** eval for search agents: groundedness / recovery / calibration from a 190-trace, 1 800-step expert-validated annotation schema. Direct prior art for per-turn labels. |
| `10008112` | `grIvSXVJ65` | Preference Leakage: A Contamination Problem in LLM-as-a-judge | Judges are biased toward **related** student models (same model / inheritance / same family). The contamination analogue of the SWE-bench-Verified deprecation risk. |
| `10010694` | `EIA1tpKYL7` | Doubly-Robust LLM-as-a-Judge: Externally Valid Estimation with Imperfect Personas | Doubly-robust combination of persona-simulated judge ratings with biased human ratings, for **external validity** of the rating population. Pairs with V2. |
| `10009560` | `QkaeTea16Y` | DeepTRACE: Auditing Deep Research AI Systems for Tracking Reliability Across Citations and Evidence | 8-dimension audit with citation/factual-support **matrices**; reports **40–80% citation accuracy across systems** and large unsupported-statement fractions. The `audit` keyword's best agent-side hit. |
| `10009605` | `QGOw6AU8Lp` | BiasScope: Towards Automated Detection of Bias in LLM-as-a-Judge Evaluation | Discovers *unknown* judge biases automatically instead of checking a predefined list; even strong judges exceed **50% error** on its harder JudgeBench-Pro. |
| `10008486` | `canA6Ef0RP` | WebArbiter: A Generative Reasoning Process Reward Model for Web Agents | States the two broken WebPRM designs (scalar = weakly grounded; checklist = brittle template matching) and fixes both; +9.1 over GPT-5 on WebPRMBench. Process-reward design for agents. |
| `10008231` | `fVTqNpny5r` | On the Shelf Life of Fine-Tuned LLM-Judges | **Future-proofing / backward-compatibility / question generalisation** of a judge as generators change — the version-drift problem for a long-lived harness. |
| `10009171` | `VAISvCsrvG` | Rethinking LLM-as-a-Judge: Representation-as-a-Judge with Small Language Models | "Semantic Capacity Asymmetry": evaluation needs far less capacity than generation, so a small model's hidden states can score nearly as well. The cost lever. |
| `10009085` | `VwNzKPqBxk` | ProfBench: Multi-Domain Rubrics requiring Professional Knowledge to Answer and Judge | 7 000+ expert response-criterion pairs; mitigates self-enhancement bias and cuts judge cost 2–3 orders of magnitude. GPT-5-high tops out at 65.9%. |
| `10010742` | `DrhWTuhtYq` | QuRL: Rubrics As Judge For Open-Ended Question Answering | Per-question rubrics mined from online sources and used as GRPO reward signals; +17.0 points. Same bridge as V6, QA-flavoured. |
| `10007351` | `pBjy4ek2QV` | Chasing the Tail: Effective Rubric-based Reward Modeling | Locates reward misspecification specifically in the **high-reward tail** — cannot separate excellent from merely great responses. Argues rubrics can use off-policy exemplars without inheriting their artefacts. |
| `10008049` | `hXNApWLBZG` | P-GenRM: Personalized Generative Reward Model with Test-time User-based Scaling | **Oral**; adaptive personas + per-scenario scoring rubrics + user prototypes. Rubric calibration under a shifting judge population. |
| `10008623` | `bJYm4v0Spr` | LFQA-E: Carefully Benchmarking Long-form QA Evaluation | Meta-evaluates **17 automatic metric methods in 5 categories** against 7 649 human pairwise comparisons and finds *"none of the existing automatic metrics perform comparably to human judgments"*. The bluntest available statement that automatic judge metrics are unvalidated — pairs with V2/V3. |
| `10007482` | `nJvgBolRcR` | ExpertLongBench: Benchmarking Language Models on Expert-Level Long-Form Generation Tasks with Structured Checklists | Domain-expert **rubrics per task** + the CLEAR framework, which derives checklists from rubric items and grounds evaluation in reference outputs. Reports that accurate checklist extraction/comparison is achievable with **open-weight models**, i.e. rubric grading can be reproducible and cheap. |
| `10007593` | `mIKqVWGjwI` | BeyondBench: Contamination-Resistant Evaluation of Reasoning in Language Models | **Deterministically verified** answers (unique or fully enumerated) plus 3-fold evaluation for statistical robustness, with three explicit contamination-resistance guarantees. The verification-integrity reference for a suite that must not be memorised. |
| `10010181` | `JfsjGmuFxz` | VerifyBench: Benchmarking Reference-based Reward Systems for Large Language Models | Argues existing reward benchmarks only test *preference comparison*, not **verification against a ground-truth reference** — the gap V7's step-level/verification evaluators occupy. |

---

## Negative results and data notes (these close search debt)

- **The dump contains 223 duplicated titles.** Every **Oral** appears
  **twice**, under two adjacent event IDs with *different* OpenReview note IDs
  and *different* synthetic `2026-Oral--NNNNN-<hash>` ids — e.g. WebDevJudge is
  both `10010879` (`CCSPm6V5EF`) and `10010880` (`2026-Oral--24064-6e20e674`);
  FRABench/UFEval is `10011292` and `10011291`; "Reliable Weak-to-Strong
  Monitoring of LLM Agents" is `10009049` and `10009050`. Six twin pairs were
  spot-checked this session (`10010880`, `10011291`, `10008050`, `10008506`,
  `10010464`, `10009050`) and all resolved HTTP 200 with matching titles.
  **Consequence:** the headline "5 691 accepted papers" in the previous
  session's ledger **over-counts unique papers by 223**; the unique-paper count
  is ~5 468. Any future pass must dedupe on normalised title before counting,
  and must dedupe against the exclusion list on *title* too — `10009050` is the
  twin of already-excluded `10009049`.
- **Title-level hit counts per label** (the diagnostic anyone re-running this
  filter needs): `verifier` **59** · `judge` **17** · `critic` **18** ·
  `rubric` **8** · `audit` **7** · `proc-reward` **7** (125 total, with
  overlaps: QuRL and ProfBench each hit both `rubric` and `judge`).
- **`audit` is the weakest of the six keywords for this project.** All **7**
  title-level `audit*` hits were inspected: exactly one (`10009560` DeepTRACE)
  is agent-evaluation; the other six are a LAION-400M bias census, a
  cross-LLM fairness audit, privacy-canary metagradient optimisation, a
  rank-based black-box API audit, a legal-inference audit, and privacy data
  audits. `audit*` should be dropped from future Cambrian keyword sets, or
  restricted to `audit.{0,20}(agent|trajector)`.
- **`critic` is ~60% RL actor–critic noise but not worthless.** Of 18
  title-level hits, 11 are actor–critic/PPO machinery, 2 are the WebDevJudge
  twin pair, and **5 are genuinely judge-adjacent LLM-critique work** that
  this keyword family is the only route to: Critique-RL (`10009376`),
  Critique-Coder (`10006945`), Critic–Adviser–Reviser (`10011245`), RLAC
  (`10008432`), and Critic-Exploited Reset (`10010299`). Keep it, with the
  `critique\w*` alternative, to avoid the "critical" false positives.
- **`verifier` is dominated by non-agent work.** Of the 59 title-level hits,
  the two largest clusters are formal-methods / neural-network verification
  (zonotopes, LTL, reachability, Lean, Dafny) and RLVR reward mechanics
  (`LongRLVR` `10007376`, `DeepSearch` `10010078`, `Adaptive Rollout
  Allocation` `10008842`, `Random Policy Valuation` `10006872`, `TrimR`
  `10007390`, `T1` `10007001`). Only **two** title-level *verifier* hits are
  genuinely agentic: WebArbiter (`10008486`, which also matches
  `proc-reward`) and ReVeal (`10007284`, self-verifying code agents). A
  future verifier pass must not title-screen on `verifier` alone — of the eight
  papers selected here only **V7** (`10011226`) is abstract-only for every one
  of its keywords; V1 is title-level via `critic`, V2 and V3 via `judge`, V4,
  V5 and V6 via `rubric` (V6 also via `verifier`).
- **No ICLR 2026 paper proposes a negative-control or placebo design for
  evaluation** (re-confirming the previous session's `placebo` / `negative
  control` / `sham` = 0 hits). ICLR 2026 is now exhausted for this axis too;
  do not spend a fourth pass looking for one.
- **Cross-check that mattered.** Because title-level keyword screening can
  miss Oral papers, I re-read the abstracts of 16 in-scope-looking Orals that
  the filter had *dropped*, and separately re-ran the funnel with corrected
  patterns and read the abstracts of the 8 newest funnel survivors
  (`10008623`, `10007482`, `10007593`, `10010181`, `10011957`, `10008917`,
  `10007273`, `10006979`). 14 of the 16 dropped Orals genuinely use none of the
  six keywords and are correctly out of scope for this family — including
  several that matter to *other* Cambrian axes: **"How Reliable is Language
  Model Micro-Benchmarking?"** (`10008505`, Oral — prior work suggests 10
  examples suffice; no method can rank model pairs 3.5 pp apart on MMLU-Pro,
  and ~250 examples are needed before random sampling becomes competitive),
  **"Is it Thinking or Cheating? Detecting Implicit Reward Hacking by Measuring
  Reasoning Effort"** (`10010463`, Oral — TRACE, a CoT-truncation AUC detector
  for *implicit* reward hacking, >65% gain over the strongest 72B CoT monitor
  in math), **AstaBench** (`10009971`, Oral), **OpenApps** (`10008477`, Oral —
  agent success swings 63%→4% across UI app variants), **"LLMs Get Lost In
  Multi-Turn Conversation"** (`10009146`, Oral — 39% average single→multi-turn
  drop decomposed into aptitude loss plus unreliability), **"Why DPO is a
  Misspecified Estimator"** (`10008563`, Oral), **CyberGym** (`10011727`,
  Oral), **EditBench** (`10010553`, Oral), **MC-Search** (`10010223`, Oral),
  **Reasoning as Representation** (`10010748`, Oral), **"The Coverage
  Principle"** (`10011029`, Oral), **P-GenRM** (`10008049`, Oral — *this one
  is in scope*, via `rubric`, and is in the reserves above), **FRABench/UFEval**
  (`10011292`, Oral — also in scope via `judge`, also in reserves).
  **Net:** there are real Cambrian-relevant Orals that this keyword family
  cannot reach (`10008505` above all); they belong in a future
  evaluation-reliability pass, not this one. Two in-scope Orals the cross-check
  recovered are in the reserves.
- **Nothing selected was promoted on title alone.** Each of the eight has at
  least one quoted, abstract-sourced number or named mechanism in its
  relevance line, and the one claim I could not source (WebDevJudge's
  benchmark size) is explicitly marked UNVERIFIED rather than filled in.
- `export.arxiv.org` was **not queried** this session — no arXiv identifiers
  were needed, since the ICLR dump supplies both the event ID and the
  OpenReview note ID directly. The rate-limit budget is therefore unspent.

---

## Search debt after this pass

1. **`LITERATURE_REVIEW.md` §3 (Benchmarks & evaluation methodology) can absorb
   V1–V8 directly.** Suggested grouping: V2 + V3 under "judge failure modes and
   the statistics of trusting a judge"; V4 + V5 under "rubric calibration";
   V6 + V7 under "verifiers as reward signals"; V1 + V8 as the two
   agent-evaluation methodology anchors.
2. **ICLR 2026 is now exhausted on the evaluation-methodology axis as well.**
   Three passes, 5 691 records, six keyword families
   (orchestrat*/rout*/memory/trajectory; verifier/rubric/judge/process
   reward/critic/audit). A fourth pass on this venue needs a *new* frame, not a
   new keyword — the productive remaining frame is **benchmark-reliability /
   harness-validity** (`reliab*`, `reproduc*`, `statistical power`, `sample
   size`, `confidence interval`, `micro-benchmark`, `contamination`), seeded by
   `10008505` (Oral) above, which the six keywords here cannot reach.
3. **Still open:** a peer-reviewed **negative-control / placebo** design for
   agent evaluation. Three ICLR 2026 passes found none. The only authority
   remains Zhang et al. `arXiv:2609.20474` (word-count-matched sham policy),
   already in `LITERATURE_REVIEW.md` §1. Do not expect an ICLR citation.
4. **Still open:** the unique-paper count for ICLR 2026 (~5 468 after the
   223-twin dedupe). If the review ever states a paper count, use the deduped
   figure and say so.
