# Cambrian Paper Outline (draft v0.1)

## Recommended flagship (12-month): The Knowledge Substrate paper

**Working title:** "An Evidence-First Knowledge Substrate for LLM Agents:
Content-Addressed, Contradiction-Tolerant, Scope-Enforced Memory"

**Claim stack (each needs its evidence arm):**
1. Content-first commit ordering prevents silent data loss (ADR-0105 crash/replay gates).
2. Append-only items + derived resolutions represent contradiction without
   destroying history; drift parity 0/244 (ADR-0106).
3. Embeddings as versioned projections survive model round-trips with zero
   refusals (ADR-0107).
4. Scope enforcement costs X recall for Y security (ADR-0118: 16/16 vs 12/16).
5. Closed query AST gives per-answer guarantee labels; refusals name reasons (ADR-0111).

**Structure:** Intro → Related work → Substrate design (7 phases) →
Guarantees (formal bits: INV-1/INV-3, implication rendering, resolution purity) →
Evaluation (gates + benchmark arms + cost) → Honest limits → Conclusion.

## Fast satellite (1-month): Capability-typed dispatch

**Working title:** "Zero-RPC Capability-Typed Dispatch for Multi-Agent LLM
Orchestration"
Evidence: `orchestration` suite routing accuracy + latency; merit-dispersion
finding; comparison vs auction-era archives (with the metric-name caveat).

## Security satellite (3–6 months): Policy-enforced retrieval

**Working title:** "Fail-Closed, Explainable Access Control for Agent Memory:
Tags, Closure, and Party Scoping"
Evidence: ADR-0085/0087/0091/0118/0121 mechanism + utility×security Pareto +
denial-explanation study.

## Venue guidance (to refine with advisor)

- Flagship substrate: CIDR / VLDB (systems+data), or NeurIPS/ICLR Datasets &
  Benchmarks if evaluation-heavy; KR for the epistemic-split theory slice.
- Dispatch satellite: EuroSys / SOSP-AI / AAMAS (systems + agents).
- Security satellite: IEEE S&P / USENIX Security / CCS (agent-memory access control
  is a new enforcement point).
- Methodology note: MLSys / FSE (open-core + data-driven development).

## Next actions

- [x] Baseline freeze recorded (`FREEZE.md` F-2026-09-26; NOT experiment-clean yet).
- [ ] Run the ADR-0103 hot-path arm (rescoped 2026-09-26: LLM-bound numbers exist, LLM-free arm missing).
- [ ] Build the policy benchmark suite (designed 2026-09-26 in
  `notes/policy-suite-design-2026-09-26.md`; prerequisites: worldsim
  artifact request, `query_hits` policy_note fix, authz transport wraps).
- [ ] Advisor review of this outline + venue picks.
