# AGENTS.md — working rules for this research repo

For any agent (or human) conducting work here. **Read `STATUS.md` first** —
it is the live state, blockers, and next actions. Then this file.

## Conducting loop

1. Read `STATUS.md` (phase, blocked, next actions).
2. Check the decision ledger (`STATUS.md` §5) — binding until lifted.
3. Take the first *unblocked* item from "Next actions"; if everything is
   blocked, say so and pick the closest unblocked writing task.
4. End every session with the maintenance protocol (`STATUS.md` §7):
   append to `notes/program-log.md`; refresh `STATUS.md`.

## Verification protocol (never skip)

- **Never recall identifiers** (DOI / arXiv / OpenReview). Fetch the
  page; title-match before use; mark UNVERIFIED otherwise.
- Venues only from the record itself (arXiv Comments/JREF/PMLR/
  Anthology) — never from memory.
- **Claims trace to run manifests**
  (`../benchmarking/out/.../run_manifest.json`) or ADR gates. Results
  cells are TBD until a manifest exists.
- **Freeze first:** new `FREEZE.md` row (kernel SHA, tree state, dataset
  shas, resource envelope) *before* any run.
- Honest limits are mandatory in every draft; concede bounded claims
  early (Salami/SPORE), don't discover them late.

## Citation conventions

- `bib/references.bib` is canonical for submission-bound citations.
- Bare arXiv IDs in notes/review are "verified-in-notes" by convention —
  do not promote or "fix" them by guessing.
- Industry posts cite as company technical reports (Letta et al.), never
  as papers.

## Commit rules

- Standing rule (2026-10-17): **do not commit `benchmarking/` or
  `academic/` changes without explicit owner/CTO approval** — the trees
  deliberately carry uncommitted work. Update this section when the CTO
  rules on `HANDOFF.md` §4.4.

## Tool realities (saves hours)

- `websearch` integration is dead; Tavily works.
- arXiv API 429s → use `arxiv.org/abs/` with 20–45 s spacing.
- mdpi.com 403 → Crossref. OpenReview v2 is challenge-walled → use
  `iclr.cc` virtual URLs. Vendor pages that resist fetch (Anthropic,
  DeepMind) work with curl + UA.
- Semantic Scholar / OpenAlex rate-limit fast; don't rely on them for
  bulk.
- Server restarts can abort long sessions: verify disk state before
  repeating completed work.

## Update obligations (the "kept up to date" promise)

| Event | Must update |
|---|---|
| Experiment run | `FREEZE.md` row (before) → draft results cells (after, manifest-cited) → `STATUS.md` → program log |
| Literature pass | bib + `LITERATURE_REVIEW.md` (+ `papers/related-work-draft.md` if citation-bearing) → `STATUS.md` |
| Decision by owner | `STATUS.md` §5 → program log |
| Any session at all | program log entry → `STATUS.md` "Last updated" |
