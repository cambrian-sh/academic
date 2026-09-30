# Study Proposal 03 — Scope-Enforced Retrieval: Utility × Security Pareto

- **Agenda topic:** 7 · **Horizon:** 3–6 months · **Status:** proposed
  (blocked on policy benchmark suite — see "Apparatus" below)

## Research question

What recall/answer-quality cost does fail-closed scope enforcement exact,
and where does the utility curve break as policy strictness increases?

## Background

ADR-0118: `QueryPlane` requires a `TagPredicate`, scope-matched per shape,
fail-closed; agent-plane premium services gated. Observed gate:
`substrate-retrieval` 16/16 vs 12/16 baseline, zero regressions. Open:
operator-plane reads run as SystemPrincipal (unscoped), namespace hardcoded.
Threat anchors: arXiv:2506.23260 (MCP/A2A taxonomy), OWASP LLM01 + Agent
Security Bench (84% vulnerable). Practice anchor: deterministic policy
checks, auditable decisions (Zanzibar/OpenFGA lineage).

## Design (updated 2026-09-26 — full design in `../notes/policy-suite-design-2026-09-26.md`)

- **New suite `policy-asks`**, not an extension of the `access-policy`
  instrument: binary gold cannot express ADR-0121's `partial` case, where
  the entitlement claim is decided. A 24-ask calibration set keeps both
  instruments cross-checked.
- **Treatments:** policy-off (OSS AllowAll) → tag predicates → closed tags
  (ADR-0091) → party scoping (ADR-0121) → full strictness (arms A0–A4 +
  report-only sub-arms). A0 (deny-nothing engine) runs first as the suite's
  own falsification test.
- **Corpus:** the `cambrian-worldsim` 328-ask set is ABSENT from the
  workspace (verified 2026-09-26) — request the artifact day 1, but ship a
  worldsim-*shaped* generator regardless (~180 records × ~28 principals,
  ground truth from the generator per DRIFT-03 precedent, 300–340 asks,
  answer:partial:deny ≈ 45:15:40). Substitution named as honest limit.
- **Gold is attribute-level and ordered** + `expected_reason_class`, which
  makes explanation calibration measurable; 10 adversarial rephrasing
  families must not move the decision (`phrasing_invariance_rate`).
- **Metrics never blended:** 4×4 gold×observed confusion matrix;
  `deny_recall` / `deny_precision` / `over_refusal_rate` /
  `partial_correct_rate` / `authorized_recall_at_k` / `aggregate_leak_rate`.
- **Transport prerequisites (verified):** `AgentClient.query_hits`
  dropped `MemoryResponse.policy_note` — FIXED 2026-09-27
  (`MemoryHit.policy_note`, round-tripped; 62 focused tests green);
  `transport/authz.py` now wraps save/delete/list policies+links, groups,
  identity bindings, `resultant_policy`, `export_decisions` — DONE
  2026-09-27 (new `tests/transport/test_authz.py`); stubs re-vendored to
  contract 0095 (regen fixed for the `premium/` rename). Remaining: live-
  kernel verification of both. A3
  (party scoping) needs no kernel change: `party_scoped_tags` /
  `AssignPartyIdentities` exist (`core/domain/tag_predicate.go`).
- **Metrics:** task success, recall@k under policy, deny precision/recall,
  over-refusal rate, `policy_note` explanation quality (human-graded sample),
  latency overhead of enforcement.

## Apparatus

New `benchmarking/` policy suite (to build) + `substrate-retrieval` suite;
`AccessDecisionOp` / `ExplainAccess` traces as evidence.

## Analysis

Pareto frontier (utility vs strictness); over-refusal taxonomy; explanation
calibration (do denials name the true cause?); comparison point: report-only
vs enforced rollout (ADR-0087).

## Risks

- Suite construction is the long pole → timebox to 3 weeks, seed from
  worldsim asks.
- Party-scoping not yet authorable over admin plane → mark arm pending,
  publish without it if blocked.

## Artifact

Pareto figure + enforcement semantics → security satellite paper.
Target venues: IEEE S&P, USENIX Security, CCS.
