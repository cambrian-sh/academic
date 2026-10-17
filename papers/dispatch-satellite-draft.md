# Dispatch Satellite Draft (v0.1 — 2026-10-09)

**Working title:** Zero-RPC Merit-Ranked Dispatch: Routing-Decision
Overhead, Not Task-Level Cost

**Status:** draft v0.1 — all non-results sections written from settled
material (proposal 01, related-work §2, METHODOLOGY.md rules). Every
results cell is a TBD placeholder tied to a future `FREEZE.md` row and
run manifest. No number in this file is a claim until its manifest
exists. Abstract written last.

Target venues (per proposal 01): AAMAS, EuroSys/SOSP-AI, MLSys.

## Abstract (stub — write last)

[One paragraph: multi-agent dispatch pays a selection tax every turn; we
retired auction selection for zero-RPC capability-typed dispatch in a
production Go kernel and measured routing-decision overhead to X with
accuracy parity Y. Fill X, Y when P1–P4 + P11 complete.]

## 1. Introduction

- Every dispatch decision in a multi-agent system consumes tokens,
  latency, and (when the router is itself a model) an LLM call — before
  any task work happens. This selection tax compounds per turn.
- Thesis: the defensible headline for a dispatch study is
  **routing-decision overhead** — the marginal cost of the selection step
  itself — not task-level cost, which restates compute (Tran & Kiela DPI
  confound, `tran2026single`).
- Cambrian's answer (ADR-0100): Gatekeeper→Dispatcher capability-typed
  dispatch with zero RPC in the selection path — `LLM calls and tokens
  per selection = 0` as the PRIMARY result; everything else a Pareto
  point at matched tokens.
- Contributions: (a) design (merit rank over declared capabilities,
  capability contracts, auction retirement); (b) evaluation (P1–P4 + P11
  arms, TES-style gain-per-token, model×harness reporting); (c) honest
  limits including the negative result the study must survive.

## 2. Related work

See `papers/related-work-draft.md` §2 (promote to prose here at draft
v0.2). Baseline pool for the paper: ZeroRouter (`zerorouter2026`),
IRT-Router (`irtrouter2025`), ContextualRouter (`varangot2026contextual`),
PORT (`wu2025trainingfree`), RouterEval (`routereval2025`); threat-model
edge Rerouting (`shafran2025rerouting`). Market-selection external
evidence: MarketBench (`fradkin2026marketbench`), Agora (`zhou2026agora`,
EMNLP 2026 Findings).

## 3. Dispatch design

[Verified against ADR-0100 text, git rev `102aa09` (2026-10-17) — the
last revision carrying `docs/adr/`; core main has since deleted the ADR
tree.]

- **Control:** bid-based auction selection (retired ADR-0100 P3).
  ADR-0100 records the P2 comparative run as **not performed** and
  permanently unrunnable ("after P3 there is no arm to compare"); the
  archived 2026-07-29 auction arm is degraded (112/120 rows errored,
  5 auctions observed) and counts as corroboration, not a clean gate.
  The dependable baseline is the dispatch-only absolute floor
  (`routing_accuracy` / `misroute_rate` / `no_candidate`); archived
  auction numbers are reported version-pinned, metric names kept for
  archive comparability (`bid_dispersion`, `candidates_per_auction`).
- **Treatment:** capability-typed dispatch at pinned kernel SHA; the
  pre-registered arms are P1–P4 + P11 only. `execution.capability_contract`
  default ON since ADR-0100 P1 (ADR-0100: "a flag that selection depends
  on is not an arm" — contract-off would be a degradation condition if
  probed, not a study arm).
- **Selection rule placement:** per the Zero-Hardcode Rule, routing
  merit lives in declared capability data, not in model calls — the
  position ContextualRouter's k-NN result independently supports
  (retrieval over past performance matches learned routers).
- **Competing cost-saver:** trajectory-reduction arm (AgentDiet) — if it
  matches merit-ranked dispatch, the dispatch result is not a win.

## 4. Evaluation — claim-evidence map

| # | Claim | Arm / evidence | Status |
|---|---|---|---|
| 1 | Selection costs 0 LLM calls / 0 tokens | P1 token-normalised control; `tokens_per_selection` row metric | TBD — needs freeze row + manifest |
| 2 | Routing accuracy parity vs auction | accuracy parity test on 120+20 gold labels | TBD — 20-label audit done 2026-10-09 (19 CONFIRM / 1 systemic AMEND); countersign + amendment pending |
| 3 | p50/p95 selection-latency delta | P1–P4 latency rows | TBD |
| 4 | Win survives harness-only control | P3 orchestration-only arm; must survive Arjmandi negative (`arjmandi2026harnessmodel`) | TBD — gating |
| 5 | Win survives LLM-router + learned-router arms | P4 (Router-R1 pattern) + P5 pool (ZeroRouter/IRT/PORT) | TBD |
| 6 | TES-style gain-per-token reported | `wani2026reasoningtax` metric family; no raw-cost-only claims | TBD — method settled |
| 7 | Misroutes break down by kind, not aggregate | Cemri taxonomy; L1 miss vs merit inversion vs manifest gap | TBD |
| 8 | Sham-policy placebo shows the rig can report null | P11 Zhang placebo (`zhang2026harnessvalue`) | TBD — implement first with P1–P4 |
| 9 | Capability reported at model×harness level | Harness-Bench rule (`yao2026harnessbench`) | TBD — reporting rule settled |

Pre-2026-07-28 runs excluded (EFE `resource_selector` void).

## 5. Honest limits (written fully now)

- Auction-era metric names kept deliberately; mapped, not renamed.
- Pre-2026-07-28 capability-contract numbers void (EFE selector bug).
- Gold labels may encode auction-era assumptions — 20-label audit done
  2026-10-09 (19 CONFIRM / 1 systemic AMEND on prompt-invisible
  `test_execution`); countersign + amendment pending; until then,
  accuracy parity stays provisional even after runs.
- Archived auction runs may not re-run (ADR-0100 P3 excised the arm; P2
  recorded not-performed) — fallback is reported numbers with version
  pins, flagged non-replicated; the recorded auction artifact
  (`runs/p2-ab-auction`, 2026-07-29) is degraded (112/120 rows errored).
- The study must survive: Tran & Kiela DPI confound (matched tokens or
  nothing), the Arjmandi neither-harness-wins negative, and its own P11
  placebo.
- Rerouting-class attacks bound the claim to deterministic merit rank,
  not learned routers.

## 6. Conclusion

[Restate thesis + primary result + what the limits exclude. Last.]

## Next actions (in order)

- [x] ADR-0100 §3 verification (2026-10-17, git rev `102aa09`).
- [ ] Gold-audit countersign + `test_execution` amendment (user).
- [ ] Prediction fill pre-P1 (`benchmarking/docs/proposal-01-pre-registration.md`).
- [ ] New freeze row + kernel SHA pin (needs `CAMBRIAN_BINARY`).
- [ ] Runs → fill §4 → abstract last → advisor review of map before
  prose expansion.
