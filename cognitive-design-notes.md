# Cognitive Science → Architecture Design Notes

> **Status:** working notes. Maps experimental findings from GWT, AST, and PP
> to design decisions for the multi-tiered memory and salience gate. Derived
> from literature search May 2026.
>
> **Purpose:** Ground architectural choices in empirical mechanisms, not just
> theoretical frameworks. Each section: what the brain does (experiment) →
> what it implies (design principle) → how we might implement it.

---

## 1. GWT: The Ignition Mechanism

### What the experiments show

**Masking paradigms (Dehaene et al., 2001, 2006; Melloni et al., 2025):**

The transition from unconscious to conscious processing is *nonlinear and
all-or-none*. Dehaene's masking experiments demonstrate three distinct
processing states:

1. **Subliminal:** stimuli below threshold activate sensory cortices (visual,
   fusiform) but never reach prefrontal-parietal areas. They produce priming
   effects but are not reportable.
2. **Preconscious:** stimuli have sufficient strength but are blocked by
   current workspace occupancy (attentional blink). They *could* reach
   consciousness but don't because the workspace is occupied.
3. **Conscious:** stimuli trigger full fronto-parietal ignition — a surge of
   synchronized activity in prefrontal and parietal cortex, with top-down
   amplification back to sensory areas.

The critical finding: there is NO gradual transition. The system is either
ignited or not. Below threshold → no workspace access. Above threshold →
full broadcast. This is a phase transition, not a dimmer switch.

**The 2025 adversarial study (Melloni et al., Nature 2025):**
- 256 participants, fMRI + MEG + iEEG
- Confirmed: transient ignition following stimulus onset in 200-800ms window
- Confirmed: content-specific synchronization between frontal and early
  visual areas
- Challenged: expected ignition at stimulus *offset* did not occur
- Implication: ignition is triggered by onset of new information, not by
  maintenance — the workspace ignites to *receive*, not to *hold*

**The bifurcation mechanism (2025 computational model):**
- All-or-none cortical activity explained by hierarchical NMDA/AMPA gradient
- Fast AMPA receptors drive feedforward signal propagation
- Slow NMDA receptors in feedback pathways sustain ignited networks
- The ignition threshold is determined by this receptor gradient

**Attentional blink (Dux & Marois, 2007; Marti et al., 2012):**
- When the workspace is processing Target 1, Target 2 (arriving within
  200-600ms) is completely missed — not degraded, *missed*
- MEG shows: sensory processing of T2 is identical whether blinked or not
- The difference: late frontal activations (>350ms) are absent for blinked
  trials — the workspace literally cannot accept a second item
- This is a SERIAL bottleneck, not a degraded parallel process
- Working memory encoding is the gateway that creates the blink

### Design implications for the salience gate

**Implication 1: The workspace gate should be binary, not graded.**

The biological workspace doesn't partially admit information. It either
ignites fully (accepts, broadcasts, encodes) or doesn't engage at all.
Information below threshold is still processed by specialists (subliminal
priming = specialists doing their work) but never enters the integrated
workspace state.

*Design:* The narrator's integration step should have an explicit admission
threshold. Specialist outputs either enter the workspace or they don't.
A graded "relevance score" that partially includes information is not how
the biology works. The workspace should maintain a sharp boundary between
"integrated into this turn's state" and "available to specialists but not
broadcast."

**Implication 2: The workspace is serial — one integration event at a time.**

The attentional blink demonstrates that the workspace processes items
sequentially, not in parallel. While one item is being encoded into working
memory, nothing else can enter. This maps to a strict serialization:
narrator integrates one coherent state per turn, and during that integration
window, new inputs queue rather than blend.

*Design:* The workspace should not attempt to integrate multiple independent
updates simultaneously. One coherent integration per cycle. Specialist
outputs that arrive while the narrator is integrating should buffer (like
the preconscious state) and wait for the next cycle.

**Implication 3: Ignition is triggered by onset (novelty), not maintenance.**

Melloni et al.'s finding that ignition occurs at stimulus onset but not
offset suggests the workspace activates to *receive new information*, not
to maintain current state. Maintenance is a different, lower-energy process.

*Design:* The expensive narrator integration call should be triggered by
NEW inputs (user message, specialist completing, memory retrieval returning)
— not by idle maintenance. Between triggers, the workspace state persists
without active recomputation. The heartbeat can trigger maintenance
operations (consolidation, decay) without full workspace re-ignition.

**Implication 4: The threshold is modulated by top-down amplification.**

What determines whether a stimulus reaches ignition? Both bottom-up strength
(signal quality, prediction error magnitude) AND top-down amplification
(current goals, prior knowledge, precision weighting). A weak signal that
matches current goals can ignite; a strong signal irrelevant to current
goals may not.

*Design:* The admission threshold for the workspace should be a function of
BOTH the input's intrinsic salience (how surprising/strong it is) AND the
current workspace state's relevance signal (does it relate to what the
system is currently working on?). This is precision weighting: the workspace
gate is not purely bottom-up.

---

## 2. AST: The Attention Schema Emerges Under Uncertainty

### What the experiments show

**Graziano et al. (2021, PNAS) — Neural network agent study:**
- Deep Q-learning agent trained on visuospatial catch task
- Agent given access to an "attention schema" — a descriptive model of its
  own attention state
- Result: significantly better performance WITH schema than without
- Key finding: performance was greatly reduced when schema was DISABLED or
  UNAVAILABLE during learning — the schema provides profound benefits for
  attention control
- The schema is not just monitoring — it actively improves the quality of
  attention allocation

**Piefke et al. (2024) — Spontaneous schema emergence:**
- RL agents in ball-catching task with noisy environments
- Agents spontaneously developed an attention schema in their neural
  resources when attention tracking was UNCERTAIN and DIFFICULT
- The schema emerges naturally in learning systems where attention is both
  important and challenging to track
- It does NOT require hard-wiring — it is a learned structure

**Right temporoparietal junction (rTPJ) studies:**
- rTPJ damage impairs awareness of attention without impairing attention
  itself
- Patients can still attend to stimuli but cannot report or control their
  attention effectively
- This dissociation confirms AST's core prediction: attention and the
  model of attention are separable

### Design implications for the workspace-as-attention-schema

**Implication 5: The workspace state IS the attention schema — and this actively improves performance.**

Graziano's finding is that having a model of what you're attending to makes
your attention better. The workspace state in our architecture — a
compressed representation of what the system is currently processing — is
functioning as an attention schema. But the implication is stronger than
"the workspace enables metacognition." The implication is: the workspace
actively improves the QUALITY of specialist outputs because specialists
read from the workspace (broadcast) and therefore know what the system is
attending to, allowing them to coordinate.

*Design:* The workspace state should be explicitly structured to be
*readable as an attention model*. Not just "here's the current context" but
"here's what the system is currently attending to, what it considers
relevant, and what it's uncertain about." Specialists reading the workspace
should be able to derive from it: what matters right now, what's uncertain,
what's been established. This is the broadcast serving as attention schema.

**Implication 6: The schema should EMERGE, not be hardwired.**

Piefke et al.'s finding is that attention schemas emerge spontaneously when
tracking attention is difficult and important. They are not pre-specified
structures — they develop through learning pressure.

*Design:* The workspace format should not be a rigidly pre-defined schema
(fixed fields, fixed structure). It should be a representation that the
narrator learns to structure through training/experience. Early in a
deployment (cold start), the workspace state may be unstructured. Over time,
the narrator should develop increasingly refined structures for representing
its own attentional state. This argues against a hardcoded workspace JSON
schema and toward a more flexible representation that the narrator shapes.

*Tension with engineering:* A fully emergent schema is hard to interpret and
debug. Compromise: provide a minimal structural scaffold (current goal,
confidence level, active items) but allow the narrator to populate it
freely. The scaffold gives specialists something to parse; the free content
allows emergence.

**Implication 7: Attention works without the schema, but CONTROL degrades.**

rTPJ patients can attend but cannot control or report their attention.
Translated: if the workspace breaks, specialists still function (subliminal
processing continues) but the system loses the ability to coordinate,
prioritize, and self-monitor.

*Design:* The architecture should degrade gracefully when the workspace is
impaired (latency spike, narrator failure). Specialists should still produce
outputs. The user should still get responses. But the responses will lack
coordination — they'll be raw specialist outputs without integration.
Build fallback paths that bypass the workspace for latency-critical
situations.

---

## 3. PP: Prediction Error Gates Learning

### What the experiments show

**Prediction error and memory organization (eLife, 2024):**
- Small prediction errors → UPDATE existing memory (assimilation)
- Large prediction errors → ENCODE new distinct memory (accommodation)
- This is not just "surprising things are remembered better"
- It's a bifurcation: the SIZE of prediction error determines WHETHER to
  update an existing representation or create a new one
- Maps directly to Piaget's assimilation/accommodation distinction

**Dopamine and novelty-driven consolidation (2024):**
Two distinct dopaminergic novelty systems in the hippocampus:

1. **VTA-hippocampal (common novelty):** Responds to experiences with SOME
   similarity to past events. Promotes *semantic* memory formation through
   systems consolidation. "I've seen something like this before, but this
   version adds new information." → Strengthen the general pattern.
2. **LC-hippocampal (distinct novelty):** Responds to experiences MINIMALLY
   related to past events. Triggers strong initial *episodic* consolidation.
   "This is genuinely new." → Store it as a distinct episode.

*Critical finding:* Dopamine has paradoxical strength-dependent effects.
Enhances encoding of WEAK memories (novel things get a boost) but LIMITS
updating of STRONG memories (established knowledge resists modification
under the same dopamine signal).

**Synaptic tagging and capture (STC):**
- Novel experiences selectively enhance retention of *nearby* everyday
  memories (occurring before or after encoding)
- A surprising event doesn't just get remembered itself — it boosts
  consolidation of temporally adjacent mundane events
- Mechanism: dopamine-dependent protein synthesis triggered by novelty
  benefits any recently tagged synapse

### Design implications for the memory tiers and consolidation

**Implication 8: Prediction error size should determine UPDATE vs. NEW ENTRY.**

The brain doesn't just "remember surprising things more." It uses prediction
error magnitude as a routing signal: small PE → modify existing memory;
large PE → create new memory entry.

*Design:* The consolidation gate should compute prediction error relative to
existing L2/L3 content. If the PE is moderate (the new information is
related to but extends existing knowledge), the system should UPDATE the
existing L2/L3 entry. If the PE is large (genuinely novel, no close match
in existing memory), the system should CREATE a new entry. This is the
assimilation/accommodation bifurcation implemented as a routing decision.

This means the consolidation gate needs:
- A similarity search against existing L2/L3 (does this match something?)
- A prediction error computation (how different is it from the match?)
- A threshold determining update-vs-create

**Implication 9: Two novelty signals map to our two memory pathways.**

The VTA (common novelty → semantic consolidation) and LC (distinct novelty →
episodic consolidation) systems map to our L2/L3 distinction:

- **Common novelty** (extends existing patterns): consolidate into L3
  semantic store. Strengthen the general pattern. This is information that
  refines existing expertise.
- **Distinct novelty** (genuinely new, no existing pattern): encode into L2
  episodic store. Preserve as a specific episode. This is information that
  may later become a new pattern after enough instances.

The promotion path L2 → L3 is then: when enough distinct episodes in L2
share structure, extract the pattern into L3. This is systems consolidation:
hippocampus (L2 episodic) training neocortex (L3 semantic) through replay.

*Design:* The consolidation gate should route based on novelty TYPE:
- Information that extends existing L3 patterns → update L3 directly
- Information with no match in L3 → store in L2 as episodic
- When L2 accumulates multiple related episodes → extract pattern to L3

**Implication 10: Strong memories should resist modification.**

The paradoxical dopamine finding: strong memories are harder to update.
This is biologically implementing the conservative kernel (L4). Information
that has been consolidated strongly should resist modification even when
new information suggests a change.

*Design:* This validates the L4 conservative kernel design. But it also
suggests a softer version for L3: well-established semantic knowledge
(frequently retrieved, extensively consolidated) should have higher
modification threshold than recently-formed semantic entries. The "strength"
of a memory entry should modulate how much prediction error is required to
update it. Weak entries update easily; strong entries require substantial
contradicting evidence.

Implementation: each L3 entry carries a "consolidation strength" score that
increases with retrieval frequency and successful predictions. The update
threshold scales with strength. This naturally produces the conservative
kernel as a continuum (L4 = entries whose strength exceeds a hard threshold)
rather than a binary distinction.

**Implication 11: Novelty boosts consolidation of NEARBY events.**

The STC mechanism: a surprising event enhances consolidation of temporally
adjacent mundane events. The surprising event triggers protein synthesis
that benefits any recently tagged synapse.

*Design:* When the system encounters something genuinely novel (high PE),
it should boost consolidation of RECENT workspace states — not just the
novel event itself but the context surrounding it. This means: if the
current turn produces high surprise, look back at the last N turns'
workspace states and boost their consolidation probability too.

This captures: "When something important happens, the context in which it
happened becomes important too." A surprising bug report makes the
preceding code review discussion more worth remembering. A novel incident
makes the configuration changes from earlier that day more worth
consolidating.

---

## 4. The Hippocampo-Neocortical RAG Model (Spens & Burgess, 2024)

### What the model shows

This is the most directly architecturally relevant finding. Spens & Burgess
(bioRxiv 2024, updated 2026) model hippocampo-neocortical interaction as
**compressive retrieval-augmented generation**:

1. **Episodic encoding:** Hippocampus stores memories in COMPRESSED form —
   a gist vector (conceptual code from which neocortex can reconstruct) plus
   unpredictable details (high-perplexity phrases that the generative model
   cannot predict from the gist alone).

2. **Recall:** Retrieval-augmented generation. Compressed hippocampal trace
   retrieved into working memory → neocortical generative model reconstructs
   the full episode from the compressed cue. ALL recall is generation from
   compressed cues. Memory is not retrieval of stored recordings — it is
   reconstruction by a generative model prompted with compressed traces.

3. **Consolidation:** Hippocampal replay trains the neocortical generative
   model. Over time, neocortex can reconstruct memories without hippocampus.
   The hippocampal trace can then fade (graceful forgetting).

4. **Problem solving:** Hippocampus retrieves relevant compressed episodes
   into working memory (limited context). Neocortex generates solutions
   using these episodes as context. This IS RAG — but with compression
   and consolidation that standard RAG lacks.

5. **Schema distortion:** As consolidation proceeds, memories become more
   schema-consistent (gist preserved, details lost). This matches human
   memory distortion patterns.

### Direct mapping to our architecture

| Spens & Burgess | Our architecture |
|---|---|
| Hippocampus (compressed episodes) | L2 episodic store |
| Neocortex (generative model, semantic knowledge) | L3 semantic store + base LLM |
| Working memory / context | L0 workspace |
| Hippocampal replay → neocortical training | L2 → L3 consolidation |
| Gist vector + surprising details | Workspace compression output |
| RAG during problem solving | Memory retrieval into workspace |
| Context compression | Workspace capacity limit |
| Schema distortion over time | L3 generalizing from specific episodes |

### Design implications

**Implication 12: Memory storage should be COMPRESSED, not verbatim.**

The hippocampus does not store full recordings. It stores: gist (from which
the generative model can reconstruct) + surprising details (what the model
cannot predict from the gist alone). Everything else is discarded because
the generative model can fill it in.

*Design:* When the workspace consolidates into L2, it should store:
- A compressed representation of the episode (the "gist" — what was this
  interaction about? what was established? what changed?)
- Surprising details that the base LLM could not predict from the gist
  alone (unusual decisions, counterintuitive findings, specific values)
- NOT a verbatim transcript of the workspace state

The perplexity-based selection of "surprising details" is directly
implementable: prompt the LLM with the gist and ask what it predicts. Store
what it gets wrong. Discard what it gets right (because the LLM can
reconstruct that from the gist).

**Implication 13: Recall IS generation from compressed cues, not retrieval of stored content.**

In Spens & Burgess, ALL recall is RAG — the hippocampus provides a
compressed cue, and neocortex generates the full memory. The retrieved
content is always a reconstruction, never a recording.

*Design:* When the workspace retrieves from L2, it should not paste raw
stored content into the workspace. It should provide compressed cues that
the narrator then RECONSTRUCTS into full context. This means:
- L2 stores compressed representations
- Retrieval returns these compressed representations
- The narrator integrates them by generating (reconstructing) their
  relevance to the current context

This naturally produces schema distortion (which is a FEATURE for us — the
system's "memories" should be shaped by its current understanding, not
frozen recordings). It also means retrieval is cheap (compressed cues are
small) even if reconstruction is expensive (narrator must generate from
the cue).

**Implication 14: Consolidation = generative model getting better at predicting episodes.**

The L2 → L3 pathway is not "copy important things to a different store."
It is: replay L2 episodes to train the system's general knowledge until
the general knowledge can reconstruct the episode without the specific
trace. Then the specific trace can fade.

*Design:* L3 semantic knowledge should be evaluated by its ability to
PREDICT episode content. A well-consolidated L3 entry is one where, given
a situation similar to stored L2 episodes, the system can predict what
will happen (what the user will need, what the correct action is) without
retrieving the specific episode. When this prediction accuracy exceeds
threshold, the L2 episode has been successfully consolidated and can decay.

Metric for consolidation success: can the narrator, given only L3 semantic
knowledge and the current context, predict what the L2 episode would have
told it? If yes → episode is redundant, can decay. If no → episode still
carries unique information, retain.

---

## 5. PFC-Basal Ganglia Gating: RL-Learned Resource Allocation

### What the experiments show

**Adaptive chunking (eLife, 2024):**
- PFC and basal ganglia implement adaptive resource allocation in working
  memory through gating strategies ADJUSTED BY REINFORCEMENT LEARNING
- Dopaminergic signaling provides the dynamic range for striatal gating
- Chunking strategies are dynamically adapted based on task demands and RL
  signals
- Effective capacity is not fixed — it increases with better chunking

**Prefrontal scaling of reward prediction error (2025):**
- Behavioral adaptability scales with recruitment efficiency of dACC and
  dlPFC in translating reward prediction errors into action
- It's not encoding capacity that limits behavior — it's the efficiency
  of translating PE into appropriate action

**Thalamic arbitration (Nature Communications, 2025):**
- Mediodorsal thalamus arbitrates between model-free and model-based RL
  strategies across prefrontal-striatal networks
- Prefrontal transthalamic processing increases during shifts from stable
  rule use to model-based updating
- The system switches between "exploit known policy" and "update policy
  based on new evidence"

### Design implications for the RL consolidation policy

**Implication 15: The gating policy should be LEARNED, not hand-tuned.**

The brain's working memory gating (what enters, what's maintained, what's
flushed) is learned via dopaminergic RL. The gating strategies are adapted
to task demands — they are not fixed heuristics.

*Design:* The consolidation policy (what moves from L1 → L2 → L3, what
decays) should be learned from reward signals, not from hand-tuned rules.
This is already the plan in `foundations.md` §7 (RL-driven consolidation
with composite reward). The cognitive science strongly validates this choice
over heuristic approaches.

The reward signal should combine:
- Retrieval utility (was this memory actually used when retrieved?)
- Prediction accuracy (did having this memory improve task performance?)
- Storage cost (is this memory worth its persistence cost?)
- Coherence (does this memory fit with the system's broader model?)

**Implication 16: Capacity is adaptive — chunking increases effective capacity.**

Biological working memory is not a fixed 4±1 items. Effective capacity
increases through learned chunking — compressing multiple items into single
representations. The capacity limit is on chunks, not on raw items.

*Design:* The workspace capacity limit should not be a fixed token count.
It should be a limit on COMPRESSED representations, where the compression
quality improves with experience. An experienced agent in a familiar domain
can fit more into the workspace because its compressions are better (each
chunk encodes more information). This naturally produces the expertise
feedback loop: more knowledge → better compression → more fits in workspace
→ better integration → more knowledge.

Implementation: workspace capacity measured in semantic units, not tokens.
A single well-compressed representation of "the database migration pattern
we always use" takes one slot. An unfamiliar concept takes one slot per
constituent part. Effective capacity scales with expertise.

**Implication 17: The system should switch between exploit and update modes.**

The thalamic arbitration finding: the brain dynamically switches between
"follow existing policy" (model-free) and "update policy from new evidence"
(model-based). This switching is itself learned.

*Design:* The consolidation system should have two modes:
- **Exploit mode:** follow existing consolidation policy. Retrieve from
  L2/L3 based on established patterns. Cheap, fast, good for familiar
  territory.
- **Update mode:** triggered when prediction errors accumulate or a novel
  situation is detected. Invest more compute in workspace integration.
  Potentially revise the consolidation policy itself.

The trigger for mode-switching: accumulated prediction error exceeding
threshold. If the system is consistently surprised (retrievals aren't
helping, predictions are wrong), switch from exploit to update. This maps
to the thalamic signal that shifts from stable rule use to model-based
updating.

---

## 6. Synthesis: The Salience Gate as Integrated Mechanism

Pulling the implications together, the salience gate is a composite
mechanism with multiple sub-processes, each grounded in specific cognitive
science findings:

```
                    New inputs (specialist outputs, user message,
                    retrieved memories)
                            │
                            ▼
                ┌───────────────────────┐
                │  IGNITION THRESHOLD   │  (GWT: binary gate,
                │  (Implication 1, 4)   │   top-down modulated)
                │                       │
                │  Inputs above threshold → enter workspace
                │  Inputs below threshold → subliminal only
                └───────────┬───────────┘
                            │ (winners only)
                            ▼
                ┌───────────────────────┐
                │  WORKSPACE            │  (GWT: serial integration,
                │  INTEGRATION          │   AST: attention schema)
                │  (Impl 2, 5, 6, 16)  │
                │                       │
                │  Narrator compresses under capacity bound
                │  Output = attention schema (what matters now)
                │  Capacity scales with expertise (chunking)
                └───────────┬───────────┘
                            │
                            ▼
                ┌───────────────────────┐
                │  PREDICTION ERROR     │  (PP: PE computation
                │  COMPUTATION          │   against L2/L3)
                │  (Implication 8, 9)   │
                │                       │
                │  Compare workspace output to existing model
                │  Compute PE magnitude and type
                └───────────┬───────────┘
                            │
                    ┌───────┴───────┐
                    │               │
              Small/moderate PE  Large PE
                    │               │
                    ▼               ▼
            ┌─────────────┐  ┌─────────────┐
            │ UPDATE       │  │ CREATE       │  (PP: assimilation/
            │ existing L3  │  │ new L2 entry │   accommodation)
            │ (Impl 8, 9) │  │ (Impl 8, 9) │
            └─────────────┘  └─────────────┘
                    │               │
                    └───────┬───────┘
                            │
                            ▼
                ┌───────────────────────┐
                │  STRENGTH-MODULATED   │  (PP: strong memories
                │  UPDATE THRESHOLD     │   resist modification)
                │  (Implication 10)     │
                │                       │
                │  Strong entries need more PE to update
                │  Weak entries update easily
                │  L4 kernel = max strength (never updates)
                └───────────────────────┘
                            │
                            ▼
                ┌───────────────────────┐
                │  TEMPORAL BOOST       │  (PP: STC mechanism)
                │  (Implication 11)     │
                │                       │
                │  High novelty boosts consolidation
                │  of temporally adjacent events
                └───────────────────────┘
```

### The full cycle (one integration event):

1. Specialist outputs arrive. Ignition threshold determines which enter the
   workspace (binary, top-down modulated).
2. Narrator integrates winners under capacity bound. The workspace state
   IS the attention schema (readable by specialists, improves coordination).
   Effective capacity scales with expertise (chunking).
3. Workspace output compared to existing L2/L3 model. Prediction error
   computed.
4. PE magnitude routes consolidation: small PE → update existing knowledge;
   large PE → create new episode.
5. PE type routes destination: common novelty (extends pattern) → L3;
   distinct novelty (new pattern) → L2.
6. Strong memories resist update (conservative kernel as continuum).
7. High-novelty events boost consolidation of temporally adjacent content.
8. Over time, L2 episodes with shared structure consolidate into L3 patterns
   (Spens & Burgess: replay training the generative model).
9. Consolidation policy is RL-learned, not heuristic (PFC-BG gating).
10. System switches between exploit mode (use existing policy) and update
    mode (revise policy) based on accumulated PE.

---

## 7. Open Design Questions Raised by the Literature

1. **How to implement the binary ignition threshold computationally?**
   In biology it's an NMDA/AMPA receptor gradient producing a bifurcation.
   In our system, what produces the all-or-none gate? Options:
   - Hard relevance cutoff (specialist output relevance score > threshold)
   - The narrator itself decides (in-context evaluation of whether to admit)
   - A small classifier trained to predict workspace-entry decisions
   The narrator-decides option is most flexible but adds latency.

2. **What is the workspace serialization format if the schema should emerge?**
   Tension between Implication 6 (schema should emerge, not be hardwired)
   and engineering practicality (specialists need something parseable).
   Possible compromise: typed fields for essential metadata (current goal,
   confidence, timestamp) + a free-form "narrator's working state" field
   that can structure itself over time.

3. **How to compute prediction error against L2/L3 without full retrieval?**
   The PE computation (Implication 8) requires comparing new information
   against existing model. This implies a retrieval step BEFORE the
   consolidation decision. Possible: maintain a lightweight approximate
   model (embeddings, summary statistics) that can cheaply estimate
   whether new content is novel without full L2/L3 search.

4. **How to implement the "compressed gist + surprising details" storage?**
   Spens & Burgess use perplexity-based selection. We could:
   - Generate a summary (gist) of the workspace state
   - Prompt the LLM with the gist and measure what it can't predict
   - Store gist + unpredicted content
   This requires an extra LLM call at consolidation time. Worth the cost?

5. **How to implement the temporal boost (STC mechanism)?**
   When a high-PE event occurs, boost recent events. Options:
   - Maintain a rolling buffer of recent workspace states with timestamps
   - When high PE detected, retroactively flag recent buffer entries for
     consolidation
   - The heartbeat naturally provides the temporal window

6. **How to measure "consolidation strength" for the resistance threshold?**
   Candidates: retrieval count, successful prediction count, time since
   last modification, explicit human tagging. Likely: composite score
   weighted by retrieval utility (how often was this memory helpful when
   used?).

7. **What's the latency budget for all this?**
   The full cycle (ignition check → integration → PE computation →
   consolidation routing) must complete within the turn's latency window
   (GWT.md §1.4.4). Which steps can be deferred to idle heartbeat?
   - Ignition + integration: must be real-time (the response depends on it)
   - PE computation + routing: could be deferred IF we accept degraded
     consolidation quality (the workspace state that made PE legible will
     be gone by next turn)
   - Temporal boost: naturally deferred (retroactive)
   - L2 → L3 consolidation: naturally deferred (replay during idle)

---

## 8. Key References

### GWT / Ignition
- Dehaene, S. (2006). Conscious, preconscious, and subliminal processing.
  Trends in Cognitive Sciences.
- Dehaene, S., Sergent, C., & Changeux, J.-P. (2003). A neuronal model of
  a global workspace in effortful cognitive tasks. PNAS.
- Melloni, L., et al. (2025). Adversarial testing of GNW and IIT. Nature.
- Dynamic bifurcation model (2025). Cell Reports.
- Dux, P. E., & Marois, R. (2007). The attentional blink. Psychonomic
  Bulletin & Review.
- Marti, S., Sigman, M., & Dehaene, S. (2012). MEG study of AB and PRP.
  HAL Archives.

### AST
- Graziano, M. S. A., et al. (2021). The attention schema theory in a
  neural network agent. PNAS.
- Piefke, T., et al. (2024). Computational characterization of the role
  of an attention schema. arXiv:2402.01056.
- Webb, T. W., et al. (2021). Attention, awareness, and the rTPJ. PNAS.

### PP / Memory
- eLife (2024). Prediction error determines how memories are organized.
- Duszkiewicz, A. J., et al. (2019). Novelty and dopaminergic modulation
  of memory persistence. (Edinburgh)
- Hippocampal dopamine uncaging (eNeuro, 2024).
- Unlocking the Memory Vault: Dopamine, Novelty, and Memory Consolidation
  (Springer, 2024).

### Hippocampo-neocortical RAG
- Spens, E., & Burgess, N. (2024/2026). Hippocampo-neocortical interaction
  as compressive retrieval-augmented generation. bioRxiv.

### PFC-BG Gating
- Adaptive chunking in PFC-BG circuit (eLife, 2024).
- Thalamic regulation of RL strategies (Nature Communications, 2025).
- Prefrontal scaling of reward prediction error (arXiv:2512.09761, 2025).
- Neural mechanisms of resource allocation in working memory (Science
  Advances, 2025).
