# External deep-research brief 1 — Frontier-lab blog corpus capture

Paste into ChatGPT Deep Research or Gemini Deep Research. Expected output:
a structured corpus, not prose — I fold it into academic/notes/ myself.

## Objective

Exhaustively capture frontier-lab and leading-startup TECHNICAL publications
on AI agents (blog posts, engineering reports, system cards' agent sections,
cookbooks with measured results), 2023 → 2026-10. Only items with technical
substance (numbers, ablations, failure analyses, scaffolds); skip pure
product announcements but COUNT them.

## Context (Cambrian)

Cambrian is an open-core Go kernel for multi-agent LLM orchestration:
planner → Gatekeeper (capability filter + merit rank) → Dispatcher;
Postgres/pgvector memory with hybrid + KG²RAG retrieval; tag-algebra access
policy enforced kernel-side; black-box benchmark harness with row traces.
Research topics: capability-typed dispatch economics, multi-signal ranking,
evidence receipts/provenance, scope-enforced retrieval, episodic→procedural
memory, typed query guarantees. Full context:
`core/docs/ARCHITECTURE.md`, `academic/RESEARCH_AGENDA.md`,
`academic/LITERATURE_REVIEW.md` (v0.3 — do NOT duplicate its ✅ entries;
extend them).

## Sources to cover (non-exhaustive — add what I missed)

Anthropic engineering blog + cookbook + system cards; OpenAI blog + system
cards + Codex reports; Google Research blog + DeepMind publications page;
Meta engineering + Llama agent stack; Microsoft Research blog (AutoGen,
GraphRAG, Magentic-One); Sierra/SWE-smith/tau-bench lineage; Cognition,
Windsurf, Cursor, Replit, Sourcegraph, Augment Code, Tessl engineering
blogs; LangChain/LangGraph, LlamaIndex blogs; Mem0/Zep/Letta/supermemory
memory vendors; smol.ai/news, Latent Space, Interconnects (use as pointers,
verify primaries).

## Output schema (one row per item — markdown table)

| title | URL | date | lab | type (post/report/card/cookbook) | substance (numbers/ablation/failure, 2 lines) | Cambrian topic (dispatch/harness/memory/retrieval/tools/eval/safety/cost) |

Plus: (a) count of skipped marketing items, (b) 5 items you judge most
important for a capability-typed-dispatch economics paper, (c) gaps you
noticed (lab topics with NO technical publication).

## Verification rules

Every URL must resolve (no recalled links). Quote the number with its
denominator. Flag second-hand claims (press coverage of a post) vs primary.
