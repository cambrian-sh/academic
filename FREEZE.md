# Experiment Freeze Record

Must be updated (new row) before any paper experiment runs.

## Freeze F-2026-09-26 (baseline, no experiment yet)

| Item | Value |
|---|---|
| core HEAD | `1fdad89a3fb641afb3d9d7c44d89e4fb2ea05375` |
| core tree | dirty: `?? .codebase-memory/` (untracked index artifact only) |
| benchmarking HEAD | `02ef800f6f847d177b110cd679d45b58d13c3621` |
| datasets present | agentic_erp_crm, airline, chunking, document_qa, gaia, musique, orchestration, tau2, techqa (+ PDFs) |
| prior runs in `out/` | none (only `beta.sum.md`) — no usable baseline archive yet |
| runtime version | v0.6.10-Alpha (proto + config stable, Go API unstable) |
| 2026-09-27 amend | benchmarking tree dirty: `MemoryHit.policy_note` transport fix (4 files, uncommitted). Any experiment from here needs a new freeze row. |
| 2026-09-27 amend 2 | authz wraps + premium re-vendor landed (uncommitted): `save_group`/`delete_group`/`delete_policy`/`list_policies`/`list_links`/`bind_identity`/`unbind_identity`/`resultant_policy`/`export_decisions` + `save_policy(party_scoped_tags)`; stubs re-vendored to contract 0095 (`party_scoped_tags`, `row_scope_note`, `party_identities`); regen fixed for the `premium/` workspace rename. Design §6.1 "no re-vendor" claim corrected — a re-vendor WAS needed. |
| 2026-09-27 amend 3 | `policy-asks` suite v0 landed (uncommitted): generator (180 records × 28 principals → 288 asks, 37-cell/≤2-ambiguous assertion), attribute-level scoring (confusion matrix + rates + new failure kinds), A0 runner + registry/CLI wiring, 17 suite tests. Live-kernel verification (ingest/setup/arms) still open; needs `CAMBRIAN_BINARY`. |
| 2026-09-29 amend 4 | Frozen dataset emitted: `datasets/policy_asks/policy_asks.v1.jsonl` (352 asks 52:39:9, sha `2df12f83…`, byte-reproducible) + `manifest.json`; paired arm comparison (`compare_pair`: Fisher + Welch + 95% CIs on ask-id-joined rows) in scoring; 24-pair calibration set vs GOV-01 with code-grounded mapping tests. Still open: live runs, prediction doc, A1–A4 arms. |

## Rules

1. No experiment counts without a freeze row + pinned SHAs in the run manifest.
2. Dirty tree must be committed or stashed before freezing (F-2026-09-26 is
   NOT experiment-clean: `.codebase-memory/` untracked; harmless but record it).
3. Dataset snapshot: record `sha256sum` of the suite dataset dir at freeze time.
4. Baselines: first runs under a freeze become the control arms for later work.
