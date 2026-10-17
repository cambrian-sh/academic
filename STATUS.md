# Program Status — living state

> **Maintenance contract.** This file is the single source of truth for
> "where the program stands". Update it at the end of every session and
> immediately after every run — before anything else. A change is not
> "done" until this file reflects it. §7 lists exactly what to update
> when. A stale status file is worse than no status file.

**Last updated:** 2026-10-17 · conductor: AI agent (handoff to CTO in progress)

## 1. Phase

- **Literature & design: ~complete.** 233 verified bib entries; ICLR 2026
  exhausted (4 passes); Letta + frontier archives done; two satellite
  drafts + flagship skeleton written (non-results prose only).
- **Experiments: 0 runs.** Everything empirical is blocked on kernel
  access (`CAMBRIAN_BINARY` + DB). No number in any draft is a claim
  until its run manifest exists.

## 2. Assets

| Asset | File(s) | State |
|---|---|---|
| Literature review (living) | `LITERATURE_REVIEW.md` v0.3 | §1–5; 233-entry bib |
| Related work draft | `papers/related-work-draft.md` v0.3 | publication-grade citations |
| Dispatch satellite | `papers/dispatch-satellite-draft.md` v0.1 | §3 verified vs ADR-0100; results TBD (P1–P4 + P11) |
| Security satellite | `papers/security-satellite-draft.md` v0.1 | §3 verified vs ADR-0091/0118/0121; results TBD (A0–A4) |
| Flagship | `papers/flagship-skeleton.md` | 10-row claim map; rows 1,2,3,5 measured in-repo; 4 pilot; rest blocked |
| Study proposals | `proposals/01–04*.md` | wired with baselines + confound pins |
| Gold audit | `../benchmarking/docs/proposal-01-gold-audit.md` | 19 CONFIRM / 1 systemic AMEND; **countersign pending** |
| Pre-registration | `../benchmarking/docs/proposal-01-pre-registration.md` | must be committed before first P1 run |
| Freeze log | `FREEZE.md` | F-2026-09-26 + amends; **new row required before any run** |
| Archives | `data/external/{letta,frontier}/` | SHA256SUMS verified |
| Narrative log | `notes/program-log.md` | running session log |

## 3. Blocked (owner)

| Blocker | Owner | Unblocks |
|---|---|---|
| `CAMBRIAN_BINARY` + DB | CTO | every results cell (A0–A4, P1–P4, parity, hot-path, ablation) |
| Gold-audit countersign + `test_execution` amendment | CTO | accuracy-parity claims |
| Commit decisions: `academic/` + `benchmarking/` trees dirty | CTO | clean reference points; pre-reg commit-before-P1 rule |
| ADR location (core main deleted `docs/adr/`) | CTO | §3 re-verification for future draft edits |
| Worldsim 328-ask artifact | owner | closes `policy-asks` provenance gap |
| 5 external-brief items (ResPol sign etc.) | owner (ChatGPT/Gemini) | venue-pending notes |
| Blog-track go/no-go | owner/CTO | publishes from verified literature now |

## 4. Next actions (ordered)

1. **Unblocked now:** `core/docs/research` integration — title-match 5
   lead IDs + reconcile `benchmarks-and-claims.md` Tier-1 values
   (`notes/docs-research-integration-2026-10-17.md`).
2. **CTO:** the §3 items above (kernel, countersign, ADRs, commits, blog).
3. **Runs (after unblock):** prediction fill → new freeze row + SHA pin →
   `policy-asks` A0 alone (falsification) → A1/A2 + parity → proposal 04
   hot-path → proposal 02 ablation.
4. **Calendar:** EMNLP 2026 proceedings after Oct 24–29; CIKM 2026 list
   re-run after Nov 7.
5. **Writing:** fill draft §4 rows from manifests → abstracts last →
   dispatch → security → flagship.

## 5. Decision ledger (binding until explicitly lifted)

| Date | Decision |
|---|---|
| 2026-09-23 | Proposals-first execution order approved |
| 2026-10-01 | Subagent cancellations = interruptions/retries; never infer workflow preferences |
| 2026-10-05 | Letta posts are company technical reports — cite as such, never as papers |
| 2026-10-06 | `policy-asks` stays a separate suite (sibling of `access-policy`); binary gold cannot express `partial` |
| 2026-10-09 | Paper order: dispatch → security → flagship; results are manifest-tied TBD until runs |
| 2026-10-11 | Bib convention: bare IDs in notes = verified-in-notes; do not promote or "fix" by guessing |
| standing | Every claim traces to a run manifest or ADR gate; freeze row before every run; honest-limits sections mandatory; raw runs stay in `benchmarking/out/` |

## 6. Open questions

- Where do core ADRs live going forward? (deleted at core main `f9e30ad`)
- Blog track: which topic ships first if approved?
- Restore `agent-selection-mechanisms/SUMMARY.md` (ADR-0100's basis)?
- Accept worldsim substitution for `policy-asks`, or wait for the artifact?

## 7. Maintenance protocol (mandatory — this is "kept up to date")

- **Before every run:** add `FREEZE.md` row — kernel SHA, tree state,
  dataset shas, resource envelope.
- **After every run:** fill the draft's TBD results cells citing
  `benchmarking/out/.../run_manifest.json`; update §1–§4 here; append to
  `notes/program-log.md`.
- **After every literature session:** title-match, then bib +
  `LITERATURE_REVIEW.md` (+ `papers/related-work-draft.md` if
  citation-bearing); update §2/§4 here.
- **After every decision:** record it in §5 here + a program-log entry.
- **After every session (even ad-hoc):** append a `notes/program-log.md`
  entry; refresh the "Last updated" line above.
