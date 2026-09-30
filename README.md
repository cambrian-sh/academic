# Cambrian Academic Workspace

Research hub for the academic paper(s) on Cambrian. Product code lives in
`~/Code/cambrian/{core,premium,benchmarking,agent-sdk-python,ui,cli}/`;
this directory holds only research artifacts.

## Layout

| Path | Content |
|---|---|
| `RESEARCH_AGENDA.md` | Study topics for 1-month / 3–6-month / 1-year horizons |
| `LITERATURE_REVIEW.md` | Survey of related academic literature (living document) |
| `PAPER_OUTLINE.md` | Candidate paper(s), claims, and evidence map |
| `METHODOLOGY.md` | How Cambrian's benchmark harness maps to academic evaluation |
| `proposals/` | One-page study proposals (one file per topic) |
| `notes/` | Reading notes, meeting notes |
| `papers/` | Drafts (source + PDF) |
| `figures/` | Paper figures (scripts preferred over binaries) |
| `data/` | Small derived tables; raw runs stay in `benchmarking/out/` |
| `bib/` | BibTeX files (`references.bib` is canonical) |

## Rules

1. Every empirical claim in a draft must trace to a benchmark run manifest
   (`benchmarking/out/.../run_manifest.json`) or a cited ADR gate.
2. Verify every citation (DOI/arXiv ID) before submission — see
   `LITERATURE_REVIEW.md` verification status column.
3. Do not copy ADRs here; link to `core/docs/adr/`.
4. Raw benchmark outputs are not committed here; commit only derived tables
   with a pointer to the source run.
