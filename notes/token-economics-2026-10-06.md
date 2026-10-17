# Token-economics / harness-cost axis sweep — 2026-10-06

The adversarial check `notes/routing-cost-2026-10-01.md` §10 called for:
a pass over the *harness-cost* and *token-economics* axes (the ICLR
passes only covered orchest*/rout*/memory/trajectory + verifier/rubric/
judge). OpenReview v2 API is now challenge-walled (403
ChallengeRequiredError 2026-10-06), so this pass ran over arXiv title
search instead — same protocol otherwise (every ID title-matched off a
fetched record).

Query: `ti:"cost per task" OR ti:"token economics" OR ti:"harness effect"
OR ti:"budget-aware routing" OR ti:"cost-aware orchestration"`, sorted
newest-first, 11 hits, 8 kept, 2 rejected (crypto token-economics, wrong
field), 1 already-covered (Harness Effect v2 — see below).

## Kept

- ✅ **Hu et al., "Beyond the Model: Demystifying Harness Effects in
  Software Engineering Agents"** — arXiv:2609.32459 (2026-09-26; no venue
  claimed). mini-SWE-agent vs OpenCode × Qwen/DeepSeek on SWE-bench Pro,
  ProgramBench, GitTaskBench; NanoHarness component ablation (tool
  registry, context compression, explicit planning, subagents, lazy
  skills). Structured tool use + task-specific subagents most stable;
  compression and general subagents can HURT repo-generation. Harness
  gains diminish with model capability on issue-repair but grow on
  open-ended repo tasks. → **P3 nuance + component-level arm menu.**
- ✅ **Arjmandi, "Harness or Model? Isolating the Harness Effect in
  Agentic Coding with a Contamination-Controlled Private Suite"** —
  arXiv:2609.11987 (2026-09-08, revised; no venue claimed). Paired
  same-model contrasts (claude-agent-sdk vs deepagents; codex SDK vs
  deepagents), 792/800 runs, isolated-oracle grading. NEITHER harness wins
  on average (−1.25pp / +1.25pp, wide CIs); strata oppose (native trails
  9pp on repo tasks, leads 23.7pp on contest tasks — post-hoc partition,
  needs replication); neutral harness costs 1.2–1.6× per solved task;
  58 runs missing usage records (billed ordering unresolved — stated, not
  hidden); revision discloses a prior telemetry defect that invalidated
  v1 cost figures. → **The negative result P3 must survive; exemplary
  honest-limits practice — cite as the model.**
- ✅ **Wani, Dholakia & Ellison, "The Reasoning Tax: Token Economics of
  LLM Reasoning Across Task Types and Deployment Contexts"** —
  arXiv:2608.26235 (accepted TPCTC 2026, Springer). TES metric = accuracy
  gain over non-reasoning baseline ÷ token multiplier; 151 runs × 7
  benchmarks; task structure predicts efficiency better than difficulty;
  systematic diminishing returns (more thinking sometimes hurts); RCS/DCM
  deployment multipliers. → **Token-normalized metric precedent for P1;
  cite TES as the metric family proposal 01's control belongs to.**
- ✅ **Yao et al., "Harness-Bench: Measuring Harness Effects across
  Models in Realistic Agent Workflows"** — arXiv:2605.27922 (no venue
  claimed). 106 sandboxed tasks, 5,194 trajectories; capability reported
  at model×harness configuration level; execution-alignment failures
  (reasoning decoupled from tool feedback/state/contracts). →
  **Configuration-level reporting rule for our own results.**
- ✅ **Wu & Deng, "Computational Challenges in Token Economics"** —
  arXiv:2605.17410 (43pp; no venue claimed). Token Economics Trilemma
  (granularity / real-time / optimality); agenda paper. → **Framing
  citation for §2's cost section.**
- ✅ **Chen et al., "Token Economics for LLM Agents: A Dual-View Study
  from Computing and Economics"** — arXiv:2605.09104 (no venue claimed).
  First comprehensive Token Economics survey; micro/meso/macro/security
  taxonomy (tokens as production factors, exchange media, units of
  account). → **The survey §2 was missing: token economics is a named
  field; proposal 01 sits inside it.**
- ✅ **Qureshi, Martin & Peng, "Budget-Aware Routing for Long Clinical
  Text"** — arXiv:2605.00336 (no venue claimed). Knapsack subset selection
  under token budget; RCD submodular objective; selector > unitization;
  ROUGE saturates for LLM summaries while BERTScore discriminates. →
  **Budget-routing precedent; metric caution (no ROUGE-equivalents in our
  scoring).**
- ✅ **Tang, "Compact Constraint Encoding for LLM Code Generation"** —
  arXiv:2604.07192 (no venue claimed). NULL result: −25–30% prompt tokens,
  no compliance change; variance from constraint type/domain, not
  encoding; self-assessed compliance overestimates rule-scored. →
  **Supports concise policy_note encoding (encoding ≠ compliance); null-
  result culture citation.**

## Already covered (update)

- **Harness Effect v2** (2607.06906v2, updated 2026-10-05, one day before
  this pass): numbers consistent with `harness2026` (−41% cost/task).
  No bib change; record the v2 existence.

## Rejected

- 1910.02064, 1808.01261 — crypto/energy token-economics. Wrong field.
