# Study Proposal 01 — Capability-Typed Dispatch vs Auction Selection

- **Agenda topic:** 1 · **Horizon:** 1 month · **Status:** proposed

## Research question

Does zero-RPC merit-ranked dispatch (Gatekeeper→Dispatcher, ADR-0100) match
or beat bid-based auction selection on routing accuracy, at what latency and
cost saving?

## Background

Cambrian retired auction selection in ADR-0100 P3. The `orchestration` suite
(plan-quality-lab, 120+20 tasks, gold routing labels in
`routing_labels.json`) still emits routing metrics under auction-era names.
Prior art: MDPI Future Internet 18(6):326 survey (capability-based allocation
via A2A Agent Cards), ProgRouter (cost-budget routing, EMNLP 2026 Findings),
ORCH (deterministic EMA routing), Mars-rover single-vs-multi-agent study
(orchestration as cost-bearing choice), Cemri et al. failure taxonomy
(arXiv:2503.13657) for the misroute breakdown, Zhang et al. sham-policy
placebo control (arXiv:2609.20474) as the ablation-design model.
ICLR 2026 contrast arms: Conductor (learned RL routing), GraphPlanner
(MDP routing on graph memory), DiSRouter (no-external-gate self-routing);
RouterArena + OrchestrationBench as external scoring harnesses.

## Design (updated 2026-10-02 — reframed per routing-cost sweep)

Per `notes/routing-cost-2026-10-01.md` §0: the defensible headline is
**routing-decision overhead** — marginal tokens/latency the selection step
itself consumes — not task-level cost. `LLM calls and tokens per selection
= 0` is promoted from expected value to PRIMARY result; everything else is
a Pareto point at matched tokens (Tran & Kiela DPI confound). Full arm
table P1–P12 (token-normalised control, workflow-measured cost,
harness-only control, LLM-router, learned routers, cascade, deliberate
test-time compute, retrieval-filtered slate, protocol accounting,
round-replicated measurement, sham control, framework cost table) lives in
the notes file; implement P1–P4 + P11 first.

- **Control:** auction-era archived runs (pre-ADR-0100), with the metric-name
  caveat documented, not renamed.
- **Treatment:** current capability-typed dispatch at pinned kernel SHA.
- **Panel:** capability-contract on/off arm (default ON since ADR-0100 P1);
  trajectory-reduction arm (AgentDiet, Xiao et al., arXiv:2509.23586) as the
  competing cost-saver — if it matches merit-ranked dispatch, the dispatch
  result is not a win;
  pre-2026-07-28 runs excluded (EFE `resource_selector` void — see
  METHODOLOGY.md).
- **Metrics:** routing_accuracy, misroute_rate, merit dispersion
  (`bid_dispersion`), slate size, p50/p95 selection latency, LLM calls and
  tokens per selection (expect zero), cost per task.

## Apparatus

`benchmarking/` orchestration suite via `BenchmarkTransport`; routing traces
on `AuctionEventOp` (L1/L2/L3 funnel + winner margin); run manifests archived.

## Analysis

Accuracy parity test + cost/latency delta; failure-kind breakdown of
misroutes (L1 capability miss vs merit inversion vs manifest gap); merit
near-zero dispersion cases written up as a finding, not a pass.

## Risks

- Archived auction runs may not be re-runnable → fall back to reported
  numbers with version pins, flagged as non-replicated.
- Gold labels may encode auction-era assumptions → audit 20 labels manually.

## Artifact

Ablation table + routing-accuracy/latency figure → dispatch satellite paper.
Target venues: AAMAS, EuroSys/SOSP-AI, MLSys.
