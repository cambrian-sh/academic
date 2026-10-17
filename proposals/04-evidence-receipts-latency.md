# Study Proposal 04 — Evidence Receipts Hot-Path Latency Arm (ADR-0103)

- **Agenda topic:** 4 · **Horizon:** 1 month · **Status:** proposed
  (single-session executable once kernel access is available)

## Research question

What does per-retrieval decision provenance cost on the hot path, and what
auditability does it buy? (The ADR's explicitly unrun arm.)

## Background (updated 2026-09-26 — premise corrected)

ADR-0103: OSS seam only — kernel emits a value-copy `RetrievalDecision` per
completed retrieval to registered observers (post-assembly, non-blocking, no
error path); consumer (hash chain, Ed25519, Postgres store, read-only lane)
lives in premium. **Correction to the original proposal premise:** the
loaded-arm cost HAS been measured since (CONTEXT.md, DECISIONS 2026-08-06 —
three-run A/B/A, locomo recall-only ×200, knob `CAMBRIAN_RECEIPTS_DISABLED`):
full lane (receipts + audit chain) ≈ +210–250 ms mean (~3%), ≈ +530 ms p95
(~5%) per retrieval question on an LLM-bound (~7.2 s) deployment; control-vs-
control −44 ms (drift ruled out); no tail amplification; recall unchanged.
What remains unmeasured per the ADR is the **hot-path (retrieval-only, no
LLM) cost** — on an LLM-bound deployment the ~3% is noise-drowned; the
honest number needs an LLM-free microbenchmark. This proposal is rescoped to
that arm. Siblings: TRACE (CIKM 2026, accountable retrieval),
OverclaimBench (unreliable agent self-reports), Gaia2/ARE verifier (0.98
agreement action-level verification as RLVR signal).

## Design (rescoped)

- **Arms:** (a) no observer (nil check); (b) OSS emit + null observer;
  (c) emit + full premium consumer (sign + store); (d) arm (c) on
  retrieval-only load (no LLM synthesis — the missing arm).
- **Metrics:** per-retrieval observer overhead isolated from LLM time
  (p50/p95/p99), throughput delta, store growth per 1k retrievals,
  chain-verify time; audit queries answered
  (`ExecutionConfig.RetrievalFingerprint`).
- **Load:** synthetic retrieval sweep (LLM-free) + cite the 2026-08-06
  LLM-bound numbers as the loaded reference; re-run them only if the kernel
  SHA changed since.
- **Wiring 2026-10-09:** pin + report the inference serving config with
  the numbers (stateful KV-cache persistence halves wall time,
  `norgren2026stateful`; ST-BoN-style early truncation −80% GPU mem,
  `wang2025stbon` — either drifting between arms voids the comparison).
  Report at model×harness configuration level (`yao2026harnessbench`).

## Apparatus

Kernel with receipts plugin; `RetrievalDecision` observer harness;
`benchmarking/` agentic-retrieval suite for the loaded arm.

## Analysis

Latency-cost table with CIs; cost-per-audit-query figure; recommendation
(always-on vs sampled emission) with the sampling-integrity caveat stated.

## Risks

- Premium consumer may need build fixes → fall back to arms (a)+(b) +
  modeled store cost, flagged.
- Stage attribution absent by design (D8) → do not claim it; note as limit.

## Artifact

Latency table + auditability note → provenance section of flagship;
standalone workshop paper if results surprise (e.g. negligible cost).
