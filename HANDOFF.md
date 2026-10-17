# HANDOFF — Cambrian academic research initiative

**Audience:** CTO (new owner). Read once, ~15 minutes. After that, the
only file you must keep current is `STATUS.md`.

## 1. What this is

A research program with two goals at once:

1. **Make Cambrian better** — every topic maps to a kernel mechanism, so
   findings have a direct route back into the codebase.
2. **Publish** — two satellite papers + one flagship paper planned, plus
   technical blog posts derived from the same verified evidence base.

The program is **verification-first and honesty-first**: identifiers are
title-matched off fetched pages; claims trace to run manifests or ADR
gates; limits are written before results. That discipline is why the
drafts can survive hostile review. Do not dilute it for speed.

## 2. Current state in one line

Literature and drafts are ~complete; **zero experiments have run** —
everything measurable waits on `CAMBRIAN_BINARY` + DB. See `STATUS.md`
for the live picture.

## 3. The three papers (and what unblocks each)

| Paper | Venue targets | Blocked on |
|---|---|---|
| Dispatch satellite — zero-RPC capability-typed dispatch; headline = routing-decision overhead | AAMAS / EuroSys/SOSP-AI / MLSys | P1–P4 + P11 runs; gold-audit countersign |
| Security satellite — utility × security Pareto of fail-closed memory scope | IEEE S&P / USENIX Security / CCS | A0–A4 runs |
| Flagship — evidence-first knowledge substrate | CIDR / VLDB / (NeurIPS D&B) / KR slice | rows 6–10 + B8 graph reproduction (flagship-gating) |

## 4. First 48 hours (what only you can do)

1. **Kernel access** — provide `CAMBRIAN_BINARY` + DB; unblocks every
   results cell in all three papers.
2. **Countersign the gold audit** — `../benchmarking/docs/proposal-01-gold-audit.md`
   (19 CONFIRM / 1 systemic AMEND; recommendation: accept, and drop the
   prompt-invisible `test_execution` for v1). Parity claims are gated on
   this.
3. **Decide the ADR location** — core main @ `f9e30ad` deleted `docs/adr/`
   (all 125 files, pushed). Fastest restore:
   `git -C ../core checkout 102aa09 -- docs/adr` then commit; or relocate
   deliberately and update references (the drafts' §3 + workspace
   `AGENTS.md` point at `core/docs/adr/`).
4. **Commit decisions** — both trees are dirty (`academic/` ~20 paths,
   `benchmarking/` 25). Pre-registration rule: that doc must be committed
   **before** the first P1 run.
5. **Blog track go/no-go** — posts can publish from the verified
   literature today, no kernel needed (candidates: routing-decision
   overhead thesis; Letta memory survey; graph-expansion skepticism).
6. Optional: restore `docs/research/agent-selection-mechanisms/SUMMARY.md`
   (ADR-0100's cited basis — absent from disk and git history).

## 5. How the program runs

- A **conductor** (AI agent or engineer) executes; the owner reviews and
  decides. Decisions are recorded in `STATUS.md` §5 — binding until
  explicitly lifted.
- Conductor rules live in `AGENTS.md` (verification protocol, freeze
  discipline, update obligations). The maintenance contract is
  `STATUS.md` §7.
- Raw runs stay in `benchmarking/out/`; `academic/` keeps derived tables
  and pointers only.

## 6. Load-bearing facts (do not re-derive; do not contradict)

- Core is v0.6.10-Alpha; Gatekeeper→Dispatcher capability-typed dispatch
  (ADR-0100 retired the auction); 7-phase Knowledge Substrate
  (ADR-0105–0111).
- Pre-2026-07-28 capability numbers are **void** (EFE selector bug);
  auction-era metric names (`bid_dispersion`, `candidates_per_auction`)
  are kept deliberately for archive comparability — map, never rename.
- The archived auction arm is degraded (2026-07-29 run: 112/120 rows
  errored); ADR-0100 records its P2 comparison as **not performed**.
  Never present it as a clean control.
- Graph-expansion signal is contested (B8, `selectiveforget2026`) —
  reproduce on our suites before any flagship use.
- Salami coalitions + SPORE bound per-write validation and scope claims —
  concede, don't discover.
- Worldsim 328-ask set absent; `policy-asks` is the named substitution.
- No peer-reviewed BSL/open-core literature exists — ADR-0057 argues,
  never cites.
- ADRs: deleted from core main @ `f9e30ad`; readable at rev `102aa09`
  until the location decision lands.
- `core/docs/research/` + `core/docs/context/` are **untracked**
  local-only libraries (June-2026 vintage; they predate the verification
  protocol — leads only until title-matched). Integration note:
  `notes/docs-research-integration-2026-10-17.md`.
- MarketBench + Agora (EMNLP 2026 Findings) verified and in the bib —
  Agora is the calibrated-auction steelman that bounds the zero-RPC
  claim.
- ICLR 2026 exhausted (4 passes, ~5,468 unique) — do not spend a 5th.

## 7. Repo map

| Path | Content |
|---|---|
| `STATUS.md` | live state (update after every session/run) |
| `HANDOFF.md` | this file |
| `AGENTS.md` | conductor working rules |
| `RESEARCH_AGENDA.md` | study topics, three horizons |
| `LITERATURE_REVIEW.md` | living review §1–5 |
| `PAPER_OUTLINE.md` | early outline (sketch; the drafts supersede it) |
| `METHODOLOGY.md` | harness → academic evaluation; minimum bar per experiment |
| `FREEZE.md` | experiment freeze record |
| `papers/` | related-work v0.3; dispatch + security drafts v0.1; flagship skeleton |
| `proposals/` | study proposals 01–04 |
| `notes/` | program log + literature sweeps + briefs + integration notes |
| `bib/references.bib` | canonical verified references |
| `data/external/` | archived industry sources with SHA256SUMS |

Outside this repo:

- `../benchmarking/docs/` — gold audit, pre-registration, prediction
  scaffolds; `policy-asks` suite v0 (uncommitted).
- `../core/` — kernel; ADRs per §4.3; untracked research/context docs.
- `../ACADEMIC.md` — workspace-root index (workspace view of the same
  facts).

## 8. Landmines

- Never pre-fill TBD results cells — they are filled only from manifests.
- Do not re-instate `test_execution` in gold labels without the amendment
  decision.
- Subagent cancellations in this program's history were interruptions,
  never workflow preferences.
- A `feynman` research agent was referenced once; it is not visible to
  stock agents — it exists only if a model ID/definition is provided.
- Long sessions can be interrupted by server restarts — verify disk state
  before repeating completed work.

## 9. Suggested posture

The program's value is not the drafts as they stand — it is the verified
evidence base plus the discipline to convert it into defensible results.
Fastest wins, in order: kernel access → countersign → commits → blog
track (converts waiting time into published artifacts).
