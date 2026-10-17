# Program log — academic initiative (started 2026-09-20)

Running narrative: decisions, findings, roadmap, debt. **Append a dated
entry at the end of every session** (see `STATUS.md` §7 and `AGENTS.md`).
For live state see `STATUS.md`; research content: LITERATURE_REVIEW.md,
RESEARCH_AGENDA.md, papers/; experiment state: FREEZE.md.

**2026-10-17:** renamed from `session-2026-09-20_10-04.md` during CTO
handoff prep.

## Mission (user, 2026-09-20)

Academic work on Cambrian: scan core, set up `~/Code/cambrian/academic/`,
find study topics (1 mo / 3–6 mo / 1 yr horizon), do literary research.

## User decisions (binding unless explicitly lifted)

1. Approved the suggested execution order: proposals first (2026-09-23).
2. Approved model `opencode/space-bunny-free` for subagents; also allowed
   opencode-go equivalents / other free models on rate limits.
3. "You pick" → I chose proposals-first; later "proceed" / "continue" /
   "go" authorise continued autonomous execution.
4. Subagent cancellations were INTERRUPTIONS + retries, NOT a preference
   for direct work. Do not infer workflow preferences from cancellations.
   (I made this false inference once — corrected 2026-10-01.)
5. feynman (user-installed academic research expert agent) offered for use.
   Status: NOT found in my agent list, skills, plugins, or opencode config
   (checked 2026-10-01). To use it I need a model ID or definition path.
6. User holds ChatGPT Plus + Gemini Pro (deep research + local agents);
   two paste-ready briefs written: `notes/brief-external-frontier-blogs.md`,
   `notes/brief-external-negative-controls.md`.
7. Latest directive (2026-10-0x): leave NO paper/report/review/blog
   unnoticed — exhaustive coverage, two purposes: (a) support Cambrian's
   claims, (b) find strengths/improvements → publishable directions.

## Findings (verified; see cited files)

- Core: v0.6.10-Alpha Go kernel; Gatekeeper→Dispatcher capability-typed
  dispatch (ADR-0100 killed auctions); 7-phase Knowledge Substrate
  (ADR-0105–0111); tag-algebra policy (ADR-0085/87/91, 0118/0121);
  black-box benchmarking harness, 9+ suites.
- No peer-reviewed BSL/open-core literature exists → ADR-0057 paper must
  argue, not cite (§5 retitled "Open-core licensing and the OSS business
  model").
- Sham-policy placebo (Zhang 2609.20474) has no ICLR counterpart after 3
  passes — unique methodological borrowing.
- ICLR 2026: ~5 468 unique papers (223 Oral duplicates); exhausted for
  orchest*/rout*/memory/trajectory AND verifier/rubric/judge (4 passes).
- B8 negative (2608.28978): graph expansion loses to flat baseline at
  matched budget — flagship-gating reproduction obligation on our suites.
- Salami coalitions + SPORE egress bound per-write/scope claims.
- websearch integration dead; Tavily works; arXiv API 429s after ~6–30
  calls — use arxiv.org/abs + 20–45s spacing; mdpi.com 403 (use Crossref);
  openreview.net walled (use iclr.cc virtual URLs); S2/OpenAlex 429 fast.
- Verification protocol (mandatory): every ID title-matched off a fetched
  page; never recall IDs (3 confirmed failures: wrong PLAN-AND-ACT,
  AdaPlanner, LLM-Modulo IDs); mark unconfirmed UNVERIFIED; venue only
  from record Comments/JREF/PMLR/Anthology. Caught live issues: Fang
  Yi→Yuwei bib bug; unsupported TRACE CIKM claim; Redis was RSALv2+SSPLv1
  not BSL; Memory³ is JML not JMLR; MCP-Bench name collision; no
  NesTools/RCI/GITM-memory papers exist.

## Code written (benchmarking/, ALL UNCOMMITTED)

- `MemoryHit.policy_note` end-to-end (agent.py, memory.py, inspect.py).
- authz.py: 9 new wraps + party_scoped_tags on save_policy; new
  test_authz.py.
- Regen fixed for `premium/` rename; stubs re-vendored to contract 0095.
- `policy-asks` suite v0: generator (180×28→352 asks, 52:39:9),
  attribute-level scoring, confusion matrix + rates, `compare_pair`
  (Fisher+Welch), 24-pair GOV-01 calibration (code-grounded), A0 runner,
  registry/CLI wiring, frozen dataset (sha `2df12f83…`, reproducible).
- Design §6.1 "no re-vendor" claim disproved and corrected in notes.
- 589 tests pass; 59 simulator/supervisor env failures proven pre-existing
  (stash comparison); ruff clean on touched files; CONTEXT.md updated.
- Prediction scaffold: `docs/policy-asks-baseline-prediction.md` (TBD-gated).

## Roadmap (in mind)

1. Needs user: CAMBRIAN_BINARY + DB (live verification); worldsim request;
   commit decision on benchmarking/ work.
2. Then: prediction fill → A0 run → A1/A2 + parity → 04 hot-path arm →
   02 ablation.
3. Calendar: EMNLP proceedings (Oct 2026), CIKM list (post-Nov-7), ResPol PDF.
4. Papers: dispatch satellite → security satellite → substrate flagship.
5. Lit re-checks at revision time (many MCP preprints arXiv-only).

## Open debt register

- FuncBenchGen OpenReview event ID; PoisonedRAG full bib entry.
- ResPol-2026 effect sign (needs PDF); BSL adoption rates (industry only).
- 11 routing-sweep IDs flagged verify; TOIS-survey second-ring entries
  in notes only.
- Live-kernel: transport smoke test, A0–A4, calibration parity run,
  ADR-0094/0095 build (topic 10).

## Addendum 2026-10-05 → 2026-10-09

- 10-05: Letta program archived (`data/external/letta/`, 16 reports +
  2 PDFs + SHA256SUMS) and folded into review/bib (`notes/letta-2026-10-05.md`).
- 10-06: debt burn-down — 19 closed (PoisonedRAG corrected to USENIX 2025;
  9 venues confirmed; 14-title router pool promoted; 2503.10657 mismatch +
  ME-Switch description corrected); related-work v0.3; METHODOLOGY
  borrowings; token-economics 4th pass, 8 kept
  (`notes/token-economics-2026-10-06.md`); proposal 01 wired; leftovers in
  `notes/brief-external-leftovers-2026-10-06.md`. Bib 185 → 216.
- 10-07/09: "what next" asked twice; direction question aborted once.
  Proceeding on standing recommendation (dispatch satellite first).
- 10-09: dispatch satellite draft v0.1 created
  (`papers/dispatch-satellite-draft.md`) — non-results sections written,
  results as manifest-tied TBD placeholders. 20-label gold audit and
  P1–P4+P11 pre-registration identified as the two unblocked next steps.
- Still blocked: CAMBRIAN_BINARY/DB, commit approvals, worldsim request,
  5 external-brief items, EMNLP proceedings (conf. Oct 24–29), CIKM (Nov 7).
- Correction: `core/docs/adr/` does not exist in this checkout (ADRs
  referenced but not re-read since earlier session) — draft v0.1 §3
  flagged for ADR re-verification before submission.
- 10-09 ("go" + "next?"): gold-audit protocol + P1–P4/P11 pre-registration written
  (`benchmarking/docs/proposal-01-gold-audit.md`,
  `benchmarking/docs/proposal-01-pre-registration.md`; untracked, no
  commits per standing rule). Pass-1 verdicts: 19 CONFIRM (4 noted),
  1 systemic AMEND (test_execution on 10 tool tasks is prompt-invisible —
  all 10 prompt templates shared across test/non-test boundary — and inert
  under intersection matching; recommend dropping for v1). Mechanical
  check: 120/120 aligned, acceptable_winners empty everywhere. Dataset
  shas pinned in both docs. Needs user countersign + amendment before any
  parity claim.
- 10-09: security satellite draft v0.1 created
  (`papers/security-satellite-draft.md`) — same house style as dispatch
  v0.1; results TBD on A0–A4 manifests. Roadmap paper order now: dispatch
  v0.1 ✅, security v0.1 ✅, flagship skeleton next for prose (rows still
  ❌-blocked).
- 10-09/10: proposals 02–04 wired (flat-baseline industry leg + config-level
  reporting + inference-confound pins; 03 gets usage-vs-generation and
  auditor-pattern arms). Frontier delta Oct 1→10 across 4 restarts:
  Anthropic containment (May, archived) + DeepMind sealed memory (Sep,
  archived) folded to review §4 + bib (218); EmbeddingGemma 2 watched not
  cited; OpenAI index + double-blind evals backlogged
  (`notes/frontier-blogs-delta-2026-10-10.md`). BSL debt converted to
  industry-sourcing checklist (`notes/bsl-industry-sourcing-2026-10-10.md`;
  only Redis anchor previously verified).
- 10-11 ("continue, review, analyze, fix, improve"): quality pass, no new
  coverage. Fixed: 13 bare-ID promotions (2 venue corrections:
  BEST-Route ICML 2025, Zhan NAACL Findings); review key-linked; ID
  coverage verified clean across review + 3 drafts; convention recorded.
  Bib 218 → 231. Deliberately deferred: router-family/second-ring pools,
  cascade-list mentions (notes-resident by policy).

## Addendum 2026-10-17 — full end-to-end review pass

- ADR §3 verification CLOSED for both satellites from git history:
  core main @ `f9e30ad` deleted `docs/adr/` (125 files, pushed); read
  ADRs from rev `102aa09` (last tree carrying them). Dispatch: control-arm
  status recorded (P2 not performed; archived auction arm degraded,
  112/120 rows errored), capability-contract "not an arm" nuance folded;
  security: D3 crippled-grant algebra + D5 denial-explanation design
  counterpart added.
- MarketBench (`fradkin2026marketbench`) + Agora (`zhou2026agora`, EMNLP
  2026 Findings) title-verified off fetched arXiv records (ADR-0100's
  external evidence) and promoted (bib 231 → 233); Agora recorded as the
  calibrated-auction steelman bounding the zero-RPC claim.
- Discovered untracked `core/docs/research/` (June-2026 library, 7 topics
  + per-paper notes; never tracked in git) and the `core/docs/context/`
  business-context stack incl. `benchmarks-and-claims.md` (Tier-1/2/3
  claim catalog). Integration cross-check scoped:
  `notes/docs-research-integration-2026-10-17.md`.
  `agent-selection-mechanisms/SUMMARY.md` (cited by ADR-0100) absent from
  checkout AND git history.
- Root `CONTEXT.md` academic row corrected (was "Not a Git repository").
- Still user-gated: countersign, commits, kernel binary + DB, worldsim,
  external briefs, ADR-location decision.
