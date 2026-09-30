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
- 🔍 AutoGen / CrewAI / LangGraph framework benchmarks (AIMultiple 22-framework
  comparison: LangGraph fastest, CrewAI slowest) — industry baselines; confirm
  citable versions.

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
- ✅ Shao et al., "Misevolve" (ICLR 2026 Poster) — safety degradation after
  memory accumulation; the risk register for self-modifying substrate
  surfaces. OpenReview 10010580
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
  2025) — empirical argument against Awareness-layer-only detection.
  arXiv:2503.00061
- ✅ Zou et al., PoisonedRAG (USENIX Sec 2024) — write-side poisoning
  baseline the substrate must be measured against. arXiv:2402.07867
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
- ✅ Praetorian indirect-injection vs supervisor agents — supervisor blind
  spot when only user messages are inspected; justifies Cambrian's
  kernel-side enforcement (never in the Awareness layer alone).
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
