# Related Work (draft v0.1 — 2026-09-27)

Covers the flagship substrate paper + dispatch and security satellites.
Every citation here is ✅-verified (`bib/references.bib`); venue-pending
items are flagged inline. Keyed to `LITERATURE_REVIEW.md` v0.3.

## 1. Flagship: evidence-first knowledge substrate

**GraphRAG systems and evaluation.** The substrate's retrieval stack
instantiates the KG²RAG pattern (vector seed → KG expansion → filter →
rerank) surveyed in Peng et al. (`peng2024graphragsurvey`, ACM TOIS 2025)
and Zhang et al. (`zhang2025graphsurvey`). The planned multi-signal
ablation (proposal 02) runs against the closest existing harness, Zhou et
al.'s unified framework (`zhou2025graphunified`, PVLDB 2025), with LightRAG
(`guo2024lightrag`) as the primary ablation baseline rather than the
Bench's own harness, plus HippoRAG (`gutierrez2024hipporag`) as the
memory-side comparator and RAPTOR (`sarthi2024raptor`) for hierarchical
retrieval. The evaluation methodology answers the gap named in the ACM
survey (`graphrag2025survey`): no standardized multifaceted benchmarks with
ground-truth retrieval elements — hence row traces over final answers.
Independent convergence: Tian et al. (`tian2026partial`, CIKM 2026)
in-trajectory probing arrives at the same trajectory-over-final-answer
position.

**Agent memory benchmarks.** LoCoMo (`maharana2024locomo`, ACL 2024) and
LongMemEval (`wu2025longmemeval`) are the wired suites for the
episodic→procedural consolidation study (topic 10); REMem (`shu2026remem`,
ICLR 2026) is its direct prior art (episodic graph + agentic retriever),
and Misevolve (`shao2026misevolve`, ICLR 2026) the risk register —
accumulation is not automatically safe accumulation. AMemGym and
MemoryAgentBench (both ICLR 2026) mark the interactive/incremental
successor generation and bound our claims to static-offline evaluation.
RAFT (`zhang2026raft`, EMNLP 2026 Industry) is the closest recent
RAG-for-agents result: entry-level retrieval over case trajectories
beating RAG + GraphRAG per stage — the stateful-retrieval contrast to our
evidence/projected memory. TRACE (`trace2026`, venue unconfirmed — do not
cite a venue) is the accountability sibling to evidence receipts
(ADR-0103).

**Failure and faithfulness analysis.** Per-row `failure_kind` reporting
follows the validated taxonomy of Cemri et al. (`cemri2025fail`, NeurIPS
2025) rather than an internal convention; EnConda-bench (`kuang2026enconda`,
ICLR 2026) is the bluntest published breakdown-over-aggregates statement,
and TRAJECT-Bench (`he2026traject`, ICLR 2026) the metric menu for scored
trajectory sub-dimensions. The overclaiming probe (topic 5)
builds on OverclaimBench (`smyth2026overclaim`): agents misreport coverage
in 80.4% of incomplete runs, which is what decision receipts are meant to
make checkable.

## 2. Satellite: capability-typed dispatch

**Allocation strategies.** The MDPI orchestration survey
(`orchestration2026survey`, lit cutoff Mar 2026) names the design space —
role-based, capability-based (A2A Agent Cards), dynamic — in which
ADR-0100's capability-typed dispatch sits. Framework baselines: AutoGen
(`wu2024autogen`) as the most-deployed comparator; Magentic-One
(`fourney2024magentic`) with its task/progress ledgers as the closest
analogue to routing traces; Liu et al.'s dynamic agent network
(`liu2024dynanet`) as the per-turn LLM-scored Gatekeeper contrast that
makes the zero-RPC claim sharp. ICLR 2026 sharpens the contrast set:
Conductor (`nielsen2026conductor`) learns topologies + roles by RL (same
problem, LLM-call cost profile); GraphPlanner (`feng2026graphplanner`)
formulates routing as an MDP over backbone × role on graph memory (the
memory-conditioned alternative to deterministic merit rank); DiSRouter
(`zheng2026disrouter`) removes the external gate entirely (per-agent
self-routing — the arm Cambrian rejects); RouterArena (`lu2026routerarena`)
supplies the standardised router-eval suite alongside OrchestrationBench
(`ahn2026orchbench`).

**Cost-aware orchestration.** ProgRouter (`progrouter2026`, EMNLP 2026
Findings) routes under cost/deadline budgets — the cost-aware contrast to
merit ranking; Optima (`arXiv:2410.08115`, ACL Findings 2025 — promote
from `notes/snowball-2026-09-26.md` before citing) is the sharpest
statement of dispatch as cost-bearing design; AgentDiet
(`xiao2025agentdiet`) is the required trajectory-side treatment arm — if
pure trajectory pruning matches merit-ranked dispatch, the dispatch result
is not a win. MultiAgentBench (`zhu2025mab`) supplies
coordination-quality metrics beyond task success for the metric gap.
Ablation discipline follows Fan et al. (`fan2026harness`) matched settings
plus Zhang et al.'s sham-policy placebo control (`zhang2026harnessvalue`)
as the negative-control model.

## 3. Satellite: policy-enforced retrieval

**Execution-boundary enforcement.** AgentBound (FSE 2026,
`agentbound2026`) and AgentSpec (ICSE 2026, `agentspec2026`) are
independent published arrivals at "model proposes, kernel disposes" —
AgentBound is the lead citation. The Zanzibar/OpenFGA/OPA lineage
(📌 ADR-0091) plus industry tool-authorization practice (short-lived
intent certificates, deterministic checks, auditable decisions) are cited
as converging practice.

**Threat model.** Protocol level: Hou et al. MCP landscape
(`hou2025mcpthreats`) + Hasan et al. server audit (`hasan2025mcpglance`)
+ the MCP/A2A taxonomy (`mcpthreats2025`); interop positioning via
Ehtesham et al. (`ehtesham2025interop`). Read channel: Dong et al.
query-only memory injection (`dong2025meminject`) — the reason policy
must bind queries, not just mutations; Qi et al. extraction
(`arXiv:2402.17840` — promote from notes before citing) as the pair.
Agent-to-agent: Prompt Infection (`lee2024promptinfection`); orchestration
layer: Jha et al. control-flow hijacking (ICLR 2026, OpenReview 10009684)
— the dispatch-layer complement. Write side: PoisonedRAG
(`arXiv:2402.07867` — promote from notes) as the poisoning baseline.
Problem size: OWASP LLM01 + Agent Security Bench (84% vulnerable).
Conceptual frame: He & Yu cognitive serializability (`he2026commit`).

## Venue-pending / do-not-cite-yet

- TRACE venue (CIKM 2026 unconfirmed); EMNLP 2026 Industry list (deferred);
  CIKM 2026 coverage partial (re-run after 7 Nov 2026).
- ResPol-2026 stock-effect direction (needs PDF); BSL adoption rates
  (industry data only, labelled as such).
- ICLR 2026 exhausted for orchest*/rout*/memory/trajectory axes (two
  passes); further coverage needs `verifier`/`rubric`/`judge` keywords.

## 4. Open-core case study (topic 13) — literature frame

No peer-reviewed BSL/Fair-Source/open-core literature exists; ADR-0057 is
framed as an extension of adjacent literatures, rationale argued not cited:
the Redis re-licensing event study (`han2026redis` — contributor attrition
+ Valkey migration after RSALv2+SSPLv1, NOT BSL) with Cambrian as the
*prevented* version; LibreOffice fork sustainability
(`gamalielsson2014libreoffice`) as consequence-side evidence; the market leg
(`yang2026stockoss`, direction unverified); the instrument precedent
(`wu2024licenseuse` — evidence from consumption surfaces); the taxonomy leg
(`shahrivar2018cosbiz` — boundary is licensing/module-structure, not
pricing); the field state (`li2025commercial`); the governance mechanism
(`he2020governance`); firm economics (`colombo2014ossfirms`). Closest
BSL-adjacent treatment is a workshop submission (`kula2025crossroads`) —
cite as such, use its agenda as the gap statement.
