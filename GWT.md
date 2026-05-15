# GWT Agent — Design Document

> **Status: early-stage design doc — intentionally underspecified.**
>
> This document derives from and co-evolves with `foundations.md`. As the
> theoretical foundations (Humean binding, GWT, the Lerchner constraint, the
> I1–I8 indicator framework) are developed and refined there, the architectural
> choices, evaluation targets, and claims here will be updated to reflect them.
> Nothing in this document should be read as converged. Sections are scaffolding
> for a design that will sharpen as the underlying theory sharpens.
>
> Specifically:
> - **§1 (Theoretical grounding)** is a compact extraction from `foundations.md`
>   at time of writing. It will drift as the source develops — always defer to
>   `foundations.md` for the current position.
> - **§2 (Architecture)** sketches a plausible GWT agent topology, not a frozen
>   spec. Component boundaries, workspace capacity, specialist roster, and
>   memory tier semantics are all open.
> - **§3–4 (Domains, Evaluation)** list candidate task families and benchmarks
>   to be narrowed after empirical scoping. The benchmark landscape is moving;
>   we will re-survey before committing.
>
> The relationship is one-directional: `foundations.md` leads, this doc follows.
>
> **What this document is.** An engineering design doc for a GWT-based
> pseudo-conscious agent aimed at measurable capability uplift on real-world
> tasks: enterprise workflows, SRE, and large-scale production codebases.
>
> **What this document is not.** A consciousness claim. The full philosophical
> treatment — including why we make no such claim — lives in `foundations.md`.
>
> **Companion document:** `foundations.md` — working thesis, paper reviews,
> benchmark plan (I1–I8 binding indicators), publication plan.

---

## 1. Theoretical grounding (compact extraction from foundations.md)

### 1.1 Global Workspace Theory

GWT (Baars, 1988, 1997; Dehaene & Naccache, 2001; Dehaene & Changeux, 2011;
Mashour et al., 2020) models consciousness as contents *broadcast* through a
capacity-limited "workspace" to many specialist modules. Non-broadcast contents
remain unconscious. The workspace is an integrative bottleneck, not a
homunculus.

The core architectural claim: a small, unified representation is formed from
multiple inputs, held in a limited-capacity buffer, and made available to all
downstream specialists simultaneously. This is why specialists that never
directly communicate can nonetheless coordinate — they all read from the same
broadcast.

**Why this matters for agents.** Current multi-agent systems are either
flat (agents as peers, no integrative bottleneck) or hierarchical (one
orchestrator delegating, but not *integrating*). Neither topology reproduces
the GWT pattern. A GWT agent has a genuine workspace: a capacity-limited
representation that fuses information from memory, tools, current context, and
specialist outputs into a single coherent state before action selection.

### 1.2 The Humean binding skeleton

From Hume (*Treatise*, 1739): the self is not a substance but a binding
process. Memory, resemblance, and causation knit successive perceptions into
the felt appearance of continuity. Memory is constitutive, not evidential — it
does not discover identity, it *produces* it.

**Architectural translation:** a well-designed memory architecture *is* an
identity architecture. Persistent state across episodes, temporal indexing,
entity tracking, and autobiographical coherence are not features bolted onto
an agent — they are the substrate of a continuous self-like agent that can
accumulate expertise, maintain context across weeks, and behave as a coherent
collaborator rather than a stateless tool.

### 1.3 The Lerchner constraint

Lerchner (*The Abstraction Fallacy*, DeepMind, 2026) argues that no syntactic
system can instantiate consciousness, because computation is map-dependent:
it requires an already-experiencing mapmaker to alphabetize continuous physics
into discrete symbols. We accept this constraint fully.

**What it means for this project:** we make no consciousness claim. We build
a *simulation* of GWT-style integration — a functional workspace that produces
the behavioral signature of a continuous, integrating, time-aware agent. The
claim is purely engineering: this architecture outperforms single-agent and
flat-multi-agent baselines on practical tasks. Whether it instantiates
anything beyond syntax is a question we explicitly leave to the philosophers,
per `foundations.md` §4.

### 1.4 The workspace as salience gate — self-adaptive learning

There is a deeper consequence of GWT-style integration that goes beyond
coordination. If the workspace loop runs fast enough — integration, broadcast,
action, consolidation — then the capacity-limited bottleneck becomes a
**real-time salience gate for what the system learns**. The workspace doesn't
just decide what to do next; it decides what crosses the threshold into
persistent memory. What survives the bottleneck is what the agent remembers.
What the agent remembers is, per Hume, what it *becomes*.

This is the central theoretical claim of this design doc and deserves careful
development.

#### 1.4.1 The gate mechanism

The workspace is capacity-limited by design (§2.2). At every integration
step, the narrator must compress the full input stream — user message,
specialist outputs, retrieved memories, entity state, temporal context —
into a fixed-capacity representation. This compression is lossy. Information
that does not survive the compression does not enter the workspace and
therefore cannot be broadcast, acted on, or consolidated into memory.

This is not a bug. This is the gate.

The question "what should the agent learn?" reduces to "what survives
workspace compression?" — which in turn reduces to "what does the narrator,
given the current integrated context, judge to be salient?" The narrator does
not need a separate "memory importance scorer." The workspace compression *is*
the importance scoring. Anything that the narrator preserves in the workspace
under capacity pressure has, by that act, been judged worth attending to.
Anything subsequently written from the workspace into L1/L2 memory has been
judged worth remembering.

The gate therefore has two stages:

1. **Attention gate** (what enters the workspace). The narrator selects from
   the input stream under capacity pressure. This is analogous to the GWT
   claim that only "winning" coalitions of processors gain access to the
   global workspace — most information is processed unconsciously and never
   broadcast.
2. **Consolidation gate** (what leaves the workspace into persistent memory).
   Not everything attended to is worth remembering. The workspace contents
   at the end of an integration cycle are candidates for memory promotion,
   but a second judgment is made: does this information have cross-episode
   utility? Will it be useful beyond this turn?

The two-stage structure means the system can attend to something transiently
(use it for the current task) without committing to remembering it. This is
the difference between working memory and long-term memory, and it falls out
of the architecture without being hand-engineered.

#### 1.4.2 The feedback loop — learned salience

The gate is not static. What the agent has already learned affects what it
attends to, which affects what it learns next.

Consider: the workspace receives a specialist output about a database
migration. If the agent's L3 semantic store already contains knowledge about
this codebase's migration patterns (from prior sessions), the narrator can
recognize this output as important — it matches, extends, or contradicts
existing knowledge. That recognition increases the probability of
consolidation. Conversely, if L3 is empty on this topic, the same specialist
output may read as generic and be compressed away.

This is a **positive feedback loop on expertise**. The more the agent knows
about a domain, the better it gets at recognizing what is worth knowing. Early
learning is slow and somewhat random (cold start, no priors to anchor
salience). But as the memory tiers fill, the gate becomes increasingly
selective and domain-appropriate. The agent develops a *taste* for what
matters in its deployment context.

The biological parallel is real: GWT models of attention argue that prior
knowledge modulates what enters the global workspace (Dehaene & Naccache,
2001, on "top-down amplification"). We are reproducing the same loop
structurally — prior memory biases current attention, current attention biases
future memory.

#### 1.4.3 The risk — salience collapse and filter bubbles

The feedback loop is double-edged. If prior knowledge too strongly biases
what enters the workspace, the agent stops learning new things. It attends
only to what it already knows about and consolidates only what confirms its
existing model. This is a filter bubble in the learning dynamics —
**salience collapse**.

Failure modes:

- **Domain narrowing.** An agent deployed for both SRE and code review
  develops stronger SRE memories early (perhaps because the first few
  incidents were salient). The richer SRE knowledge biases future attention
  toward SRE signals. Code review context gets compressed away under workspace
  pressure because the SRE context is more "recognizable." The agent becomes
  an SRE specialist by accident, not by design.
- **Confirmation bias.** The agent's entity registry records that service X
  is unreliable. Future signals about service X are filtered through this
  prior, and evidence of reliability is compressed away as anomalous. The
  belief self-reinforces.
- **Novelty starvation.** In a stable deployment, the agent stops
  encountering information that differs enough from its existing model to
  survive workspace compression. Learning plateaus not because the
  environment is static but because the gate has closed.

These are not speculative — they are structural consequences of the feedback
loop. They need architectural countermeasures:

- **Novelty bonus in the consolidation gate.** Information that is
  *surprising* relative to existing L2/L3 content should get a consolidation
  boost, not a penalty. The RL reward signal in `foundations.md` §7 includes
  "surprise-at-recall"; this is the mechanism. Surprise = prediction error
  relative to the agent's own model. High surprise = high learning value.
- **Exploration budget in the attention gate.** Reserve a fraction of
  workspace capacity for information that does *not* match existing priors.
  This is the equivalent of an epsilon-greedy policy in the attention
  mechanism: mostly exploit existing knowledge to allocate attention, but
  sometimes attend to the unexpected.
- **Periodic memory audit.** On heartbeat ticks (§2.6), the system can
  sample from L2/L3 and check whether the distribution of stored knowledge
  has drifted from the distribution of actual inputs. If the agent is
  deployed for SRE + code review but 90% of L3 is SRE, that imbalance is
  detectable and correctable.
- **Human-in-the-loop override.** For high-stakes deployments, a human can
  flag "this is important, learn it" — forcing information through the gate
  regardless of the narrator's salience judgment. This is the L4 conservative
  kernel write path repurposed as a teaching mechanism.

#### 1.4.4 The inference-speed constraint

The gate only works if the integration loop is fast enough.

The salience judgment depends on the *fused* workspace state — the narrator
must have the current user input, the specialist outputs, the retrieved
memories, and the temporal context all integrated before it can judge what
matters. If integration is too slow, one of two things happens:

1. **The consolidation decision is deferred.** The system stores everything
   from the turn into L1 and postpones the salience judgment to a later
   consolidation pass (e.g., on idle heartbeat). But a deferred judgment
   operates on a *stale* workspace — the fused context that made the salience
   signal legible has already been overwritten by the next turn. The agent is
   now doing retrospective archiving, not real-time learning. The gate
   degrades into a heuristic (recency, frequency) rather than a contextual
   judgment.

2. **The integration is truncated.** To stay within latency budget, the
   narrator receives only a partial input stream. The gate operates on
   incomplete information, and the salience judgment is noisy. The agent
   learns, but poorly.

Neither is catastrophic — both are strictly better than no memory at all —
but neither produces the self-adaptive behavior that makes the architecture
interesting. The full salience gate requires that the
integration-to-consolidation path completes within a single turn's latency
budget, while the fused context is live.

**Practical implication:** the narrator model and the workspace
serialization format must be optimized for speed, not just quality. A
smaller, faster model doing integration + consolidation on a tightly
structured workspace may outperform a larger model doing the same on
unstructured text, because the smaller model can close the loop within
the latency window. This is a case where integration speed is a first-class
architectural parameter, not a deployment detail.

#### 1.4.5 Differentiation from existing approaches

Why is this not just "memory with a relevance score"?

- **RAG** retrieves based on embedding similarity to the current query. There
  is no integration step and no consolidation judgment. Everything is stored;
  retrieval is the only filter. RAG has no salience gate — it has a
  similarity search.
- **MemGPT / Letta** adds tiered memory with explicit management, which is
  closer. But the memory management is a separate function, not a consequence
  of capacity-limited integration. The system decides what to store *after*
  processing the turn, not *as a byproduct of* processing the turn. The gate
  and the reasoning are decoupled.
- **Flat multi-agent systems** (CrewAI, AutoGen) have no integrative
  bottleneck at all. Each agent has its own context. There is no point where
  all information is fused under pressure, so there is no point where a
  salience judgment is forced. Learning, if it happens, is per-agent and
  uncoordinated.
- **The GWT agent** fuses integration and consolidation into the same
  bottleneck. The salience judgment is not a separate module — it is an
  emergent property of capacity-limited workspace compression. The narrator
  does not "score importance"; it *attends under pressure*, and what
  survives *is* what was important. This is closer to how biological GWT
  models describe the relationship between attention and memory encoding:
  what enters the global workspace is preferentially encoded into long-term
  memory (Dehaene & Naccache, 2001).

### 1.5 Position in one paragraph

A GWT-based pseudo-conscious agent wraps a frozen LLM in explicit binding
machinery: a capacity-limited global workspace, specialist sub-agents that
read from the workspace broadcast, tiered persistent memory producing
cross-episode continuity, a heartbeat producing temporal indexing, and an
entity registry producing object permanence. We claim measurable capability
improvement on real-world application benchmarks over single-agent systems,
RAG-augmented agents, and flat multi-agent architectures. We do not claim
consciousness. We claim the *functional pattern* that GWT describes —
integrate, broadcast, act — produces better agents.

---

## 2. Architecture

### 2.1 Overview

```
                     ┌─────────────────────────┐
                     │    Global Workspace      │
                     │  (capacity-limited buf)  │
                     │                          │
                     │  fused representation    │
                     │  from all inputs         │
                     └────────┬────────────────┘
                              │ broadcast
              ┌───────────────┼───────────────┐
              │               │               │
         ┌────▼────┐    ┌────▼────┐    ┌────▼────┐
         │ Spec-1  │    │ Spec-2  │    │ Spec-N  │
         │ (code)  │    │ (SRE)   │    │ (docs)  │
         └────┬────┘    └────┬────┘    └────┬────┘
              │               │               │
              └───────────────┼───────────────┘
                              │ results
                     ┌────────▼────────────────┐
                     │      Narrator           │
                     │  (sole write authority   │
                     │   to workspace + memory) │
                     └─────────────────────────┘
```

### 2.2 The Global Workspace

The workspace is the central architectural primitive. It is:

- **Capacity-limited.** Not the full context window. A structured
  representation (likely 2–4k tokens equivalent) that forces integration.
  Overfull inputs must be compressed, prioritized, or dropped — this is the
  bottleneck that produces integration rather than concatenation.
- **Broadcast.** All specialist sub-agents read from the workspace state.
  They do not communicate peer-to-peer. This eliminates the coordination
  overhead of flat multi-agent systems and ensures coherent action.
- **Write-gated.** Only the narrator writes to the workspace. Specialists
  propose; the narrator integrates. This is the single-narrator commitment
  from `foundations.md` §8.1, chosen over Dennett's multiple-drafts model
  because a single integration point is simpler to evaluate and debug.
- **Temporally indexed.** Each workspace state carries a timestamp from the
  heartbeat. The workspace knows "now" relative to its own history.

**Implementation sketch.** The workspace is a structured JSON or typed object
maintained across turns: current goal, active entities, salient memory
retrievals, pending specialist outputs, temporal context, confidence/uncertainty
signals. The narrator LLM call receives this object as its primary input, not
the raw conversation history.

### 2.3 Specialist sub-agents

Specialists are event-triggered, transient, and domain-scoped. They are not
peers; they are mental acts. Each specialist:

- Reads the workspace broadcast (current state + relevant slice).
- Executes within a bounded scope (tool calls, code generation, retrieval).
- Returns a structured result to the narrator. Does not write to memory or
  workspace directly.

**Planned specialists for the target domains:**

| Specialist | Domain | Trigger |
|---|---|---|
| **CodeAnalyzer** | Large codebase navigation, dependency tracing, impact analysis | Code-related queries; file references; PRs |
| **SREAgent** | Incident triage, runbook execution, observability queries | Alerts; system metrics; incident keywords |
| **DocSynthesizer** | Internal docs, knowledge base, onboarding material | Factual queries; how-to requests |
| **Planner** | Multi-step task decomposition, pre-mortem | Complex goals; ambiguous requests |
| **Executor** | Sandboxed code execution, shell commands, API calls | Concrete action steps from Planner |
| **Critic** | Output validation, error detection, confidence calibration | Post-action review; high-stakes outputs |

The Critic is the HOT-derived component: a higher-order representation of
the system's own outputs, enabling metacognitive self-monitoring
(`foundations.md` I3). It reviews specialist outputs before the narrator
integrates them into the workspace.

### 2.4 The Narrator

The narrator is the sole integration point. It:

1. Receives the current workspace state + specialist outputs + user input.
2. Produces the next workspace state (integration).
3. Produces the user-facing response (broadcast to the human).
4. Writes to memory tiers as appropriate (consolidation).

The narrator is not a router or orchestrator in the LangChain sense. It is a
*binder* — it fuses heterogeneous inputs into a single coherent perspective.
The workspace forces this: because it is capacity-limited, the narrator must
decide what survives and what is compressed. This decision *is* the binding.

### 2.5 Tiered memory

Adapted from `foundations.md` §7 (method paper architecture). Tiers map to
different persistence and access characteristics:

| Tier | Name | Persistence | Access pattern | Analogy |
|---|---|---|---|---|
| L0 | Working memory | Current turn | Direct in workspace | Working memory |
| L1 | Session memory | Current session | Fast retrieval | Short-term memory |
| L2 | Episodic store | Cross-session | Indexed retrieval | Episodic memory |
| L3 | Semantic store | Permanent | Embedding search | Semantic memory |
| L4 | Core identity | Permanent, write-protected | Always loaded | Conservative kernel |

**L4 — the conservative kernel.** A small set of identity-stabilizing facts,
values, and behavioral commitments that resist modification. This is what
prevents drift over long deployments. The kernel is write-protected after
bootstrap; modifications require explicit human authorization. This addresses
`foundations.md` open question §8.3 (what is in the conservative kernel) with
a practical answer: whatever the deploying team decides is invariant.

**Memory movement.** Items promote from L1 → L2 → L3 based on retrieval
frequency, salience, and recency. Items in L2/L3 that are never retrieved
decay. The full RL-driven consolidation policy from `foundations.md` §7 is a
later optimization; v1 uses heuristic promotion with tunable thresholds.

### 2.6 Heartbeat and temporal indexing

A background process ticks at regular intervals (configurable; default: every
few minutes of wall-clock time during active sessions, less frequently during
idle). Each tick:

- Timestamps the workspace state.
- Optionally triggers consolidation (memory L1 → L2 promotion).
- Updates the temporal context available to the narrator ("12 minutes since
  last user message"; "this session started 2 hours ago"; "last discussed
  topic X three days ago").

This produces the temporal binding that vanilla LLMs completely lack
(`foundations.md` I4). The agent can answer "when did we last discuss X?" and
"what did you believe before Y happened?" — questions that are trivial for
humans and impossible for stateless models.

### 2.7 Entity registry

A structured store tracking named entities (people, systems, projects,
codebases, incidents) with:

- Current believed state.
- Last-updated timestamp.
- Confidence level.
- Source (which session/interaction produced this belief).

This produces object permanence (`foundations.md` I6). When an entity goes out
of the context window, it is not forgotten — it persists in the registry and
can be retrieved when relevant. State changes are tracked, so the agent knows
that "service X was healthy last week but has been degraded since Tuesday."

---

## 3. Target domains and task families

The claim is that GWT-style integration produces measurable capability uplift
over single-agent baselines on tasks that require **cross-context integration,
temporal awareness, and persistent expertise**. We target three domains where
these properties matter most.

### 3.1 Enterprise workflows

**Why GWT helps.** Enterprise tasks routinely require integrating information
from email, docs, databases, calendars, and prior conversations. A stateless
agent treats each query independently. A GWT agent maintains a workspace that
already contains the user's role, active projects, recent decisions, and
organizational context — the integration is pre-loaded.

**Task families:**

- **OfficeQA / enterprise QA.** Multi-hop questions over heterogeneous
  enterprise data sources (docs, spreadsheets, email, Slack). Existing
  benchmarks: OfficeQA, CRUD-RAG, EnterpriseBench.
- **Cross-document synthesis.** "Summarize all decisions from last week's
  meetings that affect project X." Requires temporal awareness + entity
  tracking + multi-source integration.
- **Workflow continuation.** A task started on Monday, partially completed,
  resumed on Wednesday. The agent must remember what was done, what remains,
  and what changed in between. This is pure cross-episode persistence (I1).
- **Onboarding assistance.** New employee asks a sequence of increasingly
  specific questions over days. The agent must build a model of what the user
  already knows and adapt accordingly.

### 3.2 SRE and incident response

**Why GWT helps.** Incident response is the canonical case for integrative
bottleneck value. An SRE during an incident must fuse signals from monitoring
dashboards, logs, recent deploys, runbooks, prior incidents, and team
communication — under time pressure. This is exactly what a global workspace
does: compress heterogeneous signals into a unified situation picture and
broadcast it to specialist actions.

**Task families:**

- **Incident triage.** Given alert + metrics + recent changes, identify root
  cause and recommend action. Measure time-to-correct-diagnosis.
- **Runbook execution with adaptation.** Follow a runbook but adapt when
  actual system state diverges from expected state. Requires the agent to
  maintain an entity model of system components and their current state.
- **Post-incident review.** Synthesize a timeline from logs, alerts, and
  chat transcripts. Requires temporal binding + entity tracking across a
  multi-hour event.
- **Cross-incident pattern recognition.** "Is this the same failure mode
  we saw last month?" Requires episodic memory (L2) reaching back weeks.

### 3.3 Large-scale production codebases

**Why GWT helps.** Navigating a large codebase (100k+ files) requires
building and maintaining a mental model of architecture, dependencies,
conventions, and recent changes. Single-agent systems rebuild this model from
scratch every session. A GWT agent accumulates codebase knowledge in its
semantic store (L3) and entity registry, so each session starts with a warm
model.

**Task families:**

- **Cross-file impact analysis.** "If I change this interface, what breaks?"
  Requires dependency tracing across the codebase + knowledge of downstream
  consumers built up over prior sessions.
- **Convention-aware code generation.** Generate code that follows the
  project's actual patterns, not generic best practices. Requires the agent
  to have internalized the codebase's conventions from prior interactions.
- **Long-running refactors.** Multi-day refactoring campaigns where the agent
  tracks what has been migrated, what remains, and what complications have
  emerged. Pure cross-episode persistence.
- **Architectural Q&A.** "Why is the auth service structured this way?" —
  answerable from accumulated context, not just code search.
- **PR review with project history.** Review a PR in context of prior
  decisions, related PRs, and known technical debt. Requires episodic memory
  of past reviews and discussions.

---

## 4. Evaluation plan

### 4.1 Baselines

| System | Description | Why included |
|---|---|---|
| **Single-agent (vanilla)** | Frontier LLM, no memory, no multi-agent | The floor |
| **Single-agent + RAG** | Frontier LLM + retrieval-augmented generation | The current industry default |
| **Flat multi-agent** | Multiple agents as peers (CrewAI / AutoGen style) | Tests whether coordination topology matters |
| **MemGPT / Letta** | Single agent with tiered memory but no workspace | Isolates the GWT integration contribution |
| **GWT agent (ours)** | Full architecture from §2 | — |

### 4.2 Metrics

We measure on two axes: the practical capability metrics (this document) and
the binding-process simulation fidelity indicators (foundations.md I1–I8).
Reporting both lets us make the engineering claim (GWT agents work better)
and the theoretical claim (GWT agents score higher on binding indicators)
in companion papers.

**Practical capability metrics (per domain):**

- **Task completion rate.** Binary: did the agent produce a correct output?
  Graded: how correct, on a domain-appropriate rubric.
- **Cross-episode transfer.** Performance on session N+k vs. session 1, on
  tasks that benefit from accumulated context. The slope of this curve is the
  core number — it measures whether the agent gets better over time.
- **Time-to-correct-answer.** Wall-clock time including retrieval, tool use,
  and specialist coordination. GWT integration has overhead; we need to show
  the accuracy gain justifies it.
- **Context utilization efficiency.** How much of the available context window
  is wasted on re-establishing state vs. used for new reasoning? The workspace
  should compress prior state, freeing context for the actual task.
- **Coherence over time.** Contradiction rate in outputs over multi-day
  interactions (borrowed from I5). Enterprise users need the agent to not
  contradict itself across meetings.

**Cost-normalized variants.** GWT agents use more compute (multiple specialist
calls, workspace maintenance, memory operations). We report cost-normalized
metrics alongside raw scores. If the GWT agent is 2x more expensive but only
5% more accurate, that is not a win. The hypothesis is that the accuracy gap
widens on tasks requiring cross-episode integration — the workspace pays for
itself on hard problems.

### 4.3 Benchmark candidates

**Existing benchmarks to adopt or adapt:**

- **OfficeQA** — enterprise QA over heterogeneous sources.
- **SWE-bench** — code generation in real repositories; we extend to
  multi-session variants where prior PR context accumulates.
- **CRUD-RAG / EnterpriseBench** — enterprise document workflows.
- **AIOpsLab / AIOps benchmarks** — SRE incident response tasks.
- **RepoQA / CrossCodeEval** — cross-file code understanding.

**New benchmark components we may need to build:**

- **Multi-session variants** of existing benchmarks. Most benchmarks are
  single-turn or single-session. The GWT agent's advantage is cross-episode;
  we need tasks that span sessions with information dependency.
- **SRE incident simulation harness.** Synthetic incidents with realistic
  signal streams (alerts, logs, metric timeseries) played out over simulated
  time. May adapt from existing chaos engineering frameworks.
- **Codebase familiarity curve.** Measure performance on codebase tasks as a
  function of prior exposure sessions. No existing benchmark does this.

### 4.4 Ablation plan

To isolate which components drive the capability uplift:

| Ablation | What is removed | What it tests |
|---|---|---|
| No workspace | Specialists communicate directly; no integrative bottleneck | Is GWT integration the driver, or is it just having specialists? |
| No memory tiers | All memory in context window (standard RAG) | Is persistent tiered memory the driver? |
| No heartbeat | No temporal indexing | Does temporal awareness matter for practical tasks? |
| No entity registry | No object permanence across sessions | Does entity tracking matter? |
| No Critic | No metacognitive review of outputs | Does self-monitoring improve accuracy? |
| No conservative kernel | L4 is writable / absent | Does identity stability matter over long deployments? |
| Heuristic memory only | Replace any RL-driven consolidation with fixed rules | Is learned consolidation worth the complexity? |

---

## 5. Relationship to the arxiv papers

This design doc feeds two outputs:

1. **The applied capability paper (new).** "GWT-based pseudo-conscious agents
   outperform single-agent systems on enterprise, SRE, and codebase tasks."
   Venue: ICML, NeurIPS (main track), or an applied-AI venue. This paper leads
   with results and references the theory lightly.

2. **The method paper (`foundations.md` §7).** "A Humean Architecture for
   Time-Aware Language Agents." This paper leads with the theoretical
   architecture and evaluates on both the I1–I8 binding indicators *and* the
   practical benchmarks from this doc. The practical results serve as the
   "real-world grounding" that makes the theoretical contribution concrete.

The **benchmark paper** (`foundations.md` §6) remains separate and
consciousness-theoretic: it operationalizes the I1–I8 indicators with the
Lerchner-aware framing. The applied capability results from this doc are
complementary evidence but not the benchmark paper's focus.

```
foundations.md §6          foundations.md §7          GWT.md (this doc)
(benchmark paper)          (method paper)             (applied paper)
                  \              |              /
                   \             |             /
                    ─────── shared architecture ───────
                             (§2 of this doc)
```

The three papers share an architecture but make different claims:
- **Benchmark paper:** here is how to measure binding-process simulation
  fidelity (philosophical/measurement contribution).
- **Method paper:** here is a Humean binding architecture and it scores
  well on those indicators (theoretical/architectural contribution).
- **Applied paper:** here is a GWT agent and it outperforms on real tasks
  (engineering/capability contribution).

---

## 6. Open design questions

1. **Workspace capacity.** How small is small enough to force real integration
   without starving the narrator of information? Needs empirical tuning per
   domain. Start with 2k tokens equivalent, measure degradation as we shrink.

2. **Specialist roster per domain.** The table in §2.3 is a starting point.
   Each deployment domain will need a different specialist mix. The
   architecture should be specialist-agnostic — the workspace + narrator +
   broadcast pattern is fixed; specialists are pluggable.

3. **Memory cold start.** A fresh GWT agent has empty L2/L3/L4. How do we
   bootstrap? Options: pre-populated semantic store from docs/code; explicit
   onboarding session; or accept cold-start degradation and measure the
   warmup curve (which is itself an interesting metric).

4. **Workspace serialization format.** JSON? Typed schema per domain? Natural
   language summary? The choice affects both narrator quality and specialist
   parsing. Likely: structured JSON with natural-language narrative field.

5. **Multi-user workspaces.** Enterprise deployments may need a single agent
   serving a team. Does each user get a workspace, or is there a shared team
   workspace + per-user overlay? GWT theory says one workspace per conscious
   agent; but our agent is not conscious, so we can pragmatically split.

6. **Latency budget.** The workspace-integrate-broadcast loop adds latency
   vs. a direct single-agent call. Target: < 2x latency of single-agent
   for interactive use; unconstrained for batch/async tasks. Measure whether
   streaming the narrator's output while specialists are still running is
   viable.

---

## 7. References

Theoretical references are maintained in `foundations.md` §9. Applied
benchmark references:

- OfficeQA — **[to add: full citation when benchmark version is selected]**
- SWE-bench — Jimenez, C. E., Yang, J., Wettig, A., Yao, S., Pei, K.,
  Press, O., & Narasimhan, K. (2024). *SWE-bench: Can Language Models Resolve
  Real-World GitHub Issues?* ICLR 2024. **[to verify]**
- CRUD-RAG / EnterpriseBench — **[to add]**
- AIOpsLab — **[to add: specific AIOps benchmark TBD based on available
  public benchmarks]**
- RepoQA / CrossCodeEval — **[to add]**
