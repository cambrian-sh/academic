# External deep-research brief — literature leftovers (2026-10-06)

Paste into ChatGPT Plus / Gemini Pro deep research. Everything below
survived direct verification (arXiv API + title search, OpenReview API,
Crossref/OpenAlex/Semantic Scholar, publisher pages) and needs either
paywalled access or bibliography archaeology.

## 1. ResPol-2026 effect sign (highest value)

Paper: Wei Yang (CEIBS), "The financial implications of open innovation:
Evidence from stock market reactions to public companies' open source
software releases", Research Policy 55(7), Sept 2026, art. 105500.
DOI 10.1016/j.respol.2026.105500.
Known: event-study design, cumulative abnormal returns (OpenAlex keywords);
116 references; closed access; NO abstract in any metadata deposit.
Question: what is the SIGN of the stock-market reaction (positive /
negative / conditional)? Retrieve the abstract or full text via
institutional access and report the headline CAR result plus any
heterogeneity (license type? firm size?).
Why it matters: open-core paper (ADR-0057) needs the direction of the
market's verdict on OSS releases, not just existence.

## 2. FuncBenchGen OpenReview forum ID

Paper: Maekawa, Hassell, Pezeshkpour, Mitchell, Hruschka, "Towards
Reliable Benchmarking: A Contamination Free, Controllable Evaluation
Framework for Multi-step LLM Function Calling", arXiv:2509.26553 (v2
2026-02-06), ICLR 2026 (per arXiv Comments).
Question: the OpenReview forum ID (numeric, e.g. openreview.net/forum?id=...).
Note: OpenReview v1 search returns nothing (ICLR 2026 is on the v2 API);
DBLP is Anubis-walled; S2 lists venue only as arXiv.org.

## 3. Three routing-paper IDs (narrowed, with rejected candidates)

(a) "Difficulty-Aware Hybrid Routing for Cost-Efficient Long-Context LLM
Inference" (2026; three inference tiers from query/context/draft features).
NOT the DATS topology paper (2609.13890) — do not conflate.
(b) "More is not always better?" — routing over-scaling negative result,
authors approx. Zhang, Lv, Liu, Sung, Liu, Luan, Shang, Chen & Yan (2025).
REJECTED: 2605.05716 (scaffolding cross-component interference, wrong
authors/topic) and 2501.04070 (DrICL many-shot ICL, partial author overlap
but wrong topic — possible bibliography garbling, treat with suspicion).
(c) "Routing LLMs with Preference Data: Causal Meta-learners" (causal-
inference router with data augmentation). WARNING: arXiv:2503.10657 was
claimed for this title but title-matches to RouterEval (different paper) —
the citing bibliography mismatched title to ID. Find the real paper or
confirm it does not exist.

## 4. Standing watch items (no action, calendar)

- EMNLP 2026 proceedings (Industry track list deferred).
- CIKM 2026 coverage re-run after 7 Nov 2026.
- BSL adoption rates: industry data only, label as such.
