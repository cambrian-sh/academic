# Cambrian Methodology — From Harness to Academic Evaluation

How each Cambrian evaluation mechanism maps to a defensible academic method.

## Apparatus inventory (verified 2026-09-20)

- `benchmarking/` suites (black-box via `BenchmarkTransport`, row traces,
  `run_manifest.json` provenance): `orchestration` (plan-quality-lab, 120+20
  tasks), `agentic-retrieval`, `chunking` (220 Q, 4 categories), `document-qa`
  (600 chunks, 4060 Q), `gaia-tool-use`, `locomo`, `musique`, `memory-guard`,
  `operator-sweep`.
- Per-row artifacts: `tool_calls`, `latency_ms`, `plan_text`,
  `rubric_scores`, `critical_violations`, `failure_kind`, routing metrics,
  `agent_steps` (memory-query thrash, self-retrieval rates).
- Kernel-side gates recorded in ADRs/DECISIONS (e.g. substrate-retrieval
  16/16, drift parity 0/244, projection fidelity 50/50).

## Mapping to academic standards

| Academic requirement | Cambrian equivalent | Gap to close |
|---|---|---|
| Dataset description | `datasets/` + suite CONTEXT entries | Version + freeze a DOI-able snapshot |
| Baseline | Pre-change control arms (`runs/rt_diag4` pattern) | Name baselines explicitly per study |
| Ablation | Config-arm sweeps (signals, retrievers, policies) | Pre-register arm grids |
| Statistical validity | Row-level traces, failure kinds (external authority: Cemri et al., arXiv:2503.13657) | Add CIs / significance (mostly absent today) |
| Reproducibility | `run_manifest.json` + deterministic seeds | Publish manifests + kernel version pins |
| Threats to validity | ADR residual/honest-limit notes | Promote into paper sections |
| Human evaluation | Plan rubrics (partial) | Calibrate rubric vs human judges — recipe now exists: ResearchRubrics artifact (`sharma2026researchrubrics`) + CROME causal calibration (`srivastava2026crome`) + noisy-judge CIs (`feng2026noisyvalid`) |

## Minimum bar for any paper experiment

1. Frozen dataset snapshot + kernel version (`v0.6.10-Alpha` → exact SHA).
2. Control arm + treatment arm(s), pre-registered.
3. Row-level results committed (traces summarized, manifests referenced;
  trajectory-over-final-answer prior art: Tian et al., arXiv:2609.16453).
4. Failure-kind breakdown, not just aggregate scores.
5. Cost reported alongside quality (tokens, latency, $).
6. Honest-limits paragraph (synthetic corpora, unrun arms, residuals).

## Known methodological debts (do not hide these)

- Pin the resource envelope + time-of-day for every benchmark run (Segato
  2026: uncapped beats 1× by 6pp; gaps <3pp deserve skepticism). Add to
  every freeze row.

- Routing metrics keep auction-era names (`bid_dispersion`,
  `candidates_per_auction`) — explain, don't rename silently.
- Some suites predate fixes (e.g. pre-2026-08-08 `scout_usefulness` keys are
  not comparable to newer runs).
- ADR-0109 honest limit: synthetic vocabulary-aligned corpus; "most policy is
  quantitative" unproven on real handbooks.
- Capability-contract numbers before the 2026-07-28 EFE fix are void.
