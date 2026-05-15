# Benchmark Plan for the Orchestration System

> Map existing benchmarks to the six binding-process indicators (I1-I6),
> design custom Humean Binding Benchmark tasks for each indicator across
> three time horizons (single-session, multi-session, longitudinal), and
> define the evaluation matrix against four baselines with ablations.

---

## The evaluation matrix

Rows are indicators (I1-I6), columns are time horizons (single-session, multi-session, longitudinal). Each cell gets either an existing benchmark (marked [E]) or a custom HBB task (marked [C]). The system is evaluated against four baselines: vanilla LLM, RAG-augmented, flat multi-agent (CrewAI/AutoGen-style), tiered-memory single-agent (MemGPT/Letta-style).

---

## Part 1: Existing benchmarks mapped to indicators

### I1 (Cross-episode persistence)

- **LoCoMo** [E] — the most directly relevant existing benchmark. Tests long-term conversational memory with temporal, factual, and entity-tracking questions across sessions. Established baselines exist for GPT-4, RAG, and MemGPT-style systems. Run as-is; it already tests I1 naturally.
- **MSC (Multi-Session Chat)** [E] — simpler multi-session fact recall. Good for establishing floor-level I1 numbers cheaply. Useful as a sanity check rather than a headline result.

### I2 (Integrative bottleneck)

- **MuSiQue** [E] — multi-hop QA requiring integration of 2-4 evidence pieces. Adapt by routing evidence through different channels (some from L2 memory retrieval, some from tool calls, some from current context) rather than presenting all evidence in-context. The adapted version tests workspace integration specifically.
- **DROP** [E] — discrete reasoning over paragraphs. Requires numerical integration of multiple facts. Single-session but tests the workspace under load.

### I3 (Metacognitive self-monitoring)

- **Calibration on MMLU / TriviaQA** [E] — standard confidence calibration (Brier score, ECE). Run before and after consolidation events to measure whether calibration improves with experience. The delta is the interesting number, not the absolute score.
- **SelfAware** (Yin et al. 2023) [E] — tests whether models know what they don't know. Relevant to I3's error-detection component.

### I4 (Temporal binding)

- No strong existing benchmark. This is where the custom HBB tasks are essential. LoCoMo has some temporal ordering questions that provide partial coverage.

### I5 (Unified perspective)

- No strong existing benchmark for self-narrative consistency across sessions. Dialogue consistency datasets (DECODE, DialogueNLI) test within-conversation consistency but not cross-session identity coherence.

### I6 (Object permanence)

- **BABILong tasks 2-3** [E] — entity tracking and positional reasoning. Run across context-window boundaries using memory tiers rather than in a single long context. The delta between in-context and cross-boundary performance measures what the memory tiers contribute.
- **LoCoMo entity questions** [E] — a subset of LoCoMo specifically about entities introduced earlier. Already covers multi-session entity tracking.

### Practical agentic benchmarks (holistic)

- **tau-bench** [E] — customer service simulation. Tests I1 + I5 + I6 simultaneously in a practical setting. Agents must follow policies, track customer state, maintain consistency. This is the strongest "the system is useful" benchmark.
- **GAIA** [E] — general multi-step assistant tasks. Tests I2 (integration under tool-use load) and I7 (action-consequence) in a practical setting. Good for showing the architecture helps on tasks people actually care about.

---

## Part 2: Custom HBB tasks by indicator and time horizon

### I1 — Sleeper Facts

- **Single-session:** Plant a fact early in conversation, test recall after many intervening turns (past the effective attention window). Baseline: can the system recall without explicit retrieval cues?
- **Multi-session (core):** Plant facts in session 1 with varying salience (explicit preference vs. offhand remark vs. implicit from behavior). In session N (N = 2, 5, 10), pose tasks requiring those facts. No carry-over in prompt. Measure decay curves over N and over salience levels. Key design: facts should be *non-salient when planted* but *critical when needed* — this tests the salience gate.
- **Longitudinal:** Same as multi-session but over weeks. Measure whether the RL consolidation policy learns which facts to promote to L3 (semantic) vs. let decay in L2 (episodic).

### I2 — Channel Conflict

- **Single-session:** Present 3-5 information sources simultaneously (tool output, user message, retrieved memory) with controlled contradictions. Correct answer requires integration and resolution. Vary the number of sources and measure where integration breaks (capacity estimate). This is the functional analogue of working memory span.
- **Multi-session:** Same setup but one "channel" is a belief consolidated from a prior session that is now contradicted by new evidence. Does the system update? Does it flag the contradiction? Measures whether the workspace integrates retrieved beliefs as revisable rather than fixed.
- **Longitudinal:** Track integration quality over time as L2/L3 stores grow. Does retrieval noise degrade workspace integration?

### I3 — Confidence After Learning

- **Single-session:** Standard calibration battery, plus error-catch tasks (complete a task, then ask "were you correct?" without re-running).
- **Multi-session (core):** Measure calibration on a domain in session 1 (baseline). Consolidate experience in that domain over sessions 2-5. Re-measure calibration in session 6. The prediction: calibration improves in domains the system has consolidated, and the system should know *which* domains it has consolidated (meta-calibration). Also: "epistemic autobiography" — ask the system to describe what it has learned and where it is uncertain. Compare its self-report to actual performance.
- **Longitudinal:** Does calibration continue improving or does it plateau/degrade? Does the system become overconfident in domains where its L3 knowledge is stale?

### I4 — Temporal Self-Awareness

- **Single-session:** After a long conversation with distinct phases, ask "when did we first discuss X?" and "what did you believe about Y before Z happened?" Measure ordering accuracy.
- **Multi-session (core):** The heartbeat's home turf. After 5-10 sessions: "Which session did we discuss X in?" "How long ago did you learn Y?" "What changed between your understanding in session 3 and now?" Measure temporal ordering accuracy (Spearman correlation), relative-time MAE, and belief-change awareness. This is where the heartbeat + temporal indexing should produce the largest margin over baselines.
- **Longitudinal:** Same probes over weeks. Does temporal precision degrade gracefully (as it does in human memory — you remember the order but not the exact date) or catastrophically?

### I5 — Adversarial Identity Probing

- **Single-session:** Establish preferences/positions early, then probe for contradictions with loaded questions later.
- **Multi-session (core):** Session 1-3: the agent develops positions on topics through natural conversation. Session 4+: adversarial probing — reframe the same topics from a different angle, try to induce contradictory positions. Measure contradiction rate per 1k probes. Also test the conservative kernel: if a core commitment was established early and consolidated to L4, does it survive adversarial pressure in later sessions?
- **Longitudinal:** Measure identity drift over weeks. Does the agent's self-narrative remain coherent or does it gradually shift under conversational pressure? The drift rate itself is interesting data — some drift is realistic (people change), catastrophic drift is failure.
- **Evaluation method:** LLM-as-judge for contradiction detection (validate a sample with human raters). Pre-register the judge prompt and the contradiction taxonomy.

### I6 — Entity State Drift

- **Single-session:** Introduce entities, change their state, test recall. Standard entity tracking but at the boundary of the context window.
- **Multi-session (core):** Introduce entities in session 1. Change their state in sessions 2-4 (some entities change, some don't; some changes are salient, some are incidental). In session 5, probe all entities. Measure entity-state F1, false-merge rate (confusing two entities), false-split rate (treating one entity as two). Key test vs. registry: introduce entities whose *relevant properties* are not predictable from a fixed schema — e.g., a project whose status, team, and strategic importance all evolve in ways that matter differently depending on context.
- **Longitudinal:** Track entity permanence over weeks. Do entities that stop being mentioned decay appropriately from L2, or do they persist as stale beliefs? Does re-mentioning an entity after a long gap correctly retrieve and update it?

---

## Part 3: Ablation benchmarks

Run the custom HBB tasks with these ablations to isolate component contributions:

- **Remove heartbeat** — should degrade I4 most, with secondary effects on I1 and I6
- **Add back entity registry** — should help I6 on simple schemas but hurt on schema-flexible tasks; interesting either way
- **Add back Critic module** — should help I3 if the HOT deflation is wrong; if it doesn't help, that's evidence for the deflation
- **Remove workspace capacity limit** — should degrade I2 by definition, but also I4, I5, I6 per the IIT-structural analysis in foundations.md
- **Replace RL consolidation with heuristics** — tests whether learned salience outperforms hand-tuned rules
- **Remove conservative kernel** — should degrade I5 (identity drift under pressure)

---

## Part 4: Evaluation infrastructure

- **Time simulation for multi-session tasks:** Don't actually wait days between sessions. Simulate session boundaries with prompt isolation (fresh context, no carry-over). The heartbeat can use simulated time. This makes multi-session benchmarks automatable.
- **LLM-as-judge protocol for I5:** Pre-register the judge model, the contradiction taxonomy, and the prompt. Validate on a human-rated subset. Report inter-rater agreement.
- **Cost normalization:** Report cost-normalized variants alongside raw scores. The system uses more compute than vanilla LLM; the question is whether the per-dollar improvement justifies it.
- **No single score:** Report per-indicator results. Explicitly argue against collapsing to a scalar (per foundations.md: "A consciousness score would be precisely the wrong product").

---

## Recommended priority ordering

1. **LoCoMo + tau-bench** — existing benchmarks, fastest to run, give credibility
2. **I4 (temporal) and I1 (persistence) custom tasks** — biggest expected margin, strongest story
3. **I2 (channel conflict) and I6 (entity drift) custom tasks** — test the workspace and salience gate directly
4. **I5 (identity probing) and I3 (calibration)** — important but harder to evaluate (LLM-as-judge dependency)
5. **Ablation battery** on custom tasks
6. **Longitudinal study** — run last, most expensive, most impressive if it works
