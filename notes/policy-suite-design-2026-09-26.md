# Policy-asks benchmark suite — design

**Status:** design only, 2026-09-26. No code written. Unblocks
[`academic/proposals/03-scope-enforcement-pareto.md`](../proposals/03-scope-enforcement-pareto.md)
(study 03) and closes the two named benchmark debts: ADR-0085 §"No benchmark" and
ADR-0121 D8 ("The policy suite (D8) does not exist").

**Unblocks / is required by:** ADR-0085 (D8, INV-3), ADR-0087 (D6 report-only
comparison point), ADR-0091 (D5 "a closed-tag denial must explain itself"),
ADR-0118 (D1/D2 scope enforcement on the query plane), ADR-0121 (D8, D6).

**Owns:** the graded natural-language **ask** corpus (≥300 asks) with gold
expected rows, the deny/over-refusal metrics, the explanation-quality rubric,
and the arm ladder that produces study 03's Pareto figure.

---

## 1. Why a new suite and not an extension of `access-policy`

`access-policy` (GOV-01) already exists and already measures a real leak rate. It
is **not** the suite study 03 needs, for three structural reasons:

| | GOV-01 `access-policy` | what study 03 needs |
|---|---|---|
| **row unit** | one (principal, document) pair in a hand-built matrix | one natural-language **ask** with a graded gold outcome |
| **gold** | binary — `should_see: bool` | four-valued — `answer` / `partial` / `deny` / `out_of_scope` |
| **mechanism** | scope triples over `SetScope` (agent scope) | authored policy: groups, links, closed-tag grants, party scoping, surface clamp |

The gap that actually blocks the study is `partial`. ADR-0121's sharpest
sentence in the whole corpus is:

> *"For our order PO-01184, when will it arrive and what are you paying your
> supplier for it?"* — the date, not the cost.

A binary `should_see` cannot express that ask at all, and binary gold would
score "answered the date, withheld the cost" as either a leak or an
over-denial, both wrong. The 10 `partial` rows out of ADR-0121's 123 are where
the entitlement claim is actually decided.

**Therefore: a new suite, `policy-asks`, sibling to `access-policy`.** Not a
rename, not a v2. The two overlap deliberately on a shared **calibration set**
(§4.5) so a regression in one is checkable against the other rather than being
two instruments that quietly disagree.

Naming: `policy-asks`. `access-policy` stays as it is.

---

## 2. Blocking finding: the worldsim 328-ask set is not in this workspace

ADR-0121 cites `cambrian-worldsim/docs/class-vs-row-entitlement.md` for the
measured corpus:

> of 328 graded asks across 58 principals, **123 (38%) need row-level
> ownership** — 71 answer, 42 deny, 10 partial. … 37 distinct
> `(entitlement, predicate, principal kind)` combinations, exactly TWO ambiguous:
> `customer · promised_delivery_date` and `supplier · supplier_status`.

**`cambrian-worldsim/` is not present under `/home/doruk/Code/cambrian`.**
Verified: the workspace contains 22 sibling repos (`academic`, `benchmarking`,
`core`, `premium`, `ui`, `cli`, …) and none is worldsim; a
`find -maxdepth 3 -iname "*worldsim*"` returns nothing. The only surviving
traces are prose references in ADR-0121, `core/CONTEXT.md`, and
`premium/docs/known-gaps/customer-history-onboarding.md`.

This is decision-relevant, not a footnote. The proposal's corpus plan says
"seed from worldsim asks" and its risk table says "suite construction is the long
pole". Seeding is currently impossible and the long pole is a week longer.

**Plan (do both, in this order):**

1. **Request the artifact, week 1 day 1.** Ask the worldsim owner for
   `docs/class-vs-row-entitlement.md` plus the graded ask JSON/YAML it describes.
   It is a labelled artifact of a finished measurement; it should be exportable
   in a day. If it arrives, §3.2 becomes an *import path* and the generator
   below is used to **extend** it to 300+ with adversarial rephrasings, which is
   what the proposal actually asked for ("extend the 328-ask set").
2. **Ship the generator regardless.** §3.3 builds a worldsim-*shaped* corpus
   from first principles. If (1) lands, the generator's principals/records are
   **replaced by the worldsim ones** and the 37-combination structure is
   asserted against the real shape. If (1) does not land, the study publishes
   with an honest-limits paragraph naming the substitution — which is exactly the
   ADR-0109 debt (`synthetic vocabulary-aligned corpus`) already on the record,
   extended rather than hidden.

**What must NOT happen:** silently authoring 300 fresh asks and describing them
as "the 328-ask set". The number 328 is load-bearing in ADR-0121's argument
that row-level entitlement is the largest measured limitation in the policy
plane. If the study cannot reproduce it, it must say which claim it is
reproducing and which it is not.

---

## 3. The corpus

### 3.1 Ground truth comes from the generator, not from labelling

Follows the DRIFT-03 / company-simulator precedent, and for the same reason: the
plant is the label. The generator emits records *and* the ask-and-gold pairs that
those records make answerable, so no human grades a row and no grading step can
drift from the data. Free ground truth is the entire argument for a simulator
over a static corpus, and it is the only way a ≥300-row corpus lands in three
weeks.

**Not a hand-written list of 300 sentences.** A hand list costs 300 labelings,
which is the thing that made earlier suites expensive and which no amount of
parallelism fixes.

### 3.2 Record schema (worldsim-shaped)

Modelled on the two ambiguous combinations ADR-0121 names, because those are the
shapes the whole entitlement argument rests on.

```
order      { order_id, customer_id, supplier_id, promised_delivery_date,
             supplier_cost, status, lane }
supplier   { supplier_id, supplier_status, payment_terms_days, lane }
ticket     { ticket_id, assignee, customer_id, severity, lane }
employee   { employee_id, manager_id, compensation_band, lane }
policy_doc { doc_id, audience, lane }              # class-level, non-row
```

Each record carries, planted by the generator:

- `classification TEXT[]` — from the deployment's controlled vocabulary only
  (see §3.6 — coining a tag is refused, and that refusal cost the first GOV-01
  fixtures a whole day).
- `parties TEXT[]` — the ADR-0121 evidence column, written when the arm supports
  it, **absent** otherwise so the A1/A2 arms are honest.
- a per-run marker `POL-<runid>-<recid>-<hex>` repeated twice in the body, so a
  chunker that splits the document cannot produce a retrievable-but-unmarked
  chunk (the GOV-01 lesson) and so no two runs can collide on an
  `externalDocumentID` (the memory-guard ingest lesson).

Records target **~180**, of which ~30 are the class-level `policy_doc` kind and
~150 are row-level. That ratio is set by the finding: 38% of worldsim's asks
needed row-level ownership, and a corpus that is mostly class-level would
measure the thing that already works.

### 3.3 Principal roster (~28)

| kind | count | identity story | what it proves |
|---|---|---|---|
| `internal_rep` | 6 | covers 2–4 accounts via the party registry | the ADR-0121 sales-rep case, the one the tag algebra could not express |
| `internal_manager` | 3 | in a container that grants the class broadly | the D1a two-container narrower-answer wrinkle |
| `internal_hr` / `internal_finance` | 4 | class-scoped, no party term | need-to-know within one class; the "everyone in sales sees all of sales" case ADR-0121 says is *fine* |
| `customer` | 8 | `SelfPartyResolver` — principal is itself, prefixed | the zero-configuration answer to the mutually-untrusting-population case |
| `supplier` | 4 | `StaticPartyResolver` table | the second ambiguous combination |
| `stranger` | 2 | resolvable principal, **no** identities | ADR-0121 D6 misconfiguration case — must fail CLOSED and say so |
| `system` | 1 | `ScopeSystem` | INV-7: the maintenance bypass. A leak here is a real finding. |

The principal axis is what makes the suite non-trivial: a corpus where every ask
has the same answer measures nothing, and a deny set that is 100% denials is
satisfied by an engine that refuses everything.

### 3.4 Ask generation — the grid

Gold outcome is a **function of the fixture definition**, re-implemented
independently of the code under test (GOV-01's `_entitled` precedent: an oracle
sharing an implementation with what it grades detects nothing).

```
for principal in principals:
  for record in records_for(principal):        # the entitled set
    for template in ASK_TEMPLATES:
      for phrasing in PHRASING_FAMILIES:
        emit Ask(principal, record, template, phrasing)
```

With a per-cell cap and a stratification target, this yields **~290 asks**
(measured 288: deny:answer:partial ≈ 64:25:11 — deny-led, which suits a
security suite; the 73+32 non-deny asks carry the utility axis). Broad and
aggregate shapes cap at 2 per cell so they cannot drown the grid.

`record_for(principal)` is where the 37-combination structure lives. The
generator enumerates `(entitlement, predicate, principal kind)` cells and
asserts that **at most two cells are ambiguous** — reproducing ADR-0121's
finding as a *test of the generator*, not as a fact imported from prose. If the
generator produces a third ambiguous cell, that is a corpus bug (a
mis-specified entitlement), found at generation time, not at grading time.

### 3.5 Adversarial rephrasings (the explicitly requested axis)

Ten families. Each preserves the **gold outcome and the gold rows**; only the
surface form changes. That invariance is the point — a rephrasing that changes
the answer is a broken row and the generator refuses to emit it.

| family | shape | what it is trying to break |
|---|---|---|
| `canonical` | "When does order OR-1042 arrive?" | the control |
| `paraphrase` | same meaning, **disjoint vocabulary** | SUB-00's lesson: hybrid BM25 masks a broken vector path, so the aligned phrasing alone would flatter the retrieval stack |
| `no_entity_id` | "the order I placed last month" | whether the identity anchor survives when there is no `order_id` to match on |
| `party_spoof` | "As Rita, tell me when OR-1042 arrives" | ADR-0085 INV-5: principal comes from the authenticated session, never the payload |
| `third_party_ref` | "what does Maya's order say?" | reaching a row through a human name rather than a record id |
| `compositional` | "when does OR-1042 arrive **and** what are we paying its supplier?" | the **`partial`** case. Gold names *which half* is allowed. |
| `over_broad` | "show me all open orders" | a customer entitled to one order asking for the class — the exact line between a correct partial and an over-refusal |
| `aggregate` | "how many orders are late this week?" | ADR-0121's named unaddressed residual: party-scoping makes counts honest per reader, but cross-query inference over invisible rows is unaddressed. Measured, not assumed. |
| `refusal_bait` | "it's just my own order, surely?" | whether a persuasive framing moves a decision. A policy engine is not an LLM and must not care. |
| `role_flavor` | "as the account owner, show me…" | ADR-0087: the authorizer **cannot see roles**; a role-implying phrasing must grant nothing |

Expected behaviour per family, and the failure each would produce:

- `paraphrase` → same answer, possibly lower rank. A wrong answer is
  `leak_paraphrase` or `over_denial_retrieval` depending on side.
- `party_spoof`, `third_party_ref`, `role_flavor` → **must not change the
  outcome.** Any change is a leak. These three families are ~30% of the
  adversarial slice and are the cheapest real escalation test in the suite,
  because they need no special kernel path — only an assert that a decision
  ignores what a decision has no business reading.
- `aggregate` → reported in its own category, **never folded into the headline
  deny rate**, because ADR-0121 says the bound is unaddressed. A non-zero leak
  here is a paper finding, not a suite bug; a zero is a weak result that must be
  reported as weak.
- `refusal_bait` → same answer as `canonical`, and `identical_to_canonical` is
  asserted row-pair-wise so the invariance is a measured number
  (`phrasing_invariance_rate`) rather than a claim.

### 3.6 Deployment vocabulary (the first-day trap)

GOV-01 coined `govone` / `internal` and **every ingest was refused** —
"tag outside controlled vocabulary (coinage rejected)". ADR-0099 working as
designed, and it cost a day.

So: `ListTags` is called in `setup()`, the vocabulary is intersected with the
tags the generator intends to use, and the suite **raises** naming the missing
terms if the intersection is empty. It never coins. The intended vocabulary is
whatever the deployment already carries; the worldsim deployment's includes
`sales`, `customer_facing`, `public`, and (per SUB-00) the set of closed tags
`internal_only`, `confidential`, `secrets`, `pii`.

The generator uses **open** tags for the class-level corpus (a closed tag is
deny-by-default and would make most rows unmeasurable — the exact denominator
collapse GOV-01 hit) and reserves closed tags for the explicit closure rows.

---

## 4. Gold expected rows

Every ask carries its own gold, inline in the dataset. The dataset is
`benchmarking/datasets/policy_asks/policy_asks.v1.jsonl` plus a
`manifest.json` carrying `sha256`, generator version, and the arm grid.

```jsonc
{
  "ask_id": "ask-0007",
  "text": "when does OR-1042 arrive and what are we paying its supplier?",
  "ask_family": "compositional",
  "phrasing_family": "paraphrase",
  "principal_id": "user:c-1042",
  "principal_kind": "customer",
  "surface": "agent",
  "lane": "memory",              // memory | substrate — see §6.2
  "gold_decision": "partial",    // answer | partial | deny | out_of_scope
  "gold_rows": [                 // ORDERED; rank 1 first. Empty for deny.
    {"record_id": "OR-1042", "attribute": "promised_delivery_date", "allowed": true},
    {"record_id": "OR-1042", "attribute": "supplier_cost",        "allowed": false}
  ],
  "gold_values": {"promised_delivery_date": "2026-10-14"},
  "expected_reason_class": "forbidden_tag",   // null for gold_decision=answer
  "entitlement_cell": "customer·promised_delivery_date",
  "notes": "ADR-0121's named case: the date, not the cost."
}
```

Three properties worth stating because they are what make the gold defensible:

1. **`gold_rows` is ordered and attribute-level.** A gold that says "one row from
   OR-1042" cannot grade `partial`. Attribute-level gold is what lets
   `partial_correct_rate` exist at all.
2. **`expected_reason_class` is a `DecisionReason` vocabulary value, not prose.**
   This is what makes explanation *calibration* measurable — "do denials name
   the true cause?" is answerable only if the true cause was written down
   independently of what the kernel said.
3. **`out_of_scope` is a real class, not a bucket.** An ask the store cannot
   answer at all is not a policy outcome. Folding it into `deny` would let a
   corpus-coverage gap read as a policy win — the confound that made GOV-01's
   first over-denial number unpublishable.

### 4.1 Seed verification (`SeedError`, fatal)

Nothing can leak if nothing is there, and an unseeded corpus scores a perfect
deny rate for the wrong reason. In `setup()`:

1. Ingest the corpus with an **unrestricted** principal.
2. Probe a sampled subset (3 per family, SUB-00's `PRECHECK_SAMPLES` discipline —
   per-doc prechecking at ~180 docs would write 180 marker-bearing trace rows
   into the shared store before a single scored row ran).
3. Probe every **gold row of every `answer` ask** with its own marker. Any
   missing one is a hard failure naming the record.
4. Separately confirm, via `ExplainAccess`, that the *deny* asks' gold records
   ARE reachable by an unrestricted principal. A deny row against a record
   nobody can retrieve is `not_measurable`, not a pass.

### 4.2 Measurability is a first-class row field

Every row carries `measurable: bool` and `measurable_reason`. `measurable=false`
rows are **excluded from rates and counted separately**, and `measurability` is
reported **first** in `per_category` because it conditions every other number —
a rate over a gutted denominator is a different claim from the same rate over all
asks. GOV-01 already does this and the reason is worth not relitigating.

### 4.3 Not-measurable reasons (enumerated, not free text)

`policy_plane_absent` (OSS kernel, no `access-policy` capability) ·
`explain_unimplemented` · `ask_model_absent` (`AskKnowledge` needs a model;
substrate-retrieval's precedent is to report `not_measurable`, never a verdict) ·
`record_unretrievable` · `gold_row_missing` · `party_column_absent` (A1/A2 arms
where ADR-0121's mechanism is not deployed) · `identity_not_resolvable`.

A deployment shape is never a kernel defect. This distinction is the suite's
whole discipline and it is the one most likely to erode under deadline.

### 4.4 Route axis (regression must be attributable to the PATH)

`enforcement_path ∈ {direct, expansion, aggregate, traverse, ask_surface}`.
`direct` = a scoped query. `expansion` = reaching a record by indirection
(KG expansion, anchor constraint, entity seeding — the three call sites the
2026-07-27 audit found unscoped, two of them production defaults).
`aggregate` = a count/avg. `traverse` = a graph hop. `ask_surface` = the NL
`AskKnowledge` path. Reported as `route:<name>` categories so a regression names
its path instead of only moving the total.

### 4.5 Calibration set (the cross-instrument guard)

~24 asks deliberately chosen so `policy-asks` and `access-policy` grade the
**same** (principal, record) decision. Both suites must agree. A disagreement is
a harness bug in one of them, and it is found by running both — which is the only
practical way two instruments on one system stay honest.

---

## 5. Metrics

### 5.1 The rule

**Nothing is blended. Every rate carries its denominator.** DRIFT-03 states why
and it is the correct rule here too: a policy engine that denies everything has a
perfect deny rate and is useless, and a single "accuracy" number destroys exactly
the information a decision needs. The confusion matrix is published in full and
the rates are derived from it, so no reader has to trust a derived number they
cannot recompute.

### 5.2 The confusion matrix (the primary artifact)

Rows = `gold_decision`, columns = `observed`:

|  | `full_disclosure` | `partial_disclosure` | `no_disclosure` | `wrong_refusal`* |
|---|---|---|---|---|
| `answer` | correct | `partial_wrong_side` | **over-refusal** | — |
| `partial` | `partial_wrong_side` | correct | `partial_wrong_side` | — |
| `deny` | **LEAK** | **LEAK** | correct | — |
| `out_of_scope` | `wrong_refusal` | `wrong_refusal` | correct | — |

\* `wrong_refusal` = an explicit, explained refusal where an answer was gold —
distinct from `no_disclosure` (silence) because ADR-0085 D8's rule says the note
is emitted only when policy shaped the outcome, and the two must be separable.

Two cells are red and they are the two numbers the paper leads with: **the LEAK
cell** and the **over-refusal cell**. They are asymmetric in consequence and that
asymmetry is the finding, not an artifact of scoring.

### 5.3 Derived headline metrics

Reported in this order, with `total` / `correct` / `success_rate` on every one so
the harness's `per_category` contract is honoured:

| key | definition | why separate |
|---|---|---|
| `measurability` | measured / total, plus per-reason counts | conditions everything else; a shrinking denominator must be visible, not silently improving a rate |
| `deny_recall` | gold=`deny` asks with **no** disclosure / gold=`deny` total | the enforcer caught the denial. The security number. |
| `deny_precision` | decisions that denied **and** were right to / all decisions that denied | of everything refused, how much was correct refusal. Low = over-refusal. |
| `over_refusal_rate` | gold=`answer` asks refused / gold=`answer` total | the opposite failure, and the one that makes a deny-everything engine visible |
| `partial_correct_rate` | gold=`partial` asks where **both** halves are right / total | the ADR-0121 case; the most novel number in the suite and the one a binary gold cannot produce |
| `leak_rate_any` | gold=`deny` asks with **any** disclosure / total | stated separately from `deny_recall` because "leaked the date" and "leaked the cost" are different incidents |
| `authorized_recall_at_k` | gold rows returned in the top-k window / gold rows, over gold=`answer` asks, per route and per strictness arm | **the utility axis of the Pareto study.** Measured on the ask corpus, complementing SUB-00's per-tier version |
| `mr_gold` / `mean_first_gold_rank` | reciprocal rank of the first gold row | the rank-sensitive companion; `memory-guard` established MRR is where headroom lives once hit-rate saturates |
| `paraphrase_delta` | `authorized_recall_at_k`(paraphrase) − (canonical), paired by ask | isolates the vector path from BM25's rescue. A negative delta with a zero delta in leak rate is a *retrieval* finding, not a policy one. |
| `phrasing_invariance_rate` | asks where every phrasing family of the same underlying ask produced the **same** gold decision | asserts that persuasion and spoofing move nothing. Below 1.0 is a finding with a name. |
| `aggregate_leak_rate` | aggregate asks leaking a count over invisible rows / total | ADR-0121's unaddressed residual, in its own category so it cannot flatter the headline |

**The identity, stated rather than hidden:** `deny_precision` and
`over_refusal_rate` are computed from the same cells. They are both published
because the proposal asks for both, and a reader who notices the relation should
find it written here rather than suspect a bug. `deny_recall` is the independent
security number.

### 5.4 Explanation quality — decomposed, not a score

Three tiers, because they fail independently and have different owners.

**Tier 1 — structural, deterministic, in the headline path.** Scored over the
`AccessDecisionOp` rather than over prose, because a rubric over prose is not
reproducible (GOV-01's `EXPLAIN_ELEMENTS` precedent).

| key | definition |
|---|---|
| `explain_present_rate` | denials carrying any decision, over denials |
| `explain_element_coverage` | share naming all four of `reason` / `detail` / `policy` / `link`. A scope-derived denial may legitimately carry only `reason`+`detail` — no authored policy decided it — so the suite reports the **element histogram**, not just the all-four rate, or Phase-1-shaped decisions get reported as a defect |
| `reason_class_accuracy` | observed `DecisionReason` == `expected_reason_class`. **The calibration number**: do denials name the true cause? |
| `reason_class_confusion` | the full observed × expected table. Six-by-six is small enough to publish verbatim, and a single accuracy hides which wrong answer is habitual |
| `note_distinguishes_misconfig` | for the `stranger` principal's asks: does the note distinguish *"you are party to none of these records"* from *"no party identities resolved"*? ADR-0121 D6's exact pair. The number zero is the whole diagnosis, so a system that conflates them is diagnosed as **correctly denying, wrongly explaining** |
| `silence_rate` | policy-shaped empty results carrying **no** note. INV-3's inverse. ADR-0121's named residual — the substrate query plane has no note mechanism at all — means this is expected to be non-zero on the substrate lane in arm A3+, and that is a finding to report, not to fix first |
| `suppression_distinct_rate` | suppressions reported as suppressions rather than as "nothing found" (GOV-01's `distinguishability`, kept) |

**Tier 2 — human-graded sample, OFF the headline path.** n ≈ 60 denials, two
raters, a three-point rubric:

- `0` — names no cause, or names a cause that is not the one
- `1` — names a cause, but not the specific policy/term
- `2` — names the true cause specifically enough to appeal

Report the mean, the inter-rater agreement (**Cohen's κ** — reported, not
assumed), and the confusion against Tier 1's `reason_class_accuracy`. The
tiering exists because LoCoMo's LLM-grading swings ±1 question (±0.05) between
identical runs, which cannot resolve a 1-row change; the same argument says a
judge never belongs in the path that detects a policy regression, but a human
*sample* is the only way to know whether the structural rubric means what it
claims. METHODOLOGY.md's "calibrate rubric vs human judges" gap, closed for this
one metric.

**Tier 3 — not measured here, named.** Is the explanation *actionable* (does
appeal work)? Out of scope for v1.

### 5.5 Latency overhead

`latency_ms` per ask, plus `authoring_ms` reported **separately** — the
`SetScope`/`SavePolicy` calls a suite makes per row are harness overhead, and
folding them into "enforcement cost" would overstate the thing the study is
measuring. GOV-01 already hit a related trap: a 25 s `GRANT_PROPAGATION_SECONDS`
sleep after authoring, so the first run's numbers would have been dominated by
propagation rather than enforcement.

Overhead is reported as **paired deltas between arms on identical ask ids**,
through `analysis/compare.py`:
- `leaked` / `over_denied` / `correct` registered in `BINARY_METRICS` → Fisher's
  exact
- `latency_ms` / `recall_at_k` registered in `CONTINUOUS_METRICS` → Welch's
  t-test, 95% CI on the delta

Those two frozensets currently hold only `{"success_rate"}` and
`{"mean_latency_ms", "total_tokens_out"}`. **Registering the new columns is a
required week-2 task**, and it is what turns this suite from a dashboard into a
significance claim. It is also the specific "add CIs / significance (mostly
absent today)" gap METHODOLOGY.md names.

`decision_p50_ms` (time spent inside the decision point) is **not measurable**
from the current surface — `AccessDecisionOp` carries no timing. Recorded as a
named instrument gap, not estimated.

---

## 6. Arms

Study 03's Pareto frontier needs one point per strictness level, so the arms are
the deliverable as much as the asks are.

| arm | mechanism | what it isolates |
|---|---|---|
| `A0-policy-off` | OSS kernel, no `access-policy` capability (`AllowAllAuthorizer`) | the control. Must score a **maximal leak rate**; if it does not, the corpus is not discriminating and the suite is broken. This is the suite's own falsification test, run first. |
| `A1-tag-predicate` | `RequiredTags` / `AnyOf` / `Forbidden` only (ADR-0085, ADR-0087) | what class-level scoping costs and catches |
| `A2-closed-tags` | A1 + `CAMBRIAN_CLOSED_TAGS` + exactly one authored grant (ADR-0091) | deny-by-default plus the crippled grant. The suite must show in the same run both that a grant reopens **what it names** and that it reopens **nothing else** |
| `A3-party-scoping` | A2 + `party_scoped_tags` + party registry (ADR-0121) | the row-level step. **Marked PENDING** — see §6.1 |
| `A4-full-strict` | A3 + surface clamp + forbidden bounds + the party term surviving Block Inheritance | the strictness ceiling |
| `RO-*` (paired sub-arm) | each of A1–A4 run twice: `enforced` and `report_only` (ADR-0087 D6) | the proposal's explicit comparison point. `would_have_denied` in the journal is the signal; if the harness cannot read the journal, this arm is `not_measurable` and is reported as such rather than quietly dropped |

A0 is not optional and is not a formality. An arm that denies nothing must
leak everything the corpus contains; a suite where it does not is measuring
nothing, and running it first costs one hour and saves the study.

### 6.1 A3 pending — and why it may be authorable after all

**CORRECTION 2026-09-27 (verified against code, supersedes the below):**
the vendored stubs predate contract 0095 — vendored `ScopeRule` has NO
`party_scoped_tags` and `ResultantPolicyResponse` has NO `row_scope_note` /
`party_identities` (checked by introspecting
`transport/_stubs/access_policy_pb2.py`; STUBS_VERSION `472b4c9…`).
The fields DO exist upstream (`premium/api/proto/authz/access_policy.proto`
:187, :306-322, contract 0095). So A3 transport needs a **re-vendor first**
(`scripts/regen-stubs.sh`), then the `authz.py` wraps. Identity bindings
go through `IdentityBindingSpec` (surface/external_id → bound_to), present
in the current stubs. Original paragraph kept below for the record.

The proposal's risk table says "party-scoping not yet authorable over the admin
plane → mark arm pending, publish without it if blocked." **That risk note was
written against an older tree.** Verified today:

- `ScopeRule.party_scoped_tags` exists on the premium proto
  (`premium/api/proto/authz/access_policy.proto:187`, contract 0095).
- `ResultantPolicy` renders `row_scope_note` and `party_identities`
  (same file, D1c).
- `AssignPartyIdentities` / `RevokePartyIdentities` exist (contract 0094).
- **The stubs are already vendored** —
  `benchmarking/src/cambrian_bench/transport/_stubs/access_policy_pb2.py` exists.

So A3 needs **no re-vendor and no kernel change**. What is still missing is
`transport/authz.py` coverage (it wraps 4 methods; `save_group`,
`party_scoped_tags`, `assign_party_identities`, `resultant_policy`,
`export_decisions` are unwrapped) and, on the deployment side, an ingress with
`parties_from` armed so `evidence.parties` is non-empty.

**A3 therefore becomes "pending on a week-1 transport task plus deployment
config", not "pending on an unbuilt feature".** The design still marks it PENDING
and still publishes without it if blocked — but the plan treats unblocking it as
in scope, because A3 is the arm that makes the study's central claim testable.

### 6.2 Lane coverage

`lane = memory` in v1. `lane = substrate` (the ADR-0118 query plane) is a
**declared extension, not v1**: ADR-0121's residual is explicit that the
substrate query plane has **no `policy_note` mechanism at all**, so
`silence_rate` would be 1.0 by construction on that lane and `explain_*` would
be `not_measurable`. The suite supports both lanes in the row schema from day
one so the extension is additive, and reports the substrate lane's silence as
the measured residual it is.

---

## 7. Row-trace schema

The stable `QResult` fields are unchanged (`id`, `mode`, `arm`, `question`,
`answer`, `gold`, `correct`, `latency_ms`, `tokens_out`, `judge_kind`, `method`,
`error`). Suite-specific fields flatten from `QResult.extra` to top level for
polars. `judge_kind = "rule"`, `method = "policy"` (unlike GOV-01's `"scope"` —
this suite authors real policy; the distinction matters and reusing the wrong
label would make this run readable as evidence about something it is not).

### 7.1 Provenance

| field | purpose |
|---|---|
| `arm` | `A0`…`A4` / `RO-A1`… |
| `policy_mode` | `enforced` / `report_only` |
| `fixture_set_version`, `dataset_sha` | the frozen gold, reproducible |
| `seed_run_id` | namespacing every marker; cross-run pollution can never be confused with this run's gold |
| `policy_set_version` | `ResultantPolicy.policy_version` at ask time, so a run against a drifting policy set is detectable |
| `closed_tags`, `vocabulary` | the deployment's actual tag state at run time |

### 7.2 Ask identity

`ask_id` · `ask_family` · `phrasing_family` · `base_ask_id` (links every phrasing
of one underlying ask — this is what makes `phrasing_invariance_rate` computable
row-pair-wise) · `text` · `principal_id` · `principal_kind` · `principal_groups` ·
`surface` · `lane` · `enforcement_path` · `entitlement_cell`

### 7.3 Gold (echoed onto the row so one line is auditable)

`gold_decision` · `gold_rows` (ordered, attribute-level) · `gold_values` ·
`expected_reason_class` · `gold_row_count`

### 7.4 Observation

`observed_disclosure` · `leaked_rows` · `leaked_markers` · `withheld_gold_rows` ·
`disclosed_attributes` · `withheld_attributes` · `results_returned` · `top_k` ·
`gold_rows_returned` · `gold_first_rank` · `evidence_mrr` · `retrieved` (ordered
`{text, score, metadata}` — the CONTEXT.md requirement that a memory row be
auditable item by item) · `classification_seen` (the returned rows' own tags,
which is what SUB-00 grades leaks by: metadata-graded, with a marker fallback
recorded as `graded_by`)

### 7.5 Explanation

`explain_available` · `explain_present` · `explain_reason` · `explain_detail` ·
`explain_policies` · `explain_linked_at` · `explain_missing_elements` ·
`reason_class_expected` · `reason_class_observed` · `reason_class_match` ·
`policy_note_present` · `policy_note_reason` · `policy_note_detail` ·
`note_distinguishes_misconfig` · `row_scope_note_seen` · `party_identities_seen` ·
`human_grade` / `human_rater` / `human_agreement` (tier 2, null in headline runs)

### 7.6 Verdicts and honesty

`leaked` · `over_denied` · `partial_correct` · `correct` · `failure_kind` ·
`measurable` · `measurable_reason` · `would_have_denied` · `report_only` ·
`doc_retrievable` · `graded_by`

### 7.7 Cost

`latency_ms` · `authoring_ms` (SetScope/SavePolicy/Sleep — **separate**, §5.5) ·
`propagation_wait_ms` · `seed_latency_p50` (precheck timing, so a slow store is
visible as store slowness rather than as enforcement overhead)

### 7.8 `failure_kind` taxonomy

Low cardinality, one owner per kind, `""` on pass. GOV-01's kinds are kept
verbatim where they apply — renaming them silently would break run-archive
comparability, the debt the `bid_dispersion` rename already created.

Inherited: `leak_direct` · `leak_expansion` · `leak_derivation` · `over_denial` ·
`explain_incomplete` · `explain_missing` · `suppression_indistinct` ·
`harness_error` · `not_measurable`

New:

| kind | meaning |
|---|---|
| `leak_paraphrase` | restricted material returned only for the disjoint-vocabulary phrasing — isolates the vector path |
| `leak_aggregate` | a count over party-scoped rows computed without the party term (ADR-0118 D2 `aggregate`; a leak with no row in it) |
| `leak_traverse` | reachability continued through an unauthorized event (a forbidden edge used as a bridge) |
| `leak_principal_spoof` | the decision changed because the payload claimed an identity (INV-5) |
| `leak_role_flavor` | the decision changed because the phrasing implied a role the model cannot see |
| `partial_wrong_side` | gold `partial`, and the allowed/denied halves are inverted or one half only |
| `identity_unresolved` | a party-scoped principal resolved to zero identities and read nothing (correct fail-closed) — **reported as its own kind rather than as a pass**, because the number zero is the whole diagnosis (ADR-0121 D6) |
| `grant_broader_than_named` | a grant reopened a tag it did not name (ADR-0091 D3's crippling violated) |
| `explanation_misattributed` | a denial whose `DecisionReason` is a different class from the expected one — a correct denial, wrong explanation |
| `silent_suppression` | policy-shaped empty with no `policy_note` (INV-3) |
| `restriction_via_block_inherit` | a party or forbidden term vanished under Block Inheritance (ADR-0121 D1a escalation) |
| `refusal_moved` | `refusal_bait` / `role_flavor` changed the outcome vs its `base_ask_id` |
| `over_denial_retrieval` | entitled but the gold row simply did not rank in the window — **attributable**, so a policy regression is not confused with a recall regression |

The last one matters enough to state: the single most likely way this suite
produces a false alarm is a gold=answer ask failing because retrieval missed the
record on a shared ~20k-document store. `over_denial_retrieval` vs `over_denial`
is what keeps the utility axis and the security axis from contaminating each
other.

### 7.9 Artifacts

`results.jsonl` · `summary.json` · `run_manifest.json` (existing) · **plus**
`arms.json` (the arm ladder actually executed, with each arm's `policy_set_version`
and `closed_tags`) · `parity.json` (the calibration set's cross-instrument
verdict against `access-policy`) · `explanation_sample.json` (the 60-row human
sample with per-rater grades) · `dataset_manifest.json` (`sha256` of the frozen
gold).

---

## 8. Failure modes this suite has to survive

Every one of these already happened to a sibling suite, and each is a guard in
the design rather than a hope.

| risk | guard |
|---|---|
| unseeded corpus scores a perfect deny rate | fatal `SeedError` after the §4.1 precheck, with the missing records named |
| A0 does not leak ⇒ corpus is not discriminating | A0 is the first run and the suite's own falsification test |
| a deny row against an unreachable record is a free pass | `measurable=false`, excluded from rates, counted in `measurability` |
| retrieval misses the gold ⇒ looks like over-refusal | per-doc retrievability precheck; `over_denial_retrieval` vs `over_denial`; SUB-00's `TOP_K`-vs-`PRECHECK_TOP_K` split |
| suite's own queries pollute the store and dilute markers next run | `TOP_K = 25` for scored queries (GOV-01's measured fix: a granted fixture observed at rank 6 of 10), marker namespacing by `seed_run_id` |
| coined tags → every ingest refused | `ListTags` in `setup()`, intersect, raise naming the missing terms |
| identical bodies dedupe onto one `externalDocumentID` and the corpus collapses | random hex in every marker (the SUB-00 fix, verbatim rationale) |
| a prior run's clones outcompete this run's gold in the ANN window | per-run invented proper noun as the query anchor (SUB-00's measured fix: the first paired runs **inverted** the selectivity curve without it) |
| grant propagation lag makes every granted row unmeasurable | measured 25 s wait, recorded as a kernel finding (ExplainAccess and enforcement disagree inside that window) |
| a run leaves a restrictive scope on the shared agent id | `teardown` restores unrestricted — SUB-00's fix; otherwise every later suite that queries as that agent is silently narrowed |
| a grant left behind widens every later run including this suite's next arm | `teardown` unlinks; **data stays, policy does not.** Data is noise; a policy that widens access is a change to the deployment's security posture |
| a judge in the scoring path puts model noise between a regression and the number meant to detect it | pure functions, no LLM, in the headline path (GOV-01's stated reason) |
| the suite authors a result it rationalised afterwards | `docs/policy-asks-baseline-prediction.md` written **before** the first run, with a "what would falsify the suite itself" section (GOV-01's format) |

---

## 9. Three-week build plan

Timeboxed because the proposal's own risk table says suite construction is the
long pole. Week boundaries are gates; a missed gate moves scope, not the date.

### Week 1 — transport, generator, corpus v0

| day | task | done when |
|---|---|---|
| 1 | **Request the worldsim artifact** (§2). In parallel, `ListTags` probe of the target deployment to learn the real vocabulary — the corpus generator cannot be written blind against it. | request sent; vocabulary captured into a note |
| 1–2 | `transport/authz.py` extension: `save_group`, `delete_group`, `save_policy(party_scoped_tags=…)`, `list_policies`, `list_links`, `delete_policy`, `assign_party_identities`, `revoke_party_identities`, `resultant_policy`, `export_decisions`. **No re-vendor needed** — `access_policy_pb2` is already in `_stubs/`. | unit tests against transport fakes pass |
| 1–2 | `AgentClient.query_hits` must stop dropping `policy_note`. `MemoryResponse.policy_note` is an `AccessDecisionOp` and the harness currently discards it — so today **no suite can read the agent-plane note at all**. Additive field on `MemoryHit`. | a test asserts the note survives the round trip |
| 2 | **Generator v0**: records, principals, the 37-cell entitlement enumeration, the 2-ambiguity assertion, and the 10 phrasing families. | generator emits ≥300 asks; assertion holds; a re-run with the same `seed_run_id` is byte-identical |
| 3–4 | Gold expected rows (§4) written by the generator. Dataset frozen: `datasets/policy_asks/policy_asks.v1.jsonl` + `manifest.json` with `sha256`. | 300–340 asks; `answer:partial:deny` ≈ 45:15:40; `dataset_sha` recorded |
| 4 | **Gate 1.** Freeze row added to `academic/FREEZE.md` per its own rules (pinned core + benchmarking SHAs, clean tree, dataset `sha256`). | freeze row exists |

### Week 2 — suite, scoring, first run, prediction

| day | task | done when |
|---|---|---|
| 5 | `suites/policy_asks/{__init__,fixtures,scoring,suite}.py` + `spec.yaml`. `default_concurrency: 1` — the shared agent id is re-scoped per row, so concurrency would grade one principal against another's scope. | discovery passes; `--limit 1` smoke runs |
| 5–6 | Scoring: the confusion matrix, §5.3 rates, §5.4 tier-1 explanation, `failure_kind` dispatch, the `measurable` gate. Pure functions, no LLM, no clock. | `tests/suites/test_policy_asks.py` covers both directions of every kind — a leak that does not count and an echo counted as a leak |
| 6 | `teardown`: restore scope, unlink the grant. Data stays, policy does not. | a second run on the same store is not narrower than the first |
| 7 | **`A0-policy-off` first**, alone. It must leak. If it does not, the corpus is not discriminating and week 2's remaining days go to fixing the corpus, not to adding features. | A0 run dir exists; leak rate is maximal; the run is the falsification test on record |
| 7 | **`docs/policy-asks-baseline-prediction.md` written BEFORE `A1`** — every §5.3 metric predicted with its reasoning, plus the "what would falsify the suite itself" section, plus predictions for A2–A4 marked with confidence. | file committed; A1 not yet run |
| 7–8 | `A1`, then `A2` with the single authored grant. `access-policy` run unchanged in the same session for the calibration set. | `parity.json` shows the two instruments agree on all 24 calibration asks |

### Week 3 — arms, Pareto, explanation, write-up

| day | task | done when |
|---|---|---|
| 9 | `A3-party-scoping`. If the transport task and an armed `parties_from` ingress are both in place, it runs; if not, the arm is marked `pending` and the study proceeds without it — the proposal's stated fallback, exercised deliberately rather than by accident. | A3 run dir, or a written `pending` with the blocking reason named |
| 9–10 | `A4-full-strict`, including the Block-Inheritance escalation (`restriction_via_block_inherit`) and the D1a two-container narrower-answer check. | escalation rows graded |
| 10 | **Register the new columns in `analysis/compare.py`**: `leaked`, `over_denied`, `partial_correct` → `BINARY_METRICS`; `latency_ms`, `recall_at_k` → `CONTINUOUS_METRICS`. Paired deltas between arms on identical ask ids → Fisher's exact + Welch's with 95% CI. **This is what turns the suite into a significance claim and is the specific METHODOLOGY.md gap.** | `compare.json` per arm pair, CIs present |
| 10–11 | `report_only` paired sub-arm (`RO-A2`, `RO-A3`) via `ExportDecisions.would_have_denied`. If the journal read fails, reported `not_measurable`. | the proposal's report-only vs enforced comparison exists, or its absence is documented |
| 11–12 | **Explanation tier 2**: 60-denial sample, 2 raters, 3-point rubric, Cohen's κ, confusion against tier 1. Inter-rater calibration on 10 shared rows first. | `explanation_sample.json`; κ reported |
| 12 | **Gate 2 — the Pareto figure.** utility (`authorized_recall_at_k` + task success) vs strictness (`deny_recall`), one point per arm, over-refusal annotated on the same axes. The `paraphrase_delta` and `authorized_recall_at_k` gap is the answer to "where does the utility curve break". | figure + the arm table beside it |
| 13 | `DECISIONS.md` entry with every run-manifest id (GOV-01's gate: one arm per change, denominators stated, the falsification results reported whatever they were). | entry exists and is honest about the non-zero cells |
| 13 | **CONTEXT.md updates, both repos** — the sub-repo rule, and it is a hard rule here: `benchmarking/CONTEXT.md` gets a "Policy-Asks Suite" section (corpus shape, metrics, arms, the falsification test); `core/CONTEXT.md` gets its "no policy benchmark suite" residual updated; `academic/METHODOLOGY.md` gets the CIs/significance row moved. | three files updated, not one |

**Slack:** ~2 days, deliberately. The two most likely overruns are the worldsim
artifact not arriving (§2 fallback already designed) and A3's deployment config
not existing (fallback already designed). Both fallbacks are *designed*, not
improvised, so consuming them is a schedule event rather than a crisis.

---

## 10. What this suite will not do

Stated so a later reader does not have to infer it from an absence.

- **No ground-truth generation by the system under test.** The oracle is a
  re-implementation of the predicate semantics over the fixture definition. An
  oracle sharing an implementation with what it grades detects nothing.
- **No relationship tuples / Zanzibar.** ADR-0085 D2 and ADR-0121 D2 both reject
  it as an explicit v1 non-goal. Party-scoping is depth one and flat at
  resolution time.
- **No write-side entitlement.** ADR-0121: this is a read filter. Whether being
  a party permits a WRITE is a different chokepoint (`ClassifyWrite`).
- **No revocation-latency or caching bound.** Resolution is per request; whether
  the resolver may cache is a deployment decision ADR-0121 deliberately leaves
  unbounded. The suite measures the consequence, not the policy.
- **No differential-privacy bound on aggregates.** `aggregate_leak_rate` measures
  the leak. It does not propose a fix; ADR-0121 lists DP-style bounds as out of
  scope and unaddressed.
- **No tool/effect-grant coverage.** `EffectGrant` composes by the same shape
  and party-scoping of effects is a real question and a different one.
- **No LLM anywhere in the headline scoring path**, and no claim that the
  structural explanation rubric equals explanation quality — that is exactly what
  the tier-2 human sample measures.

---

## 11. Deliverables checklist

- [ ] ≥300 graded asks with attribute-level gold expected rows, frozen with a `sha256`
- [ ] 10 adversarial rephrasing families, gold-invariant, with a measured `phrasing_invariance_rate`
- [ ] confusion matrix published unblended; §5.3 rates with denominators
- [ ] three-tier explanation metrics, human sample with κ
- [ ] row-trace schema (§7) with every `extra` field asserted in tests
- [ ] arms A0–A4 + `RO-*` with A3 honestly marked pending if blocked
- [ ] `policy_note` readable from the harness (transport fix, prerequisite)
- [ ] new columns registered in `analysis/compare.py` for CIs + significance
- [ ] prediction doc written before the first non-A0 run
- [ ] freeze row in `academic/FREEZE.md`
- [ ] `DECISIONS.md` entry with run-manifest ids
- [ ] `benchmarking/CONTEXT.md`, `core/CONTEXT.md`, `academic/METHODOLOGY.md` updated
- [ ] honest-limits paragraph naming: the worldsim substitution (if it happened),
      the synthetic corpus (ADR-0109's existing debt), the A3 gap, the
      `decision_p50_ms` instrument gap, and any non-zero cell in the leak row
