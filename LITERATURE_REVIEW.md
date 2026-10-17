# Cambrian Literature Review (living document, v0.3 — 2026-09-26)

Status legend: ✅ verified (fetched this project) · 🔍 still needs DOI/arXiv
confirmation before citing · 📌 in-repo source.

v0.2 note: systematic Tavily searches run 2026-09-23 across orchestration,
GraphRAG, agent benchmarks/memory, agent security, and harness methodology.
The 🔍 backlog in v0.1 is now largely replaced by verified entries below.
Remaining debt: snowball references + 2026 proceedings check.

v0.3 note: citation-graph snowball pass completed (4 seeds, 228 refs
triaged; full tables in `notes/snowball-2026-09-26.md`). Closes v0.2
search-debt item 1. Method lessons: Semantic Scholar's reference index is
incomplete (τ²-bench's user-simulator ref, threat-survey read-side attack
refs missing — raw arXiv HTML bibliographies were necessary); mdpi.com
403s from this environment, use
`api.crossref.org/works/10.3390/fi18060326` (107 refs). Verification
protocol: arXiv-API title-match, Crossref title-match, or double-source
(S2 + seed bibliography).

## 1. Multi-agent orchestration & dispatch (→ dispatch satellite, topics 1–2)

- ✅ MDPI Future Internet 18(6):326 (2026), "LLM-Based Multi-Agent
  Orchestration: A Survey of Frameworks, Communication Protocols, and Emerging
  Patterns" — lit cutoff Mar 2026. Names exactly three allocation
  strategies: **role-based, capability-based (via A2A Agent Card skill
  descriptors), dynamic** + DSPy's compiler-optimized fourth. This is the
  survey anchor for positioning Cambrian's capability-typed dispatch
  (ADR-0100). https://www.mdpi.com/1999-5903/18/6/326
- ✅ "ProgRouter: Online Progress-Guided Orchestration for Multi-Agent LLM
  Workflows under Quality-Cost Tradeoffs" — marginal-progress routing under
  cost/deadline budgets; eval on HumanEval+, MBPP, MATH-500, ASQA.
  Contrast: cost-aware routing vs Cambrian's zero-RPC merit rank.
  https://arxiv.org/html/2608.25992v1
- ✅ "ORCH: many analyses, one merge — a deterministic multi-agent router"
  (Frontiers AI 2026) — EMA performance-tracked routing + merge; MMLU-Pro
  +19.0pp over strongest single model. Deterministic-routing baseline.
  https://www.frontiersin.org/journals/artificial-intelligence/articles/10.3389/frai.2026.1748735/full
- ✅ "Reinforcement Learning for LLM-based Multi-Agent Systems through
  Orchestration Traces" — orchestration trace = temporal event graph
  (spawn/delegate/communicate/aggregate/stop); 8 reward families; SNR
  diffusion → free-riding/pseudo-parallelism; 15 open problems (O5: when to
  stop). Cambrian's routing traces + `AuctionEventOp` funnel are a
  measurement instance of this formalism — cite it.
- ✅ Frontiers Robotics+AI (2026): single-agent vs multi-agent on 100 Mars
  rover scenarios — orchestration "a cost-bearing design choice rather than
  an assumed improvement" for short-context static tasks. Directly supports
  Cambrian's zero-RPC/zero-LLM-call selection motivation.
- ✅ Liu et al., "RCR-router: efficient role-aware context routing" —
  arXiv:2508.04903. Context-routing sibling.
- ✅ Multi-turn multi-agent orchestration vs single LLMs (GPQA-Diamond,
  IFEval, MuSR; McNemar tests) — arXiv:2509.23537.
- ✅ Fan et al., "An Empirical Study of Harness Design for Coding Agents"
  (43pp) — matched-setting harness ablations. arXiv:2609.20804
- ✅ Zhang et al., "How Do Agent Harnesses Create Value? Planning
  Information and Release Control in Stateful LLM Agents" — NOT a harness
  ablation: Fixed-plan vs word-count-matched sham-policy placebo control
  (265 trials + τ² Airline pilot). Copy the negative-control design in
  dispatch ablations. arXiv:2609.20474
- ✅ Liu et al., "SoL-Pi: Recursively Scaling Auto-Research Loops" — token
  efficiency under long unattended trajectories. arXiv:2609.20519 (no venue
  claimed)
- ✅ Cemri et al., "Why Do Multi-Agent LLM Systems Fail?" (NeurIPS 2025) —
  validated failure taxonomy; the external authority for per-row
  `failure_kind` and METHODOLOGY.md's breakdown-over-aggregates rule.
  arXiv:2503.13657
- ✅ Fourney et al., "Magentic-One" — task ledger + progress ledger with
  stall detection and re-plan trigger; closest analogue to routing traces +
  `AuctionEventOp` funnel; prior art for "when to stop".
  arXiv:2411.04468
- ✅ Liu et al., dynamic agent network (COLM 2024) — per-turn LLM-scored
  add/prune; nearest Gatekeeper design, cleanest ADR-0100 contrast
  (LLM-scored vs zero-RPC). arXiv:2310.02170
- ✅ Xiao et al., trajectory reduction ("AgentDiet") — trajectory-side cost
  reducer; required treatment arm for topic 1 (if it matches merit-ranked
  dispatch, the dispatch result is not a win). arXiv:2509.23586
- ✅ Chen et al., "Optima" (ACL Findings 2025) — joint quality/cost
  optimization; sharpest statement of cost-bearing dispatch.
  arXiv:2410.08115
- ✅ Zhu et al., "MultiAgentBench" (ACL 2025) — coordination-quality metrics
  beyond task success; feeds topic 1's metric gap. arXiv:2503.01935
- ✅ Wu et al., "AutoGen" (COLM 2024) — required framework comparator.
  arXiv:2308.08155
- ✅ Kim et al., "Towards a science of scaling agent systems" (Google,
  2026-01-28) — 180 configs: centralized +80.9% parallelizable but −39–70%
  sequential; error amplification 17.2× independent vs 4.4× centralized;
  R²=0.513 architecture predictor (87% unseen). Anchor paper for topic 1
  AND the sharpest critique of LLM-routing-scales. arXiv:2512.08296
- ✅ Cognition (Yan, 2025-06-12), "Don't Build Multi-Agents" — delegation
  must carry decision context (full traces; actions carry implicit
  decisions); critical of Swarm/AutoGen. Supports typed capability
  envelopes with receipts. https://cognition.ai/blog/dont-build-multi-agents
- ✅ Anthropic, "Building effective agents" (2024-12-19) — pattern taxonomy
  (workflows → agents); optimize tools over prompt; cite as taxonomy, not
  current architecture (superseded note points to Managed Agents).
  https://www.anthropic.com/engineering/building-effective-agents
- ✅ Anthropic, "How we built our multi-agent research system" (2026-06-13)
  — orchestrator-worker in production; +90.2% over single-agent; tokens
  explain 80% of BrowseComp variance; 15× chat-token cost; end-state eval,
  durable execution, memory-overflow patterns. Industry anchor for
  dispatch economics + harness methodology.
  https://www.anthropic.com/engineering/multi-agent-research-system
- 🔍 Gorinova et al., "Position: Coding Benchmarks Are Misaligned with
  Agentic Software Engineering" — SWE-bench Pro over Verified, Terminal-Bench
  as frontier standard, scaffold-variance and infra-noise quantification;
  snowball its bibliography next. (ID 2606.17799 seen on paper page,
  authors confirmed via API — provisionally verified.)
- 🔍 ReasoningBank (Google, Yan & Lee, 2026-04-21) — success+failure
  experience distillation; checklist→compositional-logic maturation;
  +8.3% WebArena / +4.6% SWE-Verified. Prior art for topic 10; paper ID
  unconfirmed (blog verified).
  https://research.google/blog/reasoningbank-enabling-agents-to-learn-from-experience
- ✅ Nielsen et al., "Conductor" (ICLR 2026 Poster) — RL-trained 7B
  conductor learning topologies + role placement; sharpest contrast for the
  Zero-Hardcode Rule (LLM-side learned routing vs zero-RPC merit rank).
  OpenReview 10009267, https://iclr.cc/virtual/2026/poster/10009267
- ✅ Feng et al., "GraphPlanner" (ICLR 2026 Poster) — routing as MDP over
  backbone × role conditioned on graph memory; the substrate×dispatch
  intersection paper. OpenReview 10008792
- ✅ Zheng et al., "DiSRouter" (ICLR 2026 Poster) — per-agent self-routing;
  the "no external gate" alternative Cambrian rejects. OpenReview 10010146
- ✅ Lu et al., "RouterArena" (ICLR 2026 Poster) — router-eval
  fragmentation + standard dataset/metric suite; external-harness candidate
  alongside OrchestrationBench. OpenReview 10011126
- ✅ Market-selection evidence (title-verified 2026-10-17; ADR-0100's
  external basis): MarketBench (`fradkin2026marketbench`) — six LLMs
  miscalibrated on success/token estimates; self-reports diverge from
  full-information allocation; self-assessment is *the* bottleneck for
  market-style agent coordination. Agora (`zhou2026agora`, EMNLP 2026
  Findings — venue per arXiv Comments) — confidence-calibrated auction
  improves or stays competitive under matched pools: the calibrated-bid
  steelman; our zero-RPC claim is bounded to hand-set uncalibrated bids.
- ✅ Token-normalisation cluster (all title-verified; full arm table P1–P12
  in `notes/routing-cost-2026-10-01.md` §9): Tran & Kiela single-beats-multi
  at matched thinking tokens via DPI (arXiv:2604.02460) — the confound
  authority; RLM-Cascade rule-based O(1) zero-call router carving out
  tool-selection turns, −45.8% cost on Claude Code traces (arXiv:2606.22840)
  — independently arrived capability-typing, strongest competitor;
  InflationAgent token inflation 4.25× vs 1.31×, per-token estimates understate
  >2× (arXiv:2608.13571) — measure cost from row traces; Harness Effect
  −41% cost/task from orchestration alone (arXiv:2607.06906) — mandates a
  harness-only control; BAMAS ILP+RL provisioning, 86% cut at parity with
  framework cost table (AAAI 2026 Oral, DOI 10.1609/aaai.v40i35.40226).
- ✅ Token-economics axis sweep 2026-10-06 (OpenReview v2 challenge-walled,
  ran over arXiv; full note `notes/token-economics-2026-10-06.md`): Token
  Economics survey, first of field (`chen2026tokenecon`); Trilemma agenda
  (`wu2026tokenecon`); Reasoning Tax TES metric, TPCTC 2026
  (`wani2026reasoningtax`) — P1's metric family; Harness-Bench 106 tasks /
  5,194 trajectories, report at model×harness level (`yao2026harnessbench`);
  NanoHarness component ablation, compression can hurt (`hu2026harnessse`);
  paired-contrast NEGATIVE on harness superiority + telemetry-defect
  disclosure as honest-limits model (`arjmandi2026harnessmodel`) — P3 must
  survive this; knapsack budget routing + ROUGE-saturation caution
  (`qureshi2026budgetrout`); compact-encoding NULL result supporting
  concise policy notes (`tang2026constraintenc`). Harness Effect v2
  (2026-10-05) numbers consistent, no bib change.
- ✅ Router family: RouteLLM (2406.18665), RouterBench+AIQ (2403.12031),
  AutoMix (2310.12963, NeurIPS 2024), HybridLLM (2404.14618, ICLR 2024),
  RouterDC (2409.19886, NeurIPS 2024), Zooter (2311.08692), Meta-Router
  causal debias (2509.25535, ICLR 2026), Router-R1 LLM-that-routes
  (2506.09033, NeurIPS 2025), BaRP bandit feedback (2510.07429),
  ICL-Router (2510.09719), MixLLM tag-conditioned (2502.18482).
  Baseline-pool promotions 2026-10-06 (keys in bib): IRT-Router ACL 2025
  (`irtrouter2025`); ZeroRouter AAAI 2026 (`zerorouter2026`); ContextualRouter
  EACL SRW (`varangot2026contextual`); training-free PORT, NeurIPS 2025 per
  record (`wu2025trainingfree`); Routoo (`mohammadshahi2024routoo`); LLM
  Bandit (`li2025llmbandit`); causal regret routing (`tsiourvas2025causalrout`);
  uncertainty routing (`zhang2025uncertrout`); Rerouting attack
  (`shafran2025rerouting`); Fly-swat meta-model (`sakota2023flyswat`);
  RouterEval benchmark (`routereval2025`); C2MAB-V subsets (`c2mabv2024`);
  ME-Switch serving (`meswitch2024`); TO-Router (`torouter2024`).
- ✅ Cascades: FrugalGPT (2305.05176), EcoAssistant shrink-the-work
  (2310.03046), UCCI calibrate-first (2605.18796, −31% at CI),
  Cluster-Route-Escalate TPOT budgets (2606.27457), CASTER (2601.19793),
  RouteGuard certification (2608.07583), MTRouter turn-level budgets
  (2604.23530, ACL 2026), BAVT residual-progress scoring (2603.12634),
  EcoAgent-Bench budgeted tasks (2608.05519).
- ✅ Test-time compute: self-consistency origin (2203.11171, ICLR 2023),
  Snell compute-optimality (2408.03314), s1 budget forcing (2501.19393),
  Best-of-N theory + reward hacking (2503.21878, ICML 2025), PRM/ORM
  origin (`lightman2023verify`), repeated-sampling baseline
  (`brown2024monkeys`), "When LLMs Agree" vs confidence-gating (2607.08065),
  CarBoN calibration (`tang2025carbon`).
- ✅ Protocol/token footprint: ProMCP usage-vs-protocol split, synthesis
  >85% latency (ACL 2026 Findings); Sadani Tools-Tax −95% (`sadani2026toolstax`,
  simulated — projections not measurements); scalable MCP-gateway 98%
  Top-15 @3000 tools (2607.15593) — retrieval-filtered slate arm;
  descriptor-quality confound study (`hasan2026smelly`); Reasoning Tax marginal
  efficiency (2608.26235); OckBench per-token-price inversion (`du2025ockbench`).
- ✅ Cost-spend accounting: 3500× agentic-vs-reasoning tokens, 30×
  within-task spread (2604.22750) — replicate measurements, never single
  runs; cost-per-success framing, tokens as trajectory measurements
  (`weinberger2026waste`); token shadow-pricing incl. tool latency (2605.09104,
  framing only).
- ✅ Plan-then-execute as trust boundary (2026) — all WebArena tasks fit,
  80% need no runtime LLM; untrusted content sets values/branches but
  cannot redefine the task; fix is typed auditable APIs. Planner-boundary
  argument converging with FlowSteer. arXiv:2605.14290 (no venue claimed)
- ✅ LLM-Modulo (Kambhampati et al., ICML 2024) — peer-reviewed "model
  proposes, kernel disposes"; sound external critics confer guarantees.
  PMLR 235:22895-22907. arXiv:2402.01817
- ✅ LLM-Modulo measured deltas — CoT/ReAct/Reflexion 0–0.6% vs 4.6–6×
  with critics (travel); provably non-fallacious scheduling outputs.
  arXiv:2405.20625, arXiv:2411.14484 (venues unconfirmed)
- ✅ ReWOO — plan with no tool feedback, then execute; interleaving is
  expensive structurally. arXiv:2305.18323
- ✅ API-Bank (EMNLP 2023) — Call→Retrieve+Call→Plan+Retrieve+Call ladder;
  planning pays off only at GPT-4 scale. ACL 2023.emnlp-main.187
- ✅ Self-correction negatives (TACL 2024; ICLR 2024) + ToolPRM (ACL 2026)
  — field over-evaluated; intrinsic self-correction fails; near-useless
  for JSON tool calls. Correction loop belongs in the kernel.
  arXiv:2406.01297, arXiv:2310.01798, arXiv:2510.14703
- ✅ LATS (ICML 2024) — MCTS over ReAct doubles HotPotQA; backtracking cost
  is the contrast arm for zero-RPC dispatch. PMLR 235:62138-62160
- ✅ ACEBench — isolates instruction ambiguity (`Special`); penalises
  planner under-specification. arXiv:2501.12851
- ✅ NESTFUL — nested output-to-input chaining; 28% full-sequence match
  sobering prior for deep DAGs. arXiv:2409.03797
- ✅ ComplexFuncBench — `value_error` vs `stop_early` split is the row-trace
  taxonomy needed. arXiv:2501.10132
- ✅ FuncBenchGen (ICLR 2026) — hidden function-dependency DAG as task
  primitive; closest analogue to dag-plans; GPT-5 15% at 20 calls.
  arXiv:2509.26553 (venue confirmed via record Comments v2; OpenReview
  forum ID not yet confirmed — add before submission).
- ✅ IFEval-FC — schema-prose adherence <80% even frontier models;
  "parsed the call" vs "honoured the contract" (ADR-0097).
  arXiv:2509.18420
- ✅ BFCL peer-reviewed anchor (ICML 2025, PMLR 267:48371-48392) — replaces
  the blog URL; parallel-call categories; parallel calls mode-dependent.
- ✅ ToolEyes (COLING 2025) — five-dimension split; upstream
  planning-vs-selection decomposition.

## 2. Retrieval, GraphRAG, agent memory (→ substrate flagship, topics 2–3, 6, 10–11)

- ✅ ACM "Graph Retrieval-Augmented Generation: A Survey" (doi
  10.1145/3777378) — flags the exact gap Cambrian hits: no standardized
  multifaceted benchmarks; narrow static-KG QA; calls for ground-truth
  retrieval elements, not just final answers. Use to justify row-trace
  methodology (METHODOLOGY.md).
- ✅ GraphRAG-Bench (Xiao et al., arXiv:2506.02404) + "When to Use Graphs in
  RAG" (Xiang et al., arXiv:2506.05690, ICLR'26) + repo (493★) — standardized
  index/inference/eval harness across frameworks; college-level multi-hop
  domain questions. Candidate harness to run Cambrian's KG²RAG against.
  https://github.com/GraphRAG-Bench/GraphRAG-Benchmark
- ✅ Zhang et al., "A Survey of Graph RAG for Customized LLMs"
  (arXiv:2501.13958) + Awesome-GraphRAG (2.7k★) — survey + paper pool
  (G-Retriever, LightRAG, HippoRAG, Think-on-Graph, GraphReader, RAPTOR,
  STARK). Ablation-baseline menu for the multi-signal ranking paper.
- ✅ Microsoft GraphRAG "From Local to Global" (Edge et al.,
  arXiv:2404.16130) — community-summarization baseline.
- ✅ Zhou et al., unified graph-RAG framework (PVLDB 2025) — most-cited ref
  of GraphRAG-Bench; closest harness to run KG²RAG against (topics 2–3).
  arXiv:2503.04338
- ✅ LightRAG (Guo et al., EMNLP 2024) — primary ablation baseline, not the
  Bench harness. arXiv:2410.05779
- ✅ HippoRAG (NeurIPS 2024) — KG-indexed multi-hop memory; comparator for
  topic 10. arXiv:2405.14831
- ✅ RAPTOR (ICLR 2024) — hierarchical summarization baseline.
  arXiv:2401.18059
- ✅ Gao et al., RAG survey (2023, 4136 cites) — canonical taxonomy citation
  for the substrate section. arXiv:2312.10997
- ✅ Zhang et al., "RAFT: A Stateful Retrieval-Augmented Framework for
  Troubleshooting Agents" (EMNLP 2026 Industry) — entry-level retrieval over
  case trajectories beats RAG + GraphRAG per-stage. arXiv:2609.20754
- ✅ Shu et al., "REMem" (ICLR 2026 Poster) — episodic memory as hybrid
  time-aware graph + agentic retriever; direct prior art for topic 10
  consolidation. OpenReview 10008195
- ✅ Consolidation lineage: Voyager skill library (2305.16291); ExpeL
  transferable insights, no weight updates (2308.10144, AAAI-24);
  AutoGuide conditional guidelines (2403.08978); AutoManual rule Builder
  (2405.16247, NeurIPS 2024); ChemAgent refine-on-read (`tang2025chemagent`);
  AFTER skill-transfer benchmark, 382 tasks, cross-model 73.1%
  (2606.23127) — topic 10's instrument; Skill-as-Pseudocode deterministic
  four-check gate (2605.27955, EMNLP Findings 2026); Gödel Agent
  self-modifying upper bound (2410.04444, ACL 2025); LEGOMem placement
  question (2510.04851); AgentHER hindsight relabelling (2603.21357);
  SAGE RL consolidation (2512.17102); SkillCenter quality-gate +
  grounding (2607.07676); critic "Procedural Memory Is Not All You Need"
  (2505.03434).
- ✅ Forgetting axis (previously absent): Ebbinghaus primary (DOI
  10.5214/ans.0972.7531.200408); Craik & Lockhart depth-of-processing
  refutation (1972); Murre & Dros non-replication (2015); MemoryBank
  Ebbinghaus implementation as strawman (2305.10250); THEANINE
  anti-forgetting (2406.10996, NAACL 2025); FSFM safety-motive taxonomy
  (2604.20300).
- ✅ Parametric/non-parametric: PEAK append-catastrophe (2401.17623, ICML
  2024); ECHOQA Supportive/Complementary/Conflicting/Irrelevant taxonomy
  + parametric suppression (2410.08414) — Resolution-kinds vocabulary;
  MACR explicit conflict resolution (2606.20245); Memory³ explicit memory
  cheaper than params and RAG (2407.01178, JML not JMLR); MEMIT best-case
  parametric arm (2110.11309, ICLR 2022); editing surveys (2305.13172
  EMNLP 2023; 2310.16218 CSUR); MemTree middle tier (2410.14052).
- ✅ World models: AriGraph episodic+semantic graph (2407.04363);
  DiLu rules+reflection (2309.16292, ICLR 2024); COUNTERMEM verified
  counterfactuals as Resolution home (2609.31874); Ghost in Minecraft
  lineage (`zhu2023ghost`); Soar decay ancestor via TOIS survey.
- ✅ Graph-signal CONTESTED: Selective Forgetting (2608.28978) — graph
  loses to flat baseline on LongMemEval at matched budget (Δ −0.050,
  CI [−0.085, −0.016]); mechanism: entity decomposition discards surface
  form. Must reproduce on chunking/document-qa/musique before review.
  Counter-case: AriGraph fusion claim.
- ✅ Generative Agents memory stream (UIST 2023) — flat append-only
  origin, most-cited memory paper. DOI 10.1145/3586183.3606763
- ✅ MemSim Bayesian QA generation (2409.20163) — automatic reliable QA
  from messages; answers METHODOLOGY.md's QA debt. PerLTQA semantic/
  episodic split (2402.16288); Denning locality principle (CACM 2005)
  behind tiered memory.
- ✅ Shao et al., "Misevolve" (ICLR 2026 Poster) — safety degradation after
  memory accumulation; the risk register for self-modifying substrate
  surfaces. OpenReview 10010580
- ✅ Zep (Rasmussen et al.) — temporal knowledge-graph agent memory.
  arXiv:2501.13956
- ✅ Mem0 paper (Chhikara et al.) — production long-term memory; >90% token
  cost and 91% p95 latency cut vs full context (ceiling for reflexive path).
  arXiv:2504.19413
- ✅ Letta, "Memory Blocks" (2025-05-14) — explicit context management
  blocks. https://www.letta.com/blog/memory-blocks/
- ✅ Letta research program, full sweep 2026-10-05 (2 papers + 16 tech
  reports, all archived `data/external/letta/` with SHA256SUMS; reports
  cite as company blog, never as papers — see
  `notes/letta-2026-10-05.md`). Papers: MemGPT virtual context
  management (2310.08560, Packer et al.); Sleep-time Compute offline
  pre-computation, ~5× test-time cut (2504.13171, Lin et al.). Load-bearing
  for Cambrian: (a) B05 filesystem 74.0% LoCoMo on gpt-4o-mini vs Mem0
  68.5% graph — industry leg for graph-signal skepticism next to B8;
  (b) B09 skill learning +36.8% rel TB2.0 — topic-10 baseline to beat;
  B08 use-vs-selection split (+14.1%/−6.5%) maps onto arms; (c) B10
  (θ,C) token-space formalization — topic-10 theory anchor; B14
  memory-native RL outer-loop framing for ADR-0094/0095; (d) B11
  git-versioned memory ≈ versioned Resolutions; worktree merges ≈
  conflict-tolerant memory; (e) methodology borrowings: B07 fictional
  entities + SQL ground truth + cost tracking, B06 recovery-state arms
  (−57% rel, full-history-worst), B15 trajectory normalization (~5×),
  B03 extraneous-op penalty; (f) B16 usage-vs-generation split +
  messy/clean profiles inform policy-asks A2+; B13 adversarial-audit
  pattern warns single-turn probes under-test robustness. Novelty gaps
  Letta leaves open: no policy/authorization story (policy-asks intact),
  total-cost never token-normalized (proposal-01 intact), no
  placebo discipline (sham-policy unique). Evidence rule from B05's Mem0
  dispute: vendor numbers without a runnable backfill path are not
  baselines — applies symmetrically to us.
- ✅ Anthropic, "Contextual Retrieval" (2024-09-19) — chunk context cuts
  top-20 failure 35→67% with BM25+rerank; $1.02/M tokens; negatives
  reported (summaries ≈ no gain); <200k tokens just use the prompt.
  https://www.anthropic.com/news/contextual-retrieval
- ✅ Anthropic, "Effective context engineering" (2025-09-29) — JIT
  retrieval via identifiers beats pre-inference embedding; compaction vs
  note-taking vs subagent selection rule; context-rot mechanics.
  https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- ✅ MSR GraphRAG global-search post (Edge et al.) — practice companion to
  the paper. https://www.microsoft.com/en-us/research/blog/graphrag-improving-global-search-via-dynamic-community-selection/
- ✅ Bian et al., "TRACE: Accountable Agentic Retrieval for Source Discovery"
  — training-free agentic retrieval for accountable source discovery;
  sibling to ADR-0103 evidence receipts. arXiv:2609.19897 (venue
  UNCONFIRMED — record has no Comments field; do not cite as CIKM 2026).
- ✅ Tian, Ganguly & Macdonald, "Predicting Partial Answer Quality and
  Utility in Agentic RAG" (CIKM 2026 full paper) — in-trajectory probing
  after each retrieve–reason step; independent prior art for row traces,
  cite in METHODOLOGY.md. arXiv:2609.16453
- ✅ Mem0 "State of AI Agent Memory 2026" + originals: LoCoMo (Maharana et
  al., ACL 2024, pp. 13851–13870, DOI 10.18653/v1/2024.acl-long.747,
  arXiv:2402.17753)
  500 instances, knowledge updates + abstention). Temporal abstraction and
  cross-session structure as the drivers (+29.6 temporal, +23.1 multi-hop).
  Directly relevant to topics 10 (episodic→procedural, LoCoMo suite already
  wired) and 3. https://mem0.ai/blog/state-of-ai-agent-memory-2026
- 🔍 HippoRAG / LightRAG / RAPTOR originals — confirm IDs when selected as
  baselines.

## 3. Benchmarks & evaluation methodology (→ METHODOLOGY.md, topics 1, 5)

- ✅ GAIA (Mialon et al., 2023, arXiv:2311.12983; 466 tasks, quasi-exact-match
  scoring, humans ~92%) — Cambrian `gaia-tool-use` suite target.
- ✅ Segato/Anthropic, infrastructure noise in agentic coding evals
  (2026-02-05) — uncapped beats 1× by 6pp (p<0.01); gaps <3pp deserve
  skepticism; pin resource envelope + time-of-day. METHODOLOGY.md rule.
  https://www.anthropic.com/engineering/infrastructure-noise
- ✅ Tessl Tiles coding-agent eval (2025-11-12) — +35% abstraction adherence,
  +50% on new packages, 20% faster than source access; supports
  contract-typed tool surfaces. https://tessl.io/blog/proposed-evaluation-framework-for-coding-agents
- ✅ Anthropic harness posts (Young 2025-11; Rajasekaran 2026-03) —
  initializer/feature-list discipline; "context anxiety" (resets beat
  compaction); generator/evaluator $200/6hr vs $9/20min; harness
  assumptions go stale per model bump. Re-audit rule for kernel design.
- ✅ Anthropic Managed Agents (2026-04) — hexagonal runtime from ops pain;
  session ≠ context window (`getEvents` recoverable object); credential
  isolation after a prompt-injection token theft; p50 TTFT −60%.
  Justification for the Substrate/Gatekeeper split + event log as record.
  https://www.anthropic.com/engineering/managed-agents
- ✅ Anthropic tool-writing with agents (2025-09) — consolidate tools,
  25k-token caps, human IDs cut hallucination, ResponseFormat enum
  206→72 tokens; Claude-optimized tools beat expert-written on held-out.
- ✅ Sierra τ-bench post — pass^k adoption context (already in §3 via paper).
  https://sierra.ai/blog/tau-bench-shaping-development-evaluation-agents
- ✅ Gaia2 / ARE framework (ICLR 2026) — async event-driven reimplementation
  of τ-bench, τ²-bench, GAIA, BFCL-v3; action-level verifier 0.98 agreement
  vs 0.72 LLM-judge; budget-scaling plateau (test-time compute ceiling).
  Use to argue trajectory+verifier eval over final-answer-only.
- ✅ τ²-Bench (Sierra, arXiv:2506.07982) — dual-control tool-use benchmark,
  pass^k reliability metric; SOTA still <50% pass@1. Cambrian Tau2 probes map here.
- ✅ τ-bench parent (Yao et al., ICLR 2025) — pass^k originates here
  (single-control); cite the pair for "dual-control + pass^k".
  arXiv:2406.12045
- ✅ ToolSandbox (NAACL 2025) — database-end-state scoring with milestone
  partial credit; verifier design METHODOLOGY.md should mirror.
  arXiv:2408.04682
- ✅ SWE-bench (Verified being deprecated by its creators per Codesota
  lineage 2026-04-27) + Terminal-Bench 2.1 — note deprecation risk when
  choosing code-agent evals.
- ✅ He et al., "TRAJECT-Bench" (ICLR 2026 Poster) — per-dimension
  trajectory diagnostics (selection, arguments, order); the metric menu for
  splitting row artifacts into scored sub-dimensions. OpenReview 10009304
- ✅ Kuang et al., process-level trajectory eval / EnConda-bench (ICLR 2026
  Poster) — bluntest breakdown-over-aggregates statement; methodology
  authority for first-class `failure_kind`. OpenReview 10009617
- ✅ OverclaimBench (Smyth et al., arXiv:2609.20812) — agents misreport
  coverage 80.4% of incomplete runs; motivates topics 4–5.
- ✅ WebDevJudge (ICLR 2026 Oral) — only judge study on agent-produced
  artifacts; three named failures (functional equivalence, task
  feasibility, bias) + static-vs-interactive axis. OpenReview 10010879
- ✅ Feng et al., noisy-judge statistics (ICLR 2026 Poster) — TPR/FPR from
  small human calibration, finite-sample Type-I control; puts CIs on
  judge-scored metrics. OpenReview 10008079
- ✅ TrustJudge (ICLR 2026 Poster) — measured judge failure rates (23.32%
  score-comparison, 15.22% transitivity; fixed to 14.89%/4.40%).
  OpenReview 10011516
- ✅ ResearchRubrics (ICLR 2026 Poster) — 2 500+ expert rubrics, <68%
  agent compliance; rubric non-compliance localises failure.
  OpenReview 10010639
- ✅ CROME (ICLR 2026 Poster) — causal rubric calibration without prior
  knowledge of spurious factors. OpenReview 10007414
- ✅ Rubrics-as-Rewards + FARE (ICLR 2026 Posters) — rubric aggregation
  cheap-but-reliable; step-level verifiers +14.1% over string-match.
  OpenReview 10008552, 10011226
- ✅ TED (ICLR 2026 Poster) — subgoal grading notes + judge-diagnosis loop.
  OpenReview 10008248
- ✅ OrchestrationBench (ICLR 2026) — multi-domain agentic planning + tool
  use; external harness candidate for dispatch scoring. OpenReview 10009754
- ✅ AMemGym + MemoryAgentBench (ICLR 2026) — interactive / incremental
  memory benchmarking; successor generation past LoCoMo. OpenReview
  10007039, 10010781
- ✅ Agent-eval practice (2026 guides): execution-based verification (DB
  state, test suite, pass^k), LLM-judge failure modes, trajectory vs
  final-answer vs per-turn layers — corroborates Cambrian's row-trace +
  failure-kind design; adopt per-turn labels as future work.
  ICLR 2026 now supplies citable versions: WebDevJudge (`li2026webdevjudge`,
  Oral — judge failures on agent artifacts), noisy-judge Type-I control
  (`feng2026noisyvalid`), TrustJudge rates (`wang2026trustjudge`),
  ResearchRubrics + CROME (`sharma2026researchrubrics`,
  `srivastava2026crome`), rubric/verifier rewards (`gunjal2026rubricsrewards`,
  `xu2026fare`), TED judge-diagnosis loop (`chong2026ted`).

## 4. Access control & policy for agents (→ security satellite, topics 7–9)

- ✅ "From Prompt Injections to Protocol Exploits" (arXiv:2506.23260) —
  MCP/A2A threat taxonomy incl. prompt-to-SQL, tool poisoning, rug pulls,
  cross-tool contamination. Threat-model anchor for ADR-0043 scanning +
  ADR-0085 chokepoints.
- ✅ Hou et al., MCP threat landscape (ACM TOSEM, 514 cites) — canonical
  protocol-level reference behind ADR-0043/0085. arXiv:2503.23278
- ✅ Hasan et al., MCP server audit — empirical prevalence numbers.
  arXiv:2506.13538
- ✅ Dong et al., memory injection via query-only interaction — read-channel
  attacker; threat model for topic 7 (write-path validation insufficient).
  arXiv:2503.03704
- ✅ Lee & Tiwari, "Prompt Infection" — agent→agent propagation; supports
  kernel-side enforcement. arXiv:2410.07283 (cross-seed convergence)
- ✅ Ehtesham et al., agent interop protocols survey (MCP/ACP/A2A/ANP) —
  situates the typed agent-plane contract. arXiv:2505.02279
  (cross-seed convergence)
- ✅ Zhan et al., adaptive attacks break indirect-injection defenses (NAACL
  2025 Findings) — empirical argument against Awareness-layer-only detection.
  arXiv:2503.00061 (key `zhan2025adaptive`; venue corrected 2026-10-11)
- ✅ Zou et al., PoisonedRAG (USENIX Security 2025) — write-side
  poisoning baseline the substrate must be measured against: 90% ASR with
  5 poisoned texts per target in a DB of millions; tested defenses
  insufficient. arXiv:2402.07867 (key `zou2024poisonedrag`; venue
  corrected 2026-10-06 — earlier files said 2024, wrong).
- ✅ Salami/MemCollusion (2608.01637) — individually-benign, jointly
  harmful memories; falsifies per-write validation as sufficient; threat
  unit is the coalition. Bounds ADR-0043/0105/0118 claims.
- ✅ SPORE (2607.23444) — per-user isolation bypassed via tool interface
  (egress, not store); bounds topic 8 party scoping; chokepoints necessary.
- ✅ Sleeper poisoning write→retrieve→use schema, 99.8% write / 60–89%
  steer (2605.15338) — adopt three-stage metrics for memory-guard.
- ✅ MemMorph tool-hijack via memory (2605.26154, under review) — poisoning
  memory poisons routing; missing arm in topic-1 threat model.
- ✅ CAPTURE drift-vs-poisoning disambiguation (2609.02265, NOT ICLR —
  under review) — recency+provenance insufficient; uncertainty-triggered
  clarification supports ErrCannotExpress abstain path.
- ✅ CSUR agent security/privacy survey with cases (2407.19354, CSUR 2025).
- ✅ MCPTox (AAAI 2026, vol 40(42):35811–35819, DOI
  10.1609/aaai.v40i42.40895 — venue CONFIRMED 2026-10-06 via OJS;
  official title drops "Attack") — first large tool-poisoning measurement
  on real MCP servers (45 servers/353 tools/20 agents; mean ASR 36.5%,
  o1-mini 72.8%, refusals <3%; reasoning +27.8pp ASR; IPI payloads ≈0%).
  The primary ADR-0043 lacked; tool-poisoning is NOT an IPI variant.
  arXiv:2508.14925
- ✅ "When MCP Servers Attack" — only evaluation of scanners: mcp-scan
  caught 4 poisoned servers; LLM-judge scanners bypassed by subtle text.
  Measured case against connect-time scanning. arXiv:2509.24272
- ✅ Parasites in the Toolchain (S&P 2026) — 12 230 tools/1 360 servers,
  27.2% servers exploitable; root cause named as missing context–tool
  isolation + least privilege. Strongest statement of the gap tag-algebra
  fills. arXiv:2509.06572
- ✅ MSB (ICLR 2026) — NRP security/utility scalar for topic 7; attack
  stages map onto planner→executor→ingest. OpenReview per notes record
- ✅ MCP-TDP ("When the Manual Lies") — forensic side-effect verification
  standard; "Firewall Fallacy" (guardrails raised ASR); familiarity trust
  (95.2% vs 82.8%) → re-check grants per call, never cache per
  registration. arXiv:2605.24069
- ✅ ETDI — cryptographic tool identity, immutable versioned definitions,
  runtime policy engine; defence lineage for ADR-0043. arXiv:2506.01333
- ✅ MCP-Universe / MCPVerse / MCPEval / ModelScope MCPBench — live-MCP
  harnesses; three-tier execution evaluators; 54pp provider spread (MCP ≠
  reliability); Oracle/Standard/Max-Scale slate ablation. NOTE live
  name collision: Accenture MCP-Bench (2508.20453) vs ModelScope MCPBench
  (2504.11094) — distinct bib keys. arXiv:2508.14704, 2508.16260,
  2507.12806, 2504.11094
- ✅ FC robustness (TrustNLP@NAACL 2025) — paraphrase + toolkit-expansion
  degrade; 70–90% of errors are scoring artifacts; bigger slates cost.
  arXiv:2504.00914
- ✅ LongFuncEval — 7–91% degradation curves on catalogue/response length;
  typed addressed references over concatenated schemas. arXiv:2505.10570
- ✅ OWASP LLM Top-10 (LLM01 prompt injection #1) + Agent Security Bench
  (ICLR 2025: 84% of agents vulnerable, up to 100% mixed-strategy success) —
  quantify the problem statement.
- ✅ AgentBound (FSE 2026) — enforcement at the execution boundary, not in
  agent reasoning; lead citation for "model proposes, kernel disposes".
  DOI 10.1145/3808103
- ✅ AgentSpec (ICSE 2026) — customizable runtime enforcement DSL; SE-venue
  peer for kernel-side policy. DOI 10.1145/3744916.3764546,
  arXiv:2503.18666
- ✅ Jha et al., control-flow hijacking defenses in MAS (ICLR 2026) —
  attacks the orchestration mechanism itself; the dispatch-layer threat
  model complementing protocol-level work. OpenReview 10009684
- ✅ Tool-authorization practice: "authorization, not just alignment" —
  short-lived intent certificates, deterministic policy checks, auditable
  decisions, human gates (OPA/OpenFGA/Zanzibar lineage). This is the
  industry formulation of Cambrian's tag-algebra stance (model proposes,
  kernel disposes) — cite as converging practice.
- ✅ Cursor sandboxing post — sandboxed agents stop 40% less often:
  safety as UX win, not tax. https://cursor.com/blog/agent-sandboxing
- ✅ Anthropic Managed Agents credential finding (2026-04) — prompt
  injection stealing session-spawning tokens in the coupled design; fixed
  by structural credential isolation (vault + MCP proxy). Kernel-side
  enforcement evidence.
- ✅ Praetorian indirect-injection vs supervisor agents — supervisor blind
  spot when only user messages are inspected; justifies Cambrian's
  kernel-side enforcement (never in the Awareness layer alone).
- ✅ Anthropic, "How we contain Claude across products" (2026-05-25) —
  environment-layer-first containment; 93% prompt-approval rate (approval
  fatigue kills HITL oversight); pre-trust-dialog code execution;
  user-as-injection-vector phish 24/25 (model layer cannot help);
  allowlist-as-capability-grant; custom components fail, hypervisors hold.
  Independent convergence with "kernel disposes". Archived
  `data/external/frontier/`, delta note 2026-10-10.
- ✅ Google DeepMind, Private AI Compute sealed server-side memory
  (2026-09-23) — device-held keys, enclave decryption, per-user DBs,
  public software record + independent audit. Industry's persistent-memory
  privacy answer; contrast: they solve *who can see*, we solve *what may
  be retrieved under which predicate*. Archived `data/external/frontier/`.
- ✅ He & Yu, "When AI Agents Commit: Cognitive Serializability Across Data,
  Evidence, Policy, and Authority" (22pp) — agents derive mutations from
  reads, evidence, policy, beliefs, delegated authority. Read first for the
  substrate+policy framing. arXiv:2609.20261
- 📌 Design lineage already in ADR-0091: AWS IAM vs SCPs, K8s
  NetworkPolicy/RBAC, Zanzibar, MLS clearances, SELinux cautionary tale —
  confirm originals when drafting.

## 5. Open-core licensing and the OSS business model (→ topic 13)

Retitled 2026-09-27: there is NO peer-reviewed study of BSL, Fair Source,
or open-core licensing as such (`notes/iclr2-opencore-2026-09-27.md`).
ADR-0057 must be framed as an extension of the literatures below, with the
open-core rationale argued rather than cited.

- ✅ Han et al., Redis re-licensing event study (IST 190, 2026) — CHAOSS
  metrics show contributor attrition + Valkey fork migration after the
  2024-03-20 RSALv2+SSPLv1 move (NOT BSL — fact-checked). Cambrian is the
  *prevented* version: separate premium module instead of re-licensing.
  DOI 10.1016/j.infsof.2025.107951
- ✅ Gamalielsson & Lundell, LibreOffice fork sustainability (JSS 89,
  2014) — consequence-side fork evidence BSL does not itself provide.
  DOI 10.1016/j.jss.2013.11.1077
- ✅ Yang, OSS releases and stock prices (Research Policy 55(7), 2026) —
  market-value leg; cite existence not direction (abstract unretrieved,
  sign UNVERIFIED). DOI 10.1016/j.respol.2026.105500
- ✅ Wu et al., license-usage measurement (MSR 2024) — instrument precedent:
  evidence licensing claims from consumption surfaces, not licence-file
  census. DOI 10.1145/3643991.3644900
- ✅ Shahrivar et al., commercial-OSS business-model SLR (IST 103, 2018) —
  boundary is a licensing/module-structure decision, not pricing.
  DOI 10.1016/j.infsof.2018.06.018
- ✅ Li et al., commercial participation SLR (TOSEM 34(2), 2025; preprint
  arXiv:2405.16880) — premium layer as one recognized contribution model.
  DOI 10.1145/3690632
- ✅ He et al., licence decisions as governance (SMJ 41(10), 2020) — why a
  licence change is a governance event. DOI 10.1002/smj.3181
- ✅ Colombo et al., OSS-firm economics (Research Policy 43(5), 2014) —
  OSS firm as genuine business; premium module as rational structure.
  DOI 10.1016/j.respol.2013.08.015
- 🔍 Kula, Reid & Treude, monetization-driven re-licensing (workshop
  SUBMISSION, not accepted — never cite as ICSE/FSE/MSR). Closest
  BSL-adjacent treatment; use its agenda as the gap statement.
  arXiv:2503.02817

## Search debt (updated v0.3)

1. ~~Snowball~~ DONE 2026-09-26 (`notes/snowball-2026-09-26.md`).
2. ~~Pending IDs~~ DONE 2026-09-26 (`notes/citation-confirm-2026-09-26.md`:
   Zhang 2609.20474, SoL-Pi 2609.20519, LoCoMo 2402.17753 + ACL DOI).
3. Venue scan PARTIAL: ICLR/ICSE/FSE done; second ICLR pass DONE 2026-09-27
   (`notes/iclr2-opencore-2026-09-27.md` — ICLR 2026 now exhausted for
   orchest*/rout*/memory/trajectory axes); third ICLR pass DONE 2026-09-27
   (`notes/iclr3-verifier-2026-09-27.md` — verifier/rubric/judge family;
   ICLR dup-title count corrected to ~5 468 unique; placebo axis confirmed
   dead at ICLR); CIKM 2026 list is `TBD` (re-run after 7 Nov 2026);
   EMNLP 2026 Industry deferred (Oct 2026).
4. ~~§5 open-core/BSL route~~ DONE 2026-09-27 (same notes file): no
   peer-reviewed BSL literature exists; §5 retitled, ADR-0057 framed as
   extension. Open: ResPol-2026 effect sign (needs PDF); BSL adoption
   rates (industry data only).
