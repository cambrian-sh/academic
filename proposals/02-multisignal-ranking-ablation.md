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

## Design

- **Arms:** full stack; minus reranker; minus KG-expansion; vector-only;
  FTS-only; KG-only. Pre-registered grid, same frozen corpus + embedder.
- **Suites:** `chunking` (220 Q, 4 categories) for retrieval metrics;
  `document-qa` (600 chunks, 4060 Q) and `musique` for end-to-end effect.
- **Metrics:** recall@10, precision@10, hit-rate@10 (utterance level, per
  GraphRAG-Bench eval), faithfulness/groundedness/coherence of answers,
  exact-match accuracy, latency + tokens per query.

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
