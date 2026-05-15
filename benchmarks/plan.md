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

The architecture's core claim is about binding across time — memory tiers,
heartbeat, consolidation, workspace integration of retrieved state. The
benchmarks that test this claim most directly are those where time horizon is
structurally load-bearing: the agent must persist information across session
boundaries, track state across many steps, or reason about temporal ordering.
We divide existing benchmarks into two tiers accordingly.

### Tier A — Time-horizon relevant (primary)

These benchmarks test capabilities that structurally require persistence,
temporal reasoning, or state tracking across extended interactions. They are
where the architecture should produce the largest margins.

- **LoCoMo** [E] — ~600-turn conversations across up to 32 sessions. Tests long-term conversational memory with five question types including temporal ordering and multi-hop reasoning across sessions. The most directly time-horizon-relevant existing benchmark. Primary: I1, secondary: I4, I6.

- **ITBench** [E] — 102 real-world IT automation scenarios (IBM Research, ICML 2025). SOTA agents resolve only 11.4% of SRE scenarios — the highest-headroom benchmark in the suite. SRE incidents are inherently temporally structured: the agent must hold a diagnostic model across many tool invocations, track system state changes, reason about failure ordering, and maintain a consistent hypothesis rather than flipping between contradictory explanations. Tests I1 + I2 + I4 + I5 + I6.

- **SREGym** [E] — 86 SRE problems on live Kubernetes clusters covering OS-level faults, metastable failures, and concurrent failures. Like ITBench but with higher fidelity (live environments, ambient noise). Incident diagnosis requires persisting observations across diagnostic steps, integrating logs/metrics/traces/topology simultaneously, and temporal ordering of failure events for root-cause analysis. Tests I1 + I2 + I4 + I6. Up to 40% performance variation across failure types.

- **SWE-bench Verified** [E] — real-world GitHub issues requiring cross-file reasoning across many retrieval steps. The horizon here is across files and tool invocations within a single task: the agent must hold information from file A while editing file B, reconcile signals from tests, docs, and source, and maintain a coherent plan. Not multi-session, but the number of retrieval steps and the need to persist gathered context makes workspace integration load-bearing. Tests I1 + I2 + I6.

- **tau-bench** [E] — customer service simulation across airline and retail domains. Multi-turn state tracking under policy constraints. The agent must track customer state, follow complex policies, and maintain consistency across many turns. The Pass^k consistency metric exposes unreliable state tracking. Tests I1 + I5 + I6.

### Tier B — Supplementary (single-session controls)

These benchmarks are single-session and do not structurally require
cross-episode persistence or temporal binding. They serve as controls:
if the architecture helps on these, the workspace integration has general
value; if it only helps on Tier A, the contribution is specific to
time-horizon tasks. They should not be headline results.

- **MuSiQue** [E] — multi-hop QA, adapted by routing evidence through different channels (L2 retrieval, tool output, current message). Tests I2 workspace integration but within a single session.

- **DROP** [E] — discrete reasoning over paragraphs. Tests I2 workspace under numerical load. Single-session.

- **SelfAware** [E] — tests whether models recognize their epistemic limits. Tests I3. Single-session. The custom I3 task (Confidence After Learning) is the time-horizon-relevant version.

- **MMLU / TriviaQA calibration** [E] — standard confidence calibration. Only time-horizon-relevant when used as a before/after measure around consolidation events, which is the custom I3 protocol.

- **BABILong tasks 2-3** [E] — entity tracking adapted to run across context-window boundaries. The cross-boundary adaptation adds some time-horizon relevance, but the underlying tasks are synthetic. Tests I6.

- **MSC** [E] — multi-session fact recall, but only 5 sessions. Redundant with LoCoMo. Sanity check only.

- **GAIA** [E] — multi-step assistant tasks. Tests I2 + I7. Single-session. Demonstrates generality but not the core time-horizon claim.

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

1. **Tier A existing benchmarks** — LoCoMo, ITBench, SREGym, SWE-bench, tau-bench. These are where the architecture should produce the largest margins. Run first, establish credibility on benchmarks reviewers recognize.
2. **I4 (temporal) and I1 (persistence) custom tasks** — biggest expected margin on the core time-horizon claim. The heartbeat and memory tiers are most directly tested here.
3. **I6 (entity drift) and I2 (channel conflict) multi-session custom tasks** — test salience gate and workspace integration across sessions specifically.
4. **I5 (identity probing) and I3 (calibration) multi-session custom tasks** — important but harder to evaluate (LLM-as-judge dependency).
5. **Ablation battery** on custom tasks — the mechanistic story.
6. **Tier B existing benchmarks** — run as single-session controls. If the architecture helps here too, great; if not, it sharpens the contribution claim to time-horizon tasks.
7. **Longitudinal study** — run last, most expensive, most impressive if it works.
