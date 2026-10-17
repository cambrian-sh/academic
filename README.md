# Cambrian Academic Workspace

Research hub for the published-facing work on Cambrian: papers, verified
literature, study proposals, and (planned) technical blog posts. Product
code lives in the sibling repos (`core/`, `benchmarking/`, `ui/`, …);
this repo holds research artifacts only.

## Start here

| You are | Read, in order |
|---|---|
| CTO / new owner | [`HANDOFF.md`](HANDOFF.md) → [`STATUS.md`](STATUS.md) |
| The conductor (agent or engineer) | [`STATUS.md`](STATUS.md) → [`AGENTS.md`](AGENTS.md) |
| Reviewing a paper | `papers/` (results sections are TBD until runs exist) |

## Layout

| Path | Content |
|---|---|
| [`STATUS.md`](STATUS.md) | **living state** — phase, blockers, next actions, decisions |
| [`HANDOFF.md`](HANDOFF.md) | CTO onboarding packet (read once) |
| [`AGENTS.md`](AGENTS.md) | working rules for the conductor |
| [`RESEARCH_AGENDA.md`](RESEARCH_AGENDA.md) | study topics (1 mo / 3–6 mo / 1 yr) |
| [`LITERATURE_REVIEW.md`](LITERATURE_REVIEW.md) | living review (§1–5), verification status inline |
| [`PAPER_OUTLINE.md`](PAPER_OUTLINE.md) | early paper sketch (the drafts supersede it) |
| [`METHODOLOGY.md`](METHODOLOGY.md) | harness → academic evaluation; minimum bar per experiment |
| [`FREEZE.md`](FREEZE.md) | experiment freeze record — new row before any run |
| `papers/` | drafts: dispatch + security satellites, flagship skeleton, related work |
| `proposals/` | one-page study proposals |
| `notes/` | program log, literature sweeps, external briefs, integration notes |
| `bib/references.bib` | canonical verified references |
| `data/external/` | archived industry sources (SHA256SUMS verified) |

## Rules (non-negotiable)

1. Every empirical claim traces to a run manifest or an ADR gate.
2. Title-match every identifier before citing — never recall IDs.
3. New `FREEZE.md` row before any experiment run.
4. Raw benchmark outputs stay in `benchmarking/out/`; keep pointers here.
5. End every session by updating `STATUS.md` + `notes/program-log.md`
   (maintenance contract: `STATUS.md` §7).

## Cross-repo pointers

- Kernel + ADRs: `../core/` — ADRs currently deleted from HEAD; read from
  git rev `102aa09` until the location decision lands (`HANDOFF.md` §4.3).
- Harness + experiment docs: `../benchmarking/docs/` (gold audit,
  pre-registration, prediction scaffolds; `policy-asks` suite).
- Workspace index: `../ACADEMIC.md`.
