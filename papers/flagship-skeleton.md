# Flagship Paper Skeleton (v0.1 — 2026-09-29)

**Working title:** An Evidence-First Knowledge Substrate for LLM Agents:
Content-Addressed, Contradiction-Tolerant, Scope-Enforced Memory

**Status:** skeleton. Each claim row gives the evidence status: ✅ measured
(manifest/DECISIONS pointer) · 🔨 built, unmeasured · ❌ missing. No
experiment counts without a `FREEZE.md` row newer than F-2026-09-26.

## Abstract (stub — write last)

[One paragraph: agents need memory that survives contradiction, enforces
policy, and explains itself; we built a 7-phase substrate in a production
Go kernel and measured X, Y, Z. Fill numbers when arms complete.]

## 1. Introduction

- Agent memory today: overwrite stores, no contradiction representation,
  policy as prompt text, evaluation by final answer.
- Thesis: memory should be an evidence-first substrate — content-addressed
  ingestion, append-only knowledge with derived resolutions, typed queries
  with guarantee labels, kernel-enforced scope.
- Contributions: (a) design (7 phases, ADRs 0105–0111); (b) guarantees
  (INV-1/INV-3, implication rendering, resolution purity); (c) evaluation
  (gates + benchmark arms + cost); (d) honest limits.

## 2. Related work

See `papers/related-work-draft.md` §1 (promote to prose here at draft v0.2).

## 3. Substrate design (7 phases)

| Phase | Mechanism | Source |
|---|---|---|
| 1. Evidence foundation | content-first commit, transactional outbox, `(namespace, source_id, source_key, source_revision)` idempotency | ADR-0105 |
| 2a. Commitments | append-only items, no uniqueness on (entity, predicate, validity); resolutions derived via pure `ResolveLatestAssertion` | ADR-0106 |
| 2b. Substrate backing | default-on, erasure parity, ~2 ms p95 write cycle | ADR-0106 addendum |
| 3. Embeddings as projection | dual-write, per-model partial HNSW, backfill RTOs | ADR-0107 |
| 4. Event-shaped knowledge | roles as rows, partitioned observations, outbox consumer | ADR-0108 |
| 5. Quantitative policy | handbook bounds extracted once, five-way outcomes | ADR-0109 |
| 6. Kind registry | `KindSpec`/`ValueSpec`, boot-refused bad specs | ADR-0110 |
| 7. Typed query plane | closed AST, 9 kinds, `ErrCannotExpress` refusals with reasons | ADR-0111 |
| Read scope | required `TagPredicate`, fail-closed shapes, agent-plane gating | ADR-0118 |

## 4. Guarantees (formal bits)

- INV-1 (intersection preservation) incl. party-scoping implication rendering.
- INV-3 (`policy_note` gate; substrate lane silence as known residual).
- Resolution order-independence (permutation-tested).
- Query inexpressibility as a feature (`ErrCannotExpress` taxonomy).

## 5. Evaluation — claim-evidence map

| # | Claim | Evidence | Status |
|---|---|---|---|
| 1 | Content-first ordering prevents silent loss | crash/replay failpoint gates (`internal/evidence/ingestor_test.go`); live 3-ingest validation | ✅ measured (in-repo) |
| 2 | Contradiction without history destruction; drift parity 0/244 | drift suite row-identical across backings; PO-800 cross-actor gate | ✅ measured (in-repo) |
| 3 | Projection fidelity across model round-trips, zero refusals | 50/50 top-10 fidelity; bge→nomic→bge trip | ✅ measured (in-repo) |
| 4 | Scope enforcement costs X recall for Y security | substrate-retrieval 16/16 vs 12/16 baseline | ✅ pilot; ❌ Pareto curve needs `policy-asks` A1–A4 (proposal 03; suite v0 built, arms need kernel) |
| 5 | Query guarantees hold; refusals name reasons | 5/5 malformed refusals; 10/10 live gate | ✅ measured (in-repo) |
| 6 | Multi-signal ranking contribution + cost | ablation grid (proposal 02) | ❌ needs kernel arms |
| 7 | Provenance costs ~3% loaded; hot-path TBD | 2026-08-06 A/B/A (+210–250 ms mean); hot-path arm open | 🔨 half measured (proposal 04 rescoped) |
| 8 | Utility×security Pareto + over-refusal taxonomy | policy-asks arms + calibration | ❌ blocked on live runs |
| 9 | Graph-signal reproduction (B8 negative) | chunking/document-qa/musique matched-budget runs | ❌ needs kernel; flagship-gating |
| 10 | Consolidation measurement (AFTER instrument) | LoCoMo before/after + ExpeL/AutoGuide arms | ❌ blocked on ADR-0094/0095 build |

## 6. Honest limits (write fully — reviewers reward this)

- ADR-0109: synthetic vocabulary-aligned corpus; "most policy is quantitative" unproven on real handbooks.
- Worldsim 328-ask set absent; policy-asks is worldsim-shaped, substitution named.
- Capability-contract numbers pre-2026-07-28 void (EFE selector bug).
- Auction-era metric names kept for archive comparability.
- Operator-plane reads unscoped (SystemPrincipal); namespace hardcoded `default`.
- Substrate lane has no `policy_note` (silence expected, reported as residual).
- Graph signal contested: Selective Forgetting (2608.28978) measured graph
  losing to flat baseline on LongMemEval at matched budget — reproduce on
  chunking/document-qa/musique before review; mechanism (entity
  decomposition discards surface form) argues for verbatim Evidence.
- Per-write validation insufficient as memory-safety story (Salami
  coalitions, SPORE egress bypass); scope claims bounded accordingly.

## 7. Conclusion

[Restate thesis + strongest numbers + what remains.]

## Next actions (in order)

- [ ] New freeze row + kernel SHA pin; run proposal 04 hot-path arm.
- [ ] `policy-asks` A0 falsification run, then A1/A2; prediction doc first.
- [ ] Proposal 02 ablation grid.
- [ ] Fill §5 rows 4, 6, 8; write abstract last.
- [ ] Advisor review of claim-evidence map before any prose expansion.
