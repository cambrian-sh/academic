# External deep-research brief 2 — Negative-control methodology for agent evaluation

Paste into ChatGPT Deep Research or Gemini Deep Research. This is the one
literature hole three systematic ICLR passes could not fill from inside
ML venues — it needs cross-disciplinary depth.

## Objective

Find negative-control / placebo / sham-treatment evaluation designs that an
LLM-agent benchmark can borrow: methods that isolate the contribution of an
intervention's CONTENT (guidance, plan, retrieval, memory) from the mere
fact of intervening (more tokens, more steps, more calls).

## Context (Cambrian)

Our dispatch ablations compare capability-typed routing against baselines.
Known confound: any treatment that spends more tokens/steps can win without
its content mattering (Anthropic measured token usage alone explaining 80%
of BrowseComp variance). The only known authority is Zhang et al.,
arXiv:2609.20474 — Fixed prewritten plans vs word-count-matched SHAM policy
text over 265 trials. I need: (a) precedent and refinement of sham/placebo
controls in ML evaluation; (b) borrowed designs from medicine (sham
surgery/placebo), psychology (active placebos, demand characteristics,
Hawthorne controls), economics (placebo tests in diff-in-diff, randomization
inference), HCI (placebo personalization); (c) how to construct a
word-count/compute-matched sham for: a planner, a retriever, a memory
module, a router; (d) statistics for placebo-controlled agent comparisons
(permutation tests, what n buys what power at effect sizes 2–10pp).

## Output schema

1. Per-field survey (medicine/psych/econ/HCI/ML): canonical citations with
   DOI, the control design in 3 lines, transferability to agent eval in
   2 lines.
2. A concrete sham-construction recipe per Cambrian module (planner /
   retriever / memory / router), each with falsification criterion.
3. Power-analysis guidance: n needed to resolve 2/5/10pp effects with a
   sham control, with assumptions stated.
4. Verdict: is Zhang-2609.20474-style sham control sufficient, or does the
   literature suggest a stronger design?

## Verification rules

DOI for every peer-reviewed claim (Crossref-checkable). Distinguish
established methods from your own synthesis — label the latter.
