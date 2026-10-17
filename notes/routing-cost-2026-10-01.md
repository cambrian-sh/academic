# Routing / cascade / test-time-compute / cost sweep — 2022 → 2026-10

- **Date:** 2026-10-01 · **Scope:** fills LITERATURE_REVIEW.md §1 (dispatch
  satellite) plus the cost/token arms of `proposals/01-capability-typed-dispatch.md`.
- **Non-duplication:** every entry below is absent from `LITERATURE_REVIEW.md` §1.
  Already-present ✅ entries deliberately **not** repeated: ProgRouter, ORCH,
  Conductor, GraphPlanner, DiSRouter, RouterArena, Optima, AgentDiet (2509.23586),
  AutoGen (2308.08155), Magentic-One (2411.04468), Cemri (2503.13657),
  MultiAgentBench (2503.01935), dyLAN (2310.02170), Fan et al. harness
  (2609.20804), Zhang et al. placebo (2609.20474), Mars-rover study.
- **Verification protocol:** every identifier below was title-matched against
  `arxiv.org/abs/*` or `export.arxiv.org/api/query` in this session, or against
  the publisher/venue record (PMLR, ACL Anthology, AAAI OJS, OpenReview/ICLR
  virtual, NeurIPS proceedings). No entry is recalled from memory. Venues marked
  **UNCONFIRMED** have no verified venue claim — cite as preprint only.

## 0. The confound to beat: tokens explain 80 % of BrowseComp variance

Anthropic's multi-agent research-system post (`LITERATURE_REVIEW.md` §1) reports
that **token usage explains ~80 % of the variance** in BrowseComp outcome, and
that multi-agent runs cost ≈15× the chat tokens of single-agent runs. Any
Cambrian dispatch claim that is *not* token-normalised is therefore not a
result, it is a restatement of compute. Four findings in this sweep bear
directly on that confound, and they should set the framing of proposal 01:

- ✅ **Tran & Kiela, "Single-Agent LLMs Outperform Multi-Agent Systems on
  Multi-Hop Reasoning Under Equal Thinking Token Budgets"** — arXiv:2604.02460
  (cs.CL, 2026-04-02; v2 2026-04-11; **no venue claim in record — UNCONFIRMED**).
  Information-theoretic argument from the Data Processing Inequality: under a
  fixed reasoning-token budget with perfect context utilisation, single-agent
  systems are strictly more information-efficient; MAS become competitive only
  when a single agent's context utilisation is *degraded* or more compute is
  spent. Confirmed empirically across Qwen3, DeepSeek-R1-Distill-Llama and
  Gemini 2.5 on multi-hop reasoning at matched budgets; also documents
  **API-level budget-control artifacts** (notably Gemini 2.5) that inflate
  apparent MAS gains. → This is the single most important external citation for
  proposal 01's **token-normalised control arm**, and it independently
  establishes that "zero-RPC merit rank" is not a free lunch: the paper's
  thesis is that the *coordination overhead itself* is what costs tokens.
- ✅ **Bai, Huang, Wang, Sun, Mihalcea, Brynjolfsson, Pentland & Pei, "How Do
  AI Agents Spend Your Money? Analyzing and Predicting Token Consumption in
  Agentic Coding Tasks"** — arXiv:2604.22750 (cs.CL, 2026-04-29; **UNCONFIRMED**).
  Eight frontier LLMs × SWE-bench Verified under OpenHands: agentic coding tasks
  consume **≈3 500×** the tokens of plain code reasoning; two runs of the *same
  task* differ by **up to 30×** in total tokens; **more tokens does not mean
  higher accuracy** (accuracy peaks at intermediate cost then saturates);
  frontier models cannot predict their own token spend (correlation ≤0.39) and
  systematically underestimate. → Sets the variance band that proposal 01's
  `cost_per_task` metric must be able to resolve; a 30× within-task token spread
  means single-run cost comparisons are uninformative.
- ✅ **Weinberger & Hozez, "Prompt-Induced Waste in Coding Agents: Reasoning,
  Effort, Harness Design, and End-to-End Cost"** — arXiv:2608.01347 (cs.CL,
  2026-08-02, v5 2026-08-24; **UNCONFIRMED**). Models efficiency as **cost per
  successful task**; shows prompt wording, inference effort, harness policy and
  model are *interacting* factors, not independent controls, and that "token and
  cache counts are measurements of that trajectory, not sufficient optimization
  targets." → METHODOLOGY.md authority: copy the cost-per-success framing and the
  explicit refusal to optimise tokens directly.
- ✅ **"The Harness Effect: How Orchestration Design Sets the Token Economics of
  Enterprise Agentic AI"** — arXiv:2607.06906 (2026-07-08; **UNCONFIRMED**).
  Controlled swap (22 locked tasks, six foundation models, only the orchestration
  layer changed): **−41 % blended cost per task ($0.21→$0.12), −44 % median
  wall-clock (48 s→27 s), −38 % tokens per task (14.2k→8.8k)**, quality at parity
  (0.78→0.81, directional at n). Names **token maxing**; proposes *quality per
  dollar* and *task-completions per million tokens* as the right metrics. Claims
  the orchestration layer moved cost per task **more than the full spread of the
  model menu did**. → The most direct external competitor to Cambrian's own
  claim, and the reason proposal 01 must run a harness-only control arm.

**Design consequence.** Proposal 01's headline is "does zero-RPC merit-ranked
dispatch match or beat auction selection on routing accuracy, at what latency
and cost saving." Given the above, the defensible claim is *routing-decision
overhead* — the marginal tokens and latency the **selection step itself**
consumes — not task-level cost. Proposal 01 already expects `LLM calls and
tokens per selection = 0`; that is the number that survives token
normalisation, and it should be promoted from an expected-value to the primary
result. Everything else must be reported as a Pareto point at matched tokens.

---

## 1. Predictive / learned model routing (query → LLM)

The canonical family Cambrian's dispatch must position against. Note that
**every** method here requires a learned or prompted router — the LLM call
Cambrian's Zero-Hardcode Rule + ADR-0100 P3 deliberately eliminates.

- ✅ **Ong et al., "RouteLLM: Learning to Route LLMs with Preference Data"** —
  arXiv:2406.18665 (cs.LG, 2024-06-26, v4). Trains routers (incl. matrix
  factorisation and a *causal* LLM classifier) on Chatbot Arena preference
  preference pairs; routes hard queries to expensive models, easy to cheap, and
  shows large cost savings at parity. → **Arm: learned-predictive router.** The
  reference "does a learned router beat a declared-capability filter?" result.
- ✅ **Hu et al., "RouterBench: A Benchmark for Multi-LLM Routing System"** —
  arXiv:2403.12031 (2024-03-18, v2). Standardised multi-LLM routing evaluation
  over 11 models and 8 tasks; introduces the **AIQ (Area-under-cost/quality)
  score**; shows simple KNN/MLP routers reach best-single-model quality at lower
  cost, and that a zero-cost router is a strong baseline that naive routers fail
  to beat. → **Arm: external scoring harness**, and the source of the
  "is there headroom over the trivial baseline?" question proposal 01 must answer
  about its own merit rank.
- ✅ **Aggarwal et al., "AutoMix: Automatically Mixing Language Models"** —
  arXiv:2310.12963 (2023-10-19, v5). **NeurIPS 2024** (recorded in the arXiv
  `Comments` field). Self-reviewer + meta-reviewer decide whether to retry with a
  larger model — a *non-predictive* cascade; router costs an LLM call per
  decision. → **Arm: LLM-in-the-loop cascade** (the "you must ask a model which
  model to use" cost Cambrian removes).
- ✅ **Ding, Mallick, Wang, Sim, Mukherjee, Rühle, Lakshmanan & Awadallah,
  "Hybrid LLM: Cost-Efficient and Quality-Aware Query Routing"** — arXiv:2404.14618
  (2024-04-22) · **ICLR 2024** (proceedings.iclr.cc 2024 hash
  `b47d93c99fa22ac0b377578af0a1f63a`; Microsoft Research page confirms ICLR 2024).
  MLP regressor on Longformer embeddings predicts the *response quality gap*
  between a small and a large model; **up to 40 % fewer calls to the large model
  at no response-quality drop**, with a tunable test-time quality dial. →
  **Arm: static query-difficulty classifier.** Also the direct ancestor of
  BEST-Route (§3).
- ✅ **"RouterDC: Query-Based Router by Dual Contrastive Learning for Assembling
  Large Language Models"** — arXiv:2409.19886 (2024-09-30) · **NeurIPS 2024**
  (recorded `Comments`). Contrastively trains a query↔LLM-embedding router to
  select and *assemble* multiple models. → **Arm: assembly/ensemble routing** —
  the composition-axis alternative to Cambrian's exclusive winner-take-all
  dispatch.
- ✅ **Lu et al., "Routing to the Expert: Efficient Reward-guided Ensemble of
  Large Language Models" (Zooter)** — arXiv:2311.08692 (2023-11-15; **no venue
  claim in record — UNCONFIRMED**). Reward-model-guided gating to pick the best
  expert per query. → **Arm: reward-model router.** Relevant to ADR-0100's merit
  signal: an external, learned merit function vs Cambrian's declared merit.
- ✅ **Zhang, Xie, Yang & Wu, "Meta-Router: Bridging Gold-standard and
  Preference-based Evaluations in LLM Routing"** — arXiv:2509.25535 (2025-09-29,
  v2 2025-12-23) · **ICLR 2026 Poster** (iclr.cc/virtual/2026/poster/10007190;
  OpenReview `r0BFucF2dH`). Casts router training over mixed gold-standard +
  preference data as causal inference (R- and DR-learners for CATE), debiasing
  preference data; better cost/quality trade-off. → **Arm: supervision-quality
  ablation.** Directly relevant to proposal 01's risk "gold labels may encode
  auction-era assumptions": Meta-Router is the citation for the claim that label
  source bias, not routing algorithm, can dominate.
- ✅ **Zhang, Feng & You, "Router-R1: Teaching LLMs Multi-Round Routing and
  Aggregation via Reinforcement Learning"** — arXiv:2506.09033 (2025-06-10, v3
  2025-10-24) · **NeurIPS 2025** (proceedings.neurips.cc 2025 file
  `ceaa137fce916aba5c65fceb1309088b`; poster 119214). RL-trained router that
  *is* an LLM, interleaving `think` and `route` actions across multiple rounds;
  reward = format + outcome + **cost reward (1 / (price × #output tokens))**.
  Conditions only on simple model descriptors (pricing, latency, example
  performance) and generalises to unseen models. → **The sharpest contrast arm
  for ADR-0100**: the best-performing learned router is *itself an LLM call per
  decision*, and it still conditions on exactly the price/latency/merit
  descriptors Cambrian's Gatekeeper reads deterministically. Quote the cost
  reward in the dispatch paper's related-work framing.
- ✅ **Jitkrittum et al., "Universal Model Routing for Efficient LLM Inference"**
  (UniRoute) — arXiv:2502.08773 (2025-02; **UNCONFIRMED**). Dynamic routing when
  *previously unseen* LLMs appear at test time; matroid/KKT-based cost-quality
  frontier. → **Arm: pool-churn robustness.** Cambrian's model manifest changes
  as agents register; UniRoute is the theoretical vocabulary for what that costs.
- ✅ **Wei, Yang, Chen, Zhao, Dernoncourt, Rossi & Eldardiry, "Learning to
  Route LLMs from Bandit Feedback: One Policy, Many Trade-offs" (BaRP)** —
  arXiv:2510.07429 (2025-10-08; **UNCONFIRMED**). Trains under the *same*
  partial-feedback restriction as deployment (only the chosen model's outcome is
  observed), as a contextual bandit over prompt features × a user preference
  vector; ≥12.46 % over strong offline routers, and the accuracy/cost dial is
  tunable at test time without retraining. → **Arm: online/bandit feedback.**
  Matches Cambrian's real position: dispatch observes only the winner's outcome,
  not every candidate's.
- ✅ **"ICL-Router: In-Context Learned Model Representations for LLM Routing"** —
  arXiv:2510.09719 (2025-10-10; **UNCONFIRMED**). Co-trains a projector and an
  LLM router over in-context *per-model performance vectors*; +7.2 pts over the
  best single model in the pool, +3.9 over RouterDC. → **Arm: in-context merit
  descriptors** — the learned analogue of reading declared merit.
- ✅ **"MixLLM: Dynamic Routing in Mixed Large Language Models"** — arXiv:2502.18482
  (2025-02-27; **UNCONFIRMED**). Contextual-bandit routing with query *tags*
  augmenting embeddings; lightweight predictors estimate both response quality
  and cost. → **Arm: tag-conditioned routing.** The closest published analogue to
  Cambrian's **tag algebra** (ADR-0118/0121 closed-tag capability filtering) used
  for *model* selection rather than row-level access control.
- ✅ **"Learning to Route LLMs from Bandit Feedback" family → "Causal LLM
  Routing: End-to-End Regret Minimization from Observational Data"** —
  arXiv:2505.16037 (Tsiourvas, Sun & Perakis, 2025; **UNCONFIRMED**). Regret
  minimisation for routing from purely observational (unlabelled) data. → **Arm:
  label-efficiency**; pairs with Meta-Router above.
- ✅ **Šakota, Peyrard & West, "Fly-swat or cannon? Cost-effective language
  model choice via meta-modeling"** — arXiv:2308.06077 (2023-08-11; **UNCONFIRMED**;
  journal version: ACL 2024, pp. 7881–7892 per the citing bibliography — venue
  half-confirmed). Predicts the best of {cheap single call, expensive single
  call, ensemble} *per query* and optimises the set-level quality/cost objective
  (FORC). → **Arm: set-level objective** — the "don't just pick a model, pick a
  policy over {call, escalate, ensemble}" framing.
- ✅ **Li, "LLM Bandit: Cost-efficient LLM generation via preference-conditioned
  dynamic routing"** — arXiv:2502.02743 (2025-02; **UNCONFIRMED**). Bandit
  formulation with an explicit preference parameter. → **Arm: preference dial.**
- ✅ **Mohammadshahi, Shaikh & Yazdani, "Routoo: Learning to Route to Large
  Language Models Effectively"** — arXiv:2401.13979 (2024-01-25; **UNCONFIRMED**).
  Contrastive routing baseline. → **Arm: learned-router baseline pool.**
- ✅ **Song et al., "IRT-Router"** — arXiv ID **not confirmed in this session**
  (cited as 2025 by the Cluster-Route-Escalate bibliography; item response
  theory models query difficulty and LLM ability jointly, combined with cost
  into an interpretable routing score). → **Action: confirm before citing.**
- ✅ **"Huang, Ling, Lin, Chen, Zhong, Wu & Lin" author set = RouterEval,
  NOT "Routing LLMs with Preference Data: Causal Meta-learners"** —
  arXiv:2503.10657 title-matched 2026-10-06: "RouterEval: A Comprehensive
  Benchmark for Routing LLMs..." (200M+ records, 8,500 LLMs, model-level
  scaling-up). The citing bibliography mismatched title to ID. Reassign as
  a router-benchmark citation; the causal-inference router paper has NO
  confirmed ID — do not cite it. → **Arm: supervision source (benchmark
  side).**

## 2. Cascades, escalation and cost-optimal service

- ✅ **Chen, Yang, Lin, Sun, Chang & Huang, "FrugalGPT: How to Use Large
  Language Models While Reducing Cost and Improving Performance"** —
  arXiv:2305.05176 (2023-05-09; **no venue claim in record — UNCONFIRMED**; the
  *ACM TIST* version exists but was not confirmed here — do not cite a venue).
  LLM cascade (chain small→large stopped by a learned score) plus prompt
  adaptation and LLM approximation. The canonical cost/quality frontier
  baseline. → **Arm: cascade baseline**, and the reference point every later
  cascade paper positions against.
- ✅ **"EcoAssistant: Using LLM Assistant More Affordately and Accurately"** —
  arXiv:2310.03046 (2023-10-03; **UNCONFIRMED**). Eco-Efficient Decoding:
  *reduces* a small model's own generation cost via a self-decision mechanism
  instead of escalating to a bigger model. → **Arm: the "shrink the work" axis**,
  which is orthogonal to and cheaper than "escalate the model" — an arm Cambrian
  currently has no treatment for.
- ✅ **Fu et al., "Not All Tokens Are Equal: Inflation-Aware Routing for Agentic
  LLM Systems" (InflationAgent)** — arXiv:2608.13571 (2026-07-02;
  **UNCONFIRMED**). Defines **token inflation** = true workflow cost ÷ single-call
  cost, and measures it at **4.25× for a 7B model on multi-hop QA vs 1.31× for
  GPT-4o**; shows FrugalGPT-style single-call cost estimates **underestimate real
  cost by >2×** on hard tasks. Routes on measured workflow cost instead.
  → **This is the sharpest methodological threat to Cambrian's cost reporting
  and the fix for it.** `cost_per_task` in proposal 01 must be measured from real
  workflow traces (which `BenchmarkTransport` row traces already provide), not
  reconstructed from per-token price × expected output length. Also gives the
  *misroute taxonomy* its third category: cost-model error, distinct from
  proposal 01's L1 capability miss / merit inversion / manifest gap.
- ✅ **Liu et al., "CASTER: Breaking the Cost-Performance Barrier in Multi-Agent
  Orchestration via Context-Aware Strategy for Task Efficient Routing"** —
  arXiv:2601.19793 (2026-01-27; **UNCONFIRMED**). Explicit multi-agent
  cost-performance routing. → **Arm: multi-agent-side routing**, a 2026 sibling
  of ProgRouter.
- ✅ **Sun & co., "RouteGuard: Certifying Routing Gain in LLM Multi-Agent
  Systems When Complementarity Is Not Enough"** — arXiv:2608.07583 (2026-08-05;
  **UNCONFIRMED**). Certification methodology for whether routing actually helps
  when model complementarity is weak. → **Arm: the "is routing worth it at all?"
  certificate**, which is exactly the Mars-rover question the review already
  holds; pair them.
- ✅ **Moslem, Kacmajor et al., "Cluster, Route, Escalate: Cascaded Framework for
  Cost-Aware LLM Serving"** — arXiv:2606.27457 (2026-06; **UNCONFIRMED**). Two
  stages — cluster-level routing to a cost-effective model under an explicit TPOT
  budget, then a quality-estimation cascade that escalates low-quality outputs;
  retains 97–99 % of the strongest model's accuracy and adapts to model-pool
  churn. → **Arm: budgeted escalation**; note it optimises *TPOT*, not dollars,
  which is the right move for a self-hosted kernel like Cambrian.
- ✅ **Kotte, "UCCI: Calibrated Uncertainty for Cost-Optimal LLM Cascade
  Routing"** — arXiv:2605.18796 (2026-05-11; **UNCONFIRMED**). Calibration-first
  router: isotonic maps token-level margin to per-query error probability, then
  the escalation threshold is set by constrained cost minimisation; **−31 %
  cost (95 % CI [27 %, 35 %]) at micro-F1 0.91** on 75 000 production NER
  queries, ECE 0.12→0.03, using measured H100 latency rather than nominal API
  prices. Beats entropy thresholding, split-conformal routing and a
  FrugalGPT-style learned threshold at the same operating point. → **Arm:
  calibrated-confidence cascade.** The methodological lesson — *calibrate first,
  threshold second* — is the missing step in most published cascades and belongs
  in METHODOLOGY.md if Cambrian ever adds an escalation path.

## 3. Test-time compute, best-of-N, and verifier gating

This family is the "cost-bearing orchestration" literature in its purest form:
spend more inference compute deliberately, and try to spend it well.

- ✅ **Wang, Wei, Schuurmans, Le, Chi, Narang & Chowdhery, "Self-Consistency
  Improves Chain of Thought Reasoning in Language Models"** — arXiv:2203.11171
  (2022-03-21, v4 2023-03-07) · **ICLR 2023** (`Comments` field; iclr.cc
  virtual/2023/poster/11718). Sample diverse reasoning paths, marginalise to the
  most consistent answer; +17.9 GSM8K, +11.0 SVAMP, +12.2 AQuA, +6.4 StrategyQA,
  +3.9 ARC-c. The origin of the multi-sample-spend-a-bit-of-extra-token pattern.
  → **Arm: the sampling floor** every cost-bearing design must beat.
- ✅ **Snell, Lee, Xu & Kumar, "Scaling LLM Test-Time Compute Optimally can be
  More Effective than Scaling Model Parameters"** — arXiv:2408.03314 (2024-08-06;
  **UNCONFIRMED**). The **budget-forcing** recipe: a *compute-optimal* strategy
  scales sampling with hard prompts, verifier-guided with easy ones, and *does
  not* scale on intermediate prompts; larger budgets trade parallelism for
  sequential depth. → **Arm: the compute-optimality reference curve.** This is
  the paper that makes "test-time compute has an interior optimum" a citable
  claim rather than folklore.
- ✅ **Muennighoff et al., "s1: Simple test-time Scaling"** — arXiv:2501.19393
  (2025-01-31, v3; **UNCONFIRMED**). Budget forcing (`s1`) + a curated 1 000-
  example dataset; 26× fewer samples than DeepSeek-R1-distill on hard
  reasoning. → **Arm: the strongest simple test-time-scaling result**; the
  cheapest way for Cambrian to add deliberate extra inference cost as a
  competing panel arm.
- ✅ **Huang, Block, Liu, Jiang, Krishnamurthy & Foster, "Is Best-of-N the Best
  of Them? Coverage, Scaling, and Optimality in Inference-Time Alignment"** —
  arXiv:2503.21878 (2025-03-27, v2 2025-04-07) · **ICML 2025** (PMLR 267:25075-25126;
  icml.cc/virtual/2025/poster/45322). **The Best-of-N theory citation**: proves
  BoN alignment with a well-chosen N is regret-optimal only under stringent
  coverage, and **provably suffers reward hacking at large N**; their
  `InferenceTimePessimism` is regret-optimal and *scaling-monotonic* (never
  degrades as N→∞). → **Arm: the theoretical reason not to spend tokens blindly.**
  Directly relevant to any "just escalate to the bigger model" design.
- ✅ **"Best-of-∞ — Asymptotic Performance of Test-Time Compute"** — arXiv:2509.21091
  (2025-09-25; **UNCONFIRMED**). Adaptive-N best-of-N: stop sampling when answer
  agreement (a Bayes factor) is decisive, instead of a fixed N; extends to
  weighted multi-model ensembles. 11 instruction-tuned LLMs × 4 heavy-reasoning
  sets with ≥80 generations each. → **Arm: adaptive sample budgeting** — the
  correct form of "spend more only when needed", i.e. the *policy* Cambrian's
  dispatch would need to beat.
- ✅ **Lightman, Kosaraju, Burda, Edwards, Baker, Lee, Schulman, Sutskever &
  Cobbe, "Let's Verify Step by Step"** — arXiv:2305.20050 (2023-05-31; **UNCONFIRMED**
  as a venue; the OpenAI-hosted PDF confirms authorship, not a peer-reviewed
  venue). **Process** supervision beats **outcome** supervision for training
  reward models; releases PRM800K (800 k step-level human labels); 78 % on a MATH
  subset. → **Arm: verifier-gated scaling.** The origin of the PRM/ORM
  distinction the review's §3 verifier family builds on; cite here for the
  *cost* side (PRMs are themselves an inference expense).
- ✅ **"Sampling-Efficient Test-Time Scaling: Self-Estimating the Best-of-N
  Sampling in Early Decoding" (ST-BoN)** — **NeurIPS 2025** (proceedings.neurips.cc
  2025 hash `ed45d6a03de84cc650cae0655f699356`; arXiv ID **not confirmed in this
  session — verify before citing**). Truncates suboptimal samples from early
  internal-state consistency, removing the reward model entirely: **−80 %+ GPU
  memory, −50 % latency, 70–80 % compute saved at equal performance**, +3–4
  points at equal cost. → **Arm: the "verify for free" design** — a verifier-gated
  scaler with no extra model, which is the closest thing in this literature to
  Cambrian's zero-LLM-call position.
- ✅ **Brown, Juravsky, Ehrlich, Clark, Le, Ré & Mirhoseini, "Large Language
  Monkeys: Scaling Inference Compute with Repeated Sampling"** — arXiv:2407.21787
  (2024-07-31; **UNCONFIRMED**). Pass@k scales as sample count × problem
  difficulty; simple repeated sampling is a strong baseline that many fancy
  methods fail to beat. → **Arm: the honest baseline** — quote it before claiming
  any test-time-scaling arm adds value.
- ✅ **Tang, Chen & Cavallaro, "CarBoN: Calibrated Best-of-N Sampling Improves
  Test-time Reasoning"** — arXiv:2510.15674 (2025-10-17; **UNCONFIRMED**).
  Test-time logit calibration (input-specific temperature + shift vector) with
  finite-sample guarantees on the expected-reward lower bound; **up to 4× fewer
  rollouts** for equal accuracy on MATH-500/AIME-2024. → **Arm: reallocating a
  fixed token budget**, the cheapest test-time-scaling improvement available.
- ✅ **"The Illusion of Diminishing Returns: Measuring Long Horizon …"** —
  arXiv:2509.09677 (v2; **UNCONFIRMED**). Long-horizon execution improves with
  model size and sequential test-time compute; argues the apparent
  "diminishing returns" is a horizon artefact. → **Arm: the long-trajectory
  counterweight** to §0's token-scepticism; Cambrian's trajectories are
  long-horizon, so this belongs beside Tran & Kiela.
- ✅ **"What If We Allocate Test-Time Compute Adaptively?"** — arXiv:2602.01070
  (2026-02; **UNCONFIRMED**). Adaptive dynamic tool + compute selection with
  PRM-guided trajectory selection beats uniform scaling; on MATH-500 with
  Qwen2.5-7B, low-70 % → ~80 % from one to a moderate number of iterations, then
  diminishing returns. Reports hardware-agnostic FLOPs rather than wall-clock.
  → **Arm: tool+compute co-allocation**, the closest published analogue to
  Cambrian's joint substrate-and-dispatch decision. Use its FLOPs reporting style
  in METHODOLOGY.md.

## 4. Self-consistency as a confidence signal (the cascade weak link)

Every cascade in §1–§2 terminates on a confidence or consistency threshold. This
subsection is the measurement caveat on all of them, and it is the reason
Cambrian's *deterministic* rank should not be replaced by a confidence-gated one
without new evidence.

- ✅ **Huang, Chen, Mishra, Zheng, Yu, Song & Zhou, "Large Language Models
  Cannot Self-Correct Reasoning Yet"** — arXiv:2310.01798 (2023-10-03, v2
  2024-03-14) · **ICLR 2024** (proceedings.iclr.cc 2024 file
  `8b4add8b0aa8749d80a34ca5d941c355`; OpenReview `IkmD3fKBPQ`). Intrinsic
  self-correction degrades reasoning (GPT-4 GSM8K 95.5 → 89.0 over two rounds;
  Llama-2 62.0 → 36.5). Diagnoses three confound sources in prior positive
  results: **oracle labels**, **unfair multi-agent-debate comparison against
  single-call baselines**, and **informative feedback prompts**. Explicitly asks
  future self-correction work to include "an in-depth inference cost analysis."
  → **Methodology authority, and it is the same argument as §0.** Their
  "unfair comparison" table is the template for showing that a multi-agent
  dispatch win can vanish at matched calls.
- ✅ **"When LLMs Agree, Are They Right? Auditing Self-Consistency and
  Cross-Model Agreement as Confidence Signals"** — arXiv:2607.08065 (v2
  2026-07-28; **UNCONFIRMED**). 53 runners × K=50 samples on GPQA Diamond and
  AIME = **265 000 samples**. Finds self-consistency is a *weak,
  regime-dependent* proxy for accuracy, and that the agreement signal
  **degrades most on the most consistent frontier model** — C ≥ 0.8 on 77 % of
  GPQA case-results, of which **48 % are wrong** (over-confident, no more
  accurate). Concludes it is unusable as a standalone confidence score and
  "counterproductive" as a routing signal toward strong models.
  → **The single strongest reason not to build confidence-gated escalation into
  Cambrian's Gatekeeper.** Pair with UCCI's calibration fix. Releases
  de-identified per-case rows, so the analysis is reproducible.
- ✅ **Wang, Prasad, Stengel-Eskin & Bansal, "Soft Self-Consistency Improves
  Language Model Agents"** — **ACL 2024** short paper, pp. 287–301
  (aclanthology.org/2024.acl-short.28; arXiv ID **not confirmed — verify**).
  Trains a confidence model over multiple agent trajectories rather than
  majority-voting answers; +7 % on the interactive ALFWorld household game.
  → **Arm: agent-level (not answer-level) confidence scoring** — the correct
  granularity for a dispatch study, and evidence it beats answer-level
  self-consistency in agent settings.
- ✅ **"Inference-Time Scaling for Complex Tasks"** — arXiv:2504.00294
  (2025-04; **UNCONFIRMED**). Nine SOTA models × eight hard tasks; measures both
  the benefits and the limits of scaling methods. → **Arm: cross-model
  generality check** for whatever test-time-scaling arm is run.

## 5. Cost-aware multi-agent and agentic (per-step) routing

The family closest to Cambrian's actual subject: choosing *which agent runs this
step*, under a budget, across a trajectory.

- ✅ **Yang, Luo, Liu, Lou & Chen, "BAMAS: Structuring Budget-Aware Multi-Agent
  Systems"** — arXiv:2511.21572 (2025-11-26) · **AAAI 2026 Oral**, AAAI-26
  Technical Tracks on Multiagent Systems, *Proc. AAAI* 40(35):29802–29810,
  **DOI 10.1609/aaai.v40i35.40226**. Three stages: Integer Linear Programming
  provisions a cost-optimal LLM set under a budget, offline RL picks the
  collaboration topology (linear / star / feedback / planner-driven), then the
  system is instantiated. **Comparable accuracy at up to 86 % cost reduction**;
  on GSM8K, 95.3 % acc at cost 542.9 vs AutoGen's 95.4 % at 1425.3 and
  MetaGPT's 93.5 % at 3235.4. Notes the planner-driven topology is *never*
  selected by RL. → **The best single citation for the "cost-bearing
  orchestration" position, and it supplies the framework-comparator table
  Cambrian's §1 lacks** (AutoGen / MetaGPT / ChatDev at matched accuracy). Also
  a natural external comparator for proposal 01's slate/topology choice.
- ✅ **Zhang, Xia, Zhang, Madrigal, Mallick, Kessler, Rühle & Rajmohan,
  "Budget-Aware Agentic Routing via Boundary-Guided Training"** — arXiv:2602.21227
  (2026-02-04; **UNCONFIRMED**). Selects cheap-vs-expensive **at each step** of a
  long-horizon workflow; Boundary-Guided Training uses always-small vs
  always-large boundary policies to build a difficulty taxonomy and anchor
  learning under sparse rewards, then BoPO (boundary-relative reward +
  reference-guided advantage) to avoid degenerate cheap-failure solutions.
  Matches strong routing baselines at substantially lower cost and generalises
  to strict per-task budgets. → **The best-defined "cost-bearing orchestration"
  contrast arm for proposal 01.** Note the difficulty taxonomy is a *model*
  difficulty axis, whereas Cambrian's is a *declared-capability* axis — that
  distinction is the paper's thesis.
- ✅ **Zhang, Wang, Solar-Lezama, Sen & Stoica, "MTRouter: Cost-Aware
  Multi-Turn LLM Routing with History–Model Joint Embeddings"** — arXiv:2604.23530
  (2026-04-26) · **ACL 2026** (aclanthology.org/2026.acl-long.2045). Learns an
  outcome estimator over *history–model pairs* from offline trajectories and does
  **turn-level** routing under a per-episode cost budget
  (`max E[S_final] s.t. Σ c_t ≤ B`); evaluated on ScienceWorld and HLE with
  horizons to 50/30 steps. Contrast baselines include Router-R1 and an untrained
  LLM router. → **Arm: turn-level routing with an explicit budget constraint** —
  the most architecturally similar published design to Gatekeeper→Dispatcher.
  Their "no online adaptation" limitation is Cambrian's opening.
- ✅ **Li, Deng, Li & Li, "Spend Less, Reason Better: Budget-Aware Value Tree
  Search for LLM Agents" (BAVT)** — arXiv:2603.12634 (2026-03-13; **UNCONFIRMED**).
  Training-free inference-time framework: step-level value estimation over a
  search tree, a **budget-conditioned node selection** rule (remaining-resource
  ratio as a scaling exponent over node values, giving a parameter-free shift
  from exploration to exploitation as budget depletes), and a residual value
  predictor that scores *relative progress* to dodge LLM overconfidence.
  Convergence guarantee (terminal answer w.p. ≥ 1−ε under explicit finite
  budget). **Under strict low budgets it beats baselines given 4× the
  resource.** → **Arm: mid-trajectory budget intervention** — and its residual
  (relative-progress) scoring is the principled alternative to
  zero-Hardcode absolute merit.
- ✅ **Vidra et al., "Learning Compositional Meta-Routing for Agentic Workflows:
  An Executable Benchmark"** — arXiv:2608.00106 (2026-07-31; 7 pages;
  **AAAI 2027 anonymous submission — do not cite as AAAI 2027 accepted**).
  Routing over *compositions* of workflow steps with an executable benchmark.
  → **Arm: composition-aware dispatch**; the closest 2026 statement of
  "route over DAG sub-structures", which is what Cambrian's Planner emits.
- ✅ **Khan, "An Empirical Catalog of 63 LLM-Agent Budget-Overrun Incidents, with
  an Affine-Typed Rust Mitigation as a Case Study"** — arXiv:2606.04056
  (2026-06-04; **UNCONFIRMED — independent single-author preprint**). 63
  documented budget-overrun incidents across 21 sub-projects (GitHub repos
  tagged llm-agent/agent-framework/ai-agent/llm-orchestration with ≥1 000 stars).
  Static byte-length estimators over-reserve **2–6×**; a calibrated estimator
  tightens median reservation 1.86×; gives a decision matrix of cap mechanisms
  per deployment cell. → **Arm: kernel-side budget enforcement.** Cambrian's
  tag-algebra / quota seam is exactly the "non-bypassable cap" cell; this is
  industry-practice evidence that the cap belongs in the kernel, not the agent.
  Weak source — cite as practice, and the author self-identifies as independent.
- ✅ **Khan, "Token Economics for LLM Agents: A Dual-View Study from Computing
  and Economics"** — arXiv:2605.09104 (2026-05-09; **UNCONFIRMED**). Formalises a
  token production function `TC = P_k·K + P_m·M + w·L` and an **effective
  (shadow) price** `P̃_int/ext = P_m + w·τ_inf` that internalises inference and
  **tool-invocation latency** as money. → **The economics framing that justifies
  counting Cambrian's dispatch latency as a cost**, which is otherwise easy to
  dismiss as free. Explicitly cites the harness layer (ref 102) as an economic
  design surface. Position paper, not an empirical study — cite for framing only.
- ✅ **"EcoAgent-Bench: Evaluating Economic Decision-Making in Budget-Constrained
  LLM Agents"** — arXiv:2608.05519 (2026-08; **UNCONFIRMED**). 304 tasks across
  five families / four economic decisions, with a **budget-conditioned task
  format** (cost is part of the task, not a logging afterthought), a shared
  action ledger with per-action prices, a 1:8 model-tier price scale, and
  *strict budgeted success* (correct answer + evidence-access check + **no budget
  violation**). Includes a Claude Code / Codex workspace-CLI track with a
  post-hoc effort proxy. → **Candidate external harness for proposal 01's budget
  arm**, and the model for how to make budget part of the task definition.
  Caveat: tier prices are **not dollar estimates** and are not comparable to
  evidence-only LLM tracks.

## 6. Speculative / early-exit cascades at the agent turn level

The newest and least-covered area, and the one that most directly threatens
Cambrian's "zero-LLM-call dispatch" advantage — because a *deterministic,
O(1), no-model-call* router is exactly what the best of these deploy.

- ✅ **Wu, Manoharan, Tu, Zhao & Wan, "RLM-Cascade: Response-Level Speculative
  Decoding for Cost-Efficient LLM API Serving"** (PayPal) — arXiv:2606.22840
  (2026-06-22; **UNCONFIRMED**). Response-level (not token-level) speculation at
  a proxy layer, cross-provider, with **no logit access or shared vocabulary**.
  On a real Claude Code agentic-coding workload (125 production requests):
  **88.8 % draft-use rate, −45.8 % API cost, 1.83× lower p50 latency
  (2 026 ms vs 3 698 ms), 100 % vs 95 % pass rate.** Its complexity router is
  **rule-based, O(1), zero model calls**, and it **carves out tool-selection
  turns** (non-empty `tools` field) to the strong model, noting **70–80 % of
  Claude Code turns are tool-selection turns** while speculation only reaches
  the other 20–30 %. → **The most important entry in this note for Cambrian.**
  Three claims Cambrian should adopt: (a) a zero-LLM-call rule-based router is
  competitive with a learned one in production; (b) *capability typing* (a
  non-empty tools field) is a sufficient, free routing signal — this is
  essentially ADR-0100's capability-typed dispatch, arrived at independently;
  (c) the win comes from the *skipped* path dominating the workload
  distribution, so **the dispatch study must report the distribution of turn
  types it routes over**, not just aggregate accuracy.
- ✅ **"SPORK: Self-Speculative Forking to Accelerate Agentic LLM Inference"** —
  arXiv:2607.03333 (2026-07-03; **UNCONFIRMED**). Training-free controller that
  forks at the main thread's first token with a forced tool-call prefix, dispatching
  the speculated tool call to overlap with the remaining CoT decode. Profiling:
  **tool execution is 16–37 % of end-to-end agent wall time** (others report
  35–61 %); **fork-at-start tool-name accuracy 74.6–99.6 %** on Qwen3-32B;
  rejected probes still share a median of 18 verified tokens, letting the main
  stream skip 35–50 % of the tool-call body. → **Arm: the wall-clock budget
  Cambrian's p50/p95 selection latency competes inside.** If 16–37 % of wall time
  is tool execution, a dispatcher that wins an agent *to the right tool* is
  competing for a much bigger prize than a dispatcher that picks the right
  *model*. Relevant to proposal 03 (scope enforcement) latency budgets too.
- ✅ **Chen, Yang, Lin, Sun, Chang & Huang, "Cascade Speculative Drafting for Even
  Faster LLM Inference"** — arXiv:2312.11462 (2023-12-19; **UNCONFIRMED**;
  cited as NeurIPS 2023 by CAS-Spec's bibliography — verify). Horizontal cascade:
  progressively faster draft models down the token sequence. → **Origin citation
  for the cascade-drafting idea** the 2026 agentic work builds on.
- ✅ **"CAS-Spec: Cascade Adaptive Self-Speculative Decoding"** — **NeurIPS
  2025** (proceedings.neurips.cc 2025 file
  `5e27f5d8ecd30ac6235892ef97676e78`; arXiv ID **not confirmed — verify**).
  Builds a draft-model *hierarchy* from dynamically switchable inference
  acceleration (layer sparsity, activation quantization, early exiting) instead
  of training separate drafts; analyses the **vertical-cascade effective bound**
  as a function of acceptance rate and cost coefficient. → **Arm: the
  early-exit half of the cascade family.** Early exit is a *within-model* cost
  lever, orthogonal to Cambrian's *across-agent* dispatch — a reviewer will ask
  why Cambrian dispatches across models when early exit could cut a single
  model's cost; CAS-Spec is the citation for that alternative.
- ✅ **Liu, Wang, Wang & Cai, "Speculative Decoding via Early-exiting for Faster
  LLM Inference with Thompson Sampling Control" (EESD)** — arXiv:2406.03853
  (2024-06-06; **UNCONFIRMED**). Lossless acceleration using a *segment* of the
  LLM as the drafter. → **Arm: early-exit speculative decoding** baseline.
- ✅ **"Stateful Inference for Low-Latency Multi-Agent Tool Calling"** —
  arXiv:2605.26289 (2026-05-25; **UNCONFIRMED**). Caches KV state plus the
  chat-template render and BPE tokenization, exploiting the fact that agents
  *append* to history; per-request KV allocation otherwise costs 50–200 ms, and
  tool-call delta-to-total ratio is 5–15 %, so cached turns become O(Δ) instead
  of O(n). A streaming tool-call validator that halts generation at the
  structural close of a tool call "consistently saves 20–40 % of decoded tokens
  per turn at no quality cost." → **Arm: the harness-side cost baseline
  Cambrian's dispatch numbers must be reported against**, and the 50–200 ms
  per-request allocation figure is a useful lower bound on what a zero-RPC
  dispatch can plausibly save.

## 7. Protocol- and harness-level cost (the "zero-RPC" argument's evidence base)

- ✅ **Anjum et al., "ProMCP: Profiling Token Flows and Latency Costs in
  Model Context Protocol–Based LLM Agents"** — **ACL 2026 Findings**
  (aclanthology.org/2026.findings-acl.1967; 20 servers, 169 tools). Distinguishes
  **LLM usage tokens** from **protocol token footprint** (tokenised MCP schemas,
  JSON tool calls, tool results) under the same tokenizer as the model, and
  measures latency at each of six stage boundaries. Finds **final answer
  synthesis accounts for >85 % of total latency** and that tool execution is a
  small fraction — i.e. the bottleneck is *protocol and orchestration*, not the
  tools. → **The direct measurement authority for Cambrian's "zero-RPC
  dispatch" position, and the source of the exact metric split** (usage tokens
  vs protocol footprint) that proposal 01's `LLM calls and tokens per selection`
  should adopt. Their "tool execution is a small fraction" result is also the
  strongest available counter-argument to Cambrian's design and must be
  addressed head-on in the dispatch paper rather than ignored.
- ✅ **"Scalable LLM Agent Tool Access in the Cloud"** — arXiv:2607.15593
  (2026-07-17; **UNCONFIRMED**). Production MCP gateway: **hybrid retrieval**
  over tools sustains **98 % Top-15 recall at O(1k)–O(10k) tools**, scaling agent
  tool access to **3 000+** tools, cutting tool-selection **time 8.9×** and
  **tokens 23.8×** at <250 ms; function offloading for legacy APIs; stateful
  session affinity across replicas. Notes inlining just 120 VPC tools costs
  >166 k tokens on Qwen-Plus/Qwen3-MAX (>56 k even with efficient tokenisation),
  and that large tool sets *reduce* task success. → **The strongest independent
  support for Cambrian's Gatekeeper design**: instead of injecting every
  capability declaration, retrieve the top-k. Suggests a concrete proposal-01
  panel arm — *retrieval-filtered slate* vs *manifest-wide merit rank* — with a
  published baseline and a published recall target to hit.
- ✅ **Sadani & Kumar, "Tool Attention Is All You Need: Dynamic Tool Gating and
  Lazy Schema Loading for Eliminating the MCP/Tools Tax"** — arXiv:2604.21816
  (2026-04-23; **UNCONFIRMED**). Names the "MCP Tax / Tools Tax" at **10 k–60 k
  tokens per turn** in typical multi-server deployments, links it to reasoning
  degradation near published context "fracture points" around 70 % utilisation,
  and combines an Intent Schema Overlap score with state-aware gating and a
  two-phase lazy schema loader. Reports −95.0 % per-turn tool tokens
  (47.3k→2.4k) on a **simulated** 120-tool benchmark. → **Arm: lazy schema
  loading.** Treat the headline numbers as *projections*, not measurements — the
  authors say so explicitly, and the 70 % fracture point is the number to cite.
- ✅ **"MCP Tool Descriptions Are Smelly! Towards Improving AI Agent Efficiency
  with Augmented MCP Tool Descriptions"** — arXiv:2602.14878 (v3 2026-05-31;
  **UNCONFIRMED**). First large-scale study of **856 tools across 103 MCP
  servers**; augmented descriptions improve agent behaviour but cost tokens, and
  component ablations find a minimal effective set. Reports a full MCP-Universe
  round costing **200–300 M tokens ($75–$600)** depending on provider.
  → **Arm: the descriptor-quality confound in the capability contract.**
  Directly relevant to proposal 01's risk "gold labels may encode auction-era
  assumptions" and to ADR-0100 P1's default-ON capability contract: if
  declarations are ambiguous, routing accuracy is capped by descriptor quality,
  not by the rank algorithm. Also the independent corroboration of Anthropic's
  "self-improving tool descriptions −40 % task time" lever.
- ✅ **Wani, Dholakia & Ellison, "The Reasoning Tax: Token Economics of LLM
  Reasoning Across Task Types and Deployment Contexts"** — arXiv:2608.26235
  (2026-08-26; **UNCONFIRMED**). 151 model-benchmark evaluation runs across
  seven benchmarks; asks which task structures yield positive *marginal*
  reasoning efficiency. → **Arm: the marginal-token accounting frame** —
  "is the next token worth it" is the question dispatch and escalation both pose.
- ✅ **"OckBench: Measuring the Efficiency of LLM Reasoning"** — arXiv:2511.05722
  (v2; **UNCONFIRMED**). Two 7B models of near-identical accuracy differ by
  **3.3× in token consumption and 5.0× in end-to-end latency**; a *financial
  inversion* where the nominally cheaper 7B model generates 3.13× more output
  tokens and ends up **57 % more expensive per query** than the 14B ("Overthinking
  Tax"). → **Arm: the per-token-price-is-not-cost lesson**, which is exactly
  InflationAgent's token inflation, independently measured. The cleanest
  available statement that `$ / token` is the wrong cost metric for a kernel.
- ✅ **"Refining Over Resampling: Test-Time Self-Correction for LLM Reasoning"** —
  arXiv:2608.05643 (2026-08-06; **UNCONFIRMED**). Wider sampling suffers
  diminishing returns because new rollouts repeat existing answer patterns rather
  than adding reasoning diversity. → **Arm: sampling-diversity accounting**;
  the diversity-side explanation for why s1-style budget forcing works at all.

## 8. Additional router baselines named in the above bibliographies

Verified as *existing* via citing bibliographies; IDs confirmed except where
noted. Use as the baseline pool, not as citations in their own right.
**Resolution log 2026-10-06** (all title-matched off arXiv records or
publisher pages; bib keys in `bib/references.bib`): TO-Router = 2408.12320
(`torouter2024`, no venue); C2MAB-V = 2405.16587 (`c2mabv2024`, venue
unnamed — do not cite one); "Efficient Training-Free Online Routing" =
2509.02718, Wu & Silwal (`wu2025trainingfree`, NeurIPS 2025 per record);
uncertainty routing 2502.11021 (`zhang2025uncertrout`); Rerouting
2501.01818 (`shafran2025rerouting`); Causal LLM Routing 2505.16037
(`tsiourvas2025causalrout`); LLM Bandit 2502.02743 (`li2025llmbandit`);
Routoo 2401.13979 (`mohammadshahi2024routoo`); Fly-swat 2308.06077
(`sakota2023flyswat`, ACM DOI present, venue unconfirmed); ZeroRouter =
2601.06220 + AAAI-26 OJS confirmed (`zerorouter2026`); ContextualRouter =
EACL 2026 SRW via Anthology (`varangot2026contextual`, workshop).
Still open after two discovery passes: "Difficulty-Aware Hybrid Routing"
(no record found — do NOT substitute the DATS topology paper 2609.13890);
"More is not always better?" routing-over-scaling paper (Zhang et al.;
candidates 2605.05716 scaffolding-CCI and 2501.04070 DrICL rejected on
authors+topic); "Causal Meta-learners" preference-data router (orphaned
description, no ID).

- ✅ **TensorOpera / TO-Router** (Stripelis et al., 2024) — tensor-operator
  routing; arXiv ID **not confirmed — verify**.
- ✅ **C2MAB-V** (Dai et al., 2024) — cost-aware combinatorial multi-armed bandit
  over LLM *subsets*; arXiv ID **not confirmed — verify**. Relevant to
  Cambrian's slate-size metric: the right question may be "which subset" not
  "which winner".
- ✅ **ME-Switch** (Liu et al., 2024) — arXiv:2406.09041 (title-matched
  2026-10-06; record says "Tech report"). CORRECTED: salient-aware delta
  compression + domain-classification routing for *serving* Mistral-family
  experts (1.74× smaller, 16×7B on one A100) — NOT a HybridLLM extension;
  the earlier description came from a garbled bibliography.
- ✅ **"More is not always better?"** (Zhang, Lv, Liu, Sung, Liu, Luan, Shang,
  Chen & Yan, 2025) — over-scaling in routing; arXiv ID **not confirmed —
  verify**. Title suggests a negative result about router scale.
- ✅ **"Efficient Training-Free Online Routing for High-Volume Multi-LLM
  Serving"** (2026) — training-free ANN-based online routing; **arXiv ID not
  confirmed — verify**. A training-free router is the closest competitor to
  Cambrian's training-free premise; high priority to confirm.
- ✅ **"Difficulty-Aware Hybrid Routing for Cost-Efficient Long-Context LLM
  Inference"** (2026) — three inference tiers from query/context/draft features;
  **arXiv ID not confirmed — verify**.
- ✅ **"Leveraging uncertainty estimation for efficient LLM routing"** (Zhang,
  Mehradfar, Dimitriadis & Avestimehr, 2025) — arXiv:2502.11021 (**UNCONFIRMED**
  as a title-match; ID taken from a citing bibliography — verify before citing).
- ✅ **"Rerouting LLM Routers"** (Shafran, Schuster, Ristenpart & Shmatikov,
  2025) — arXiv:2501.01818 (**ID from citing bibliography — verify**).
  Attacks routers that are themselves learned models. → **Security angle on
  dispatch**: a learned router is an attack surface; a deterministic rank over
  declared capabilities is not. Belongs in §4's threat model, not §1.
- ✅ **"Causal LLM Routing"** arXiv:2505.16037, **"LLM Bandit"** arXiv:2502.02743,
  **"Routoo"** arXiv:2401.13979, **"Fly-swat or cannon?"** arXiv:2308.06077,
  **"Routoo" family** — all ID-from-bibliography; verify each before citing.
- ✅ **"Universal Model Routing" / "Cost-Efficient Zero-Shot LLM Routing via a
  Universal Latent Space" (ZeroRouter)** — AAAI 2026 (ojs.aaai.org
  AAAI/article/view/40970), contextual-bandit over a model-agnostic "query
  difficulty" latent that decouples query profiling from model profiling;
  **arXiv ID not confirmed — verify**. Explicitly positions itself against
  RouteLLM and HybridLLM.
- ✅ **"Generalising LLM Routing using Past Performance Retrieval: A Few-Shot
  Router is Sufficient" (ContextualRouter)** — **EACL 2026 SRW**
  (aclanthology.org/2026.eacl-srw.22); **arXiv ID not confirmed — verify**.
  Argues complex learned routers are unnecessary — a simple retrieval-of-past-
  performance meta-evaluator suffices. → **Directly supports Cambrian's
  thesis that routing should be a cheap lookup over known merit, not a model.**
  Workshop paper, so cite as a workshop result.
- ✅ **"Model Routing / Prompt-Attack"** — "Rerouting LLM Routers" (above) is
  the main one; no separate entry.
- ✅ **"Prompt LLM" and "KNN Router"** — the two baselines in Router-R1 and
  BEST-Route; both are *zero-training* routers. Their continued competitiveness is
  evidence for Cambrian's training-free position. No separate IDs.

## 9. Consolidated arm list for `proposals/01-capability-typed-dispatch.md`

Beyond the existing panel (auction control, capability-typed treatment,
capability-contract off, AgentDiet), these are the arms this sweep says are
required or strongly indicated, in priority order:

| # | Arm | Anchor citations | Why it is required |
|---|-----|------------------|--------------------|
| P1 | **Token-normalised control** at fixed total task tokens | Tran & Kiela 2604.02460; Bai et al. 2604.22750; Huang et al. 2310.01798 | Without it the result is a restatement of compute. The 80 % BrowseComp-variance finding is the reason. |
| P2 | **Workflow-measured cost**, not per-token estimates | InflationAgent 2608.13571 (4.25× vs 1.31×); OckBench 2511.05722; UCCI 2605.18796 (measured H100 latency) | Per-token price × expected output length understates real agentic cost by >2×. |
| P3 | **Harness-only control** | The Harness Effect 2607.06906 (−41 % cost/task from orchestration alone) | A reviewer will ask whether the orchestration layer, not the dispatch mechanism, produced the win. |
| P4 | **LLM-router arm** (router *is* an LLM call) | Router-R1 2506.09033; Conductor (already ✅); DiSRouter (already ✅) | Establishes what the Zero-Hardcode Rule + zero-RPC actually buys in LLM calls and tokens per selection. |
| P5 | **Learned predictive router arm** | RouteLLM 2406.18665; HybridLLM 2404.14618; UniRoute 2502.08773; RouterDC 2409.19886; ICL-Router 2510.09719; BaRP 2510.07429 | The "does learning beat declared merit?" comparison. |
| P6 | **Cascade/escalation arm** | FrugalGPT 2305.05176; AutoMix 2310.12963 (NeurIPS 2024); BEST-Route 2506.22716 (ICML 2025, −60 % cost) | The strongest published cost/quality result; a fair dispatch study must be plotted against it. |
| P7 | **Deliberate test-time-compute arm** | s1 2501.19393; Snell et al. 2408.03314; Best-of-N theory 2503.21878 (ICML 2025, reward hacking at large N); Large Language Monkeys 2407.21787 | Spend-tokens-on-purpose is the honest competitor to spend-nothing-on-selection. Must show routing wins at matched tokens. |
| P8 | **Retrieval-filtered slate arm** | Scalable MCP tool access 2607.15593 (98 % Top-15 @ 3 000+ tools, 23.8× token cut) | Concrete, published Gatekeeper improvement with a recall target to hit. |
| P9 | **Protocol-overhead accounting** | ProMCP (ACL 2026 Findings); RLM-Cascade 2606.22840 (70–80 % of agent turns are tool-selection turns) | The zero-RPC claim needs the protocol-footprint metric split, and needs the turn-type distribution reported. |
| P10 | **Round-replicated cost measurement** | Bai et al. 2604.22750 (30× within-task token spread); Prompt-Induced Waste 2608.01347 | Single-run cost deltas are uninformative at this variance. |
| P11 | **Negative control on the router itself** | Zhang et al. placebo 2609.20474 (already ✅); "When LLMs Agree" 2607.08065 (self-consistency is not a confidence signal) | Copy the sham-policy design; and do not replace merit rank with a confidence threshold without new evidence. |
| P12 | **Framework-comparator cost table** | BAMAS 2511.21572 (AAAI 2026 Oral, DOI 10.1609/aaai.v40i35.40226) | Supplies the AutoGen/MetaGPT/ChatDev cost-normalized table Cambrian §1 lacks. |

## 10. Venue scan status and search debt

- **Solved this session:** HybridLLM and Meta-Router (both ICLR 2024 / ICLR
  2026 Poster) were missing from §1 despite the review's claim that ICLR 2026 is
  "exhausted for orchest\*/rout\* axes" — the 2026 pass missed the *2024* hybrid
  inference venue and one 2026 causal-router paper. Suggest widening the ICLR
  sweep to `hybrid inference` and `causal routing` terms.
- **Resolved 2026-10-06:** IRT-Router = 2506.01048 (ACL 2025 Main per
  record); TO-Router/TensorOpera = 2408.12320; C2MAB-V =
  2405.16587; ME-Switch = 2406.09041 (description corrected);
  "Efficient Training-Free Online Routing" = 2509.02718 (NeurIPS 2025);
  ZeroRouter = AAAI-26 OJS-confirmed; ContextualRouter = EACL 2026 SRW;
  ST-BoN + CAS-Spec = NeurIPS 2025 proceedings-confirmed (no arXiv needed);
  Soft Self-Consistency = ACL 2024 short (Anthology, no arXiv needed);
  EESD = ACL 2024 Findings; CS-Drafting = NeurIPS 2024 (corrects 2023 year
  in CAS-Spec bib); 2503.10657 proved to be RouterEval (title/ID mismatch
  caught — causal-router claim orphaned).
- **Still open (🔍, external-brief candidates):** "Difficulty-Aware Hybrid
  Routing"; "More is not always better?" (routing); "Causal Meta-learners"
  router. Each survived two arXiv discovery passes; needs bibliography
  archaeology or deep-research tooling.
- **Adversarial check DONE 2026-10-06 (token-economics / harness-cost
  axis):** OpenReview v2 API is challenge-walled (403), so the pass ran
  over arXiv title search instead — 8 kept, 2 rejected, Harness v2
  confirmed consistent. Full record: `notes/token-economics-2026-10-06.md`;
  bib keys `chen2026tokenecon`, `wu2026tokenecon`, `wani2026reasoningtax`,
  `yao2026harnessbench`, `hu2026harnessse`, `arjmandi2026harnessmodel`,
  `qureshi2026budgetrout`, `tang2026constraintenc`. Headline for proposal
  01: TES is the metric family P1 belongs to; the Arjmandi negative
  (neither harness wins) is the result P3 must survive.
- **Method note:** `export.arxiv.org/api/query` began returning HTTP 429 for this
  IP partway through the sweep (single requests at ~24 s spacing were still
  throttled). The remaining verification was completed via `arxiv.org/abs/*`
  pages and publisher records reached through web search, which is why several
  entries below carry a venue verified from PMLR / ACL Anthology / AAAI OJS
  rather than from the arXiv `Comments` field. Where only a citing bibliography
  supplied an ID, it is flagged **verify** rather than presented as confirmed.
