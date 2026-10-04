# Related Work (draft v0.2 — 2026-10-02)

Covers the flagship substrate paper + dispatch and security satellites.
Every citation here is ✅-verified (`bib/references.bib`); venue-pending
items are flagged inline. Keyed to `LITERATURE_REVIEW.md` v0.3 (expanded
past its version number — treat this draft as the fresher document).

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
Contested: Selective Forgetting (`selectiveforget2026`) measured graph
expansion losing to a flat baseline on LongMemEval at matched budget —
reproduced-or-refuted on our own suites before review; mechanism (entity
decomposition discards surface form) argues for verbatim Evidence either
way. Counter-case: AriGraph fusion (`arigraph2024`).

**Agent memory: consolidation and forgetting.** Consolidation lineage:
Voyager skill library (`voyager2023`), ExpeL transferable insights without
weight updates (`expel2024`, AAAI-24 — direct prior art for ADR-0094),
AutoGuide conditional guidelines (`autoguide2024`), AutoManual rule Builder
(`automanual2024`, NeurIPS 2024), AFTER skill-transfer benchmark, 382
tasks incl. cross-model (`after2026` — topic 10's instrument),
Skill-as-Pseudocode deterministic gate (`sap2026`, EMNLP Findings 2026),
AgentHER hindsight relabelling as append-only analogue (`agenther2026`),
SkillCenter quality-gate (`skillcenter2026`); critic Procedural-Memory-Is-
Not-All-You-Need (`procnotall2025`) keeps episodic/semantic first-class.
Forgetting: Ebbinghaus primary cited to argue against
(`ebbinghaus2013`); Craik & Lockhart depth-of-processing refutation
(`craik1972levels`); Murre non-replication (`murre2015replication`);
MemoryBank as strawman implementation (`memorybank2023`); THEANINE
opposition — outdated memories are change evidence (`theanine2025`,
NAACL 2025); FSFM safety-motive taxonomy (`fsfm2026`). Our stance:
versioned resolutions, never age-keyed decay.
Parametric side: PEAK append-catastrophe (`peak2024`, ICML 2024) rules out
parametric writes; MEMIT (`memit2022`, ICLR 2022) is the best-case arm that
loses on auditability; Memory³ explicit memory cheaper than params and RAG
(`memorycube2024`); ECHOQA taxonomy + parametric suppression
(`echoqa2024`) is the Resolution-kinds vocabulary; MACR explicit conflict
resolution (`macr2026`); COUNTERMEM verified counterfactuals as Resolution
home (`countermem2026`); origins Generative Agents (`generative2023`,
UIST) and Denning locality (`denning2005locality`).

**Memory security bounds.** Salami coalitions (`salami2026`) falsify
per-write validation as sufficient; SPORE egress bypass (`spore2026`)
bounds party scoping and makes chokepoints necessary; sleeper
write→retrieve→use schema (`sleeper2026`) is adopted for memory-guard;
MemMorph poisons routing via memory (`memmorph2026` — missing arm in
topic-1 threat model); CAPTURE (`capture2026`, NOT ICLR) shows
recency+provenance insufficient and supports the abstain path; CSUR survey
(`agentsecurity2025`) anchors the section.

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
make checkable. Judge methodology: WebDevJudge failures on agent artifacts
(`li2026webdevjudge`, Oral), noisy-judge Type-I control (`feng2026noisyvalid`),
TrustJudge rates (`wang2026trustjudge`), ResearchRubrics+CROME calibration
(`sharma2026researchrubrics`, `srivastava2026crome`), rubric/verifier rewards
(`gunjal2026rubricsrewards`, `xu2026fare`), TED diagnosis loop
(`chong2026ted`).

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
merit ranking; Optima (`optima2025`, ACL Findings 2025) is the sharpest
statement of dispatch as cost-bearing design; AgentDiet
(`xiao2025agentdiet`) is the required trajectory-side treatment arm — if
pure trajectory pruning matches merit-ranked dispatch, the dispatch result
is not a win. MultiAgentBench (`zhu2025mab`) supplies
coordination-quality metrics beyond task success for the metric gap.
Ablation discipline follows Fan et al. (`fan2026harness`) matched settings
plus Zhang et al.'s sham-policy placebo control (`zhang2026harnessvalue`)
as the negative-control model.
Token-normalisation cluster (P1–P12 arms in proposal 01):
Tran & Kiela DPI confound (`tran2026single`); RLM-Cascade O(1) zero-call
router as strongest competitor (`rlmcascade2026`); InflationAgent
workflow-measured cost (`inflation2026`); Harness Effect harness-only
control (`harness2026`); BAMAS framework cost table (`bamas2026`).
Router family: RouteLLM (`routellm2024`), RouterBench+AIQ
(`routerbench2024`), AutoMix (`automix2024`, NeurIPS 2024), HybridLLM
(`hybridllm2024`, ICLR 2024), RouterDC (`routerdc2024`, NeurIPS 2024),
Router-R1 router-is-an-LLM-call (`routerr12025`, NeurIPS 2025), Meta-Router
causal debias (`metarouter2026`, ICLR 2026). Cascades: FrugalGPT
(`frugalgpt2023`), EcoAssistant (`ecoassistant2023`). Test-time compute:
self-consistency origin (`selfconsistency2023`, ICLR 2023), Snell
optimality (`snell2024ttc`), s1 (`s1scaling2025`), Best-of-N theory
(`bon2025`, ICML 2025), MTRouter turn-level budgets (`mtrouter2026`,
ACL 2026), agreement-vs-confidence (`whenagree2026`). Protocol footprint:
ProMCP usage-vs-protocol split (`promcp2026`, ACL Findings);
retrieval-filtered slate (`mcpgateway2026`). Spend accounting: 3500×/30×
spread (`tokenspend2026`).
Industry anchors: Anthropic multi-agent post
(`anthropic2025multiagent` — +90.2%, tokens explain 80%, 15× cost);
Cognition counter-case (`cognition2025nomulti`); Google scaling science
(`kim2026scaling` — +80.9% parallel, −39–70% sequential, 17.2× vs 4.4×
amplification); Anthropic harness/managed-agents/tool posts
(`segato2026noise`, `anthropic2026managed`); Zep/Mem0 production memory
(`rasmussen2025zep`, `chhikara2025mem0`).

**Planner boundary.** Plan-then-execute as trust boundary
(`planexecute2026`: 80% of WebArena programmatic; untrusted content sets
values, never redefines the task) converges with FlowSteer from the
opposite direction; ReWOO (`xu2023rewoo`) makes interleaving's structural
cost explicit. LLM-Modulo (`kambhampati2024modulo`, ICML 2024) is the
peer-reviewed "model proposes, kernel disposes" with measured deltas
(`modulo2025travel`, `modulo2024compound`). Self-correction negatives
(`huang2024selfcorrect` TACL, `huang2024noselfcorrect` ICLR, `toolprm2026`
ACL) rule out the self-reflect arm: correction loop belongs in the kernel.

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
(`spillbeans2024`) as the pair.
Agent-to-agent: Prompt Infection (`lee2024promptinfection`); orchestration
layer: Jha et al. control-flow hijacking (ICLR 2026, OpenReview 10009684)
— the dispatch-layer complement. Write side: PoisonedRAG
(`arXiv:2402.07867` — poisoning baseline; full bib entry pending, add
before submission).
Problem size: OWASP LLM01 + Agent Security Bench (84% vulnerable).
Conceptual frame: He & Yu cognitive serializability (`he2026commit`).

## Venue-pending / do-not-cite-yet

- TRACE venue (CIKM 2026 unconfirmed); EMNLP 2026 Industry list (deferred);
  CIKM 2026 coverage partial (re-run after 7 Nov 2026).
- ResPol-2026 stock-effect direction (needs PDF); BSL adoption rates
  (industry data only, labelled as such).
- ICLR 2026 exhausted for orchest*/rout*/memory/trajectory AND
  verifier/rubric/judge axes (four passes); unique count ~5 468 (223 Oral
  duplicates); placebo axis dead at ICLR — do not spend a fifth pass.
- MCPTox venue unconfirmed (no "AAAI 2026"); MCP-Bench name collision
  (Accenture 2508.20453 vs ModelScope 2504.11094 — distinct keys);
  CAPTURE not ICLR; Memory³ is JML not JMLR; no NesTools/RCI papers exist.

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
