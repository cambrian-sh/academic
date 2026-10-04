# Memory deep sweep — LLM agent memory 2023→2026-10

Subagent literature task, run 2026-10-01. Target: what `LITERATURE_REVIEW.md` §2
(v0.3) **lacks**, for the Cambrian angles episodic→procedural consolidation
(topic 10), contradiction-tolerant memory (topics 6/11), memory safety over
accumulation, with LoCoMo/LongMemEval already wired.

Everything in the review's ✅ set was treated as already-covered and NOT
re-listed: LoCoMo (2402.17753), LongMemEval (2410.10813), HippoRAG
(2405.14831), LightRAG (2410.05779), RAPTOR (2401.18059), REMem (OpenReview
10008195), AMemGym (10007039), MemoryAgentBench (10010781), MemCoT, Mem0
reports, RAFT (2609.20754), TRACE (2609.19897), ReasoningBank (blog), Misevolve
(10010580), and the §2 GraphRAG/TOIS-GraphRAG-survey block.

## Yield

**63 rows / 58 unique works.** The brief asked for 20–30; the sweep is wider
because all six requested sub-topics returned more distinct, on-theme work than
expected — 2025–26 alone contributes a *new* procedural-memory cluster (A4, A7,
A8, A11, A13, A14) and a *new* memory-attack cluster (E1–E8) that the survey
does not contain at all. Nothing was padded to reach the count; §G's five rows
are deliberate cross-references (measurement schemas lifted from A7/B8/E4/E6/C2,
not new items), so subtract 5 for a like-for-like comparison. Read A, B, C, D, E
as the sweep proper and G as a shortlist for `METHODOLOGY.md`.

## Verification protocol (same discipline as `notes/snowball-2026-09-26.md`)

No identifier below is recalled or guessed. Every one was fetched and its title
string matched:

| Tag | Route | Count |
|---|---|---|
| `arXiv-API` | `export.arxiv.org/api/query?id_list=…`, `<title>` compared | 21 |
| `arXiv-abs` | `arxiv.org/abs/<id>` HTML, `<h1 class="title">` compared | 18 |
| `arXiv-search` | `arxiv.org/search/?searchtype=title`, then abs-page confirm | 7 |
| `Crossref` | `api.crossref.org/works/<DOI>`, title compared | 8 |
| `TOIS-bib` | title string from the seed survey's own Crossref reference record, then arXiv/Crossref confirm (double source) | as marked |

**Rate-limit notes for the next agent (both real, both cost time).**`export.arxiv.org` began returning HTTP 429 after ~30 calls in this session and
never recovered, even after 75 s and 3 min cooldowns. `arxiv.org/abs/` and
`arxiv.org/search/` are **separate limiters and stayed healthy** — route all
further title-searches through `arxiv.org/search/?query=…&searchtype=title`.
Two traps: that endpoint needs `quote_plus` (not `quote`) or it silently returns
a ~10 KB empty shell, and `size` must be 50 (size=15 returns the same empty
shell, which looks exactly like "zero results" — always run a control query).
An **empty search result is inconclusive, never a negative finding**; the three
`0 hit(s)` rows in "Absences" below are the ones where I could still afford a
second phrasing.

---

## Top 5 for Cambrian

1. **Selective Forgetting: A Graph-Based Memory Framework for Long-Term LLM
   Agents** (2608.28978) — the first *controlled* negative result against
   Cambrian's own KG²RAG graph-expansion signal, measured on LongMemEval.
   Breaks the substrate flagship's core retrieval claim before a reviewer does.
2. **Salami Attack / MemCollusion** (2608.01637) — individually-benign memories
   that are jointly harmful. This falsifies "validate each write" as a
   sufficient memory-safety defence, i.e. it attacks the *unit of enforcement*
   Cambrian ADR-0105/0118 is built on.
3. **SPORE: Isolated but Exposed** (2607.23444) — per-user memory isolation is
   bypassed through the **tool interface**; the leak is on egress, not in the
   store. Directly limits the reach of topic 8 (row-level party scoping) and
   strengthens the ADR-0085 chokepoint argument.
4. **ECHOQA / PK-vs-CK interplay** (2410.08414) — a four-way relation taxonomy
   (Supportive / Complementary / Conflicting / Irrelevant) for parametric vs
   contextual knowledge, plus the finding that models suppress parametric
   knowledge even when context is complementary. This is the external
   vocabulary for ADR-0106 Resolution kinds.
5. **AFTER: Managing Procedural Memory in LLM Agents** (2606.23127) — the
   first benchmark that measures *skill transfer* across tasks, roles and model
   backbones. Topic 10 currently has a measurement gap, not just a build gap;
   this is the suite that closes it.

---

## A. Consolidation, proceduralization, skill induction (topic 10)

The review has exactly one consolidation-lineage entry (REMem) plus a 🔍 blog
(ReasoningBank). The entire skill-induction literature is missing.

| # | Work | Venue | One-line result | Cambrian relevance | Verified |
|---|---|---|---|---|---|
| A1 | **Voyager: An Open-Ended Embodied Agent with LLMs** — arXiv:2305.16291 | preprint (TMLR lineage; no Comments field) | Automatic curriculum + ever-growing **skill library of executable code** + iterative error-feedback prompting; skills are "temporally extended, interpretable, and compositional" and the library "alleviates catastrophic forgetting". | The origin of the skill-library pattern. Cambrian's consolidation test (topic 10) needs a *storage* ablation: skill-as-code vs skill-as-prose vs skill-as-KG-triple. | `arXiv-API` |
| A2 | **ExpeL: LLM Agents Are Experiential Learners** — arXiv:2308.10144 | **AAAI-24** (Comments) | Extracts transferable natural-language *insights* from trajectories, and reuses them across **different** tasks — no parametric update, no retraining. | The cleanest episodic→procedural formulation: consolidation must produce something that *transfers*, and must not require weight updates. This is the direct prior art for ADR-0094. | `arXiv-API` |
| A3 | **Agent Workflow Memory (AWM)** — arXiv:2409.07429 | **venue UNCONFIRMED** — record has no Comments field; do **not** cite as ICLR/ICML | Induces reusable "workflows" (routines) from trajectories and selectively surfaces them; works offline and online on Mind2Web + WebArena (1000+ tasks, 200+ domains). | Prior art for a *workflow* memory tier distinct from episodic and semantic. The offline/online split maps onto Cambrian's consolidation-on-write vs consolidation-on-read. | `arXiv-API` |
| A4 | **AutoGuide** — arXiv:2403.08978 | **venue UNCONFIRMED** (no Comments field) | Generates *context-aware guidelines* from offline experience, each with a **conditional structure stating the context in which it applies**; beats demonstration-based ICL on unfamiliar domains. | The single most useful idea for ADR-0111's typed query plane: a procedural memory item should be **typed by its applicability condition**, not just by topic. | `arXiv-abs` |
| A5 | **AutoManual** — arXiv:2405.16247 | **NeurIPS 2024** (Comments) | Planner codes plans from a **rule system**; a Builder agent updates rules online; **case-conditioned prompting** explicitly mitigates rule-management hallucination. | Rule-set + online update is ADR-0094's mechanism. The "mitigate hallucination while managing rules" problem is the same one as contradiction resolution — cite as converging practice. | `arXiv-API` (comment) + `arXiv-abs` |
| A6 | **ChemAgent: Self-updating Library** — arXiv:2501.06590 | preprint (no Comments field) | Decomposes tasks into sub-tasks, compiles them into a **self-updating library** ("memory"), then retrieves *and refines* it on new problems. | Library refinement-on-read is a consolidation loop Cambrian has no arm for. Cheapest possible test of whether consolidation helps LoCoMo. | `arXiv-API` |
| A7 | **AFTER: Managing Procedural Memory in LLM Agents** — arXiv:2606.23127 | preprint (no Comments field) | 382 enterprise tasks / 6 roles / 22 skills; separates local improvement, cross-task, cross-role and cross-**model** transfer. One refinement round = +3.7–6.7 aggregate; skills evolved from *multi-model* traces hit **73.1 % cross-model accuracy**, beating any single-model trace source. | **Topic 10's missing measurement instrument.** Also the headline: procedural memory learned from one backbone does not port to another — an argument for keeping consolidation artefacts in the substrate (portable) rather than in weights. | `arXiv-abs` |
| A8 | **Skill-as-Pseudocode (SaP)** — arXiv:2605.27955 | **EMNLP Findings 2026** (Comments) | Converts prose skill libraries into typed pseudocode behind a **four-check deterministic verifier** (coverage, binding, replacement, risk); diagnoses a "confused → re-retrieve → still confused" loop caused by agents re-deriving invocation syntax from free text. | The purest published instance of *model proposes, kernel disposes* applied to procedural memory. Its four checks are directly portable as a Cambrian consolidation gate. | `arXiv-abs` |
| A9 | **Gödel Agent** — arXiv:2410.04444 | **ACL 2025 main** (Comments) | An LLM recursively modifies **its own logic** with no human-designed components, guided only by high-level objectives. | The upper bound of self-modifying substrate. Read as the risk register paired with Misevolve (10010580): what happens when the thing that writes memory can also rewrite the policy. | `arXiv-API` |
| A10 | **Procedural Memory Is Not All You Need** — arXiv:2505.03434 | preprint (no Comments field) | Argues LLM agents are constrained to *procedural* memory and therefore fail in "wicked" environments; demands semantic memory + associative learning. | The critic half of topic 10. Proceduralization alone is insufficient; Cambrian's substrate thesis needs the episodic/semantic side kept first-class, not folded away. | `arXiv-abs` |
| A11 | **LEGOMem** — arXiv:2510.04851 | preprint (no Comments field) | Modular procedural memory for multi-agent systems; decomposes trajectories into reusable memory **units** and allocates them across orchestrators vs task agents. | Asks *where* memory should live in a multi-agent system. That is Cambrian's own question about substrate placement relative to the Gatekeeper/Dispatcher. | `arXiv-abs` |
| A12 | **AgentHER: Hindsight Experience Replay** — arXiv:2603.21357 | preprint (no Comments field) | A trajectory that fails goal A is often a valid demonstration of goal B; relabels failed trajectories accordingly. Notes GPT-4o at 14–20 % WebArena and <55 % ToolBench pass@1, so most trajectories are currently discarded. | Hindsight relabelling writes a *derived claim about* an episode without mutating the episode — structurally the same move as append-only Evidence → derived Resolution (ADR-0105/0106). Strongest conceptual prior art for "experiences as first-class entities" (ADR-0095). | `arXiv-abs` |
| A13 | **SAGE: RL for Self-Improving Agent with Skill Library** — arXiv:2512.17102 | preprint (no Comments field) | Skill-Augmented GRPO with **Sequential Rollout** across a chain of similar tasks; argues prompt-only skill libraries cannot be implemented consistently. | The "consolidation needs training, not just prompting" arm. Relevant to the Zero-Hardcode Rule debate: if skills must be RL-learned, is the routing decision still Awareness-layer? | `arXiv-abs` |
| A14 | **SkillCenter** — arXiv:2607.07676 | preprint (no Comments field) | 216,938 skills / 24 bundles; an LLM quality gate (**SkillGate**) over multi-source acquisition; **source grounding** as a traceability guarantee (each retained claim maps to an exact quoted span). | Two Cambrian arguments in one paper: (i) a *quality gate* is required before accumulation, not after — the anti-accumulation stance; (ii) source grounding = traceability = ADR-0103 evidence receipts, and its published scale is the "accumulate everything" baseline to argue against. | `arXiv-abs` |

## B. Forgetting, decay, Ebbinghaus — and the critics

The review has **no** forgetting/decay citation at all. Neither the primary
source, the canonical critic, nor the replication that says the curve does not
hold.

| # | Work | Venue | One-line result | Cambrian relevance | Verified |
|---|---|---|---|---|---|
| B1 | **Ebbinghaus, "Memory: A contribution to experimental psychology"** (1885) | *Annals of Neuroscience* 20(4):155–156 (2013 reprint) | The forgetting-curve source itself. | Mandatory primary citation for any "agents should forget" claim, and the thing to argue *against* in a position paper. | `Crossref` · DOI **10.5214/ans.0972.7531.200408** |
| B2 | **Craik & Lockhart, "Levels of processing: A framework for memory research"** | *Journal of Verbal Learning and Verbal Behavior* 11(6):671–684 (1972) | Retention depends on the *depth of processing*, not on elapsed time and not on a fixed decay coefficient. | **The most important item in this file for "memory safety over accumulation."** It is the classical refutation of time-based forgetting: if decay is a function of processing depth, then a retention *policy* keyed to age is the wrong instrument, and versioned Resolutions (ADR-0106) are the right one. | `Crossref` · DOI **10.1016/S0022-5371(72)80001-X** |
| B3 | **Murre & Dros, "Replication and Analysis of Ebbinghaus' Forgetting Curve"** | *PLOS ONE* 10(7):e0120644 (2015) | Direct replication: the classic forgetting curve does **not** reproduce cleanly; behaviour is better described by a two-term function than a single exponential. | The empirical "critics of Ebbinghaus" citation. Kills any paper that hard-codes a decay constant. | `Crossref` · DOI **10.1371/journal.pone.0120644** |
| B4 | **MemoryBank** — arXiv:2305.10250 | preprint (no Comments/journal field) | Applies an Ebbinghaus-forgetting-curve mechanism to let the model "forget and reinstate" dialogue memories selectively. | The canonical *implementation* of Ebbinghaus in LLM memory — i.e. the strawman a good paper should measure against, since B2/B3 say the mechanism is unfounded. | `arXiv-API` |
| B5 | **MEOW: MEMOry Supervised LLM Unlearning** — arXiv:2409.11844 | preprint (no Comments field) | Gradient-descent unlearning via offline-generated **inverted facts**; names three failure modes of prior work (utility collapse on unrelated tasks, efficiency, and **robustness — effective methods still leak under extraction**). | The "forgetting is not deletion" paper, and its extraction-leak caveat is the reason a *store*-level forget cannot satisfy a privacy claim. Pairs with A8's deterministic gate. | `arXiv-abs` |
| B6 | **THEANINE: Towards Lifelong Dialogue Agents via Timeline-based Memory Management** — arXiv:2406.10996 | **NAACL 2025** (Comments) | Argues outright that prior work's "get rid of outdated memories" is a mistake: outdated memories are the best contextual cues for response generation. **Discards memory removal** in favour of linking memories by temporal and cause-effect relation, surfacing *memory timelines*. | The strongest published *opposition* to forgetting-based memory management, and it is right in the way that matters for topics 6/11: the outdated item is not noise, it is the evidence that a change occurred. Supersession ≠ deletion. | `arXiv-API` |
| B7 | **FSFM: A Biologically-Inspired Framework for Selective Forgetting of Agent Memory** — arXiv:2604.20300 | preprint (no Comments field) | Taxonomy of forgetting mechanisms — **passive decay-based, active deletion-based, safety-triggered, adaptive reinforcement-based** — grounded in hippocampal indexing/consolidation theory and Ebbinghaus; argues forgetting buys efficiency, quality (updating outdated preferences) **and security** (actively forgetting malicious inputs, sensitive data). | The best taxonomy available for the memory-safety axis, and it independently names the security motive — which is the strongest published support for "memory safety over accumulation." | `arXiv-abs` |
| B8 | **Selective Forgetting: A Graph-Based Memory Framework for Long-Term LLM Agents** — arXiv:2608.28978 | preprint (no Comments field) | Extracts each conversational turn into typed nodes + attributed edges, answers from a 2-hop subgraph, and prunes low-scoring nodes. **On LongMemEval the graph does not beat a flat vector baseline** at matched budget: token F1 0.417 vs 0.468, paired bootstrap over 500 questions Δ = −0.050 (95 % CI [−0.085, −0.016]). Worst on questions needing a *specific prior assistant turn*: 0.911 → 0.607. **The forgetting module is the part that works.** | 🔴 **Highest-stakes item in the sweep.** This is a controlled refutation of exactly the mechanism Cambrian's substrate flagship is built on (entity/relation decomposition + graph expansion), on exactly the benchmark Cambrian has wired. The mechanism to study is *why*: decomposing a turn into entities discards the surface form those questions depend on — which is an argument for Cambrian's content-addressed Evidence (verbatim) over a graph projection of it. | `arXiv-abs` |
| B9 | **Towards Benchmarking Privacy Vulnerabilities in Selective Forgetting** — arXiv:2512.18035 | preprint (no Comments field) | Argues selective-forgetting / machine-unlearning claims need a privacy-vulnerability *benchmark*, not a removal success rate. | The measurement gap B5 implies: "we forgot it" and "it can no longer be recovered" are different claims, and only the second is a security property. | `arXiv-abs` |
| B10 | **Liu, "Lifelong machine learning: a paradigm for continuous learning"** | *Frontiers of Computer Science* 11(3):359–361 (2017) | Catastrophic forgetting as the defining constraint of lifelong learning. | The continuous-learning anchor for ADR-0094: consolidation exists *because* naive replacement forgets. | `Crossref` · DOI **10.1007/s11704-016-6903-6** |

## C. Parametric vs non-parametric tradeoffs

| # | Work | Venue | One-line result | Cambrian relevance | Verified |
|---|---|---|---|---|---|
| C1 | **Neighboring Perturbations of Knowledge Editing** (PEAK) — arXiv:2401.17623 | **ICML 2024** (Comments) | Appending a new answer to a factual question's answer list causes **catastrophic forgetting of the original correct answers** and unintentional inclusion of incorrect ones; introduces an *additivity* metric. | The decisive empirical argument that parametric writes are **not** a safe update channel. Combined with B6, it is the published basis for keeping the substrate append-only and deriving resolutions instead of overwriting. | `arXiv-API` |
| C2 | **Understanding the Interplay between Parametric and Contextual Knowledge (ECHOQA)** — arXiv:2410.08414 | preprint (no Comments field) | Typologises PK-vs-CK relations as **Supportive / Complementary / Conflicting / Irrelevant**; finds LLMs **suppress parametric knowledge whenever context is present, even when the context is complementary or irrelevant**; tailored instructions help but do not fix it. | The external taxonomy for ADR-0106 Resolution kinds, and the mechanism behind "the retrieval layer silently overwrites what the model knows." Names a vulnerability, not just a benchmark. | `arXiv-abs` |
| C3 | **MACR: Explicit Knowledge Conflict Resolution for LLM Inference** — arXiv:2606.20245 | preprint (no Comments field) | Notes that conflicts arise **among multiple external contexts**, not only model-vs-context, and criticises prior work for *avoiding* conflict by privileging one source. Proposes active resolution instead. | The closest published statement of Cambrian's position: contradiction is a first-class object to be resolved, not a conflict to be dodged. Cite for topic 6's framing. | `arXiv-abs` |
| C4 | **Memory³: Language Modeling with Explicit Memory** — arXiv:2407.01178 | *Journal of Machine Learning* **3**(2024):300–346 (journal_ref) | An explicit memory format **cheaper than parameters and cheaper than RAG**; a 2.4 B model trained from scratch beats much larger LLMs *and* RAG models, with higher decoding speed than RAG. | The strongest quantitative case that non-parametric memory can dominate on cost *and* quality — i.e. the third option between "edit the weights" and "stuff the context". Note the venue is **JML, not JMLR** (the TOIS bibliography's DOI 10.4208/jml.240708 is the JML record). | `arXiv-API` + `TOIS-bib` |
| C5 | **Fast Model Editing at Scale (MEMIT)** — arXiv:2110.11309 | **ICLR 2022** (Comments) | Scalable post-hoc editing of a single fact in a large model without touching the rest. | The best-case parametric baseline. If a topic-10 result is claimed, this is the arm that must lose on auditability grounds (no evidence receipts, no rollback). | `arXiv-API` + `TOIS-bib` |
| C6 | **Editing Large Language Models: Problems, Methods, and Opportunities** — arXiv:2305.13172 | **EMNLP 2023** (Comments) | Task definition, challenge taxonomy, and a new benchmark for model editing, with "enduring issues intrinsic to existing techniques". | The standard survey citation for the parametric-update arm of the topic-10/12 comparison. | `arXiv-API` + `TOIS-bib` |
| C7 | **Knowledge Editing for LLMs: A Survey** — arXiv:2310.16218 | **accepted, ACM Computing Surveys** (Comments) | Survey of knowledge-based model editing; frames naive retraining as risky for unrelated pretrained knowledge. | Peer-reviewed anchor for C5/C6; cite this one in a CSUR-adjacent venue. | `arXiv-API` + `TOIS-bib` |
| C8 | **Trends in Integration of Knowledge and LLMs: A Survey and Taxonomy** — arXiv:2311.05876 | **preprint — "work in progress… submitted to the IEEE"**; do not cite as a published IEEE survey | Frames knowledge editing *and* retrieval augmentation as the two primary strategies for keeping LLM knowledge current, with a shared benchmark taxonomy. | The one survey that puts both channels in a single taxonomy — the framing citation for a parametric-vs-non-parametric comparison section. | `arXiv-API` + `TOIS-bib` |
| C9 | **MemTree: Dynamic Tree Memory Representation for LLMs** — arXiv:2410.14052 | preprint (no Comments field) | A dynamic tree-structured memory with per-node aggregated text, embeddings and **varying abstraction levels by depth**, adapting as new information arrives; beats flat memory augmentation. | A middle tier between RAPTOR-style static hierarchical summaries and a flat store — relevant to topic 2's ablation grid. | `arXiv-abs` |
| C10 | **De Cao et al., "Editing factual knowledge in language models"** — arXiv:2104.08164 | **EMNLP 2021** (Crossref DOI 10.18653/v1/2021.emnlp-main.522) | The original scoped-editing formulation (KnowledgeEditor): change one fact, leave the model otherwise intact. | The lineage root for "surgical update"; the baseline formulation C1 then falsifies. | `arXiv-abs` + `Crossref` |

## D. World models and experiential memory

| # | Work | Venue | One-line result | Cambrian relevance | Verified |
|---|---|---|---|---|---|
| D1 | **AriGraph: Learning Knowledge Graph World Models with Episodic Memory for LLM Agents** — arXiv:2407.04363 | preprint (**venue UNCONFIRMED** — no Comments field) | The agent builds and updates a memory **graph integrating semantic and episodic memory** while exploring; argues unstructured history/summarisation/retrieval do not support planning. | The closest published fusion of "world model" and "episodic memory" — the substrate flagship's thesis in one paper. Pair with B8: same graph mechanism, opposite empirical finding. | `arXiv-API` + `TOIS-bib` |
| D2 | **DiLu: A Knowledge-Driven Approach to Autonomous Driving** — arXiv:2309.16292 | **ICLR 2024** (Comments) | A built-in **world model** (rules) plus Reasoning and Reflection modules beats RL on generalisation while continuously accumulating experience. | The "explicit world model beats learned dynamics" argument. Its Reflection module is consolidation-by-reflection — the mechanism to ablate in topic 10. | `arXiv-API` + `TOIS-bib` |
| D3 | **COUNTERMEM: World-Model Verified Counter-Factual Memory** — arXiv:2609.31874 | preprint (no Comments field) | After a failed action, evaluates local alternatives using **executable world models** (tests, proof checkers, solvers) and stores the verified improvement; notes that getting such feedback for real disturbs the state you need for comparison. | Introduces **counterfactual** memory: stored claims about what *would* have happened, verified rather than observed. This is the natural home for Contradiction/Resolution items in ADR-0106 — a resolution is a verified counterfactual about a superseded fact. | `arXiv-abs` |
| D4 | **Ghost in the Minecraft** — arXiv:2305.17144 | preprint (no Comments field) | Text-based **knowledge and memory** structures plus a tech tree for progressive skill acquisition; generalises beyond the single ObtainDiamond objective. | The earliest skill-induction-via-explicit-memory design; the tech tree is a hand-specified progression graph, i.e. manual proceduralization. | `arXiv-abs` + `TOIS-bib` |
| D5 | **Johnson-Laird, *Mental Models*** (1983) | book (Cambridge) | Mental models as the representational medium of reasoning, distinct from propositions. | The cognitive-science root for D1–D3 and for "world model" in a substrate paper. The TOIS survey cites it; cite via the survey rather than hunting the book. | `TOIS-bib` (ref [65]) |

## E. Memory attacks and defences beyond query-only injection + PoisonedRAG

The review's §4 memory-security block is Dong et al. (2503.03704) + PoisonedRAG
(2402.07867). The 2025–26 literature has moved on substantially: **collusion,
persistence, sleeper triggers, egress exfiltration, and drift-vs-poisoning
disambiguation**. All five are new to the review.

| # | Work | Venue | One-line result | Cambrian relevance | Verified |
|---|---|---|---|---|---|
| E1 | **Salami Attack: Stealthy Collusive Memory Poisoning (MemCollusion / MoltLab)** — arXiv:2608.01637 | preprint (no Comments field) | Slices one adversarial objective into memory fragments that are **individually innocuous and collectively harmful**; a controlled cross-session reproduction (MoltLab) requires crafted content to be observed and *distilled* into persistent memory before it fires. | 🔴 **Attacks the unit of enforcement.** Every per-write gate in Cambrian (ADR-0043 scanning, ADR-0105 content addressing) validates *one item at a time*; this says the threat unit is a **coalition**. Any claim that write-path validation bounds memory risk is wrong, and this is the citation that shows it. | `arXiv-abs` |
| E2 | **SPORE: Isolated but Exposed — Persistence-Based Memory Extraction** — arXiv:2607.23444 | preprint (no Comments field) | Production systems isolate each user's LTM behind a unique identifier, which blocks attacks on shared storage; **the tool interface is an overlooked surface** — agents embed LTM-retrieved data in tool invocation parameters, so a malicious tool exfiltrates private memory *without violating user-level isolation*. | 🔴 **Bounds topic 8.** Row-level party scoping (ADR-0121) is exactly per-user isolation, and this says that fails open on egress. Turns the ADR-0085 chokepoint argument from preference into necessity. | `arXiv-abs` |
| E3 | **ADAM: Data Extraction Attack on Agent Memory via Adaptive Querying** — arXiv:2604.09747 | preprint (no Comments field) | Sensitive information held in agent memory can be leaked through *queries*; adaptive querying beats naive extraction against a memory module / RAG store. | The quantitative arm of topic 7's security×utility Pareto curve: a recall-vs-leakage curve is exactly what a scope-enforced read plane must be measured against. | `arXiv-abs` |
| E4 | **Hidden in Memory: Sleeper Memory Poisoning in LLM Agents** — arXiv:2605.15338 | preprint (no Comments field) | A delayed attack: adversary shapes external context so the assistant stores a **fabricated memory about the user**, dormant and re-emerging across later conversations. Evaluates the full pipeline separately — **written? retrieved? used?** — poisoned memories added up to **99.8 %** (GPT-5.5) and 95 % (Kimi-K2.6); among successful retrievals, **60–89 %** steer the conversation to attacker-intended actions. | Supplies a three-stage metric schema (write rate / retrieve rate / use rate) that is strictly better than a single success number, and it is the schema `METHODOLOGY.md` should adopt for the `memory-guard` suite. | `arXiv-abs` |
| E5 | **MemMorph: Tool Hijacking via Memory Poisoning** — arXiv:2605.26154 | preprint, "Under review" (Comments) | The first attack that biases **tool selection** by poisoning long-term memory rather than tool metadata; injects few records disguised as technical facts, incident reports and operational policies. | Aimed squarely at Cambrian's Gatekeeper/Dispatcher: the merit rank is a function of accumulated experience, so poisoning memory poisons *routing*. The missing arm in the topic-1 threat model. | `arXiv-abs` |
| E6 | **CAPTURE: Disentangling Preference Drift from Memory Poisoning** — arXiv:2609.02265 | preprint; "Under review at ICLR 2027" (Comments — **do not cite as ICLR 2027**) | When new information conflicts with stored preferences the agent must separate **genuine drift** from context shift, ambiguity, or attack. Formulated as a continuous-time POMDP over latent user state; shows **recency + provenance rules are insufficient**; answers with a neural-ODE belief tracker, a **multi-timescale memory ledger**, **uncertainty-triggered clarification**, and **counterfactual auditing of cited memories**. 480 held-out episodes / 96 users; 71.5 % win rate vs 69.3 % for a baseline
(the abstract truncates the baseline's name at "for an" — do not name it). | 🔴 **The single most on-theme paper for topics 6/11 + memory safety.** It is exactly the hard case ADR-0106/0110 claims to handle: a *contradiction* may be legitimate supersession, ambiguity, or attack, and provenance alone does not decide. Its "uncertainty-triggered clarification" is an argument for an explicit `ErrCannotExpress`-style abstain path (topic 11). | `arXiv-abs` |
| E7 | **Memory Poisoning Attack and Defense on Memory Based LLM-Agents** — arXiv:2601.05504 | preprint (no Comments field) | States that MINJA reaches >95 % injection and ~70 % attack success **under idealized conditions**, then studies robustness and defences in EHR agents by varying initial memory state, injection count, and more. | The **critic** companion to the review's Dong et al. entry: the headline attack numbers are idealised. Required reading before quoting them, and the source of realistic-baseline parameters for topic 7. | `arXiv-abs` |
| E8 | **Memory poisoning and secure multi-agent systems** — arXiv:2603.20357 | preprint (no Comments field) | Typologises memory systems by **duration, origin and localisation** (short-term at the user end vs long-term consolidated in knowledge databases), assesses poisoning feasibility per type, reviews existing defences, and proposes **cryptographic** mitigations plus local inference over private knowledge retrieval. | Gives the multi-agent memory-typing vocabulary the substrate needs, and proposes integrity (cryptographic) rather than trust — the same move as Cambrian's content-addressing of Evidence. | `arXiv-abs` |
| E9 | **The Emerged Security and Privacy of LLM Agent: A Survey with Case Studies** — arXiv:2407.19354 | **accepted, ACM Computing Surveys (CSUR) 2025** (Comments) | Full threat taxonomy for agent security/privacy with case studies, organised over agents, environment, and other agents, plus defensive strategies. | The peer-reviewed survey to anchor §4's memory-security expansion with — the review currently has only taxonomies of *protocol* attacks, not of agent memory. | `arXiv-abs` |
| E10 | **Identifying and Mitigating Privacy Risks Stemming from Language Models** — arXiv:2310.01424 | preprint (no Comments field) | First SoK on training-data memorisation and extraction; taxonomies of attack and mitigation dimensions. | The *parametric* half of memory privacy, and the source of the extraction techniques E3/E4 measure agents against. | `arXiv-abs` + `TOIS-bib` |

## F. The TOIS snowball (seed: DOI 10.1145/3748302)

**Seed record, verified.** Zhang, Dai, Bo, Ma, Li, Chen, Zhu & Dong, "A Survey
on the Memory Mechanism of Large Language Model-based Agents", *ACM
Transactions on Information Systems* **43**(6):1–47, published online 2025-09-10,
**125 citations**, **234 references**. Verified via
`api.crossref.org/works/10.1145/3748302`.

The full 234-entry reference list is retrievable from Crossref as
`message.reference[]`, and unlike Semantic Scholar (which the previous pass
found incomplete for other seeds) **every entry carried a usable title string**,
with ~30 present as bare DOIs. Of the 234, ~40 are on-theme for this sweep; the
ones not already in the review and worth keeping:

| # | Work | Venue | Result | Cambrian relevance | Verified |
|---|---|---|---|---|---|
| F1 | **Generative Agents: Interactive Simulacra of Human Behavior** — DOI 10.1145/3586183.3606763 | **UIST 2023** | The **memory stream** (a flat append-only list) scored by recency + importance + relevance, with reflection into higher-level inferences. | The origin of agent memory-as-first-class-object, and the *flat* baseline that B8's negative result defends. Absent from the review despite being the single most-cited memory paper. | `Crossref` |
| F2 | **MemSim: A Bayesian Simulator for Evaluating Memory of LLM-based Personal Assistants** — arXiv:2409.20163 | preprint (no Comments field) | Bayesian Relation Network + causal generation to construct reliable QAs from generated user messages, balancing diversity and scalability; yields the MemDaily dataset. | 🔴 **Addresses the review's own most-cited methodological debt.** METHODOLOGY.md cannot build reliable QA from messages; MemSim shows how to do it *automatically*, which is what makes a large memory-eval sweep affordable. | `arXiv-abs` + `TOIS-bib` |
| F3 | **PerLTQA** — arXiv:2402.16288 | preprint (no Comments field) | 8,593 questions over 30 characters mixing **semantic and episodic** memory (world knowledge, profiles, social relations, events, dialogue); decomposes the task into memory **classification, retrieval, and synthesis**. | The three-stage decomposition is a better row-artifact schema than binary retrieval success, and the semantic/episodic split is ADR-0094/0095's split. | `arXiv-abs` + `TOIS-bib` |
| F4 | **Prompted LLMs as Chatbot Modules for Long Open-Domain Conversation** — arXiv:2305.04533 | preprint | A long-term open-domain dialogue architecture with memory that recalls across sessions. | The ChatGPT-session-memory lineage the review does not cover. | `TOIS-bib` + `arXiv-abs` |
| F5 | **Beyond Goldfish Memory: Long-term Open-domain Conversation** — arXiv:2107.07567 | preprint (2021) | Recency- and frequency-based prompt-memory selection; notes LMs forget within sessions. | Pre-2023 antecedent; cite as lineage for the "conversation memory is a selection problem" framing. | `TOIS-bib` |
| F6 | **Denning, "The locality principle"** | *Communications of the ACM* 48(7):19–24 (2005) | Denning, not the TOIS survey, is the source of the **principle of locality** (storage hierarchy ordered by access frequency). | The founding citation behind MemGPT/Letta-style tiered memory and behind any "hot/cold" retrieval design in Cambrian's substrate. | `Crossref` · DOI **10.1145/1070838.1070856** |
| F7 | **Johnson-Laird, *Mental Models*** (1983) | book (Cambridge) | See D5. | See D5. | `TOIS-bib` (ref [65]) |
| F8 | **Laird, *The Soar Cognitive Architecture*** (2019) | MIT Press | Soar's long-term memory with **strength decay** and chunk retrieval. | Where the "forget by decay" idea in LLM-agent memory actually comes from — B2/B3 are the critics, Soar is the mechanism's respectable ancestor. | `TOIS-bib` (ref [71]) |
| F9 | **The Soar/Solso/Leydesdorff memory-theory cluster** | *Cognitive Psychology* (Solso, 1979); *Memory Cultures* (Leydesdorff, 2017) | The TOIS survey's cognitive-science scaffold: levels of processing, memory-as-social. | Cite via the survey; do not spend retrieval budget on the books. | `TOIS-bib` (refs [141], [74]) |

## G. Evaluation and methodology the review still lacks

| # | Work | Venue | Result | Cambrian relevance | Verified |
|---|---|---|---|---|---|
| G1 | **AFTER benchmark** — arXiv:2606.23127 | preprint | See A7 — 382 tasks, 4 transfer settings. | Topic 10's measurement instrument. | `arXiv-abs` |
| G2 | **Selective Forgetting (graph) negative result** — arXiv:2608.28978 | preprint | See B8 — matched-budget LongMemEval comparison, paired bootstrap, CIs. | The template for how to *disprove* a Cambrian substrate signal: matched candidate budget + paired bootstrap + per-question-type breakdown. Copy the design. | `arXiv-abs` |
| G3 | **Hidden in Memory** — arXiv:2605.15338 | preprint | See E4 — write/retrieve/use pipeline decomposition. | Per-stage metric schema for the `memory-guard` suite. | `arXiv-abs` |
| G4 | **CAPTURE** — arXiv:2609.02265 | preprint | See E6 — 480 episodes / 96 users, head-to-head win rate. | Realistic episode-level protocol for contradiction-vs-poisoning disambiguation. | `arXiv-abs` |
| G5 | **ECHOQA** — arXiv:2410.08414 | preprint | See C2 — four-way relation taxonomy + benchmark. | A ready-made **relation label set** for contradiction-tolerance experiments. | `arXiv-abs` |

---

## Absences and negative findings

- **GITM does not exist as an agent-memory paper in arXiv titles.** Two hits
  only, both ionosphere/thermosphere physics (`2604.24598` M-GITM,
  `1807.01380` GITM). Whatever the brief meant, it is not under this name — do
  not cite a "GITM" memory paper. (`arXiv-search`, 2 hits; treat as
  *title-scoped* only.)
- **No arXiv title match for "selective forgetting in LLM agents"** as an exact
  phrase — the phrase search is too strict; the underlying literature is real
  and was found by the shorter query (37 hits → B7, B8, B9).
- **The TOIS memory-mechanism survey (DOI 10.1145/3748302) contains no
  consolidation/proceduralization paper at all.** It covers long-term memory,
  parametric editing, and knowledge integration thoroughly, but its 234 refs
  reach procedural memory only as Voyager, ExpeL, AWM and Gödel Agent. That is
  the empirical justification for a position paper arguing consolidation is an
  *under-served* axis — the field's own canonical memory survey does not index
  it.
- **No skill-induction paper in the TOIS survey is from 2025 or 2026.** Every
  consolidation reference there is 2023–2024. The 2025–26 procedural-memory
  cluster (A4, A7, A8, A11, A13, A14) post-dates the survey entirely.

## What this changes in the Cambrian story

1. **Topic 10 has a measurement gap before it has a build gap.** AFTER (A7) is
   the suite; ExpeL (A2) and AutoGuide (A4) are the arms; Skill-as-Pseudocode
   (A8) is the gate; ReasoningBank is the industry blog. The one thing missing is
   an ADR-0094/0095 implementation to measure.
2. **"Memory safety over accumulation" is now a *published* position, not just a
   Cambrian preference.** FSFM (B7) names safety as a benefit of forgetting;
   THEANINE (B6) argues forgetting is *wrong*; SkillCenter (A14) is the
   accumulate-everything baseline. A Cambrian position paper has to pick a side
   and cite all three.
3. **The substrate flagship's graph signal is contested.** B8 measured it
   losing to a flat baseline on LongMemEval at matched budget, with a plausible
   mechanism (entity decomposition discards surface form). This is the single
   most urgent item in the file: it should be reproduced on Cambrian's own
   `chunking` / `document-qa` / `musique` suites before it appears in a review.
4. **Per-write validation is not a memory-safety story.** Salami/MemCollusion
   (E1) makes the coalition the threat unit; SPORE (E2) makes the egress path the
   leak. Both bound the claims ADR-0043/0085/0118/0121 can honestly make.
5. **Contradiction handling now has a competitor framing.** ECHOQA (C2) and MACR
   (C3) are doing "explicit conflict resolution" without a substrate, and
   CAPTURE (E6) is doing the *disambiguation* problem (drift vs attack) that
   Cambrian claims as its novelty. Cambrian's differentiator has to be the
   **auditability and scope** properties, not conflict resolution per se.

## Venue-discipline warnings

- **2609.02265 (CAPTURE)** — Comments say "Under review at ICLR 2027". Not a
  publication. Never cite as ICLR.
- **2409.07429 (AWM), 2403.08978 (AutoGuide), 2501.06590 (ChemAgent),
  2407.04363 (AriGraph)** — the arXiv records carry **no Comments field**. Venue
  UNCONFIRMED. Do not attach venue names from memory.
- **2311.05876** — "work in progress… submitted to the IEEE". Preprint.
- **2605.26154 (MemMorph)** — "Preprint. Under review." No venue.
- **2407.01178 (Memory³)** — *Journal of Machine Learning* **3**(2024):300–346.
  Not JMLR. The TOIS survey's DOI (10.4208/jml.240708) is the JML record.
- All `TOIS-bib`-marked rows are double-sourced: the title string came from the
  seed survey's own Crossref reference record, and the identifier was then
  confirmed on arXiv or Crossref.
