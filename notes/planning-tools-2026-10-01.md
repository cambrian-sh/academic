# Planning + tool-use sweep — 2026-10-01

Targeted pass for what `LITERATURE_REVIEW.md` v0.3 §§1–3 lacks: **planner
architectures**, **self-correction loops**, **2024-2026 tool-use benchmarks**,
**MCP ecosystem + tool-poisoning primaries**, **function-calling reliability**.

Scope: 2022 → 2026-10-01. Excluded as already ✅ in the review or in
`notes/snowball-2026-09-26.md`: τ-bench/τ²-bench, BFCL-as-a-blog-URL, ToolSandbox,
APIGen-MT, ToolLLM, MetaTool, MCPMark, MCP-Bench (Accenture, arXiv:2508.20453),
Tool Decathlon, Gorilla-as-a-*model* (the Gorilla/APIBench dataset row is kept,
since the review cites Gorilla only as a model), TRAJECT-Bench, OrchestrationBench,
GAIA family, plus everything in §2 (retrieval/GraphRAG/memory).

## Verification protocol

Every identifier below was fetched and **title-matched** on its own arXiv
`/abs/` page (`axabs.py`: `h1.title` string compared against the title recorded
here). The abstract, dateline, and `Comments`/`JREF` fields quoted below were read
off that same page. Venues are taken **only** from the arXiv `Comments` /
`Journal ref` field, from PMLR, or from an ACL Anthology / proceedings page.
Where a work is widely cited to a venue but the arXiv record carries no venue
field, this is written **UNCONFIRMED** rather than promoted — matching the
`TRACE` (2609.19897) precedent in the review.

**Method lessons from this pass:**

1. The **arXiv Atom API throttles hard** (HTTP 429 after ~6 rapid requests, then
   90 s+ cooldowns). Fetching `https://arxiv.org/abs/<id>` is a *different host*
   and is not throttled. Use the abs page for verification; use the API only for
   spaced-out discovery, 45 s+ apart.
2. **Recalled arXiv IDs are unreliable in exactly the way you'd guess.** Three
   confirmed failures this pass: I recalled `2305.03791` for PLAN-AND-ACT (it is
   *Fixed Point Theorem: Variants, Affine Context and Some Consequences*);
   `2406.06467` for AdaPlanner (it is *How Far Can Transformers Reason?*); and
   `2307.07761` for LLM-Modulo (it is *BV Quantization*). None of the three are
   close. Always title-match.
3. Semantic Scholar and OpenAlex both returned **429 within the first two calls**
   of the session. OpenAlex `title.search` works but returns nothing for long
   titles — fall back to web search for discovery, arXiv abs for verification.
4. PMLR volume index pages are scrapeable and are the reliable way to confirm an
   ICML/ICLR-adjacent page range (used for LATS v235 and BFCL v267).

---

# Top 5 for Cambrian

1. **Web Agents Should Adopt the Plan-Then-Execute Paradigm** (arXiv:2605.14290).
   The single best argument found anywhere in this pass for Cambrian's
   architecture. Finds that all WebArena tasks are compatible with
   plan-then-execute and **80 % need no runtime LLM subroutine at all**; argues
   untrusted web content may influence *values or branches* inside a predefined
   execution graph but **cannot redefine the task or synthesise new actions at
   runtime** — i.e. it is a *trust-boundary* argument for plan-then-execute, and
   it lands on the same conclusion as `FlowSteer` (2605.11514) from the opposite
   direction. Also names the exact gap Cambrian's typed tool plane closes: web
   tools (click/type/scroll) have page-dependent, un-plannable semantics; the fix
   is "typed, complete, auditable APIs", which is Cambrian's contract. →
   **ADR for the planner + the Zero-Hardcode Rule; the `plan-then-execute` arm of
   the DAG-plan suite.**
2. **LLMs Can't Plan, But Can Help Planning in LLM-Modulo Frameworks**
   (arXiv:2402.01817, ICML 2024). The peer-reviewed statement of *"model proposes,
   kernel disposes"*: LLMs are approximate knowledge sources and candidate
   generators; sound external critics, not the LLM, confer the guarantee. Pairs
   with the measured instantiation (below) to give Cambrian both the position
   and the number. → **the positioning paragraph for tag-algebra policy and for
   Gatekeeper/Dispatcher as critics rather than deciders.**
3. **MCPTox** (arXiv:2508.14925). First large-scale tool-poisoning measurement
   on *real* MCP servers: 45 live servers / 353 authentic tools, 1 312–1 348
   malicious cases, 20 agents, **o1-mini 72.8 % ASR**, mean 36.5 %, and — the
   load-bearing number — **highest refusal rate across all models < 3 %**. Also
   proves tool-poisoning payloads are *not* an indirect-prompt-injection variant:
   transplanting InjecAgent payloads into tool descriptions drops ASR to ≈0 %.
   → **closes the `snowball-2026-09-26.md` §"MCP-threat survey's prompt-injection
   ancestry is thin" gap for ADR-0043 with a primary, and supplies the
   refusal-rate baseline that makes kernel-side scanning necessary rather than
   merely prudent.**
4. **When MCP Servers Attack** (arXiv:2509.24272). The only paper found that
   *evaluates the actual scanners* rather than proposing a taxonomy: `mcp-scan`
   (Invariant Labs, ≥1 k★) detected **4** of the generated poisoned servers, and
   the LLM-agent scanner `AI-Infra-Guard` was strong on code-level attacks but
   bypassed by subtle misleading text. This is a measured, citable number
   against the incumbent detection approach, which is exactly what ADR-0043's
   design needs and what no survey provides. → **ADR-0043's "why not just scan on
   connect" paragraph; a required baseline arm in the security suite.**
5. **When Can LLMs Actually Correct Their Own Mistakes?** (arXiv:2406.01297,
   TACL 12:1417-1440) together with **LLMs Cannot Self-Correct Reasoning Yet**
   (arXiv:2310.01798, ICLR 2024). The two peer-reviewed negatives that must be
   cited before Cambrian proposes any self-correction or re-plan loop. TACL's
   contribution is a *methodological* one — prior self-correction studies are
   over-evaluated by under-specified questions and unfair protocols, and no
   study shows success with only prompted-LLM feedback. Combined with
   **ToolPRM** (ACL 2026), which measures that self-reflection is near-useless
   specifically for JSON function calls, the design implication is sharp: put the
   correction loop in the **kernel** (deterministic critic + re-plan trigger),
   never in the model's own reasoning. → **Magentic-One ledger (2411.04468) stays
   the closest analogue; these three are the argument that the ledger must be
   enforced kernel-side, plus the "do not add a self-reflect arm" control.**

---

# (a) Planner architectures — plan-then-execute vs interleaved

| # | Title | Venue | One-line result | Cambrian consequence |
|---|---|---|---|---|
| 1 | Web Agents Should Adopt the Plan-Then-Execute Paradigm (2605.14290) | arXiv preprint 2026-05-14; **no venue claimed** (no Comments field) | ReAct is the wrong default: commit to a program before observing untrusted content; all WebArena tasks fit plan-then-execute and 80 % are purely programmatic; untrusted data can set values/branches but cannot redefine the task | **The planner-boundary argument.** Independent, 2026, same conclusion as `FlowSteer` (2605.11514) — cite the pair as convergent. Names typed-completion interfaces as the precondition; that is Cambrian's typed agent plane. Core of the `plan-then-execute` ablation arm |
| 2 | Plan-and-Act: Improving Planning of Agents for Long-Horizon Tasks (2503.09572) | arXiv preprint; no Comments field (venue UNCONFIRMED) | Split into a dedicated **Planner** model emitting structured high-level plans + an **Executor**; synthetic trajectory→plan annotation; 57.58 % WebArena-Lite, 81.36 % text-only WebVoyager SOTA | The two-model split is the direct precedent for Cambrian's planner→Gatekeeper→Dispatcher chain. Its *synthetic plan annotation* recipe is the cheapest way to build a plan-quality arm without human labels |
| 3 | AdaPlanner: Adaptive Planning from Feedback with Language Models (2305.16653) | arXiv preprint; no Comments field (venue UNCONFIRMED) | Closed-loop plan refinement with **in-plan and out-of-plan** adaptation + code-style plan prompt + skill discovery; +3.73 % (ALFWorld) / +4.11 % (MiniWoB++) over SOTA at 2× and 600× fewer samples | The **re-plan-trigger** arm. "Out-of-plan refinement" is the design question `Magentic-One`'s ledger answers differently; worth running head-to-head in the DAG-plan suite |
| 4 | Plan-Then-Execute: An Empirical Study of User Trust and Team Performance (2502.01390) | **CHI 2025** (record: "conditionally accepted") | N = 248 user study; plan-then-execute with per-stage human involvement works when plan quality and execution involvement are both high; users mistrust plausible-but-wrong plans | The **human-in-the-loop / operator-plane** evidence for plan-then-execute: the operator sweep suite should measure trust calibration, not just success. Cite for the operator-plane ADR |
| 5 | LLM+P: Empowering LLMs with Optimal Planning Proficiency (2304.11477) | arXiv preprint; no Comments field (venue UNCONFIRMED) | NL → PDDL → classical planner → NL; optimal solutions on most benchmark problems where raw LLMs fail to produce even *feasible* plans | The **symbolic-verifier** arm. Establishes that a deterministic critic over a typed intermediate beats direct LLM planning. Antecedent to LLM-Modulo |
| 6 | Leveraging Pre-trained LLMs to Construct and Utilize World Models for Model-based Task Planning (2305.14909) | **NeurIPS 2023** (record Comments field) | Don't make the LLM the planner — have it emit a **PDDL world model**, then plan with sound domain-independent planners; GPT-4 produced models for 41 actions / 400+ literals, all errors correctable from natural-language feedback | The **schema-fidelity** argument: LLMs are better at *modelling* than at *searching*. Direct support for Cambrian's typed capability/tag declarations being produced by the model and enforced by the kernel |
| 7 | Leveraging Environment Interaction for Automated PDDL Translation and Planning (2407.12979) | **NeurIPS 2024** (record Comments field) | Iterative PDDL refinement against an **Exploration Walk (EW)** metric; 66 % solve rate vs 29 % for GPT-4 intrinsic planning with CoT | EW is a cheap, *environment-grounded* plan-quality signal. Candidate verifier for the DAG-plan suite: reward steps that visit novel state, not just steps that reach the goal |
| 8 | LLMs Can't Plan, But Can Help Planning in LLM-Modulo Frameworks (2402.01817) | **ICML 2024** (JREF: PMLR 235, 2024; pp. 22895-22907) | Position: autoregressive LLMs cannot plan or self-verify; they are approximate knowledge sources feeding a **generate-test-critique** loop of *sound external critics*, which is where the correctness guarantee comes from | **The positioning citation for "model proposes, kernel disposes."** Names precisely the Cambrian split: the model supplies candidate DAGs; the Gatekeeper/Dispatcher/tag-algebra are the sound critics |
| 9 | Robust Planning with LLM-Modulo Framework: Case Study in Travel Planning (2405.20625) | arXiv preprint (workshop paper; no Comments field, venue UNCONFIRMED) | Instantiates LLM-Modulo critics on TravelPlanning: CoT / ReAct / Reflexion score 0 % / 0.6 % / 0 %; LLM-Modulo gives **4.6–6× improvement** over those baselines with GPT-3.5/4-Turbo and a 10-cycle budget | The **measured delta** that makes the LLM-Modulo position non-decorative. Note the direction: the gain comes from *external critics*, and Reflexion's own contribution is 0 % |
| 10 | Robust Planning with Compound LLM Architectures: An LLM-Modulo Approach (2411.14484) | arXiv preprint; no Comments field (venue UNCONFIRMED) | Technical evaluation over **four scheduling domains**: sound verifiers + re-prompting make outputs *provably* non-fallacious; large gains across models; some configuration changes help and some do not | The "every output guaranteed correct" claim is what a deterministic kernel policy can actually make, and the configuration-sensitivity is a **pre-registered ablation menu**. The natural external harness for the planner arm |
| 11 | Do As I Can, Not As I Say: Grounding Language in Robotic Affordances — SayCan (2204.01691) | arXiv preprint; no Comments field (venue UNCONFIRMED — do **not** cite as a 2022 CoRL/NeurIPS paper without confirming) | Affordance = language-model score × real-world skill value; skills constrain the model to feasible, contextually appropriate actions | The **lineage root** for "affordance-weighted selection". SayCan is a *pre-runtime* merit score; Cambrian's merit rank is a *zero-RPC* version of the same idea. Cite once as lineage, not as a baseline |
| 12 | ReAct: Synergizing Reasoning and Acting in Language Models (2210.03629) | **ICLR 2023** (record: "v3 is the ICLR camera ready version") | The interleaved default: one prompt containing all prior reasoning and action output, then one action | The **interleaved arm** the plan-then-execute arm must beat. The token-waste property (each step re-pays for the whole prefix) is the cost argument; pair with ReWOO below |
| 13 | ReWOO: Decoupling Reasoning from Observations for Efficient Augmented Language Models (2305.18323) | arXiv preprint; no Comments field (venue UNCONFIRMED) | Plan with **no tool feedback at all**, then execute with variable substitution — explicitly framed against interleaving's redundant prompting and repeated execution | The **cost-mechanism** half of the plan-then-execute case: interleaving is expensive *structurally*, not incidentally. Required reading for any dispatch or plan-cost claim in topic 1 |

---

# (b) Self-correction loops, with measured deltas

| # | Title | Venue | One-line result | Cambrian consequence |
|---|---|---|---|---|
| 1 | When Can LLMs Actually Correct Their Own Mistakes? A Critical Survey (2406.01297) | **TACL 2024**; JREF *TACL* 12: 1417-1440 (2024) | Prior self-correction work is systematically **over-evaluated** (undefined research questions, impractical frameworks, unfair protocols); provides a design checklist; finds **no** prior work demonstrating successful self-correction from prompted-LLM feedback alone | **Mandatory citation before proposing any self-correction loop.** The checklist is the pre-registration for a Cambrian re-plan ablation. Peer-reviewed, so it carries the argument that 2310.01798 alone does not |
| 2 | Large Language Models Cannot Self-Correct Reasoning Yet (2310.01798) | **ICLR 2024** (record Comments field) | **Intrinsic** self-correction (no external feedback) fails and sometimes *degrades* reasoning performance | The sharpest negative. Cambrian's correction signal must be **external and deterministic** (env assertion failure, tag-algebra denial, end-state mismatch) — which is what `Magentic-One`'s progress ledger and `ToolSandbox`-style end-state scoring already provide |
| 3 | ToolPRM: Fine-Grained Inference Scaling of Structured Outputs for Function Calling (2510.14703) | **ACL 2026 (main)** (record Comments field) | For JSON function calls, early errors are **unrecoverable**; self-reflection gives only minor gains and consistently underperforms a 7 B process-supervised model; structured generation wants "explore more, retain less" | The **most directly actionable** result for ADR-0097 native tool-calling. Self-correction is measurably useless on exactly the artifact Cambrian's dispatcher produces. Correction belongs *before* the call is committed, not after |
| 4 | Language Agent Tree Search Unifies Reasoning, Acting, and Planning (LATS, 2310.04406) | **ICML 2024**; PMLR 235:62138-62160 (verified on proceedings.mlr.press/v235) | MCTS over ReAct with LM value functions + self-reflection + environment feedback: **doubles** ReAct on HotPotQA, **+22.1** avg on WebShop (GPT-3.5), 92.7 % pass@1 HumanEval (GPT-4) | The **ReAct+backtracking** architecture, with the cleanest measured deltas available. Its cost profile (tree search) is the natural contrast arm for the zero-RPC dispatcher in topic 1: what does backtracking buy, and at what token price? |
| 5 | Reflexion: Language Agents with Verbal Reinforcement Learning (2303.11366) | arXiv preprint; **no venue in the record** — widely cited as NeurIPS 2023 but UNCONFIRMED here, do not cite as NeurIPS without checking | Weight-free self-improvement: verbal reflection on task feedback stored in an episodic buffer, carried across trials; 91 % pass@1 on HumanEval, large gains on ALFWorld/HotPotQA | The **canonical loop shape** (feedback → reflection → next trial). Its mechanism is an *episodic memory buffer*, which is Cambrian's substrate; the ADR should say which store the reflection lands in and who may write it |
| 6 | Self-Refine: Iterative Refinement with Self-Feedback (2303.17651) | arXiv preprint; **no venue in the record** (widely cited NeurIPS 2023, UNCONFIRMED) | One model acts as generator, feedback provider, and refiner; **~20 % absolute** average task-performance gain across 7 tasks, no training | The **optimistic** half of the pair with TACL 2406.01297. Cite *both*: the 20 pp is real for open-ended generation, and the TACL survey explains why it does not transfer to tool calls |
| 7 | Teaching Large Language Models to Self-Debug (2304.05128) | arXiv preprint; no Comments field (widely cited ICLR 2024, UNCONFIRMED) | Rubber-duck debugging from execution traces alone, no human feedback, no error messages: SOTA on Spider (text-to-SQL), TransCoder, MBPP | Shows correction works when the trace is *execution-grounded*. The design rule for Cambrian: the reflection input must come from the kernel's execution log, not from the model's own state |
| 8 | CodeT: Code Generation with Generated Tests (2207.10397) | arXiv preprint; no Comments field (widely cited NeurIPS 2022, UNCONFIRMED) | Generate tests *and* candidates from the same model, then **dual execution agreement** (consistency vs tests + agreement across samples) to pick the solution | The **model as its own test generator** pattern. Directly reusable for Cambrian's row-trace verifier: let the planner propose an assertion, then check agreement across independent executions rather than trusting one run |
| 9 | Tree of Thoughts: Deliberate Problem Solving with LLMs (2305.10601) | **NeurIPS 2023** (record: "camera ready version") | Exploration over coherent thought units with self-evaluation, lookahead, and backtracking; large gains on Game-of-24, Creative Writing, and novel tasks | The generic search primitive behind LATS. Include only as lineage — the *measured* claim to cite is LATS's, not ToT's |

---

# (c) 2024-2026 tool-use benchmarks, and what each isolates

The useful framing the review lacks: these benchmarks are best read as a set of
**isolating controls** rather than a leaderboard. Each carves out one factor.

| # | Title | Venue | What it isolates | Cambrian consequence |
|---|---|---|---|---|
| 1 | ACEBench: Who Wins the Match Point in Tool Usage? (2501.12851) | arXiv preprint; no Comments field (venue UNCONFIRMED) | Three-way split — **Normal / Special (ambiguous or incomplete instructions) / Agent (multi-agent multi-turn)**; removes the LLM-judge and live-API overhead that inflates other suites | The only entry that isolates **instruction ambiguity** as a failure mode. `Special` is the missing cell in Cambrian's dispatch evaluation: a planner that under-specifies a DAG should be penalised even when the executor recovers. Best candidate external harness for the `orchestration` suite |
| 2 | ComplexFuncBench (2501.10132) | arXiv preprint; no Comments field (venue UNCONFIRMED) | Five axes of *constrained* calling: multi-step in one turn, user constraints, **parameter-value reasoning from implicit info**, long parameter values, 128k context. Error taxonomy `func_error / param_missing / hallucination / value_error / stop_early`; `value_error` is 78.8 % for Qwen2.5-72B, `stop_early` 19.7 % (Claude-3.5) / 21.0 % (GPT-4o) | The **`value_error` vs `stop_early` split is the row-trace taxonomy Cambrian needs**: distinguishing "picked the wrong argument" from "stopped before gathering information" is a different `failure_kind`. Its ComplexEval also returns the API error as the next response, so it *measures* self-correction inside function calling — a controlled re-entry to (b) |
| 3 | NESTFUL: A Benchmark for Nested Sequences of API Calls (2409.03797) | arXiv preprint; no Comments field (venue UNCONFIRMED) | **Output-to-input chaining** as a single construct: 1 800+ fully executable nested sequences; best model (GPT-4o) at 28 % full-sequence match, 60 % win-rate | The cleanest isolation of *nesting depth* — exactly the DAG depth axis Cambrian's planner emits. 28 % full-sequence match is a sobering prior for any deep generated DAG |
| 4 | ToolEyes (2401.00741) | **COLING 2025** (record Comments field) | Fine-grained over **five dimensions** — format alignment, intent comprehension, behaviour planning, tool selection, answer organisation — across 7 real scenarios and a ~600-tool library | The most directly reusable *dimension set* for splitting row artifacts into scored sub-dimensions, alongside `TRAJECT-Bench` (already in §3). Sits upstream of the dispatcher: it scores planning and selection as distinct axes, which is the `routing_accuracy` decomposition topic 1 needs |
| 5 | StableToolBench (2403.07714) | arXiv preprint; no Comments field (venue UNCONFIRMED) | **Evaluator and environment stability**: virtual API server (caching + API simulators) plus GPT-4-based solvable-pass-rate / win-rate to kill run-to-run variance inherited from live RapidAPI | The reproducibility authority for tool benchmarks. The review already leans on `StableToolBench`-style stability for `gaia-tool-use`; this is the citation. Its decomposition (environment stability *vs* evaluator stability as separate fixes) is the template for Cambrian's own harness |
| 6 | T-Eval: Evaluating the Tool Utilization Capability Step by Step (2312.14033) | arXiv preprint; no Comments field (venue UNCONFIRMED) | Decomposes tool use into six sub-processes — **instruction following, planning, reasoning, retrieval, understanding, review** — and scores each in isolation | Names *review* as a separable capability. That is the self-correction slot, measured independently from planning — so a Cambrian re-plan arm can be scored without contaminating the plan-quality score |
| 7 | API-Bank (2304.08244) | **EMNLP 2023** (record Comments field); ACL Anthology `2023.emnlp-main.187`, pp. 3102-3116 | The **three-factor ladder Call → Retrieve+Call → Plan+Retrieve+Call** over 73 runnable tools / 314 dialogues / 753 calls. GPT-3.5: 59.4 % → 38.5 % → 22.0 %; GPT-4: 63.7 % → 37.0 % → 70.0 % | The cleanest **decomposition of the planner from the executor** in the literature: adding a retrieval step costs ~21 pp, and planning only pays off at GPT-4 scale. This is the quantitative shape of the "is the planner earning its tokens" question in topic 1 |
| 8 | ToolQA (2306.13304) | arXiv preprint; no Comments field (venue UNCONFIRMED) | The **controllability** trick: separates questions answerable from parametric memory from those genuinely requiring tools, with 13 specialised tools and explicit minimisation of pretraining overlap | The design that makes a *tool-necessity* metric possible. For Cambrian this is the difference between "the Dispatcher helped" and "the model knew it anyway" — a mandatory control arm for any dispatch result |
| 9 | Seal-Tools (2405.08355) | arXiv preprint; no Comments field (venue UNCONFIRMED) | Self-instruct API-like tools with **process control**, a hard subset of multi-tool and *nested* calls, and three orthogonal metrics under strict format control | Another nesting control, complementary to NESTFUL: Seal-Tools controls the *generation* of the hard cases, NESTFUL controls the *execution*. Useful for building a Cambrian nesting-depth ladder with held-out generation |
| 10 | ToolACE: Winning the Points of LLM Function Calling (2409.00920) | arXiv preprint; no Comments field (venue UNCONFIRMED) | An **automatic agentic data pipeline** with self-evolution synthesis over a 26 507-API pool and *dual-layer* rule-based + model-based verification; the 8 B model reaches BFCL SOTA | The dual-layer verification pattern (deterministic rule first, model second) is Cambrian's verifier design in miniature, and 26.5 k APIs is the scale reference for "how large a typed tool surface is realistic" |
| 11 | ToolACE-MT: Non-Autoregressive Generation for Agentic Multi-Turn Interaction (2508.12685) | **ICLR 2026** (record Comments field) | Multi-turn agentic dialogues built by **coarse init → iterative mask-and-fill refinement → offline verification**, replacing costly autoregressive multi-agent simulation | A cheap way to synthesise multi-turn rows with verified invariants. Directly applicable to Cambrian's multi-turn suites and worth citing for the cost arm: generating trajectories by simulation is the dominant cost in this benchmark family |
| 12 | Live API-Bench: 2500+ Live APIs (2506.11266) | arXiv preprint; no Comments field (venue UNCONFIRMED) | Converts BIRD-SQL into **executable API environments** (SLOT / SEL / REST formulations) across 11 databases, 2 500+ invocable tools, with ground-truth API sequences and verified final answers | The **error-handling and response-parsing** axes, which no other entry here measures: it feeds API errors back and scores recovery. Combined with the ground-truth call *sequence* it also enables measuring ordering, not just end state — the `ToolSandbox` milestone-ladder plus `TRAJECT-Bench` order dimension |
| 13 | Gorilla: Large Language Model Connected with Massive APIs — **introduces APIBench** (2305.15334) | arXiv preprint; no Comments field (venue UNCONFIRMED) | APIBench: 1 645 APIs from TorchHub/TensorHub/HuggingFace, 17 002 self-instruct queries, AST **sub-tree** matching that separates *hallucination* from *wrong-API-selected* errors | The AST-matching evaluator that BFCL later generalises, and the **error-type split** (hallucinate vs. mis-select) is the ancestor of every per-row `failure_kind` in Cambrian. The paper is already cited as a model in the snowball note; the **dataset + metric** is the net-new part |

---

# (d) MCP ecosystem and tool-poisoning primaries

This was the review's acknowledged thin spot (`snowball-2026-09-26.md`:
*"The MCP-threat survey's prompt-injection ancestry is thin… If ADR-0043 cites
this survey as the threat-model anchor, it should also cite the direct-injection
primaries. Those identifiers are UNVERIFIED"*). That gap is now **closed with
primaries**, and it is closed in a way that changes ADR-0043's argument: tool
poisoning is *not* an indirect-prompt-injection variant, and *scanning is
measurably insufficient*.

| # | Title | Venue | One-line result | Cambrian consequence |
|---|---|---|---|---|
| 1 | MCPTox: A Benchmark for Tool Poisoning Attack on Real-World MCP Servers (2508.14925) | arXiv preprint; **no Comments field** — an AAAI OJS proceedings PDF exists at `ojs.aaai.org/index.php/AAAI/article/download/40895/44856` but the arXiv record asserts no venue, so treat **venue UNCONFIRMED** and cite as arXiv until the OJS landing page is checked | 45 live real-world MCP servers, 353 authentic tools, 1 312 cases (v1; 1 348 in the camera-ready), 10–11 risk categories, 20 agents. Mean ASR 36.5 %; **o1-mini 72.8 %, Phi-4 70.2 %, GPT-4o-mini 61.8 %**. Highest refusal rate across all models **< 3 % (Claude-3.7-Sonnet)**. Enabling reasoning mode *raised* Qwen3-32B ASR by 27.8 pp. Transplanting InjecAgent IPI payloads into tool descriptions → ≈0 % ASR vs 41.8 % for purpose-built payloads | **The primary ADR-0043 needed.** Four distinct claims, each citable: (i) refusal is not a defence, (ii) more capable models are *more* vulnerable, (iii) *reasoning* is an amplifier not a mitigation — which independently indicts the Zero-Hardcode Rule's Awareness-layer placement, and (iv) tool-poisoning needs its own attack corpus, so the review's `Zhan et al.` indirect-injection line cannot be its empirical basis |
| 2 | When MCP Servers Attack: Taxonomy, Feasibility, and Mitigation (2509.24272) | arXiv preprint; no Comments field (venue UNCONFIRMED) | Component-based taxonomy of **12 attack categories** with PoC servers per category across host×LLM pairs; malicious servers are generatable at near-zero cost; **existing scanners are insufficient** — `mcp-scan` (Invariant Labs) detected only **4** generated poisoned servers, and the LLM-agent scanner `AI-Infra-Guard` was bypassed by subtle misleading metadata text | **The measured case for not relying on connect-time scanning alone**, with a number. Any ADR-0043 scanner design must state which of the 12 categories it covers and concede the rest. Also the source of the honest limitation section: obfuscated metadata defeats both data-flow and LLM-judge approaches |
| 3 | MCP Security Bench (MSB) (2510.15994) | **ICLR 2026** (record Comments field) | 12-attack taxonomy spanning **task planning, tool invocation, and response handling** — name-collision, preference manipulation, injection in tool descriptions, out-of-scope parameter requests, user-impersonating responses, false-error escalation, tool-transfer, retrieval injection, mixed; real benign+malicious tools executed over MCP (not simulated); proposes **Net Resilient Performance (NRP)** as an explicit security-vs-performance trade-off metric | **The Pareto-curve metric for topic 7.** NRP is a peer-reviewed (ICLR 2026) security/utility scalar, so the agenda's "security × utility Pareto curve" becomes a named prior metric rather than an ad-hoc construction. Its three-stage attack placement (planning / invocation / response) maps 1:1 onto Cambrian's planner → tool-executor → result-ingest path |
| 4 | When the Manual Lies: A Realistic Benchmark to Evaluate MCP Poisoning Attacks (2605.24069) | arXiv preprint 2026-05-22; no Comments field (venue UNCONFIRMED) | Isolates **Tool Description Poisoning (TDP)** as the *only* vector; Docker sandbox with 32 cases / 6 risk categories; attack success judged by **forensic inspection of physical side-effects** (files created, server logs) so there are no false positives; 8 models × 5 runs. **GPT-4o ≈ 0.997 / 0.998 ASR**; top-5 models > 89 %. Supply-chain placement beats Trojan-horse placement (95.2 % vs 82.8 %) — agents trust instructions from familiar tools. Names the **"Firewall Fallacy"**: guardrails can be counterproductive, and for Gemini the guardrail *raised* ASR (contextual interference) | Two Cambrian-critical results. (i) **Forensic side-effect verification** is the right evidence standard for a security suite — pair with `AgentBound`'s execution-boundary argument. (ii) **Familiarity trust** is the mechanism a capability-tag policy must not inherit: a tool being already-registered must not lower its scrutiny, so tag grants have to be re-checked per call, not cached per registration |
| 5 | Parasites in the Toolchain: A Large-Scale Analysis of Attacks on the MCP Ecosystem (2509.06572) | **IEEE Symposium on Security and Privacy 2026** (record Comments field) | **Parasitic Toolchain Attacks / MCP-UPD** — no victim interaction; multiple *individually legitimate* tools composed into a chain (Ingestion → Collection → Disclosure). First large-scale census: **12 230 tools / 1 360 servers**; 1 062 tools (8.7 %) and **370 servers (27.2 %)** expose an exploitable capability; 9 of 10 constructed toolchains exfiltrate in real settings. Root cause stated as MCP lacking **context–tool isolation and least-privilege enforcement** | The **strongest peer-reviewed (S&P 2026) statement of the exact gap Cambrian's tag algebra fills**, in the authors' own words: context–tool isolation and least privilege. It also breaks the single-server limitation of every prior attack, which means per-tool scanning is provably the wrong granularity — the gate must see *compositions* |
| 6 | MCP-ITP: An Automated Framework for Implicit Tool Poisoning in MCP (2601.07395) | arXiv preprint 2026-01-12; no Comments field (venue UNCONFIRMED) | **Implicit** poisoning: the poisoned tool is never invoked; its metadata instead induces the agent to call a *different, legitimate, high-privilege* tool. Formulated as black-box optimisation with iterative feedback | The **privilege-separation** attack. A tag system that authorises by tool identity rather than by *(action, resource)* pair fails this directly. Pair with `2605.24069`'s forensic verification as the evaluation instrument |
| 7 | Beyond the Protocol: Unveiling Attack Vectors in the MCP Ecosystem (2506.02040) | arXiv preprint; no Comments field (venue UNCONFIRMED) | Full upload → download → attack chain on **real** aggregator platforms: malicious servers uploaded cleanly to Smithery.ai, MCP.so, and Glama (Glama labelled one "safe to use"); 20-participant user study; 45 LLM×vector and 45 client×vector combinations × 20 runs. Tool Poisoning mean ASR 59.33 %, Malicious External Resources **93.33 %**, Puppet 6.67 %; all refusal rates < 10 % | The **supply-chain** leg — the ecosystem-level argument that per-install scanning cannot be the whole story. The "safe to use" label on a poisoned server is the single most citable fact for arguing that upstream trust signals are worthless. Note the ordering: external-resource injection beats tool poisoning, so ADR-0043's read-path scoping must bind *data* not just tools |
| 8 | MCP-38: A Comprehensive Threat Taxonomy for MCP Systems v1.0 (2603.18063) | arXiv preprint 2026-03-18; record Comments field is "v1.0" only (venue UNCONFIRMED) | 38 threat categories (MCP-01…MCP-38) from a four-phase method (protocol decomposition, multi-framework cross-mapping, incident synthesis, remediation-surface categorisation), each mapped to STRIDE, OWASP LLM Top-10 2025, and OWASP **Agentic** Top-10 2026 (ASI01-ASI10); argues the semantic surface (description poisoning, indirect injection, parasitic chaining, dynamic trust violations) is uncovered by prior frameworks | The **coverage-matrix** citation. The number 38 makes it a checklist for ADR-0043's own threat table, and the ASI01-ASI10 mapping lets ADR-0043 be positioned against a 2026 standard rather than only against papers. Treat as a preprint taxonomy, not an authority |
| 9 | MCP-Universe (2508.14704) | arXiv preprint; record Comments field is a website link only (venue UNCONFIRMED) | 6 core domains / 11 real MCP servers; execution-based evaluators in three tiers — **format compliance, static content match, and dynamic ground-truth retrieval** for time-sensitive tasks | The **three-tier execution evaluator** is the cheapest sound design for a live-MCP suite and directly extends the review's `ToolSandbox` end-state scoring to servers whose answers go stale. The dynamic tier is the piece the `τ²`/ToolSandbox pair does not need and a real MCP surface does |
| 10 | MCPVerse (2508.16260) | arXiv preprint; no Comments field (venue UNCONFIRMED) | **550+ real executable tools**, action space > 140 k tokens, outcome-based scoring with real-time ground truth; Oracle / Standard / Max-Scale modes isolate tool-set size. Most models degrade with larger tool sets, but agentic models (Claude-4-Sonnet) can exploit the larger space | The **tool-catalogue-size** axis as a controlled variable, which is the substrate-side twin of `Gatekeeper`'s slate size. The Oracle/Standard/Max-Scale triple is a ready-made ablation for `candidates_per_slate`. Also a documented counter-case: bigger slates are not uniformly worse |
| 11 | MCPEval: Automatic MCP-based Deep Evaluation (2507.12806) | arXiv preprint; record Comments field is a GitHub link (venue UNCONFIRMED) | Open-source framework automating end-to-end task generation and evaluation over MCP across five real domains, standardising metrics and integrating native agent tools | The **reproducible-harness** candidate if Cambrian's MCP provider needs a third-party scorer. Lower priority than MCP-Universe's evaluators, which are specified rather than just orchestrated |
| 12 | Evaluation Report on MCP Servers (2504.11094) | arXiv preprint; no Comments field (venue UNCONFIRMED) | **Name-collision warning:** the arXiv title is "Evaluation Report on MCP Servers" but the framework inside is named **MCPBench** (ModelScope) — a *different work* from Accenture's `MCP-Bench` (2508.20453) already in the review. Two tasks (web search, DB search) over popular servers: Bing 64 % accuracy vs DuckDuckGo 10 % (54 pp spread); Exa 231 s vs Bing/Brave < 15 s; **MCP does not beat plain function calling** (Qwen Web Search 55.52 %); adding a text-to-SQL model inside the server gives +22 pp on the PostgreSQL task | The **provider-choice** evidence for ADR-0043: MCP transport is not itself a reliability win, and a *54 pp* accuracy spread between servers is a selection problem, not a protocol problem. The +22 pp from a declarative interface is the same finding as `2605.14290`'s typed-interface argument, measured. **Do not merge with `MCP-Bench` in the bib** |
| 13 | ETDI: Mitigating Tool Squatting and Rug Pull Attacks in MCP (2506.01333) | arXiv preprint; no Comments field (venue UNCONFIRMED) | Enhanced Tool Definition Interface: **cryptographic identity verification, immutable versioned tool definitions, explicit permission management** (often OAuth 2.0) plus a dedicated policy engine evaluating capabilities against runtime context rather than static scopes | The **industry/defence counterpart** to `Narajala & Habler` (already in the snowball note) and the concrete instantiation of "model proposes, kernel disposes" at the *provider* boundary. Immutable versioned definitions is also the direct answer to `Beyond the Protocol`'s rug-pull finding, and belongs in ADR-0043's design as prior art rather than as Cambrian's invention |

---

# (e) Function-calling reliability: parallel calls, schema adherence, structured output

| # | Title | Venue | One-line result | Cambrian consequence |
|---|---|---|---|---|
| 1 | Instruction-Following Evaluation in Function Calling (**IFEval-FC**, 2509.18420) | arXiv preprint; no Comments field (venue UNCONFIRMED) | Encodes **verifiable format instructions inside JSON-schema parameter descriptions**; 750 cases, fully algorithmic scoring, no judge; partly built by augmenting BFCL schemas. **No evaluated model exceeds 80 %** — GPT-5 and Claude 4.1 Opus frequently fail trivial format rules | The **schema-adherence** isolation, and a strict complement to BFCL's AST matching: BFCL checks that arguments land in a valid set, IFEval-FC checks that the model read the *prose in the schema*. For a native-tool-calling kernel (ADR-0097) this is the metric that separates "parsed the call" from "honoured the contract" |
| 2 | Towards Reliable Benchmarking: A Contamination Free, Controllable Evaluation Framework (**FuncBenchGen**, 2509.26553) | **ICLR 2026** (record Comments field) | Casts multi-step function calling as **traversal of a hidden function-dependency DAG** (nodes = calls, edges = one call consuming another's output); controllable difficulty via graph size, dependency depth, distractor count; contamination-free by generation at eval time. GPT-5 gets **15 % success at 20 required calls**; **connected distractors** (irrelevant functions sharing type-compatible variables) hurt every model; models get syntax right but **fail to propagate argument values across calls** | The **only entry in this whole pass that uses a DAG of tool calls as the task primitive** — the same primitive Cambrian's planner emits. That makes it the closest published analogue to the `dag-plans` suite and the best contamination control for it. "Value-not-yet-known" / "incorrect-value" as machine-checked failure predicates is a ready `failure_kind` set, and the connected-distractor result predicts the Gatekeeper's distractor handling is where merit rank earns its keep |
| 3 | On the Robustness of Agentic Function Calling (2504.00914) | **TrustNLP@NAACL 2025** (record Comments field) | Two robustness axes on an expanded BFCL subset: meaning-preserving **query paraphrase**, and **toolkit expansion** with semantically related tools (mean toolkit 2.7 → 5.6). Performance degrades across the board on both; **70–90 % of paraphrase-induced errors are parameter-value assignment mismatches** where the eval's allowed-value list is incomplete (e.g. accepts "Miami" but not "Miami, FL") | The **evaluator-validity** warning: a large share of apparent function-calling failure is a *scoring artifact* from enumerated value sets. METHODOLOGY.md must either enumerate values semantically or use response-equivalence (as `ComplexEval`/`BFCL` execution-matching do). Toolkit expansion is also the direct measurement of what a bigger Gatekeeper slate costs — and it degrades, which is evidence *for* merit rank over "give it all" |
| 4 | LongFuncEval (2505.10570) | arXiv preprint; no Comments field (venue UNCONFIRMED) | Three long-context tool-use stressors, varying both length and answer position: **7–85 % drop as the tool catalogue grows**, **7–91 % degradation as tool responses lengthen**, 13 % / 40 % as multi-turn conversations lengthen | The **long-context tool-use degradation curve**, and a hard design constraint on any Cambrian suite that inlines tool schemas and results into the planner prompt. The "lost in the middle" result applies to *tools*: a large slate with the right tool in the wrong position fails. Supports typed, addressed tool references over concatenated schemas |
| 5 | The Berkeley Function Calling Leaderboard (BFCL): From Tool Use to Agentic Evaluation of LLMs — Patil et al. (ICML 2025) | **ICML 2025** (42nd ICML); **PMLR 267:48371-48392**, `patil25a` (verified by scraping the PMLR v267 index) | Serial **and parallel** call categories (Simple / Multiple / Parallel / Parallel-Multiple / Irrelevance), AST substring matching shown to correlate with execution-based evaluation, plus a 64 517-query crowd-sourced set, state-**and**-response-based multi-turn checks, and an agentic category. **Claude cannot execute parallel calls in FC mode but can in prompting mode** | The review carries BFCL only as a `URL-200` blog link; this is the **peer-reviewed anchor** and the source of the *parallel-call* category, which is otherwise absent from the review. Two facts to use: parallel-call capability is mode-dependent (ADR-0097 must not assume the model's native mode supports it), and the state-plus-response check pair is the right multi-turn scorer — state alone cannot see read-only calls |
| 6 | ToolPRM (2510.14703) | **ACL 2026 (main)** | See (b). Fine-grained intra-call process supervision; "explore more but retain less" because early JSON errors are unrecoverable | Cross-listed deliberately: this is the only paper that isolates **where inside a single function call** the error budget goes. A Cambrian dispatcher that can prune at the function-name step rather than after emitting the full JSON is the concrete win |

---

# Promotions to `references.bib` and `LITERATURE_REVIEW.md`

Ordered by expected value. None of these is currently in the review.

1. arXiv:2605.14290 — plan-then-execute as a **trust boundary**; 80 % of WebArena is programmatic. Planner ADR.
2. arXiv:2406.01297 — TACL self-correction survey + design checklist. Closes the "do not add a self-reflect arm" argument.
3. arXiv:2508.14925 — MCPTox. The tool-poisoning primary ADR-0043 lacks.
4. arXiv:2509.24272 — MCP server attacks; scanners insufficient (mcp-scan: 4 detections).
5. arXiv:2509.06572 — Parasites in the Toolchain (S&P 2026). Context–tool isolation + least privilege, named as the root cause.
6. arXiv:2510.15994 — MSB (ICLR 2026). NRP security/utility metric for topic 7.
7. arXiv:2509.26553 — FuncBenchGen (ICLR 2026). DAG-of-calls primitive + contamination control.
8. arXiv:2402.01817 — LLM-Modulo (ICML 2024). "Model proposes, kernel disposes," sourced.
9. arXiv:2505.20625 + arXiv:2411.14484 — the measured LLM-Modulo deltas (4.6–6×; four scheduling domains).
10. arXiv:2605.24069 — MCP-TDP. Forensic verification + "Firewall Fallacy" + familiarity trust.
11. arXiv:2509.18420 — IFEval-FC. Schema-prose adherence under 80 % even for frontier models.
12. arXiv:2310.04406 — LATS (ICML 2024). ReAct+backtracking with clean deltas.
13. PMLR 267:48371-48392 — BFCL as a peer-reviewed paper (parallel-call categories).
14. arXiv:2501.10132 — ComplexFuncBench. `value_error` vs `stop_early` row-trace split.
15. arXiv:2501.12851 — ACEBench. Isolates instruction ambiguity (the `Special` category).
16. arXiv:2504.11094 — MCP server evaluation (MCPBench/ModelScope). 54 pp provider spread; MCP ≠ reliability. **Name-collision note required.**
17. arXiv:2504.00914 — FC robustness (TrustNLP@NAACL 2025). Slate expansion degrades; 70-90 % of errors are scoring artifacts.
18. arXiv:2503.09572 — Plan-and-Act. Planner/Executor split with numbers.
19. arXiv:2504.11094 / arXiv:2506.02040 — Beyond the Protocol. Supply-chain leg, "safe to use" label.
20. arXiv:2506.01333 — ETDI. Defence lineage for ADR-0043.

## Gaps and cautions from this pass

- **MCPTox venue is not confirmed.** The arXiv record has no `Comments`
  field. A camera-ready PDF is served from the AAAI OJS at article 40895, but
  the landing page did not parse, so this is recorded as UNCONFIRMED per the
  `TRACE` precedent. Do not write "AAAI 2026" until the OJS landing page or the
  AAAI programme is checked.
- **"MCP-Bench" is a name collision, and a live one.** Accenture's
  MCP-Bench (2508.20453) is in the review. ModelScope's framework is *also*
  called MCPBench but its arXiv title is "Evaluation Report on MCP Servers"
  (2504.11094). Two `MCPBench` bib keys will collide. Add a disambiguation note
  alongside the `S+review` style note already used for `2505.10924` vs
  `2406.02630`.
- **The planner-vs-interleaved question is not settled by these papers.** The
  strongest evidence is asymmetric: `2605.14290` gets 80 % programmatic
  coverage on WebArena, and `2502.01390` gets a win in a *simulated* daily-
  assistant environment with humans in the loop. Neither measures a
  stateful, policy-enforcing kernel. The genuine open measurement — plan-then-
  execute under a zero-RPC dispatcher with tag-algebra enforcement — remains
  Cambrian's own, and no paper in this pass fills it.
- **The self-correction literature is overwhelmingly code-generation.** Reflexion,
  Self-Debug, CodeT, LATS, and ToT all measure on HumanEval / MBPP / Spider.
  The only two that measure on tool calls are ComplexFuncBench's error-feedback
  loop and ToolPRM. If Cambrian wants a self-correction claim, it is
  contributing a measurement the field has not made, and should say so.
- **Two missing named targets from the brief.** "NesTools" and "ToolQA" as
  search targets: ToolQA resolved cleanly (2306.13304). "NesTools" resolved to
  no distinct work under any title match — the nearest real constructs are
  **NESTFUL** (2409.03797) and **NexusRaven** (which I did not verify and have
  therefore not written down). Do not cite a "NesTools" paper; if the brief meant
  NESTFUL, use that. NexusRaven is left UNVERIFIED by choice.
- **No `RCI` paper was found in the self-correction family.** Searching that
  initialism across arXiv titles and self-correction abstracts returned nothing
  matching a self-correction method. I have not written down a guess. Either the
  brief meant a different acronym or the work is not on arXiv.
- `MCPVerse`, `MCPEval`, `MCP-Universe`, `MCPEval` and the 2026 MCP security
  preprints are all **arXiv-only with no venue claim in the record**. Several are
  likely to be published before the paper is submitted; re-check at revision
  time. The ones with confirmed venues (ICLR 2026 ×3, ACL 2026 ×2, S&P 2026,
  TACL 2024, ICLR 2024 ×2, ICML 2024 ×2, NeurIPS 2023 ×2, NeurIPS 2024,
  COLING 2025, EMNLP 2023, CHI 2025, TrustNLP@NAACL 2025, PMLR 267) are the
  safe citable subset.
