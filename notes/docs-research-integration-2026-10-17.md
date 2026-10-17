# docs/research integration — cross-check plan (2026-10-17)

Discovered during the full-review pass: `core/docs/research/` is an
**untracked** June-2026 literature library (author "research gather",
last updated 2026-06-22) that predates and partly overlaps `academic/`.
This note scopes the integration so the two bases don't silently fork.

## What exists (disk-only, never tracked in git)

- `core/docs/research/` — 7 topic folders with `SUMMARY.md` + selected
  per-paper notes: world-modelling, agentic-orchestration,
  agent-task-assignment, reactive-planning, agent-loops, agent-prompting,
  agent-memory — plus `Recurrent_Processing_Theory.md` and a
  cross-cutting findings section (plan-then-execute convergence, handoff
  as unit of correctness, recurrence-gate novelty claim, etc.).
- `core/docs/context/` — business-facing context stack (glossary,
  capabilities, moat, benchmarks-and-claims, competitive-technical, …).
  Of academic interest: `benchmarks-and-claims.md` (Tier-1/2/3 claim
  catalog for investor/marketing artifacts).
- **Gap:** ADR-0100 cites `docs/research/agent-selection-mechanisms/
  SUMMARY.md` — absent from this checkout AND from git history. The
  routing-diagnosis research doc that ADR-0100 rests on is not retrievable
  here (`core/docs/adr/` is likewise deleted at HEAD `f9e30ad`; ADRs read
  from rev `102aa09`).

## Paper candidates — title-match BEFORE adoption

June notes predate the verification protocol (never recall IDs;
title-match off fetched pages): treat as leads, not citations.

| ID | Note file | Why it matters | Status |
|---|---|---|---|
| 2605.21932 | agent-task-assignment/auction-consensus-learned-bidding | learned bidding in CBBA auctions — the "learned-bid auction" companion to the Agora steelman for dispatch | lead |
| 2606.19795 | agentic-orchestration/handoff-perspective-eda | handoff as unit of correctness vs Cambrian `Handoff` | lead |
| 2606.09032 | world-modelling/text-world-models | world-model lineage vs ToolRegistry/WorkspaceStage | lead |
| 2606.15405 | agent-memory/t-mem | memory architecture comparator | lead |
| 2605.13037 | reactive-planning/map-then-act | planning-paradigm contrast | lead |
| 2605.14290 | reactive-planning/web-agents-plan-then-execute | already in bib (`planexecute2026`, verified earlier) | done |

## Claims cross-check (next pass)

- `docs/context/benchmarks-and-claims.md` Tier-1 numbers (e.g., LoCoMo
  Stage-A blend recall 0.673→0.763; Stage-B reranker regression
  0.168→0.089) should be reconciled against the flagship claim map —
  same evidence, two registers (papers vs business artifacts).
- Rule: no academic draft claim may exceed what this file permits;
  the papers need not adopt its tiers, but must not contradict its
  measured values.

## Action list

1. Title-match the five lead IDs off fetched arXiv pages; promote those
   that carry claims (bib + review + relevant draft).
2. Reconcile benchmarks-and-claims Tier-1 values with flagship rows
   (add missing measured numbers where manifest-traceable).
3. User decision: ADRs + `docs/research/` + `docs/context/` fate —
   tracked or local-only; if local-only, ACADEMIC.md must say so so
   future agents don't expect these paths in fresh clones.
