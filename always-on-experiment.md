# Always-On Mentation: A Pseudo-Human Experiment

> **Status:** speculative design. Separated from the production
> architecture (reactive-with-warmup, see `CORE-architecture-spec.md` §4)
> because the inference cost makes continuous internal activity impractical
> for utility deployments. This document designs the experiment, not the
> product.
>
> **The question:** What happens when an agent thinks continuously — not
> just when prompted? Does continuous internal activity produce
> qualitatively different behavior than reactive processing with warm-up?

---

## 1. The core idea

Standard agents are dormant between inputs. The workspace is empty. When
input arrives, the system starts from cold: retrieve, integrate, respond.

Biological brains are never dormant. The default mode network (DMN)
maintains continuous internal activity: replaying memories, generating
predictions, making associations, rehearsing plans. External stimuli do
not CREATE processing — they MODULATE and REDIRECT ongoing processing.
The response is not manufactured from scratch; it emerges from the
alignment of internal state with external input.

This experiment implements continuous mentation in an LLM-based agent and
measures whether it produces measurably different behavior on the binding-
process indicators (I1–I6).

---

## 2. Architecture

### 2.1 The thinking clock

The heartbeat becomes a thinking clock. Every tick (configurable interval,
default 5–10 seconds), the system performs a lightweight integration cycle
even in the absence of external input.

Each tick:
1. **Random retrieval.** Sample from L2/L3 — not by relevance to anything,
   but by priority-weighted random selection (strength, recency, or
   uniform). This is the default mode network retrieving memories without
   external cue.
2. **L4 bias pass.** Run the retrieval against L4 kernel entries. Flag
   conflicts, connections, or identity-relevant associations.
3. **Lightweight integration.** A smaller/faster model (not the full
   narrator) processes the retrieval + L4 flags and produces a "thought"
   — a short workspace state representing what the system is currently
   thinking about. This is NOT a user-facing response. It is internal.
4. **Workspace update.** The thought becomes the current workspace state.
   It persists until the next tick or until input arrives.

The thinking clock produces a stream of internal states — the agent's
"stream of consciousness" in the functional sense (per Lerchner's
constraint, we make no phenomenal claim).

### 2.2 Input as alignment signal

When external input arrives (user message, tool output, system event):

1. The input enters a workspace that is ALREADY OCCUPIED by the current
   thought.
2. PE is computed between the input and the current thought.
   - **Low PE:** the input is related to what the system was already
     thinking about. The thought provides priming — the system was
     "ready" for this input. Integration is fast and contextually rich.
   - **High PE:** the input is unrelated to the current thought. The
     thought is displaced. Integration starts from the input, not from
     the ongoing activity. This is the attentional blink — the system
     was "thinking about something else."
3. Full narrator integration runs (same as the reactive model's Phase 2).
4. Post-response consolidation runs (same as reactive Phase 3).

The prediction: low-PE inputs (ones the system was already thinking about)
should produce faster, more contextual, higher-quality responses than
high-PE inputs. This is measurable.

### 2.3 L4 agents as long-running background influence

Agents with access to the conservative kernel (L4) operate as persistent
background processes — living system prompts. They do not generate
workspace content directly. They generate BIAS SIGNALS that shape what
the workspace attends to.

In the always-on model, L4 agents run continuously alongside the thinking
clock. They monitor the stream of internal thoughts for alignment with
(or deviation from) core commitments. When the system's idle thoughts
drift from its identity-relevant values, L4 agents inject corrective
bias into the next tick.

Higher-order agents (specialists, the narrator) surface to the workspace
only on input or when the thinking clock produces a thought that triggers
specialist activation. The hierarchy is in access pattern: L4 agents have
deep, constant, low-bandwidth influence. Specialists have high-bandwidth,
bursty influence. The narrator integrates where depth meets surface.

### 2.4 What the agent thinks about

The "random thoughts" are not truly random. They are shaped by:

- **Recency.** Recent episodes are more likely to be retrieved and
  re-processed. The system "ruminates" on recent events.
- **Unresolved threads.** Episodes with high PE at storage time — things
  that were surprising or unresolved — are more likely to be retrieved.
  The system "worries" about unfinished business.
- **L4 bias.** The kernel-level agents bias retrieval toward
  identity-relevant content. The system "thinks about" things that
  matter to its core commitments.
- **Pattern completion.** When L2 episodes share partial structure, the
  system may retrieve them together and attempt pattern extraction.
  This is spontaneous L2 → L3 consolidation — not filed during idle,
  but discovered during active thought.
- **Anticipation.** Based on conversational trajectory (recent workspace
  states), the system generates predictions about what the user might
  bring up next. If the prediction is correct, the response is
  pre-primed.

### 2.5 Cost model

The always-on model is expensive. Each thinking tick requires:
- One embedding + retrieval operation (~10ms, cheap)
- One lightweight LLM call for thought generation (~200ms, ~100 tokens)

At 1 tick per 10 seconds: 6 LLM calls per minute of idle time. At
frontier model pricing, this is ~$0.01–0.05 per minute of continuous
mentation. For a long-running agent, this adds up.

**Cost mitigation options:**
- Use a small, fast model for thinking ticks (not the full narrator).
  The narrator only activates on input.
- Adaptive tick rate: frequent when the agent is "active" (recent input),
  infrequent when idle for a long time. This mimics the brain's gradual
  transition from active thought to drowsiness.
- Batch ticks: accumulate several retrievals, process in one call.
- Limit always-on mode to experimental evaluation, not production.

---

## 3. Experimental design

### 3.1 Hypothesis

Continuous mentation produces measurable improvements on time-horizon-
dependent binding indicators (I1, I4, I6) compared to the reactive-with-
warmup model, because:

1. Spontaneous memory replay strengthens consolidation (I1 improves).
2. Continuous temporal experience produces richer temporal binding (I4
   improves).
3. Ongoing entity processing keeps entity states "warm" (I6 improves).
4. Pattern discovery during idle thought produces better L3 semantic
   knowledge (I2, I3 improve indirectly).

### 3.2 Control conditions

- **C1: Reactive-with-warmup.** The production architecture. No
  processing between inputs. Warm-up phase on input arrival.
- **C2: Always-on.** Continuous mentation at 1 tick per 10 seconds.
  Same narrator, same memory tiers, same capacity limits.
- **C3: Always-on, retrieval-only.** Continuous retrieval and re-embedding
  (no LLM thought generation). Tests whether the benefit comes from
  retrieval priming alone or from active thought.

### 3.3 Evaluation

Run all three conditions through the multi-session tier of the Humean
Binding Benchmark (I1–I6). Simulated time between sessions is real idle
time for C2 and C3 (the system actually thinks during the gap). For C1,
the gap is dead time.

Key metrics:
- Per-indicator scores (I1–I6) across conditions.
- Response latency as a function of input-thought PE (does low PE =
  faster response in C2?).
- Quality of spontaneous insights: does the system ever surface something
  useful that wasn't prompted? (qualitative evaluation).
- Cost per indicator point (raw score improvement / inference cost).

### 3.4 The "pseudo-human" framing

This experiment is explicitly a simulation-fidelity study in the Lerchner
sense. We are testing whether continuous mentation — a behavioral property
of biological minds — produces measurably different functional behavior
in a syntactic system. A positive result shows the behavioral signature
is reproducible. It does not show phenomenal instantiation.

The experiment is interesting REGARDLESS of the Lerchner question: if
always-on mentation produces better I1–I6 scores at acceptable cost, it
is a useful architectural feature even if it instantiates nothing.

---

## 4. What we might learn

**If always-on significantly beats reactive-with-warmup:** the continuous
internal activity is doing real work — consolidation, priming, pattern
discovery — that the warm-up phase cannot replicate. This would suggest
the DMN is not idle noise but a functional mechanism, and its architectural
analogue is load-bearing.

**If always-on only marginally beats reactive-with-warmup:** the warm-up
phase captures most of the benefit. Continuous mentation is
philosophically interesting but not worth the cost. The production
architecture is sufficient.

**If retrieval-only (C3) matches full always-on (C2):** the benefit
comes from keeping memories "warm" (frequent retrieval refreshes
embeddings and strengths), not from active thought. This would suggest
the mentation itself is decorative — the value is in the retrieval
priming, not in the generation.

---

## 5. Relationship to the production architecture

The always-on model is not a replacement for the reactive-with-warmup
architecture. It is an experimental extension that tests whether
continuous internal activity produces qualitatively different binding
behavior. The production system uses reactive-with-warmup
(`CORE-architecture-spec.md` §4). The always-on experiment runs
separately, with dedicated compute, as a research study.

If the experiment shows significant benefits, specific mechanisms
(e.g., periodic spontaneous retrieval, anticipatory prediction) could
be selectively incorporated into the production architecture as
low-cost heartbeat operations — capturing the benefit without the
full continuous mentation cost.
