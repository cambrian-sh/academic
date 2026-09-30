# Cambrian Research Agenda — What to Study

Derived from a scan of `core/` (ARCHITECTURE.md, CONTEXT.md, ADRs 0001–0125,
`benchmarking/` suites) on 2026-09-20. Each topic names the Cambrian
mechanism, the open research question, and the concrete artifact expected.

## System in one paragraph

Cambrian is an open-core Go kernel (`github.com/cambrian-sh/core`, v0.6.10-Alpha)
for multi-agent LLM orchestration: a Planner (Awareness layer) emits DAG
execution plans; a Gatekeeper filters agents by declared capability and ranks
by merit; a Dispatcher calls the winner over a typed agent-plane gRPC
contract. Memory is a Postgres/pgvector substrate with hybrid retrieval
(vector + FTS + KG²RAG graph expansion + reranker), evolving toward an
evidence-first epistemic split (content-addressed Evidence → KnowledgeItems →
versioned Resolutions → typed QueryPlane). Access policy is tag-algebra
enforcement (deny-by-default closed tags, grants, row-level party scoping).
`benchmarking/` evaluates the kernel black-box via `BenchmarkTransport`
(row-level traces + run manifests). Premium adds reactive execution,
ingress studio, and tracing via `app.Options` (ADR-0057).

---

## 1-month horizon (paper-ready material: measure what exists)

1. **Capability-typed dispatch vs auction selection (ADR-0100).**
   Question: does zero-RPC merit-ranked dispatch match or beat bid-based
   auction selection on routing accuracy and latency?
   Artifact: `orchestration` suite comparison (routing_accuracy, merit
   dispersion, candidates/slate) + write-up. Closest to a publishable
   systems result today.
2. **Multi-signal ranking ablation (ADR-0054).**
   Question: marginal contribution of vector / FTS / KG-expansion / reranker
   signals to recall@k on the `chunking` + `document-qa` suites.
   Artifact: ablation table + per-row failure-kind analysis.
3. **Agentic retrieval loop evaluation.**
   Question: does the `agentic-retrieval` suite (4-hop, grounded
   decomposition) beat single-pass retrieval on multi-hop QA (MuSiQue)?
   Artifact: suite comparison with cost (tokens/latency) attached.
4. **Evidence receipts / decision provenance (ADR-0103).**
   Question: what is the hot-path latency cost of emitting per-retrieval
   decision records, and what auditability does it buy?
   Artifact: latency arm (the ADR's unrun arm) + threat-model note.
5. **Overclaiming / faithfulness probe.**
   Fresh literature (OverclaimBench, Sep 2026) shows agents misreport
   coverage. Cambrian's routing traces + `AgentStepOp` feed events give a
   measurement apparatus. Artifact: small probe study on plan-quality-lab.

## 3–6-month horizon (needs building + measuring)

6. **Epistemic split: contradiction as a first-class citizen (ADR-0106/0110/0111).**
   Question: can append-only knowledge items + derived resolutions represent
   and resolve cross-actor contradictions better than overwrite stores?
   Artifact: gate results (PO-800 contradiction case) expanded into a
   KR-style evaluation; position paper candidate.
7. **Scope-enforced retrieval / read-scope (ADR-0118).**
   Question: what is the recall cost of fail-closed policy enforcement
   (16/16 vs 12/16 baseline already observed), and where does utility break?
   Artifact: security × utility Pareto curve — strong usable-security venue fit.
8. **Row-level entitlement / party scoping (ADR-0121).**
   Question: formal properties of the implication-rendered qualifier
   (intersection-preservation, INV-1/INV-3) + benchmark suite for policy
   benchmarks (currently missing — must be built).
9. ** Quantitative policy knowledge (ADR-0109).**
   Question: can handbook bounds be extracted once and enforced
   deterministically (0.667 + abstention triple)? Needs a REAL handbook
   corpus — the honest-limit note is itself a research contribution.
10. **Experiences as first-class entities (ADR-0095) + procedural memory (ADR-0094).**
    Question: does episodic→procedural consolidation improve longitudinal
    benchmarks (LoCoMo suite)? Artifact: LoCoMo before/after consolidation.
11. **Typed query plane guarantees (ADR-0111, §14).**
    Question: which query classes admit completeness/validity labels, and what
    is formally inexpressible (`ErrCannotExpress`)? Theory + gate evidence.

## 1-year horizon (program-level bets)

12. **The Knowledge Substrate as a thesis.**
    Evidence → commitments → events → policy → kinds → typed queries
    (Phases 1–7, ADR-0105–0111) is a coherent epistemology for agent memory:
    content-addressed, contradiction-tolerant, scope-enforced, query-typed.
    A full paper needs the missing pieces named in CONTEXT residuals
    (consumer lag, per-model prefixes, event roles, operator scoping).
13. **Open-core separation as a research artifact (ADR-0057).**
    Mechanism (OSS fails open / premium fails closed, `app.Options` seam,
    CI-enforced boundary) + case study. Fits SE/OSS venues.
14. **Data-driven development methodology.**
    The benchmark-harness-as-specification practice (row traces, failure
    kinds, arm comparisons, DECISIONS log) generalized as an evaluation
    methodology paper for agent systems.
15. **Reactive-first execution (premium ADR-0032 + core ADR-0104/0113/0114).**
    Once mature: durable reactive pipelines vs DAG orchestration, formally
    and empirically.

## Recommended order

Month 1: topics 1–3 (all apparatus exists) + start 4.
Months 3–6: topics 6–8 (strongest novelty: substrate + policy).
Year: topic 12 as the flagship paper; 13–15 as satellites.
