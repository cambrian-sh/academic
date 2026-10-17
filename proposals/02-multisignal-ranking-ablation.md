# Study Proposal 02 — Multi-Signal Ranking Ablation (ADR-0054)

- **Agenda topic:** 2 · **Horizon:** 1 month · **Status:** proposed

## Research question

What is the marginal contribution of each retrieval signal — dense vector,
FTS, KG-expansion (ADR-0053 D3), reranker — to recall@k and answer quality?

## Background

Cambrian's retrieval pipeline is KG²RAG (vector seed → KG expansion → graph
filter → rerank) with multi-signal ranking (ADR-0054) over per-chunk triplets
(migration 009: confidence + sources). Baselines: GraphRAG-Bench harness
(Xiao 2506.02404; Xiang 2506.05690), Microsoft GraphRAG, LightRAG/HippoRAG
family, STARK.

## Design (updated 2026-10-02)

- B8 control (Selective Forgetting, `selectiveforget2026`): the ablation
  must include a graph-pruning arm AND a flat-baseline arm at matched
  budget — the negative result is the alternative hypothesis, not an
  afterthought. MemTree (`memtree2024`) as the middle tier between static
  hierarchy and flat store.

- **Arms:** full stack; minus reranker; minus KG-expansion; vector-only;
  FTS-only; KG-only. Pre-registered grid, same frozen corpus + embedder.
- **Suites:** `chunking` (220 Q, 4 categories) for retrieval metrics;
  `document-qa` (600 chunks, 4060 Q) and `musique` for end-to-end effect.
- **Metrics:** recall@10, precision@10, hit-rate@10 (utterance level, per
  GraphRAG-Bench eval), faithfulness/groundedness/coherence of answers,
  exact-match accuracy, latency + tokens per query.
- **Wiring 2026-10-09:** flat-baseline arm has an industry leg (Letta
  filesystem 74.0% LoCoMo on gpt-4o-mini, `letta2025filesystem`) alongside
  the B8 academic negative — the skepticism prior is now two-sourced.
  Report all arms at model×harness configuration level (Harness-Bench
  rule, `yao2026harnessbench`); pin inference config (stateful KV-cache
  serving halves wall time per `norgren2026stateful` — a confound if it
  drifts between arms).

## Apparatus

`benchmarking/` chunking + document-qa suites; GraphRAG-Bench eval module
for cross-framework comparability; per-row failure kinds (retrieval miss vs
synthesis failure).

## Analysis

Signal-ablation table with CIs; interaction check (does KG-expansion help
only multi-hop?); cost-per-recall-point figure — the publishable core.

## Risks

- Corpus/embedder drift across arms → freeze both, record SHAs.
- Reranker flag is opt-in (ADR-0123 residual) → test both defaults, report.

## Artifact

Ablation table + cost/quality figure → retrieval section of substrate
flagship or standalone SIGIR-style short paper.
