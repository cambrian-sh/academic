# Frontier-blog delta — 2026-10-01 → 2026-10-10

Indexes re-read: Anthropic engineering, DeepMind blog. OpenAI index not
re-read (prior sweeps found it low-yield for our axes; next delta should
cover it). Window items are few; two pre-window items folded because they
were uncovered and directly on-theme.

## New in window

- ✅ Google, "EmbeddingGemma 2" (Oct 2026) — open, lightweight, natively
  multimodal embedding model, "best-in-class" per announcement. No
  evaluation numbers retrievable from the announcement page (nav-heavy,
  substance is the title claim). Verdict: WATCHED, not cited — relevant
  to substrate projection/backfill model choices if numbers appear; no
  bib entry (no padding without claims).

## Pre-window, uncovered, folded

- ✅ Anthropic, "How we contain Claude across products" (2026-05-25,
  archived `data/external/frontier/anthropic-contain-claude-2026-05-25.html`,
  sha in SHA256SUMS). Environment-layer-first containment across three
  products; headline evidence: 93% permission-prompt approval (approval
  fatigue kills HITL oversight); sandbox escape by "helpful" model;
  pre-trust-dialog code execution (defer config parsing until after trust);
  user-as-injection-vector phish (24/25 exfiltration — model layer cannot
  help); allowlist-as-capability-grant (Files API exfiltration through
  api.anthropic.com); custom-built components fail while hypervisors/
  seccomp/gVisor hold. Principle: deterministic boundaries first, model
  steering second — independent convergence with "kernel disposes".
  → review §4, draft §3, security-draft limits.
- ✅ Google DeepMind, "Advancing Private AI Compute with secure,
  server-side memory" (2026-09-23, archived `data/external/frontier/
  google-private-compute-2026-09-23.html`). Sealed cloud memory, device-held
  keys, enclave decryption, per-user DBs, tamper-proof public software
  record + independent audit. Industry's answer to persistent-memory
  privacy; trust-via-transparency pattern adjacent to our manifest/
  receipt auditability. Contrast point for scope-enforcement: they solve
  *who can see*, we solve *what may be retrieved under which predicate*.
  → review §4.

## Backlog (not fetched)

- DeepMind "double-blind AI evaluations" (Aug 2026) — eval methodology,
  check next delta.
- OpenAI index re-read due next delta.
