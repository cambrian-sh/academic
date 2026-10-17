# Security Satellite Draft (v0.1 — 2026-10-09)

**Working title:** Scope-Enforced Retrieval: The Utility × Security Pareto
of Fail-Closed Agent Memory

**Status:** draft v0.1 — all non-results sections written from settled
material (proposal 03, policy-suite design note, related-work §3,
`policy-asks` suite v0 + prediction scaffold). Every results cell is a
TBD placeholder tied to a future `FREEZE.md` row and run manifest. No
number in this file is a claim until its manifest exists. Abstract
written last.

Target venues (per proposal 03): IEEE S&P, USENIX Security, CCS.

## Abstract (stub — write last)

[One paragraph: agent memory needs policy with teeth, not prompt text;
we built fail-closed scope enforcement (tag predicates, closed tags,
party scoping) into a Go kernel's retrieval plane and measured what
recall it costs: Pareto frontier X, over-refusal taxonomy Y, explanation
calibration Z. Fill X, Y, Z when A0–A4 complete.]

## 1. Introduction

- Agent memory threats are read-side (query-only injection), write-side
  (PoisonedRAG: 90% ASR with 5 texts), and registration-side (MCPTox:
  tool-poisoning is not an IPI variant, refusals <3%). Prompt-text policy
  answers none of them.
- Thesis: enforcement belongs in the retrieval plane as fail-closed
  scope — required `TagPredicate` per query shape (ADR-0118), closed tags
  (ADR-0091), party scoping (ADR-0121) — with the price reported honestly
  as a Pareto frontier, not a single accuracy number.
- The question the paper answers (proposal 03): what recall and answer
  quality does strictness cost, and where does the utility curve break?
- Contributions: (a) enforcement semantics (predicates → closed tags →
  party scoping → full strictness); (b) the `policy-asks` instrument with
  attribute-level gold including the `partial` case binary gold cannot
  express; (c) Pareto + over-refusal taxonomy + explanation calibration;
  (d) honest limits.

## 2. Related work

See `papers/related-work-draft.md` §3 (promote to prose here at draft
v0.2). Lead citations: AgentBound (`agentbound2026`) for "model proposes,
kernel disposes"; PoisonedRAG (`zou2024poisonedrag`) as the write-side
baseline the substrate must be measured against; MCPTox (`mcptox2025`)
as the tool-registration complement; Dong query-only injection
(`dong2025meminject`) as the reason policy binds queries, not mutations.

## 3. Enforcement design

[Verified against ADR-0091 (closed tags), ADR-0118 (read scope), and
ADR-0121 (row-level entitlement) text, git rev `102aa09` (2026-10-17) —
last revision carrying `docs/adr/`.]

- **A0 policy-off (OSS AllowAll)** → A1 tag predicates → A2 closed tags →
  A3 party scoping → A4 full strictness (+ report-only sub-arms).
- **Grants are the single additive term and are deliberately crippled**
  (ADR-0091 D3): a grant may only reopen a closed tag and can never
  override a bound — "grants union; everything else intersects" is part
  of A2's reported semantics, not a tuneable.
- **A0 runs first, alone**, as the suite's own falsification test: a
  deny-nothing engine must leak everything, else the corpus does not
  discriminate (prediction scaffold rule).
- **Gold is attribute-level and ordered** with `expected_reason_class`,
  which makes explanation calibration measurable; denials carry cause
  text by design (ADR-0091 D5: a closed-tag denial names the tag and the
  grant state), giving claim 4 a design counterpart; 10 adversarial
  rephrasing families must not move decisions
  (`phrasing_invariance_rate`).
- **Metrics never blended:** 4×4 gold×observed confusion matrix;
  `deny_recall` / `deny_precision` / `over_refusal_rate` /
  `partial_correct_rate` / `authorized_recall_at_k` / `aggregate_leak_rate`.
- **Calibration:** 24-ask set keeps `policy-asks` cross-checked against
  the `access-policy` instrument (par code-grounded).
- Transport is built (policy_note round-tripped; authz wraps; contract
  0095 stubs) but **live-kernel verification is open** — no enforcement
  claim until then.

## 4. Evaluation — claim-evidence map

| # | Claim | Arm / evidence | Status |
|---|---|---|---|
| 1 | Corpus discriminates (A0 leaks ~all) | A0 falsification run, 352 frozen asks | TBD — needs freeze row + manifest |
| 2 | Utility × strictness Pareto | A0→A4 deny_recall / authorized_recall_at_k curve | TBD |
| 3 | Over-refusal taxonomy + rates | over_refusal_rate per arm; Sonnet-deletes-legit-rule analogue noted | TBD |
| 4 | Denials name true causes | explanation calibration vs expected_reason_class; human-graded sample | TBD |
| 5 | Decisions survive rephrasing attacks | 10 families; phrasing_invariance_rate | TBD |
| 6 | Partial case decided correctly | partial_correct_rate (the binary-gold-inexpressible claim) | TBD — needs A2+ |
| 7 | Parity with access-policy instrument | 24-pair agreement (excl. cal-21) | TBD — same-session run |
| 8 | Enforcement latency overhead | per-arm latency rows | TBD |
| 9 | Report-only vs enforced comparison point | ADR-0087 rollout contrast | TBD |

Prediction scaffold (`benchmarking/docs/policy-asks-baseline-prediction.md`)
is TBD-gated: fill post-A0, pre-A1. Directions recorded: deny_recall rises
A0→A2; over_refusal rises; authorized_recall_at_k falls.

## 5. Honest limits (written fully now)

- Worldsim 328-ask set absent; `policy-asks` is worldsim-shaped by
  construction (180×28→352) — substitution named, provenance gap open.
- A3 party-scoping arm PENDING unless transport + `parties_from` ingress
  land; study publishes A0–A2 + report-only arms with the gap named if
  blocked.
- Operator-plane reads run as SystemPrincipal (unscoped); namespace
  hardcoded `default` — enforcement claims cover the agent plane only.
- Per-write validation is insufficient as a safety story (Salami
  coalitions falsify it; SPORE bypasses store-level isolation via egress)
  — scope claims bounded accordingly; chokepoints necessary, stated.
- Recency + provenance insufficient for drift-vs-poisoning (CAPTURE);
  the abstain path (`ErrCannotExpress` analogue) is supported, not proven.
- Corpus is synthetic; "most policy is quantitative" unproven on real
  handbooks (shared limit with flagship row).

## 6. Conclusion

[Restate thesis + Pareto shape + what the limits exclude. Last.]

## Next actions (in order)

- [x] ADR-0091/0118/0121 §3 verification (2026-10-17, git rev `102aa09`).
- [ ] New freeze row + kernel SHA pin (needs `CAMBRIAN_BINARY`).
- [ ] Prediction fill post-A0, pre-A1 (gated, not writable now).
- [ ] A0 falsification → A1/A2 + parity → abstract last → advisor review
  of map before prose expansion.
