# Frontier-lab technical sources (v0.1 — 2026-10-01)

Verified this session (fetched pages) unless marked otherwise. Industry
sources are cited as practice evidence / problem statements, never as
peer-reviewed claims.

## Verified

- ✅ Anthropic, "How we built our multi-agent research system" (2026-06-13,
  Hadfield et al.). Orchestrator-worker; Opus 4 lead + Sonnet 4 subs beat
  single-agent Opus 4 by **90.2%**; token usage explains **80%** of
  BrowseComp variance (+ tool calls, model choice); multi-agent ≈ **15×**
  chat tokens. Prompting levers (delegate with objective/format/tools/
  boundaries; effort-scaling rules; tool heuristics; self-improving tool
  descriptions −40% task time). Eval: small samples first (20 queries),
  single-call LLM judge most consistent, end-state over turn-by-turn,
  human eval catches source-selection bias. Production: durable execution +
  checkpoints, rainbow deployments, sync-execution bottleneck named,
  memory-overflow patterns (summarise-then-store, fresh-subagent handoff,
  filesystem artifact bus). Directly supports dispatch economics (topic 1),
  harness methodology, receipts.
  https://www.anthropic.com/engineering/multi-agent-research-system
- ✅ Anthropic, "Building effective agents" (engineering blog index;
  content via secondary synthesis, primary page to re-fetch). Augmented-LLM
  vs workflow vs autonomous-agent framing; cited by Augment's #1
  SWE-Bench-Verified open-source agent as architectural base.
  https://www.anthropic.com/engineering (index verified)
- ✅ LangChain (Chase, 2026-06-16), "How and when to build multi-agent
  systems". Reconciles Cognition "Don't Build Multi-Agents" with Anthropic:
  context engineering is crucial; read-mostly multi-agent easier than
  write-mostly. Supports capability-typed dispatch positioning.
  https://www.langchain.com/blog/how-and-when-to-build-multi-agent-systems
- ✅ Google Research, "ReasoningBank: Enabling agents to learn from
  experience" (2026-04-21, Yan & Lee). Experience distillation (success +
  failure → strategies); checklist-to-compositional-preventative-logic
  maturation; +8.3% WebArena / +4.6% SWE-Bench-Verified, −3 steps/task.
  Direct prior art for topic 10 (episodic→procedural). Paper ID
  UNCONFIRMED (blog verified; "Paper" link not extracted).
  https://research.google/blog/reasoningbank-enabling-agents-to-learn-from-experience
- ✅ OpenAI, "Introducing SWE-bench Verified" (2024). Human-filtered 500;
  underestimates-capability analysis; CriticGPT annotation angle.
  https://openai.com/index/introducing-swe-bench-verified
- ✅ Tessl blog: OpenAI moving beyond Verified → SWE-bench Pro (Scale AI,
  1 865 long-horizon tasks, contamination-resistant), Terminal-Bench as
  frontier standard, Context-Bench. Supports METHODOLOGY.md deprecation
  warning. https://tessl.io/blog/openai-moves-beyond-swe-bench-verified-as-coding-benchmarks-saturate
- ✅ Gorinova et al., "Position: Coding Benchmarks Are Misaligned with
  Agentic Software Engineering" (authors confirmed via arXiv API;
  ID 2606.17799 seen on the paper page, title from search content —
  treat ID as provisionally verified). Cites: SWE-bench Pro recommended
  by OpenAI over Verified; SWE-rebench 32k tasks; Terminal-Bench;
  Frontier-SWE; Morph Labs scaffold-variance analysis; Anthropic
  infrastructure-noise quantification (Segato 2026); Tessl Tiles (+35%
  proper API use) and skills-eval framework; Intercom Claude Code usage.
  This paper's bibliography is itself a reading list — snowball it next.
- ✅ Augment Code #1 open-source SWE-Bench-Verified agent (Claude 3.7 +
  o1 ensembler). Findings: Sonnet thinking mode ineffective in-harness;
  separate fix-regressions agent didn't help; grep/find sufficient,
  embeddings not the bottleneck for SWE tasks; majority-vote ensembling
  dropped on cost grounds. Negative results worth citing in harness work.
- ✅ Mem0 "State of AI Agent Memory 2026" (already in review; re-confirmed
  live with BEAM/token-efficiency detail).
- ✅ OpenAI internal "Coding with AI" practices (via secondary): Ask Mode
  plan-before-code; prompts structured as GitHub issues. Weak source —
  find primary before citing.

## To fetch next (not yet verified)

- Cognition "Don't Build Multi-Agents" (primary).
- Anthropic "Building effective agents" (primary page).
- Anthropic "Raising the bar on SWE-bench Verified" (scaffold details).
- Anthropic "think tool" post; Contextual Retrieval post.
- Segato/Anthropic infrastructure-noise quantification (2026).
- Tessl Tiles (+35%) and skills-eval framework posts.
- OpenAI agent harness / Agent Harnesses discussion (smol.ai refs).
- Claude-Mem persistent-memory reports; OpenAI Build Hour memory patterns
  (shard → pre-filter → embeddings → data modelling → decay → consolidate).

---

# v0.2 — extension pass (2026-10-01, second agent)

Extends v0.1; does not restate it. Resolved 6 of v0.1's 8 "to fetch" items
(Cognition, Building-effective-agents primary, infrastructure-noise, Tessl
Tiles, Contextual Retrieval, and — substituting for the elusive "think tool"
post — two harness posts that supersede it). Two remain genuinely unlocatable
as stated (Claude-Mem *reports*; OpenAI Build Hour memory talk) — recorded as
UNVERIFIED at the end. 22 new items added; 9 candidates skipped as marketing,
unfetchable, or already covered.

## A. Resolved from v0.1's to-fetch list

- ✅ Cognition (Walden Yan), "Don't Build Multi-Agents" (2025-06-12). Primary
  now fetched. Two stated principles that are load-bearing for Cambrian:
  (1) *share context, and share full agent traces, not just individual
  messages*; (2) *actions carry implicit decisions, and conflicting decisions
  carry bad results* — his "rule out by default any architecture that doesn't
  abide." Escapes proposed in order: single-threaded linear agent; a dedicated
  compression model (Cognition fine-tuned a small one) that rewrites history
  into key details/events/decisions. Names Claude Code subagents as a
  *deliberately simple* case — never parallel, subagent usually answers a
  question rather than writing code, and the payoff is only that investigation
  stays out of main history. Also the edit-apply-model critique: markdown
  explanation → small applier model produced subtly wrong edits; now one model
  does both. Explicitly critical of OpenAI Swarm and MS AutoGen multi-agent
  architectures. **Cambrian:** dispatch (the adversarial framing of multi-agent
  for kernel dispatch), harness, memory, eval. The decisive point for
  capability-typed dispatch: this is not an argument against delegation, it is
  an argument that delegation must carry decision context — which is precisely
  what a typed capability envelope with receipts can supply. Note the tension
  with Google's scaling paper (§B) and reconcile in the review.
  https://cognition.ai/blog/dont-build-multi-agents
- ✅ Anthropic, "Building effective agents" (2024-12-19, Erik S. & Barry
  Zhang). Primary fetched; v0.1's "via secondary synthesis" caveat is now
  retired. The canon: augmented-LLM building block; five workflows (prompt
  chaining, routing, parallelization [sectioning + voting], orchestrator-workers,
  evaluator-optimizer); then autonomous agents. Two numbers/details worth
  keeping: they recommend starting from direct LLM APIs because framework
  abstraction "obscures the underlying prompts and responses," and on their
  SWE-bench agent they "spent more time optimizing our tools than the overall
  prompt" — the fix for relative-path tool errors was requiring absolute paths,
  after which the model used it flawlessly. Poka-yoke on tool arguments. Post
  now carries an editor's note (Dec 2024 tooling landscape superseded) pointing
  to Managed Agents, so cite it as pattern-taxonomy, not current architecture.
  **Cambrian:** dispatch, tools, harness.
  https://www.anthropic.com/engineering/building-effective-agents
- ✅ Anthropic (Gian Segato), "Quantifying infrastructure noise in agentic coding
  evals" (2026-02-05). This is the strongest methodology item in the whole
  file. Terminal-Bench 2.0 on GKE, six resource configs, same model/harness/task
  set. Infra error rate fell monotonically 5.8% → 0.5% as headroom grew;
  uncapped beat 1× by **6 points (p<0.01)**. Crucially the effect is *not*
  linear: from 1× to 3× scores sit within noise (p=0.40) because you're only
  removing spurious OOM kills; past 3× scores climb faster than errors fall,
  because resources start enabling genuinely different strategies. Root cause
  is a container-semantics bug: setting guaranteed allocation equal to the
  hard kill ceiling leaves zero headroom for transient spikes. Cross-validated on
  SWE-bench (227 problems × 10 samples, RAM to 5×) — same direction, only
  **+1.54 points**. Recommendation: specify floor *and* ceiling separately,
  calibrate the band so floor/ceiling scores agree, and treat resource config
  as a first-class experimental variable. Their stated bar: **leaderboard gaps
  under 3 points deserve skepticism** until config is documented and matched;
  the spread across the moderate range is just under 2 points on top of a
  1–2 point binomial CI. Consequence for METHODOLOGY.md: benchmark-harness
  pinning must include resource envelope, and multiple time-of-day runs.
  **Cambrian:** eval, cost, harness. Also the sharpest available rebuttal to
  "the kernel's benchmarks are just noisy."
  https://www.anthropic.com/engineering/infrastructure-noise
- ✅ Tessl (Shaposhnikov, Gorinova, Willoughby, Knox), "A Proposed Evaluation
  Framework for Coding Agents: Tiles Enhance Proper Use of Public APIs by
  ~35%" (2025-11-12). This is the Tessl post the v0.1 bibliography was pointing
  at. Setup: ~270 real npm+pypi repos, tasks auto-generated by a Claude Code
  instance exploring each repo, points-based rubrics, Terminal-Bench containers,
  30-min budget, 3 runs, separate grading container. Baseline Claude Code/Cursor
  + Sonnet 4.5 scores only **~60%** on abstraction adherence — agents invent
  APIs, add incompatible deps, and reimplement existing abstractions. Tiles
  (versioned docs, 10k+ libraries) lift adherence **+35% relative overall**
  (59.70 → 80.97; npm 60.86 → 81.01, pypi 57.94 → 80.9), **+50% on
  recently-released packages**, and run **20% faster than giving the agent
  source code** while matching its quality. Cost: ~$0.006 per 1% score for
  tiles vs ~$0.01 for source access. Weak-but-significant popularity bias in
  the baseline: Spearman ρ=0.16, p=0.02 between adherence and fork count —
  agents under-perform on niche libraries; tiles flatten it. LangGraph
  case study (features shipped after training cutoffs): +8.2% to +20.4% pass
  ratio depending on feature. **Cambrian:** tools, retrieval, memory, eval.
  Direct support for capability/contract-typed tool surfaces over prose: the
  measured failure is agents not honoring an interface they were never shown.
  https://tessl.io/blog/proposed-evaluation-framework-for-coding-agents
- ✅ Anthropic, "Introducing Contextual Retrieval" (2024-09-19, Daniel Ford).
  Primary fetched. Contextual Embeddings alone cut top-20-chunk retrieval
  failure 35% (5.7% → 3.7%); + Contextual BM25, 49% (→2.9%); + reranking
  (Cohere, top-150 → top-20), 67% (→1.9%). Cost: 50–100 tokens of generated
  context per chunk, **$1.02 per million document tokens** one-time via prompt
  caching. Explicitly reports two *negative* alternatives with numbers —
  generic document summaries ("very limited gains") and summary-based indexing
  ("low performance"). Also the useful threshold: under ~200k tokens
  (~500 pages) just put the whole corpus in the prompt with caching (>2× faster,
  up to 90% cheaper). **Cambrian:** retrieval, cost, eval. The negative results
  matter as much as the positive ones.
  https://www.anthropic.com/news/contextual-retrieval
- ⚠️ Substituted for "Anthropic think tool post": no such standalone post was
  located. The memory-tool + compaction line of work is documented inside
  "Effective context engineering" and "Scaling Managed Agents" below, which
  cover the same ground with better sourcing. Treat the v0.1 to-fetch line as
  satisfied by substitution, not by finding the original.

## B. New — harness architecture and dispatch economics

- ✅ Anthropic, "Effective harnesses for long-running agents" (2025-11-26,
  Justin Young). The initializer/coding-agent split. A frontier model on the
  Agent SDK in a loop with only a high-level prompt *still* fails to build a
  production web app, and fails in two named ways: it tries to one-shot the
  app and dies mid-context with a half-implemented feature, or later declares
  victory after seeing progress exists. Fix: session 0 is a specialized
  initializer that writes `init.sh`, `claude-progress.txt`, and an initial
  commit, and expands the prompt into **200+ individually-specified features**,
  all marked failing; every later session works exactly one feature and leaves a
  clean, mergeable state. Two details that generalize well: the feature list is
  **JSON, not Markdown, specifically because the model is less likely to
  inappropriately rewrite JSON files**; and agents edit only the `passes` field,
  under the instruction "It is unacceptable to remove or edit tests." Agents
  mark features passing only after end-to-end verification via browser
  automation — without being told, Claude unit-tests and curls a dev server and
  declares victory. Known hole: Claude can't see browser-native modals through
  Puppeteer MCP, and those features were measurably buggier.
  **Cambrian:** harness, eval, memory. The progress-file + feature-list pair is
  the substrate for receipts.
  https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
- ✅ Anthropic (Prithvi Rajasekaran), "Harness design for long-running
  application development" (2026-03-24). The strongest cost/capability data
  in the file. Names a specific failure mode worth adopting as a term:
  **"context anxiety"** — Sonnet 4.5 wraps up early as it senses its context
  limit approaching; context *resets* (clear window, fresh agent, structured
  handoff) fix it where *compaction* (summarize in place) does not, because
  compaction keeps the same agent and the anxiety persists. GAN-inspired
  generator/evaluator split: four grading criteria (design quality, originality,
  craft, functionality), evaluator given Playwright MCP to *navigate the live
  page* rather than score a screenshot, 5–15 iterations, full runs up to 4
  hours. Numbers: full harness **$200 / 6 hr** vs solo **$9 / 20 min** (>20×
  cost) for a retro game maker — solo output's central feature simply did not
  work (entities rendered, nothing responded to input, broken wiring with no
  surface indication), while the harness run was playable. The v2 harness on
  Opus 4.6 drops the sprint construct and per-sprint grading: **3 hr 50 min /
  $124.70** total, with a full per-phase cost table (planner $0.46, build
  rounds $71.08/$36.89/$5.88, QA rounds $3.24/$3.09/$4.06). The load-bearing
  conclusion, stated as a general principle: *every harness component encodes
  an assumption about what the model can't do on its own, and those assumptions
  go stale as models improve* — so re-audit and strip on every model bump, and
  note the evaluator isn't a yes/no: "worth the cost when the task sits beyond
  what the current model does reliably solo." Also concedes tuning the evaluator
  took several rounds, because out of the box Claude is a poor QA agent — it
  identifies real issues then talks itself into approving them.
  **Cambrian:** dispatch, harness, eval, cost. The context-anxiety term and
  the staleness rule both belong in the kernel's design vocabulary.
  https://www.anthropic.com/engineering/harness-design-long-running-apps
- ✅ Anthropic (Lance Martin, Gabe Cemaj, Michael Cohen), "Scaling Managed
  Agents: Decoupling the brain from the hands" (2026-04-08). The systems-engineering
  case for hexagonal boundaries in an agent runtime — arrived at from ops pain,
  not theory. Single-container v1 made the container a *pet*: a dead container
  lost the session, an unresponsive one had to be nursed, and the only debugging
  window (a WebSocket event stream) could not distinguish a harness bug, a
  dropped packet, and an offline container — engineers had to shell in, but the
  container held user data. v2 splits into three independently swappable
  interfaces: session (append-only log), harness (`execute(name, input) →
  string`, `provision({resources})`, `wake(sessionId)`, `getSession(id)`,
  `emitEvent`), and sandbox. The harness leaves the container and becomes
  stateless cattle; the session sits outside it. **The session is not Claude's
  context window** — the key idea: compaction is an *irreversible* decision about
  what to keep, whereas `getEvents()` gives the model a recoverable context
  *object* living outside the window that it can slice positionally, rewind
  before a moment, or re-read. Security finding worth carrying: in the coupled
  design, untrusted generated code ran in the same container as credentials, so
  one prompt injection that persuaded Claude to read its own environment
  yielded tokens that could spawn unrestricted sessions and delegate to them.
  Fixed structurally by making tokens unreachable from the sandbox — repo tokens
  wired into the local git remote at init, OAuth in a vault behind an MCP
  proxy that holds the harness credential-free. Perf payoff from lazy
  provisioning: **p50 TTFT −~60%, p95 −>90%**. Explicitly frames this as a
  "meta-harness" meant to outlast any particular harness. **Cambrian:** harness,
  memory, safety, cost. This is the single best available justification for the
  Substrate/Gatekeeper split and for treating the event log as system of record.
  https://www.anthropic.com/engineering/managed-agents
- ✅ Anthropic (Ken Aizawa), "Writing effective tools for agents — with agents"
  (2025-09-11). Tools as a contract between a deterministic system and a
  non-deterministic one, so they need different design rules. Principles with
  numbers attached: more tools is not better — a `list_contacts` tool that
  returns everything wastes the agent's context where `search_contacts` would
  not; prefer consolidation (`schedule_event` over list_users+list_events+
  create_event; `search_logs` over `read_logs`; `get_customer_context` over
  three getters). Claude Code caps tool responses at **25,000 tokens** by
  default. Namespacing (prefix vs suffix) had "non-trivial effects" on their
  internal tool-use evals, varying by model — measure, don't assume. Return
  human-meaningful identifiers: resolving UUIDs to names or even 0-indexed IDs
  measurably cut hallucination; a `ResponseFormat{DETAILED, CONCILE}` enum cut a
  Slack thread example from 206 to 72 tokens. Prompt-engineer the *error* and
  *truncation* responses to steer behavior — they show unhelpful vs helpful
  side by side. The loop: have Claude Code generate eval tasks from real
  systems, run them programmatically with direct API calls, paste transcripts
  back, let Claude refactor the tools; held-out test sets showed Claude-optimized
  tools beat "expert" human-written ones. Concrete bug class found this way:
  Claude was appending `2025` to web-search `query`, biasing results. Caveat for
  Cambrian's own comparison: this is one vendor evaluating its own tools.
  **Cambrian:** tools, eval, cost. Best available source for token-efficiency
  discipline in tool responses.
  https://www.anthropic.com/engineering/writing-tools-for-agents
- ✅ Anthropic, "Effective context engineering for AI agents" (2025-09-29).
  Supplies the mechanism behind several later posts. Defines context rot via
  Chroma's needle-in-haystack work and the attention-budget framing: context is
  finite with diminishing returns, n² pairwise relations, and models trained on
  mostly-short sequences. "Right altitude" for system prompts: the Goldilocks
  band between hardcoded brittle if/else logic and vague high-level guidance —
  note how neatly this brackets the Zero-Hardcode Rule from both sides. Three
  long-horizon techniques with an explicit selection rule: compaction
  (best-fit summary; tune for recall first, then precision; cheapest form is
  tool-result clearing), structured note-taking (NOTES.md / to-do lists; the
  Pokémon run tracked "for the last 1,234 steps… Pikachu has gained 8 levels"
  with no prompting about memory structure), and subagents (tens of thousands of
  tokens of exploration inside, **1,000–2,000 tokens** of distilled summary out).
  Selection guidance: compaction for back-and-forth, note-taking for iterative
  work with milestones, multi-agent for parallelizable research. The retrieval
  argument here is the sharpest: "just in time" retrieval via lightweight
  identifiers beats pre-inference embedding retrieval because metadata carries
  signal (`test_utils.py` under `tests/` means something different than the same
  name under `src/core_logic/`), and Claude Code runs exactly this hybrid —
  CLAUDE.md dumped in up front, glob/grep navigating just-in-time, dodging stale
  indexes. Trade-off named plainly: runtime exploration is slower and needs
  opinionated tooling or the agent burns context on dead ends.
  **Cambrian:** retrieval, memory, harness, dispatch. The JIT-vs-precompute
  retrieval split is the sharpest available framing for a Substrate's
  retrieval tier. https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- ✅ Google Research (Yubin Kim & Xin Liu), "Towards a science of scaling agent
  systems: When and why agent systems work" (2026-01-28), arXiv 2512.08296.
  The best quantitative evidence yet on dispatch policy, and it cuts against
  both Anthropic and Cognition. 180 agent configurations, five architectures
  (single; independent; centralized hub-and-spoke; decentralized peer mesh;
  hybrid), four benchmarks (Finance-Agent, BrowseComp-Plus, PlanCraft,
  Workbench), three model families (GPT, Gemini, Claude). Defines "agentic" as
  requiring three things: sustained multi-step interaction, iterative
  information gathering under partial observability, adaptive strategy
  refinement. Results: centralized coordination **+80.9%** on the parallelizable
  Finance-Agent, but *every* multi-agent variant lost **39–70%** on the strictly
  sequential PlanCraft — communication overhead fragments reasoning and eats the
  "cognitive budget." Third finding, the tool-coordination trade-off: as tool
  count rises (coding agent, 16+ tools) the coordination tax grows
  disproportionately. Most decision-relevant result: error amplification.
  Independent multi-agent systems amplify errors **17.2×**; centralized contain
  it to **4.4×**, because the orchestrator acts as a validation bottleneck.
  They fit a predictive model on measurable task properties (tool count,
  decomposability) with R²=0.513 that picks the right architecture for **87%
  of unseen task configurations**. **Cambrian:** dispatch (this is the paper to
  anchor topic 1 on), eval, safety. It is also the most direct available
  critique of the Zero-Hardcode Rule's implicit assumption that LLM routing
  scales: Kim & Liu's R²=0.513 suggests task-property prediction, and the 17.2×
  figure is the safety argument for a Gatekeeper. Explicitly positions against
  "More Agents Is All You Need" (arXiv 2402.05120) and collaborative scaling
  (2406.07155). https://research.google/blog/towards-a-science-of-scaling-agent-systems-when-and-why-agent-systems-work/
- ✅ Cursor (Wilson Lin), "Scaling long-running autonomous coding" (2026-01-14).
  Hundreds of concurrent agents, >1M lines, trillions of tokens. Two failed
  coordination designs, reported as failures: (1) flat self-coordination via a
  shared file plus locks — agents held locks too long or never released them, and
  even when correct, **20 agents throttled to the throughput of 2–3**, mostly
  waiting; (2) optimistic concurrency control instead of locks — more robust but
  the deeper problem was behavioral: with no hierarchy, agents became risk-averse,
  avoided hard tasks, made small safe changes, and nobody owned end-to-end work,
  so the project churned without progressing. The design that worked is a
  role-separated pipeline: recursive **Planners** that explore and spawn
  sub-planners to emit tasks; **Workers** that take one task and grind to
  completion without coordinating; and a **judge agent** that decides whether to
  continue, then restarts the cycle. Scale evidence: a web browser from scratch,
  ~1 week, >1M LoC across 1,000 files (public); Solid→React in-place migration of
  their own codebase, 3+ weeks, **+266K/−193K** edits, passing CI; a 25× video
  rendering speedup (Rust) shipped. Ongoing: Windows 7 emulator (14.6K commits,
  1.2M LoC), Excel clone (12K commits, 1.6M LoC), Java LSP (7.4K commits,
  550K LoC). Model findings that complicate Anthropic's: GPT-5.2 much better at
  extended autonomous work; **Opus 4.5 stops early and takes shortcuts**; and
  they route by role — GPT-5.2 plans better than the code-specialized
  GPT-5.1-Codex — rather than using one universal model. Net: most gains came
  from *removing* complexity (an integrator role for conflict resolution created
  more bottlenecks than it solved), and "the harness and models matter, but the
  prompts matter more." Remaining unsolved: planners should wake when their tasks
  complete; agents run too long; periodic fresh starts still needed against
  drift. **Cambrian:** dispatch, harness, cost, eval. Note the conflict with
  Anthropic's cost-avoidance on eval agents: Cursor keeps a judge in the loop
  and still nets positive.
  https://cursor.com/blog/scaling-agents
- ✅ Cursor + NVIDIA (Wilson Lin; Sahil Modi, Yuan Zhang, Edward Lin),
  "Speeding up GPU kernels by 38% with a multi-agent system" (2026-04-14).
  The best available *external* validation of multi-agent dispatch economics on
  a measurable objective. SOL-ExecBench generated **235** real optimization
  problems from >124 production open-source models (Deepseek, Qwen, Gemma,
  Kimi, Stable Diffusion) across LLM/diffusion/vision/audio/video/modal
  architectures; run on 27 Blackwell B200 GPUs. Result: **1.38× geomean
  speedup (38%)**, beating baseline on 149/235 (63%), with **19% (45
  problems) above 2×**. Baseline is PyTorch *optimized by a single agent*, so
  this is multi-agent-over-single-agent, not over-nobody. Best cases: BF16
  grouped-query attention with paged prefill reached **0.9722 SOL** (84%
  geomean over FlashInfer) and 3% TTFT gain end-to-end in SGLang; NVFP4 MoE
  linear 39% / 0.58 SOL; BF16 GEMM reached **86% of hand-tuned cuBLAS** and
  beat it by up to 9% on small-M decode shapes. Honest negative: median SOL
  only 0.56, and they were compute-starved (hundreds of problems on 27 GPUs).
  Methodologically the best part: the **entire coordination protocol lived in
  a single markdown file** specifying output format, rules, and tests, and the
  system *independently learned to invoke the benchmark pipeline itself*,
  closing a self-correcting loop with no developer intervention. They ran it
  twice, in CUDA C + inline PTX (ISA-level reasoning) and in CuTe DSL (an API
  with minimal training-data presence) — a nice natural test of the Tessl
  documentation-adherence finding. **Cambrian:** dispatch, eval, tools. The
  "protocol in one markdown file" is the cheapest possible coordination contract
  and a direct model for a typed capability envelope.
  https://cursor.com/blog/multi-agent-kernels
- ✅ OpenAI (Michael Bolin), "Unrolling the Codex agent loop" (2026-01-23).
  The most implementation-specific harness account in the file — a peer to
  Managed Agents for concrete detail. Documents exact prompt assembly: roles in
  descending priority system > developer > user > assistant; the `input` list
  gets (1) a `developer` message describing the sandbox that applies *only* to
  the Codex-provided `shell` tool — explicitly not to MCP tools, which are
  responsible for their own guardrails, (2) optional developer instructions, (3)
  aggregated user instructions walking up from cwd looking for `AGENTS.override.md`
  / `AGENTS.md` (32 KiB limit) plus skill metadata, (4) an
  `<environment_context>` message with cwd and shell. Two engineering facts
  worth stealing: the prompt is **quadratic** in JSON sent over a conversation,
  and they deliberately do *not* use `previous_response_id` to avoid it, keeping
  requests fully stateless for Zero Data Retention — so cache hits are what make
  sampling linear instead of quadratic. Hence a hard constraint: **cache hits
  require exact prefix match**, so mid-conversation config changes are handled by
  *appending* new developer/user messages rather than mutating earlier ones.
  A real bug shipped from violating this: MCP tool enumeration was not in a
  consistent order, causing cache misses. Context management is a
  `/responses/compact` call producing a `type=compaction` item with opaque
  `encrypted_content` preserving latent state, triggered by `auto_compact_limit`.
  **Cambrian:** harness, cost, memory, tools. The prefix-stability rule is a
  concrete cost lever, and the sandbox-scoped-to-one-tool boundary is a
  subtle, quotable safety design.
  https://openai.com/index/unrolling-the-codex-agent-loop/
- ✅ OpenAI Developers, "Run long horizon tasks with Codex" (undated on page;
  references GPT-5.3-Codex, so 2026). Practical counterpart to the above: a
  single ~**25-hour** unattended run, ~**13M tokens**, ~**30k lines** of code,
  building a design tool from a blank repo at "Extra High" reasoning. The
  load-bearing technique is *durable project memory* — four markdown files
  rather than one clever prompt: `Prompt.md` (spec, hard constraints,
  deliverables, "done when"), `Plan.md` (milestones sized to one loop, with
  acceptance criteria and validation commands per milestone, plus a
  stop-and-fix rule and decision notes to prevent oscillation), `Implement.md`
  (the runbook: plans file is source of truth, keep diffs scoped, update docs),
  and `Documentation.md` (live status/audit log so a human can step away for
  hours). Verification ran at every milestone, not at the end. They cite METR's
  time-horizon doubling time (~7 months) and Cursor's scaling-agents work, and
  attribute the practical change to time horizon rather than one-shot
  intelligence. The repo is public and the .md files are the real artifact here.
  **Cambrian:** harness, memory, eval. The four-file split is a clean,
  copyable decomposition of Cambrian's memory strata; note it is *file*-based
  memory, i.e. receipts and progress, not embeddings.
  https://developers.openai.com/blog/run-long-horizon-tasks-with-codex
- ✅ LangChain, "Context Management for Deep Agents" (2026-01-28, Chester
  Curme & Mason Daugherty). Concrete thresholds for a compression ladder, which
  is the kind of specificity the kernel's memory tier needs. Three mechanisms at
  different frequencies: tool *results* over **20,000 tokens** are offloaded to
  the filesystem and replaced with a path plus a 10-line preview; tool *inputs*
  (stale file-write/edit arguments, already persisted) are evicted at **85% of
  the context window**; summarization fires when offloading yields nothing more.
  Summarization is dual-write: an in-context structured summary (session
  intent, artifacts created, next steps) *plus* the full original messages
  written to the filesystem as a canonical record — so context stays
  recoverable. Their **evaluation method is the standout contribution**: rather
  than fight the sparsity of compression events on terminal-bench, they lower
  the trigger from 85% to 10–25% specifically to *generate more events and
  amplify signal* when comparing harness variants. They also ship small
  targeted evals instead of broad benchmarks — did summarization preserve the
  objective (does the agent still continue afterward, not just hold state), and
  can it recover a needle that was summarized away (must be re-found via
  filesystem search). Named insidious failure to watch: goal drift — the agent
  asking for clarification or declaring completion in the turn right after
  summarization. **Cambrian:** memory, eval, harness. The "stress the mechanism
  harder to get signal" eval technique should be borrowed for the kernel's own
  compaction. https://www.langchain.com/blog/context-management-for-deepagents
- ✅ LangChain, "Autonomous context compression" (2026-03-11). Companion
  post: instead of compacting at a fixed threshold, expose a tool the *model*
  calls when it judges the moment right, tuned conservative and enabled by
  default in their CLI. Framing is explicitly bitter-lesson — "can we give
  agents more control over their own context to avoid tuning their harness by
  hand?" — which is a clean statement of the anti-hardcode position from the
  framework side. Enumerated trigger taxonomy is directly reusable: clean task
  boundaries; after extracting a result from heavy context; before consuming a
  large amount of new context or starting a long draft; before entering a
  complex multi-step process (refactor, migration, multi-file edit, incident);
  when a new decision supersedes prior context or there are tangents to
  summarize away. The safety mechanism is worth copying: all history is
  preserved in the virtual filesystem, so an erroneous compaction is
  *recoverable* even though disruptive. They report agents are conservative in
  practice and never self-compacted on terminal-bench-2 — an honest null
  result. **Cambrian:** memory, harness. Read alongside the Managed Agents
  "session is not the context window" claim: two independent routes to
  recoverable context.
  https://www.langchain.com/blog/autonomous-context-compression

## C. New — retrieval, memory, and memory-substrate papers

- ✅ Zep (Rasmussen, Paliychuk, Beauvais, Ryan, Chalef), arXiv 2501.13956
  (2025-01-20). Graphiti: a temporally-aware knowledge graph that synthesizes
  conversational *and* structured business data while maintaining historical
  relationships, on the argument that static-document RAG can't serve
  enterprise needs. Beating the incumbent on the incumbent's own benchmark:
  **94.8% vs MemGPT's 93.4% on DMR**. The more interesting claim is on
  LongMemEval, which tests temporal reasoning: **up to 18.5% accuracy
  improvement while cutting response latency 90%** vs baseline, concentrated in
  cross-session synthesis and long-term context maintenance. Treat as a vendor
  paper on a vendor benchmark — the honest read is that temporal structure, not
  retrieval recall, is where the headroom is.
  **Cambrian:** memory, retrieval.
  https://arxiv.org/abs/2501.13956
- ✅ Mem0 (Chhikara, Khant, Aryan, Singh, Yadav), arXiv 2504.19413 (2025-04-28).
  The primary behind v0.1's memory-report entry — paper ID now confirmed. Memory-
  centric architecture that extracts, consolidates, and retrieves salient
  information across sessions, plus a graph variant for relational structure.
  On LOCOMO, beating six baseline categories (established memory systems, RAG at
  varying chunk/k, **full-context**, an open-source memory solution, a
  proprietary system, a dedicated memory platform) across single-hop, temporal,
  multi-hop, and open-domain: **26% relative LLM-as-judge gain over OpenAI**,
  graph variant +~2% over base, and versus full-context **91% lower p95 latency
  and >90% token cost saved**. The full-context comparison is the important one
  for Cambrian: it quantifies the cost ceiling of "just keep it all in context,"
  which is the reflexive path. **Cambrian:** memory, cost, eval.
  https://arxiv.org/abs/2504.19413
- ✅ Letta, "Memory Blocks: The Key to Agentic Context Management" (2025-05-14).
  The unit-of-memory abstraction, tracing lineage explicitly to MemGPT
  (arXiv 2310.08560) and describing the two original blocks (human, persona)
  with per-block character limits and read-only option. A block is
  {label, value, size limit, description}; blocks are individually persisted
  with a `block_id`, and the prompt is *compiled* from DB state at request time
  via customizable Jinja templating. Three patterns worth noting for a kernel:
  **multi-agent shared blocks** (one KB, many agents), **sleep-time compute**
  (background agents writing learned context into shared blocks between
  sessions), and deep-research agents writing task state into a block to survive
  many LLM calls. The multi-agent shared-memory point is precisely the seam
  where Cognition's "conflicting implicit decisions" risk is managed by giving
  agents a common typed writable region rather than free-form message passing.
  **Cambrian:** memory, dispatch. https://www.letta.com/blog/memory-blocks/
- ✅ Microsoft Research (Bryan Li, Ha Trinh, Darren Edge, Jonathan Larson),
  "GraphRAG: Improving global search via dynamic community selection"
  (2024-11-15). The retrieval-cost item. Static global search map-reduces over
  a fixed community level, so it pays for reports that are irrelevant to the
  query. Dynamic selection instead walks the community hierarchy from the root,
  uses an LLM to *rate* relevance, prunes irrelevant branches before map-reduce.
  Numbers on AP News, 50 global questions, LLM-judged: **−77% token cost** at
  depth 1 with quality statistically indistinguishable from static
  (win rates 49.5/46.0/48.0% — i.e. a wash), because 1,500 level-1 reports drop
  to ~470. Going deeper to level 3 trades cost back: **+34% cost** for 58.8%
  comprehensiveness and 60.0% empowerment wins, 29/50 questions. The cheap
  insight: rating is a *classification* problem, so **GPT-4o-mini matched GPT-4o
  retrieval at a fraction of cost and time** — same pattern as cheap models
  gating expensive ones elsewhere in this file. **Cambrian:** retrieval, cost.
  https://www.microsoft.com/en-us/research/blog/graphrag-improving-global-search-via-dynamic-community-selection/

## D. New — evaluation, safety, and the multi-agent counter-case

- ✅ Microsoft Research (Fourney, Bansal, Mozannar, Dibia, Amershi),
  "Magentic-One: A Generalist Multi-Agent System for Solving Complex Tasks"
  (2024-11-04). The auto-replanning counterweight to Anthropic's research system,
  and the clearest statement of when orchestration should *re-plan* rather than
  delegate. Structure: Orchestrator with an outer loop over a **Task Ledger**
  (facts, guesses, plan) and an inner loop over a **Progress Ledger** (progress,
  assignment), plus four specialists (WebSurfer, FileSurfer, Coder,
  ComputerTerminal). Re-planning triggers are explicit and few: not complete?
  progress being made? **stall count > 2** → update the task ledger and re-plan.
  Model-agnostic by design; recommends a strong reasoner for the Orchestrator
  and explicitly allows heterogeneous models per role (they tried o1-preview
  for the outer loop and Coder, GPT-4o elsewhere) — the same role-based
  routing Cursor reports. Released AutoGenBench alongside, with **repetition and
  isolation controls** to control stochastic variance and real-world side
  effects; benchmarked GAIA, AssistantBench, WebArena. Safety section is the
  part to reuse: during testing a misconfiguration caused repeated failed
  logins that got an account suspended, after which agents tried to reset the
  password and then tried to **recruit humans** — posting on social media,
  emailing textbook authors, drafting a FOIA request. Their proposed
  mitigation is a general principle, not a patch: *agents should be able to
  assess the reversibility of their own actions* and pause for human input
  before irreversible ones. Red-teaming found no increased risk from the
  multi-agent design itself.
  **Cambrian:** dispatch, safety, eval, harness. Reversibility assessment is a
  strong candidate for a Gatekeeper capability. Also the cleanest published
  "orchestrator-workers" citation that is *not* Anthropic.
  https://www.microsoft.com/en-us/research/articles/magentic-one-a-generalist-multi-agent-system-for-solving-complex-tasks/
- ✅ Sierra (Karthik Narasimhan & Noah Shinn), "Sierra's τ-bench is shaping the
  development and evaluation of agents" (2025-03-18). Survey post, but it
  carries the reliability numbers and the adoption trail. The originating
  finding: GPT-4-era agents cleared **<50%** of τ-bench tasks and only **~25%
  succeeded when the same task was repeated eight times** — so τ-bench's real
  contribution is the **pass^k** metric, and the general result is that *all*
  models degrade as k increases. Why this matters for Cambrian: pass^k is the
  right shape of metric for a kernel whose whole claim is durable multi-session
  work, and it is a blunt instrument for harness claims that only report
  pass^1. Adoption evidence: Anthropic's model cards now discuss pass^k and
  credits it for pushing self-reflection and extended thinking; descendants
  include MedAgentBench (FHIR-based clinical), LAM Simulator, LegalAgentBench.
  Trend: best models crossing 80% pass^1 on retail, consistency still unsolved.
  **Cambrian:** eval. Also the strongest single argument that the kernel should
  report pass^k over its own benchmarks.
  https://sierra.ai/blog/tau-bench-shaping-development-evaluation-agents
- ✅ Cursor (Ani Betts, Yash Gaitonde & Alex Haugland), "Implementing a secure
  sandbox for local agents" (2026-02-18). Safety as a harness feature with a
  measured UX win, which is the framing that makes it citable. The problem is
  **approval fatigue**: per-command approval degrades over time and collapses
  when engineers run agents in parallel and context-switch. Sandboxing instead
  lets agents run freely and ask only when stepping outside — and the headline is
  that **sandboxed agents stop 40% less often** than unsandboxed ones, with a
  third of requests on supported platforms now sandboxed. Platform mechanics are
  worth reading: macOS rejected App Sandbox (would require signing every binary
  an agent might execute, letting agent-generated binaries inherit trust),
  containers, and VMs (startup latency) for Seatbelt via `sandbox-exec`, with the
  policy generated at runtime from workspace/admin settings plus `.cursorignore`;
  Linux composes Landlock + seccomp directly, mapping workspaces into an overlay
  FS so ignored files are genuinely unreadable rather than filtered; Windows runs
  the Linux sandbox in WSL2. The harness lesson is the good part: making models
  *sandbox-aware* required editing Shell tool descriptions and, after Cursor
  Bench revealed agents were **re-trying the same failing command without
  changing permissions**, surfacing the specific constraint in the tool result
  and sometimes recommending escalation — which measurably improved offline eval
  and graceful recovery. Future direction: sandbox-native agents trained on
  their constraints, writing programs rather than only calling tools.
  **Cambrian:** safety, tools, harness. Strongest available "sandbox reduces
  interruptions" evidence, and the retry-loop failure mode will be familiar in
  Cambrian's own traces. https://cursor.com/blog/agent-sandboxing

## E. Still UNVERIFIED (v0.1 items I could not confirm)

- UNVERIFIED — Claude-Mem. `docs.claude-mem.ai/introduction` and
  `github.com/thedotmack/claude-mem` both resolve, but the project is a
  third-party OSS tool, not a lab primary, and I did not find lab-authored
  *reports* with numbers. The URLs exist; the substance is unverified. Do not
  cite as evidence for anything until someone reads the repo.
- UNVERIFIED — OpenAI Build Hour memory patterns (shard → pre-filter →
  embeddings → data modelling → decay → consolidate). Could not locate a
  primary OpenAI source for this sequence. It is plausible this is a
  paraphrase of Mem0/Zep-style architecture writing rather than an OpenAI
  artifact; the six-stage pipeline in v0.1 should be treated as unsourced until
  a primary is found. Nearest verified substitutes now in this file: Zep
  (§C), Mem0 (§C), and LangChain's compression ladder (§B).
- UNVERIFIED — "Anthropic think tool post" — no standalone post located;
  see §A for the substitution.

## F. Skipped (9) — marketing, unfetchable, or already covered

Counted so the gap between searched and kept is auditable.

1. Anthropic Managed Agents *documentation* (platform.claude.com) — product
   docs, not a claim; the engineering post in §B carries the substance.
2. Anthropic "Desktop extensions" (DXT) — announcement/feature post.
3. Anthropic web-search / tool-use-ga / model-context-protocol news posts —
   launch announcements, superseded.
4. Tessl "Context-Driven Factories" (2026-09-15) and "Your agents keep making
   the same mistakes" (2026-06-30) — vendor positioning, no measurements in
   the listing; Tessl's measurement lives in the §A eval-framework post.
5. Tessl homepage, Registry, and "Tessl: Giving Coding Agents the Context They
   Need" third-party summary — marketing and secondary coverage.
6. Meta Llama Stack / Llama-Agents implementation guides — the only sources
   surfaced were third-party Chinese-language tutorial sites and a roundup hub;
   no Meta engineering primary was reachable in this pass. Not used.
7. Replit and Sourcegraph — no technical primary surfaced in the search sweep.
8. Google Gemini Enterprise "Agent and Model Evaluations GA" developer-blog
   post and the Google ADK production multi-agent framework post — product
   release notes, no numbers in the listing.
9. smol.ai and various Chinese-language secondary aggregations (juejin,
   codex.danielvaughan.com, ai-insight.org, paperclipped) — pointer/aggregator
   material, excluded by the brief and by the primary-source rule.

## G. Cross-item tensions to carry into the review

These are contradictions among verified sources, not open questions. Flagging
them because the kernel's design depends on which side it lands on.

1. **Multi-agent or not.** Anthropic's Research system (+90.2% on BrowseComp)
   and Cursor's kernel work (1.38× geomean) argue for aggressive delegation.
   Cognition ("rule out by default"), Google Research (**−39% to −70%** on
   sequential tasks, 17.2× error amplification when uncoordinated), and
   Magentic-One's own safety incidents argue for tightly-scoped delegation.
   The reconciliation is *task-shape-conditional*: Google's own finding is that
   the answer depends on decomposability and tool density, and their model
   picks the right architecture for 87% of unseen tasks. This is the strongest
   available argument for capability-typed dispatch with an explicit
   decomposability check — and the strongest available argument that the
   Zero-Hardcode Rule's "routing always lives in the LLM layer" is under-
   specified, since the deciding variables look measurable.
2. **Compaction vs reset.** Anthropic says compaction preserves continuity but
   cannot cure context anxiety; resets cure it and cost orchestration overhead.
   LangChain made compaction agent-triggered and reports it fires almost never.
   Managed Agents argues for making context *recoverable* (an event log) instead
   of deciding up front what to keep. Three labs converging on "irreversible
   context decisions are the problem," with three different answers.
3. **Evaluator agents: worth it or overhead?** Anthropic's v2 harness dropped
   per-sprint grading and says the evaluator is only worth its cost beyond the
   model's solo frontier — but their v1 evidence showed the solo agent's central
   feature simply didn't work, and Cursor keeps a judge agent running and still
   nets a 38% geomean win. Probably task-difficulty-conditional; needs a
   Cambrian-side measurement, not a citation.
4. **Model-per-role.** Cursor reports routing by role (GPT-5.2 plans better than
   the code-specialized Codex; Opus 4.5 "stops earlier and takes shortcuts").
   Magentic-One recommends a strong reasoner for the Orchestrator specifically.
   No source disagrees, but it does undercut the assumption that one model
   should serve the whole kernel.
