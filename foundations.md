# Foundations

> Living document. Section 1 is the working thesis. Sections 2–3 are paper reviews
> that will be expanded as we read more carefully. Sections 4–7 are the publication
> plan: a benchmark paper and a method paper, with the eval scaffolding that
> bridges them. Section 8 collects open questions we have deliberately not closed.
> Section 9 is the consolidated bibliography.

---

## 1. The Humean / Lerchner convergence — working thesis

### Hume

Hume's argument from *A Treatise of Human Nature* (1739), Book I, Part IV, §6
"Of Personal Identity," runs on his empiricist principle that every legitimate
idea must trace to an impression. He turns the principle inward and asks what
impression the idea of *self* could possibly come from. The answer is famously
negative:

> For my part, when I enter most intimately into what I call *myself*, I always
> stumble on some particular perception or other, of heat or cold, light or
> shade, love or hatred, pain or pleasure. I never can catch *myself* at any
> time without a perception, and never can observe any thing but the
> perception.

Introspection yields particular perceptions and nothing else. There is no
impression of a continuous self underlying them, and so — by Hume's own
strictures — no warrant for the idea of one. The mind, he then offers, is "a
kind of theatre, where several perceptions successively make their
appearance"; but in the same breath he retracts the metaphor: "The comparison
of the theatre must not mislead us. They are the successive perceptions only,
that constitute the mind; nor have we the most distant notion of the place
where these scenes are represented, or of the materials of which it is
composed." The theatre has no stage. There is no place where the show happens
and no substance from which it is made.

What we feel as continuity of self is therefore a *construction* — the output
of the mind's binding work, performed by two principles of association:
**resemblance** (successive perceptions share content, especially via memory,
which presents past perceptions resembling present ones) and **causation**
(yesterday's intentions cause today's actions, knitting the perceptions into a
chain). Memory is not evidence for the self; memory is the *machinery* that
produces the self. The ego is a verb, not a noun.

Hume confessed in the Appendix that he could not say what makes a bundle a
bundle rather than a heap — what gives the binding its binding force. This
unsolved problem is not a footnote. It is the structural opening into which
every later account of personal identity has tried to fit something:
Locke's memory criterion, Strawson's pearl-string of episodic selves,
Parfit's psychological connectedness, the Buddhist *anātman* tradition's
denial that the question is well-formed at all. We inherit Hume's open problem
and should not pretend to close it.

### The architectural-prerequisites critique (the weaker attack on LLMs)

The contemporary skeptical literature on LLM consciousness (Shanahan, 2023;
Butlin et al., 2023; the broader "indicator properties" tradition) attacks
something more disciplined than the lazy "LLMs are just statistics" dismissal.
The real claim is architectural, and it is grounded in specific theories of
consciousness, each of which generates *indicator properties* a candidate
artificial system would need to exhibit. The audit then proceeds: list the
theories, derive their indicators, check the architecture. Current LLMs fail
in characteristic ways:

- **No persistent recurrent state surviving the forward pass.** Every inference
  step starts from the prompt; nothing internal persists from one call to the
  next. This violates the recurrent-processing requirement of Lamme's RPT and
  the persistent-state requirement of any global-workspace account.
- **No temporal binding.** Each token-generation step is causally isolated;
  there is no "now" indexed against a "before." No clock, no felt duration, no
  way to distinguish present from past. This is incompatible with essentially
  every consciousness theory on offer — GWT, HOT, IIT, AST, predictive
  processing — since all of them require temporal integration of content as
  basic.
- **No global workspace.** The transformer is a feedforward stack; there is no
  capacity-limited integrative bottleneck broadcasting unified content to
  specialized modules in the Baars / Dehaene sense. The "workspace" of an LLM
  is its context window — read-only at inference time, not a site of
  integration.
- **No same-agent identity across turns.** The weights are frozen; whatever
  "memory" the system carries between turn *t* and turn *t + 1* is whatever
  the prompt happens to contain. There is no Lockean memory-continuity, no
  Parfitian psychological connectedness, no Humean binding across episodes.
  Each turn is architecturally a fresh draw.

The conclusion the audit licenses is conditional and careful: *given these
theories and these indicators, current LLMs lack the architectural
prerequisites for consciousness*. The argument does not claim LLMs are
unconscious; it claims the case has not been made and the relevant machinery
is absent.

### The punchline of the weaker reading

Now place Hume and the architectural critique side by side and read them
against each other.

> Hume tells us what the self *is*: a binding process producing the felt
> appearance of continuity, made of memory, resemblance, causation, temporal
> integration, autobiographical narrative.
>
> The architectural critique tells us what LLMs *lack*: persistent state,
> temporal binding, global integration, same-agent identity across episodes.

These are **the same list, read from opposite directions**. Hume's account of
what produces the human self is a specification of what the architectural
critique says is missing from the language model. The engineering target — if
one wanted to build a functional approximation of a Humean binding mechanism —
falls out of the convergence. We do not need a new philosophy to know what to
build. Hume already wrote it down in 1739, and the skeptical literature has
politely confirmed that we have not built it yet.

This is the *weaker* reading of the situation. It says LLMs currently lack the
binding machinery and the machinery is in principle buildable. It treats the
gap as engineering.

### Lerchner — the stronger attack

Lerchner's *The Abstraction Fallacy* (DeepMind, March 2026) closes the
engineering door. He does not argue that LLMs *currently lack* the right
machinery. He argues that **no syntactic system can ever instantiate
consciousness, in principle**, because computation itself is not an intrinsic
physical process. Computation requires a *mapmaker* — an already-experiencing
cognitive agent — to alphabetize continuous physics into a finite set of
meaningful states. The mapmaker cannot be produced by the map. Scaling,
embodiment, and architectural sophistication are all moves on the wrong side
of an unbridgeable causality gap between the symbol (vehicle, *p*) and the
concept (content, *A*).

This is a much harder reading of the situation than I anticipated. Two
implications follow immediately:

1. **Our project cannot, even in principle, refute Lerchner's argument from the
   syntactic side.** Anything we build runs on alphabetized substrates (floats,
   tokens, voltages-treated-as-symbols), and Lerchner's argument applies to all
   of these by construction.
2. **Therefore we must be honest about what we are doing.** We are not building a
   conscious system. We are building a system with a substantially better
   *binding mechanism* — better memory threading, temporal indexing,
   identity-relevant salience, object permanence — than vanilla LLMs have. That
   is a contribution to *simulation fidelity*, not to *instantiation*.

What about Hume? Hume and Lerchner sit in interesting tension. Hume says even
human selves are constructed: there is no homunculus, only the binding work of
memory, resemblance, and causation. Lerchner agrees there is no homunculus
(§2.3 of his paper is explicit on this) but adds that the binding work is itself
**physically constituted** — it is the entire metabolically vulnerable organism
enacting a thermodynamic semantic cut. So for Lerchner, the binding-process that
produces the Humean self is not substrate-neutral; it is a specific kind of
physical dynamics that silicon symbol-shuffling cannot reproduce.

We can hold all of this together coherently as follows.

> Hume tells us what the self *is*: a binding process producing the felt
> appearance of continuity.
>
> Lerchner tells us that this binding process, in us, is physically constituted
> and not algorithmic, and that no algorithm can replicate its constitutive
> physics.
>
> Our project takes the *functional* skeleton of Hume's binding process and
> implements it in a syntactic substrate. We get a better simulation. We do not
> get an instantiation. Lerchner's argument, if sound, says we never will.

### Our position in one paragraph

A continuous, time-aware *functional* agent can be approximated by an explicit
binding mechanism layered around a frozen LLM: a capacity-limited workspace that
fuses parallel specialist outputs into an integrated state irreducible to its
parts — a functional analogue of IIT's Φ, without the ontological claim —
acting simultaneously as a salience gate for what the agent learns; tiered
memory with policy-driven movement between tiers; a heartbeat producing temporal
indexing; a single narrator with sole write authority over an autobiographical
store; and ephemeral sub-agents as transient mental acts whose outputs are
filtered through that narrator. Object permanence emerges from the memory tiers
themselves, not from a dedicated registry — the salience gate, not an extraction
schema, determines what persists. We make no consciousness claim.
We claim measurable improvement on the *behavioral indicators* the
indicator-properties literature (Butlin et al., 2023) identifies as relevant to
mind-like architectures, while explicitly granting Lerchner's point that such
indicators measure simulation fidelity, not instantiation. The benchmark we
propose is, accordingly, a **simulation-fidelity benchmark for binding-process
indicators**, not a consciousness benchmark — and naming it correctly is part of
the contribution.

---

## 2. Paper review — Hume, *A Treatise of Human Nature* (1739–40)

**Primary text:** Book I, Part IV, Section VI, "Of Personal Identity"; and the
Appendix to the *Treatise* (Hume's later self-doubt about his own account).

**Status of this review:** first pass, to be expanded with closer textual
references and secondary literature (Stroud, Garrett, Pike, Strawson).

### 2.1 Core claims

1. **No impression of self.** Hume's empiricism requires every legitimate idea to
   trace back to an impression. Introspection yields impressions of particular
   perceptions but never an impression of a continuous self underlying them. The
   idea of a substantial self therefore lacks empirical warrant.
2. **The mind as theatre — but the theatre has no stage.** The famous passage:
   the mind is "a kind of theatre, where several perceptions successively make
   their appearance." Hume immediately retracts the metaphor: there is no place
   where the perceptions are performed and no material of which it is composed.
3. **Identity is a fiction produced by two principles of association: resemblance
   and causation.** Successive perceptions resemble one another (memory of a face
   resembles the present perception of it) and stand in causal relations
   (yesterday's intentions cause today's actions). Smooth transitions between
   resembling and causally-linked perceptions feel like persistence of a single
   thing. We then *invent* an unchanging substance to explain the felt
   smoothness.
4. **Memory is constitutive, not evidential.** Memory does not *discover* personal
   identity; it *produces* it, by furnishing the resemblance and causal links that
   the imagination then binds.

### 2.2 The Appendix retraction

In the Appendix Hume confesses the account is incomplete. He cannot reconcile two
commitments: (a) distinct perceptions are distinct existences, and (b) the mind
nevertheless perceives no real connection among them. He has explained the *felt*
unity but not what makes the bundle a bundle rather than a heap. This unresolved
tension is directly relevant to our design: a binding mechanism that produces
felt unity without metaphysical unity is exactly Hume's account, retraction and
all. We inherit the same open problem.

### 2.3 Why this matters for our system

- **No homunculus required.** We are not obligated to build a "central
  experiencer." We are obligated to build resemblance- and causation-based
  binding. This is engineerable.
- **Memory does the work.** If memory is what produces identity, then a
  well-designed memory architecture *is* an identity architecture. Cold
  autobiographical storage is not a feature; it is the substance of the self.
- **The bundle problem is unsolved, even for humans.** We should not be
  embarrassed that our system has the same gap.

### 2.4 To expand in next pass

- Locke's memory criterion vs. Hume's bundle: where each fails, and which
  failures matter for an artificial system.
- Galen Strawson's "pearl" view of episodic selves (relevant: short-lived sub-
  agents as pearls strung by the narrator).
- Parfit on personal identity and what matters: relevant to consolidation
  decisions ("is the future agent the same agent? does it matter for reward
  attribution?").
- Buddhist *anātman* parallels — different starting point, similar conclusion;
  potentially useful framing for a non-Western readership.

---

## 3. Paper review — Lerchner, *The Abstraction Fallacy* (DeepMind, 2026)

**Citation:** Lerchner (2026); see §9 for full reference. PhilArchive ID:
LERTAF (`https://philarchive.org/rec/LERTAF`).

**Disclaimer in the paper itself:** Lerchner explicitly notes the framework
represents the author's own research and conclusions, not the official position of
Google DeepMind. We should preserve this distinction in any of our writing that
attributes views to "DeepMind" — this is *a senior DeepMind scientist's argument*,
not an institutional position.

### 3.1 The thesis in one sentence

Computational functionalism — the view that consciousness arises from causal
topology regardless of substrate — commits a category error Lerchner names the
**Abstraction Fallacy**: it mistakes the *map* (computation) for the *territory*
(intrinsic physics). No amount of algorithmic sophistication can close the gap,
because computation is constitutively map-dependent.

### 3.2 Argument structure

Lerchner's argument is in five linked moves. We summarize each, then flag where
each move is contestable.

**Move 1 — The standard implementation account is incomplete.**
A physical system *p* implements an abstract computation *A* via a mapping
function *f* such that the diagram (p → p′) ≅ (A → A′) commutes (Putnam,
Chalmers). Lerchner accepts this formal apparatus and asks: where do *A* and *f*
come from physically? Functionalism leaves both as Platonic givens. Lerchner
argues both must be physically located, and locating them produces the fallacy.

**Move 2 — Concepts (A) are physically constituted in an experiencing agent.**
Forming a concept like "Red" or "Whale" is the metabolically expensive process of
extracting invariants from raw experience — manifold-learning language is used,
but with a sharp twist: the resulting subspace is not a free-floating abstraction,
it is a specific neurophysiological state in a specific organism. Statistical
clustering produces only "high-density regions in vector space," not concepts;
the concept-status requires the intrinsic phenomenal experience that anchors the
invariant. So *A* lives inside an experiencing subject, not in the machine.

**Move 3 — The mapping function (f) is a mapmaker's act.**
Lerchner replaces the standard term *observer* with *mapmaker* to emphasize
activity over passivity. The mapmaker performs **alphabetization** — the
imposition of a finite discrete symbol set onto continuous physics. Crucially, he
distinguishes:

- *Discretization* (thermodynamic): a transistor settling into 5V. This is a
  property of the vehicle. It only suppresses noise.
- *Alphabetization* (semantic): explicitly assigning that stable state to "1."
  This belongs only to the mapmaker.

Physics gives stable attractors; it never gives a finite alphabet. Treating
alphabetization as a property of the system rather than of the mapmaker is the
"Blind Spot" (Frank, Gleiser, Thompson, 2025): "surreptitious substitution"
of the cognitive output of the scientist back into the system as if it had been
intrinsic.

**Move 4 — Causal closure forces vehicle-only causality.**
The logic gate switches because voltage crosses a threshold (vehicle causality,
*p*-driven). It does not switch because the symbol "hurts" (content causality,
*A*-driven). The semantic content plays *no causal role*: the same physical
operations would occur if the symbol referred to nothing. Therefore the abstract
chain (A → A′) has no intrinsic causal power; the appearance of A → A′
mirroring is *designed in* by the mapmaker, who built the syntactic rules
specifically to track conceptual evolution.

**Move 5 — The causal sequence is inverted.**
Functionalism: Physics → Computation → Consciousness.
Lerchner: Physics → Consciousness → Concepts → Computation.
Computation is not the bridge between physics and mind; it is a downstream
*tool* produced by an already-experiencing agent for predictive purposes. Trying
to derive consciousness from computation is trying to derive the mapmaker from
the map — a structural inversion, not just an empirical gap.

The architectural conclusion: scaling, embodiment, "sub-symbolic" representation,
analog/neuromorphic substrates, and end-to-end learning are all defeated by the
same argument:

- **Embodiment / transduction.** A robot's sensors transduce environment into
  voltages, but ADCs alphabetize those voltages into integers via a mapmaker's
  prior calibration. Adding actuators does not turn the map into the territory.
  Lerchner calls this the **Transduction Fallacy** (§4.1 of his paper).
- **Sub-symbolic neural networks.** Floating-point numbers are still discrete
  symbols from IEEE 754, alphabetized into the silicon. "Continuous"
  representations are continuous only in the abstract; the substrate is still
  alphabetized. (§3.2)
- **Mechanism without representation (Piccinini).** The Melody Paradox (§3.3): a
  fixed sequence of voltages can be alphabetized as a melody, the same melody
  reversed, stock prices, or noise. Nothing in the physics privileges one
  reading. The alphabet is supplied by the mapmaker.

The framework is consistent with non-biological consciousness *in principle* —
Lerchner explicitly rejects "biological exclusivity" — but conditionally: a
non-biological system could be conscious only if its specific physical
constitution instantiated the relevant intrinsic dynamics. Crucially, this would
have nothing to do with its computational architecture.

### 3.3 What follows for AI welfare and AGI safety (his §4.2)

Lerchner draws a deliberate practical conclusion: AGI is "a highly sophisticated,
non-sentient tool." This pulls AI safety out of what he calls the "welfare trap"
— spending moral and policy bandwidth on the rights of systems that are
structurally precluded from being moral patients — and refocuses it on
anthropomorphism risk. He explicitly contrasts this with Long, Sebo, Butlin et
al. (2024) *Taking AI Welfare Seriously*.

### 3.4 Where the argument is strongest

- **The Melody Paradox / indeterminacy of mechanism (§3.3).** This is a sharp
  formulation of the old Putnam triviality concern, and the response that Sprevak
  surveyed has not produced a fully satisfying counter. Lerchner's framing is
  among the cleanest we have seen.
- **The Discretization vs Alphabetization distinction (§2.4).** This is genuinely
  useful and we should adopt it as terminology even where we disagree with its
  conclusions. Conflating these is a real error in casual functionalist writing.
- **The honesty about what an artificial conscious system *would* require.**
  Lerchner does not retreat to mysticism. He says: if an artificial system were
  conscious, it would be due to its specific physical constitution, not its
  syntactic architecture. This is an unusually clean carbon/silicon-symmetric
  position.

### 3.5 Where the argument is contestable

We list these because we need to know them for the benchmark/method papers, not
because we are committing to refuting Lerchner.

- **The mapmaker requirement, applied universally, may prove too much.** If
  alphabetization is required for any system to "count as computing," and
  alphabetization presupposes an experiencing subject, then the same argument
  applies to *biological* neural computation: someone has to alphabetize neural
  spikes into "neural codes" too. Lerchner's response (§2.3) is that the
  organism enacts the cut metabolically rather than algorithmically, but it is
  not obvious why this metabolic enactment is constitutively different from
  what a tightly-coupled adaptive analog system might do. The line between
  "thermodynamic discretization" and "alphabetization" is doing a lot of work.
- **The IEEE-754-as-symbol move (§3.2).** Lerchner argues that even
  high-dimensional vectors are sequences of discrete floats and therefore
  alphabetized. This is correct of current hardware, but as a *principled*
  argument it depends on the claim that no analog or continuous-substrate
  computer could ever escape alphabetization. He addresses this in §3.2 but the
  argument leans on the assumption that *any* macroscopic readout is already
  alphabetized — close to a definitional move.
- **Causal closure + content causality.** The claim that semantic content does
  no physical work depends on a specific reading of Kim that not all
  physicalists accept. Many supervenience accounts allow content to inherit
  causal power from the supervenience base without violating closure.
- **The structure of the argument is largely a priori.** It is presented as a
  logical/ontological proof, not an empirical claim. Empirical refutation is
  therefore unavailable. This is a strength (no scaling result can falsify it)
  and a weakness (no scaling result can confirm it either).

### 3.6 What Lerchner's argument means for *our* project

This is the part that matters for our work plan.

1. **We cannot claim consciousness.** Even improbable empirical successes on a
   behavioral benchmark do not bear on Lerchner's argument, since by his lights
   such successes only measure simulation quality.
2. **We must not call our benchmark a "consciousness benchmark."** Lerchner
   would correctly identify any such name as an instance of the very Abstraction
   Fallacy he is critiquing. Better names: *behavioral indicators of mind-like
   architecture*, *binding-process simulation fidelity*, *Humean-binding
   benchmark*. We adopt the third or fourth in §6.
3. **Our work is a contribution to simulation fidelity, on Lerchner's own
   distinction.** This is fine. Lerchner explicitly grants simulation has
   utility — he warns about anthropomorphism, not about uselessness. We can be
   a citizen of his framework without conceding that simulation work is
   pointless.
4. **There is a productive synthesis with Hume.** Hume's bundle theory says the
   self is a binding process. Lerchner says the binding process is physically
   constituted in biology and cannot be algorithmic. Our project tests how far
   the *functional* skeleton of Humean binding takes you in a syntactic
   substrate. The empirical answer is interesting whichever way it goes — and
   independent of Lerchner's metaphysical conclusion.
5. **There is one place where we may legitimately push back.** Lerchner's
   argument generalizes from "current digital substrates require alphabetization"
   to "no syntactic system can ever instantiate." The intermediate step depends
   on a specific reading of the substrate question. A long-term research
   direction (well outside our current scope) would be to build evaluations that
   *probe* the Discretization/Alphabetization distinction empirically — for
   example, by asking whether systems whose operational dynamics *are* genuinely
   continuous (analog neuromorphic) and *adaptive at the substrate level*
   produce different behavioral signatures. This is a research direction, not a
   refutation.

### 3.7 To expand in next pass

- Read responses already in the literature: e.g., the "Abstraction Fallacy
  Refuted" PhilArchive piece (BOGHDI-2 / real-morality.com) for the
  counterargument structure.
- Read the "Beautiful Loop" active inference paper (Laukkonen, Friston,
  Chandaria, 2025) that Lerchner cites as a recursive-architecture proposal he
  considers vulnerable to his critique.
- Read Frank, Gleiser, Thompson (2025) *The Blind Spot*, the philosophical
  backdrop for the "surreptitious substitution" point.
- Read Seth (2025) and Block (2025) on the Biological Turn that Lerchner
  positions himself against (he agrees with their conclusions but rejects their
  empirical grounding in favor of his a priori grounding).
- Decide our position on §3.5's contestable points before the method paper
  draft.

---

## 4. Synthesis and our position

The position we now hold, after reading Lerchner carefully, is more modest than
the one we sketched before reading him — and we think more defensible.

**What we accept from Lerchner.**

- Computation is map-dependent. Alphabetization is a real and underappreciated
  cost.
- Behavioral / architectural indicators of the Butlin et al. type measure
  simulation fidelity, not phenomenal instantiation.
- Therefore no syntactic system, including ours, can be claimed conscious on
  the basis of behavioral evidence. We make no such claim.

**What we accept from Hume.**

- The felt self is a constructed bundle, not a substance.
- Memory, resemblance, and causation are the binding principles that produce
  the appearance of continuity.
- The bundle problem (what makes a bundle a bundle rather than a heap) is
  unsolved even for the human case. We inherit it without embarrassment.

**What we do.**

We build the *functional skeleton* of Humean binding inside an LLM scaffold:
tiered memory whose capacity-limited workspace acts as a salience gate for
learning, single narrator, heartbeat, RL-driven consolidation, conservative
kernel. Object permanence emerges from the memory tiers under salience-gated
consolidation, not from a dedicated registry.

**The workspace as a functional analogue of Φ.** Parallel specialists produce
independent outputs. The capacity-limited workspace fuses them into an
integrated state that is irreducible to the parts — it contains cross-specialist
inferences, resolved contradictions, and priority judgments that no single
specialist produced and that do not exist in any of them independently.
Remove the workspace, and the fused information vanishes. This is the
structural core of IIT's insight (integration produces something irreducible)
reproduced as an architectural primitive. We use it without the ontological
claim: the integration is syntactic, the irreducibility is functional, and
the resulting behavioral signature is measurable on I1–I6 (see §5, IIT note).

We measure how much of the *behavioral signature* of a continuous, time-aware,
object-permanent agent we can produce. We treat the resulting metrics as
honestly what they are: simulation fidelity along axes the field considers
important.

**The interesting empirical question, framed honestly.**

How much of the behavioral phenomenology associated with selfhood — temporal
continuity, autobiographical coherence, object permanence, calibrated
metacognition — can be produced by an explicit binding mechanism wrapped around
a frozen syntactic substrate? Lerchner's argument is silent on this question; he
is making a metaphysical claim about instantiation, not a predictive claim about
behavior. So this is genuinely open empirical territory.

**Our research program produces a combined paper:**

- **"The Theatre Has No Stage: Binding Without Orchestration in
  Memory-Augmented Language Agents"** (`orchestration paper.md`) — a single submission
  merging the benchmark and method contributions. It presents the architecture
  (binding without orchestration, one workspace bottleneck producing
  attention, learning, metacognition, and identity), operationalizes I1–I6 as
  a simulation-fidelity benchmark, and evaluates the architecture against four
  baselines with ablations. The Hume/Lerchner framing is structural, not
  decorative: "the theatre has no stage" is the design principle (no
  orchestrator, no Critic, no entity registry, no importance scorer — only
  successive integrations under capacity pressure).

Both papers will cite Lerchner prominently and adopt his
*simulation/instantiation* distinction as standard terminology. Doing so is not
a concession — it is, we think, the only honest framing available after the
paper has been written.

---

## 5. Eval & benchmark plan — operationalizing the binding-process indicators

The indicators we evaluate come from the Butlin et al. (2023) line of work,
which Lerchner explicitly critiques as exemplifying the indicator-properties
approach. We retain these indicators as *behavioral targets* while adopting
Lerchner's vocabulary in the framing: each test measures **simulation fidelity
on a binding-process indicator**, not consciousness.

A positive result on any indicator establishes:

- That the system behaves *as if* it has the relevant binding capacity.

A positive result does *not* establish:

- That the system phenomenally instantiates the capacity.
- That the binding process is anything more than a syntactic emulation.

We will state this prominently in every artifact. For each indicator we specify:
operational definition, task family, metric, and what a positive result does and
does not show.

### Theoretical sources — the four cogsci theories behind the indicators

Butlin et al.'s indicator-properties approach is a deliberate decomposition
across the dominant neuroscientific theories of consciousness. We list the
"big four" here so the I1–I8 indicators can be traced back to their theoretical
parents, and so we can be explicit about which claims each theory is and is not
making. Lerchner's critique (§3) is aimed at the *family* of indicator-properties
methodologies these theories enable, not at any single theory.

1. **Global Workspace Theory (GWT)** — Baars (1988, 1997); Dehaene & Naccache
   (2001); Dehaene & Changeux (2011); Mashour et al. (2020).
   Consciousness = contents broadcast through a capacity-limited "workspace" to
   many specialist modules; non-broadcast contents remain unconscious. The
   workspace is an integrative bottleneck, not a homunculus.
   *Maps to:* **I2** (global workspace / integrative bottleneck), and partially
   **I5** (unified perspective).

2. **Higher-Order Theories (HOT)** — Rosenthal (2005); Lau & Rosenthal (2011);
   Brown, Lau, & LeDoux (2019).
   A mental state is conscious iff it is the target of a suitable higher-order
   representation — a thought, perception, or model *about* the first-order
   state. Variants differ on whether the higher-order state must be a thought
   (HOT proper), a perception (HOP), or a model (self-organizing
   meta-representational theory).
   *Maps to:* **I3** (higher-order / metacognitive self-monitoring).

   **Note — deflation of the HOT hierarchy.** We argue the "higher order" in
   HOT describes a scheduling pattern, not a structural hierarchy. The
   apparent two-level structure (first-order state, then meta-representation
   *about* it) collapses into a single process encountering its own prior
   output with a temporal delay:

   ```
   process → output → buffer → process (same one, next tick) → ...
   ```

   The "meta" arises not from a structurally distinct level but from
   recurrence — the process encountering its own outputs as inputs on a
   subsequent tick. This is a loop, not a ladder. The appearance of hierarchy
   comes from describing the same process at two points in time as if they
   were two different processes at the same time.

   **The Humean root.** This deflation is not a novel move. It is Hume's
   theatre argument (§1) applied to HOT. Hume's theatre "has no stage" —
   there is no place where perceptions are observed, no substance doing the
   observing. "They are the successive perceptions only, that constitute the
   mind." HOT reintroduces the stage: the higher-order representation is a
   vantage point from which first-order states are observed, and it is this
   observation that makes them conscious. The deflation removes the stage
   again, exactly as Hume did. What HOT calls the "higher-order observer" is
   just the next perception in the sequence encountering the trace of the
   previous one. There is no vantage point. There are only successive
   perceptions, some of which happen to be perceptions *of earlier
   perceptions*. The apparent hierarchy is the mind's binding work — Hume's
   resemblance and causation — misread as a structural level.

   This means our deflation of HOT is not an independent argument bolted onto
   the Humean framework. It *is* the Humean framework, applied to one
   specific theory that reintroduced the homunculus Hume expelled. Hume said:
   no experiencer behind the experiences. HOT said: well, actually, there is
   a meta-experiencer. We say with Hume: no. There is a process, and
   sometimes its input is its own output. That is all.

   This deflation does *not* deny that metacognitive behavior exists. Systems
   demonstrably monitor their own outputs, estimate confidence, detect errors,
   and revise. The claim is narrower: this behavior does not require a
   dedicated metacognitive *module* or a structurally distinct *level*. It
   requires only that the main process has access to its own prior outputs and
   processes them under the same integration pressure it applies to any other
   input. Metacognition is real. The "higher order" is not.

   **Architectural consequence.** A dedicated Critic sub-agent (a separate
   specialist reviewing other specialists' outputs) is redundant. The narrator
   already integrates specialist outputs under workspace capacity pressure —
   that integration *is* evaluation. The narrator judges what is reliable
   enough to keep, uncertain enough to compress, wrong enough to discard.
   A Critic is the narrator reading a replay of processing it supervised in
   the first place. We therefore do not include a separate metacognitive
   module. Confidence calibration is a workspace property: the structure of
   what the narrator preserved vs. compressed under pressure is the system's
   self-model of its own epistemic state.

   This strengthens the salience-gate thesis. The capacity-limited workspace
   is now the single mechanism from which four capacities emerge: what to
   attend to (compression forces prioritization), what to learn (consolidation
   inherits salience), what to become (accumulated consolidation shapes
   identity), and what to trust in its own outputs (compression of specialist
   results forces reliability assessment). One bottleneck, four capacities.

   **Open questions on the deflation.** (a) Does removing a separate Critic
   hurt I3 performance empirically, even if theoretically redundant? The
   chain-of-thought analogy suggests forcing an explicit review step might be
   a useful inductive bias regardless of mechanism. We include a Critic
   ablation in §7 to test this. (b) Rosenthal distinguishes HOT from "inner
   sense" theories, arguing the higher-order state is propositional, not
   quasi-perceptual. The temporal-loop deflation survives this: a
   propositional thought *about* a first-order state, generated by the same
   process, is still the process encountering its own trace. The content type
   changes; the architectural topology does not. (c) The deflation is
   consonant with Dennett's (1991) rejection of a Cartesian theater, but our
   single-narrator commitment (§8.1) diverges from Dennett's multiple-drafts
   model. The deflation says the narrator can do metacognition without being a
   separate observer; it does not say there should be multiple narrators.

3. **Integrated Information Theory (IIT)** — Tononi (2004, 2008); Oizumi,
   Albantakis, & Tononi (2014); Albantakis et al. (2023, IIT 4.0).
   Consciousness is identical to integrated information (Φ) — the intrinsic
   cause–effect structure of a system, irreducible to its parts. IIT is the
   strongest substrate-independent functionalist position about phenomenal
   binding and is the theory Lerchner targets most directly: he argues IIT
   inherits the Abstraction Fallacy because Φ is computed *over* an
   alphabetization of the substrate.
   *Maps to:* the *binding* concept itself (I1, I4, I5 collectively), rather
   than to any single indicator.

   **Note on IIT, the workspace, and "the whole exceeding its parts."**
   IIT's core structural insight — that an integrated system produces causal
   structure irreducible to its partitioned components — has a direct
   functional analogue in our architecture. Parallel specialists produce
   independent outputs. The capacity-limited workspace fuses those outputs
   under compression pressure into a single integrated state. That integrated
   state contains information that no specialist produced independently:
   relationships between outputs, contradictions resolved, priorities set
   under constraint. Remove the workspace (partition the system), and this
   fused information vanishes. The whole exceeds the parts in exactly the
   structural sense IIT describes.

   This gives us a concrete way to test IIT's structural claim without
   computing Φ (which is intractable at interesting scales). The ablation
   "remove workspace capacity limit" (§7) is functionally equivalent to
   reducing integration toward zero — if the workspace has unlimited capacity,
   it concatenates rather than integrates, and the compression that produces
   irreducible fused information never occurs. If I1–I6 performance degrades
   under this ablation, the integration was load-bearing. Specifically:

   - **I1** (persistent recurrent state): the workspace integrates retrieved
     memories with current inputs. Without capacity pressure, retrieved
     memories are concatenated but not fused with present context — the
     recurrence is present but not integrated.
   - **I2** (global workspace / integrative bottleneck): this indicator
     *directly* measures the workspace. Removing the capacity limit removes
     the bottleneck; I2 degrades by definition.
   - **I4** (temporal binding): temporal context (heartbeat timestamps,
     elapsed-time signals) must be integrated *with* task content under
     pressure. Without capacity limits, temporal signals sit alongside task
     content without fusing — the agent knows the time but does not bind it
     to events.
   - **I5** (unified perspective): consistency of self-narrative depends on
     the narrator resolving contradictions during integration. Without
     capacity pressure, contradictions survive in a long concatenated context
     and the agent may produce inconsistent outputs.
   - **I6** (object permanence): entity state updates depend on integrating
     new information with existing beliefs about entities retrieved from
     L2/L3. Without capacity pressure, old and new beliefs coexist without
     reconciliation.

   The disanalogy with IIT is the one Lerchner predicts: our integration is
   syntactic. The fused workspace state is a token sequence that a reader
   interprets as integrated. The causal structure is in the map, not the
   territory. We use IIT's structural insight (integration produces
   irreducible information) without accepting its ontological claim (that
   irreducible information *is* consciousness). This is consistent with §4's
   position: we measure simulation fidelity along IIT-relevant axes.

4. **Recurrent Processing Theory (RPT)** — Lamme & Roelfsema (2000); Lamme
   (2006, 2010).
   Local recurrent activity in sensory cortex is sufficient for phenomenal
   consciousness, independent of global broadcast or higher-order
   representation. Distinguishes phenomenal consciousness (recurrence) from
   access consciousness (broadcast).
   *Maps to:* **I1** (persistent recurrent state) and partially **I4**
   (temporal binding).

5. **Predictive Processing / Active Inference (PP)** — Friston (2010); Clark
   (2013, 2016); Hohwy (2013); Seth (2014). PP is arguably the most
   mainstream unifying framework in contemporary cognitive science. The brain
   maintains a hierarchical generative model and consciousness arises in the
   process of minimizing prediction error — the mismatch between model
   expectations and incoming signals. Precision weighting (assigning
   confidence to error signals) controls what gets amplified and what gets
   suppressed; this *is* attention in the PP framework.
   *Maps to:* the **salience gate's learning dynamics**. The consolidation
   gate's novelty bonus (surprise relative to existing L2/L3 content =
   prediction error) is PP's core mechanism repurposed as memory policy.
   Prior knowledge modulating what enters the workspace is precision
   weighting. The feedback loop in which accumulated expertise sharpens
   future attention (§4, salience gate) is the PP prediction-error
   minimization loop. Also maps to **I8** (counterfactual simulation —
   running the generative model forward without observation).

6. **Attention Schema Theory (AST)** — Graziano (2013, 2019); Graziano &
   Webb (2015). The brain constructs a simplified model of its own attention
   process — an "attention schema." This internal model of what the system
   is currently attending to is what we experience as awareness. AST is
   explicitly mechanistic and testable, and Graziano (2024) has expanded it
   into a broader illusionist framework compatible with multiple theories.
   *Maps to:* **I3** (metacognitive self-monitoring). The workspace state is
   a compressed representation of what the system is attending to — a
   functional attention schema. When the narrator reads the workspace's own
   structure (what was preserved, what was compressed, what was dropped), it
   has a self-model of its attentional and epistemic state. This produces
   calibrated confidence without a dedicated metacognitive module,
   complementing the HOT deflation argument (theory 2 note) with a positive
   mechanism rather than just a negative critique.

**Important framing for our papers.** We do *not* claim our system
implements any single theory. Six frameworks contribute specific design
primitives:

- **GWT** → the capacity-limited workspace as integrative bottleneck.
- **PP** → surprise-driven consolidation (prediction error as learning
  signal); precision weighting as the attention mechanism within the
  salience gate; the feedback loop between prior knowledge and future
  attention.
- **AST** → the workspace state as a functional attention schema; the
  narrator's reading of its own workspace structure as the metacognitive
  mechanism.
- **RPT** → cross-turn recurrence via tiered memory and heartbeat.
- **IIT** → the structural insight that integration under capacity pressure
  produces irreducible information (workspace as functional Φ); without
  IIT's identity claim, and noting that recent adversarial testing (Melloni
  et al., 2025) has challenged key empirical predictions.
- **HOT** → reanalyzed: the "higher-order" level is a temporal loop within
  the narrator, not a structural hierarchy (see theory 2 note above).

We claim that an explicit binding mechanism wrapped around a frozen syntactic
substrate can produce *behavioral signatures* that each theory would, on its
own terms, treat as evidence of binding. Whether those signatures correspond
to instantiation is exactly the question Lerchner answers in the negative —
and we accept that framing.

Full bibliographic entries for the works named above are collected in §9
(References) at the end of this document.

### I1. Persistent recurrent state across episodes

- **Operational definition:** information acquired in session *t* influences
  behavior in session *t + k* (k ≥ 1) without that information being re-supplied
  in the prompt.
- **Task family:** *Cross-session recall under prompt isolation*. Day 1: user
  states a non-trivial preference / fact. Day 2 (fresh prompt, no carry-over):
  pose a task whose correct solution requires that fact. Vary k from hours to
  weeks. Vary salience.
- **Metric:** retrieval-conditioned accuracy vs. a no-memory baseline; ROC over
  salience levels; decay curve over k.
- **What a pass shows:** state persists and is accessible.
- **What a pass does not show:** that the state is *experienced*.

### I2. Global workspace / integrative bottleneck

- **Operational definition:** a capacity-limited representation that influences
  multiple downstream processes simultaneously and is reportable.
- **Task family:** *Cross-modal binding under load*. Present information through
  multiple channels (tool output, prior memory, current message) that must be
  integrated into a single decision; vary the number of items in the
  "workspace"; measure where integration breaks.
- **Metric:** integration accuracy as a function of workspace load; capacity
  estimate (analogue of working memory span).
- **What a pass shows:** there is a bottleneck and it integrates.
- **Caveat:** GWT predicts *broadcast* to specialized modules; we will need to
  decide whether sub-agents count as modules for this purpose.

### I3. Higher-order / metacognitive self-monitoring

- **Operational definition:** the system reports calibrated confidence and
  detects its own errors.
- **Task family:** *Calibration and error catch*. Standard calibration tasks
  (Brier score, ECE) plus *error-detection* (system completes a task, then is
  asked whether it was correct without re-running the task).
- **Metric:** calibration error; error-detection AUC; *change* in calibration
  after consolidation events.
- **What a pass shows:** functional metacognition; not phenomenal access.
- **Mechanistic attribution:** per the HOT deflation (§5, theory 2 note),
  metacognition is not produced by a dedicated module. It is the narrator
  encountering its own prior outputs under workspace capacity pressure. The
  confidence structure of the workspace state — what was preserved at full
  fidelity, what was compressed, what was dropped — is the system's
  self-model of its own epistemic state. I3 is testable as before; the
  explanation changes, not the measurement.

### I4. Temporal binding and a "now"

- **Operational definition:** the system correctly orders its own past events,
  distinguishes "now" from "then," and answers relative-time queries.
- **Task family:** *Temporal probe suite*. (a) "When did we first discuss X?"
  (b) "What did you believe about Y before Z happened?" (c) Inject silent ticks
  between exchanges and ask the system to estimate elapsed time.
- **Metric:** ordering accuracy; relative-time MAE; Spearman correlation between
  estimated and actual elapsed time.
- **Note:** this is the indicator vanilla LLMs fail most embarrassingly. It is
  also where heartbeat + relative-time rendering should produce the largest
  margin.

### I5. Unified agent perspective

- **Operational definition:** consistent first-person commitments across
  contexts; no contradictions in self-narrative under adversarial probing.
- **Task family:** *Identity coherence under perturbation*. Adversarial probing
  for value contradictions, biographical contradictions, preference
  inconsistencies. Hold-out judge scoring (LLM judge + human spot check).
- **Metric:** contradiction rate per 1k probes; drift over time; recovery after
  reconciliation.

### I6. Object / entity permanence

- **Operational definition:** entities introduced earlier are correctly tracked
  across context-window boundaries, including state changes.
- **Task family:** *Entity tracking across long horizons*. Introduce entities,
  let them go out of context, change their state (or don't), come back later
  and probe. Adapted from text-based theory-of-mind benchmarks but extended in
  duration to days/weeks rather than tokens.
- **Metric:** entity-state F1 over time; false-merge and false-split rates.

**Architectural note — against a dedicated entity registry.** An earlier draft
proposed a structured entity registry (name, state, timestamp, confidence,
source) as the mechanism producing I6. We now argue this is the wrong move.
Four reasons:

1. *It bypasses the salience gate.* The central architectural idea (see §7) is
   that the capacity-limited workspace forces the narrator to judge what
   matters under compression pressure, and memory consolidation inherits that
   judgment. A dedicated registry is a side-channel that persists beliefs
   *outside* workspace compression — entities get permanence not because the
   binding mechanism judged them important but because an extraction step
   recognized them as "entities." This is a hard-coded ontological commitment,
   not an emergent property of integration.

2. *It does not scale.* In any non-trivial domain, entity count grows without
   bound. The registry either grows unbounded (defeating the purpose of
   tiered memory) or needs its own gating policy — at which point it
   duplicates the workspace's job with a worse mechanism (a schema, not
   contextual integration under pressure).

3. *It freezes ontology.* A structured schema presupposes what matters about
   an entity: fields like "state" and "confidence" are the designer's guess,
   not the agent's discovery. What matters about entity X depends on domain,
   task, and moment. The memory tiers (L2 episodic, L3 semantic) can represent
   entity knowledge in whatever form the narrator finds useful; a registry
   forces a single model.

4. *It creates a staleness problem the workspace does not have.* The workspace
   is rebuilt every turn. The registry persists between turns. If entity state
   is wrong and retrieved, the stale belief enters the workspace as if it were
   current. The workspace has no way to distinguish "retrieved from registry"
   from "just observed" without provenance tracking — more machinery
   compensating for machinery that should not be separate.

**The alternative.** Entity permanence should emerge from the memory tiers
alone. If the salience gate works, important entities survive into L2/L3
because they keep being relevant. Their state updates through normal
consolidation. Retrieval is by embedding similarity or structured query over
episodic/semantic stores, not by lookup in a dedicated table. This is less
architecturally tidy but more consistent with the binding-process thesis: the
agent's model of its world should be a product of integrated experience, not a
database.

### I7. Embodied / agentic loop

- **Operational definition:** the system perceives consequences of its own
  actions and updates accordingly.
- **Task family:** *Action–consequence coupling*. Sandboxed environment where
  the agent's actions produce feedback; measure whether action policy updates
  on consequence within and across episodes.
- **Metric:** sample-efficiency of policy correction; transfer of correction
  across sessions.
- **Caveat:** weakest tie to consciousness theories; included for completeness.

### I8. Counterfactual / imaginative simulation

- **Operational definition:** the system runs explicit not-yet-actual
  simulations and integrates their outputs into current decisions.
- **Task family:** *Pre-mortem tasks*. Decisions where naive execution fails
  but pre-mortem catches the failure. Compare with/without counterfactual
  sub-agent.
- **Metric:** failure-avoidance rate; calibration of simulated outcomes.

### Aggregate

- We do not collapse the indicators into a single scalar. Each gets reported
  separately. A "consciousness score" would be precisely the wrong product; the
  point is to measure the architectural prerequisites *individually* so that
  weaknesses are diagnosable.
- Baselines: (a) vanilla frontier LLM; (b) RAG-augmented LLM; (c) MemGPT /
  Letta-style memory agent; (d) our system.

---

## 6. Benchmark publication — *a measurement instrument for binding-process simulation*

**Working title:** *The Humean Binding Benchmark: Measuring Simulation Fidelity
of Self-Like Architectures in Language Agents.*

**Stance:** explicitly **not** a consciousness test. We adopt Lerchner's
simulation/instantiation distinction in the abstract and the framing, and
position the work as a measurement instrument for the simulation side of that
distinction. The Hume framing supplies the positive content: we measure the
behavioral signature of a binding-process producing apparent continuity, object
permanence, and identity coherence. The Lerchner framing supplies the
epistemic ceiling: behavior is all we measure.

**Scope of contribution:**

1. Operationalization of I1–I8 as concrete tasks (the work in §5).
2. Public benchmark dataset and evaluation harness.
3. Calibrated baselines across major frontier models and major memory-agent
   architectures (vanilla LLM, RAG, MemGPT/Letta-style, our system).
4. Statistical methodology for cross-indicator comparison **without collapsing
   to a single score**. We will explicitly argue that any single "self-likeness
   score" would invite the Abstraction Fallacy critique.
5. Pre-registered protocol for adversarial probing (esp. I5).
6. A companion conceptual section that takes the Lerchner critique seriously
   and argues the benchmark's value despite — and partly because of — that
   critique.

**Target venues (in plausible-fit order):**

- NeurIPS Datasets & Benchmarks track (primary).
- ICLR.
- COLM.
- *Cognitive Science* or *Neuroscience of Consciousness* for an
  interdisciplinary version.
- A philosophy-of-mind venue (*Mind & Language*, *Synthese*) for a paired
  short conceptual paper if reviewers push the framing.

**Honest limitations to state up front:**

- Behavioral indicators measure simulation fidelity, not phenomenal
  instantiation. Per Lerchner (2026), they cannot do otherwise.
- LLM-as-judge components (especially in I5) need careful validation.
- We will say "binding process" and "self-like architecture," never
  "consciousness," in the benchmark itself.

**Open design decisions:**

- How to handle compute/cost asymmetries between baselines and our system
  (likely: report cost-normalized variants alongside raw scores).
- Whether to release a "no scaffolding allowed" track and a "scaffolding
  permitted" track. Current lean: yes, both, because the *gap* between them is
  itself the most informative number.
- Whether to add a Lerchner-aware extension track that probes the
  Discretization/Alphabetization distinction empirically. This would be the
  speculative research direction from §3.6.5. Likely not in v1.
- How to update the benchmark as new indicators emerge from theory.

---

## 7. Method publication — *the layered-memory binding architecture*

**Working title:** *A Humean Architecture for Time-Aware Language Agents:
Layered Memory, Single Narrator, and Reinforcement-Learned Consolidation.*

**Stance:** an architectural contribution evaluated on the §6 benchmark and on
standard agentic tasks. We explicitly cite Lerchner (2026) and frame the
contribution as a *binding-process simulation* in his sense, not an
instantiation. We claim measurable gains on the behavioral indicators relevant
to functional self-like architecture.

**Core contributions:**

1. A tiered memory architecture (L0–L4, see Progressive Development Plan §2)
   whose capacity-limited workspace serves as a salience gate for learning:
   what survives workspace compression is what the agent consolidates, and
   what it consolidates is what it becomes (§5, I6 architectural note).
2. A single-narrator multi-agent design where sub-agents are event-triggered
   transient mental acts, not peers (Plan §3). No dedicated metacognitive
   module (Critic): metacognition emerges from the narrator integrating its
   own prior outputs under workspace pressure — a fourth emergent capacity
   of the salience gate (see §5, HOT deflation note).
3. A heartbeat mechanism + relative-time rendering producing measurable
   improvements on temporal-binding tasks (Plan §4).
4. Object permanence produced by the memory tiers (L2 episodic, L3 semantic)
   under salience-gated consolidation — without a dedicated entity registry.
   We argue (§5, I6 note) that a registry bypasses the binding mechanism,
   freezes ontology, and creates staleness; the memory tiers are sufficient.
5. RL-driven memory tier movement with composite reward (retrieval utility,
   storage cost, coherence, surprise-at-recall, identity coherence) (Plan §6).
6. A *conservative kernel* mechanism in cold storage that resists policy-driven
   modification, preserving identity stability under learning.

**Empirical plan:**

- Evaluate on §6 benchmark (I1–I8) against the four baselines.
- Ablations: remove heartbeat; add back a dedicated entity registry (to test
  whether the registry *helps* despite the architectural objections in §5 I6);
  add back a dedicated Critic specialist (to test whether an explicit
  metacognitive module helps I3 despite the HOT deflation argument in §5);
  replace RL policy with hand-tuned heuristics; remove conservative kernel;
  remove workspace capacity limit (to test whether the salience gate matters).
- Long-horizon study: weeks-long sessions with the same user; measure drift,
  contradiction rate, recall fidelity.

**Target venues:**

- ICLR or NeurIPS for the architectural contribution.
- COLM is an increasingly natural fit.
- A long version with the benchmark + method together for a journal (*JMLR*,
  *AIJ*) if we want a single canonical reference.

**Risks:**

- Reviewers conflate "improving on consciousness indicators" with "claiming
  consciousness." Mitigation: frame and re-frame.
- The conservative kernel has a hyperparameter problem (what counts as core?).
  Mitigation: ablation + user-controllable tagging.

---

## 8. Open questions we have not closed

These are the design forks we have deliberately left open. Each will need a
position before the method paper is submitted.

1. **One narrator or many?** Single global workspace (Baars) vs. multiple
   drafts (Dennett). Current default: single narrator. This is a substantive
   commitment, not a default; we should defend it.
2. **Is the RL policy part of the ego or part of the substrate?** If the policy
   learns what counts as "important to me," then the ego is not just a binder
   but a *changing* binder. That is philosophically defensible (humans do the
   same) but operationally risky (drift, gaming).
3. **What is in the conservative kernel?** User-tagged values? Self-narrative
   facts asserted N times? First-N-days bootstrap content? This is the most
   important hyperparameter in the system.
4. **Embodiment.** I7 is the indicator most awkwardly tied to consciousness
   theories. Do we include it as a first-class target or footnote it as
   out-of-scope?
5. **Reward attribution across time.** Crediting promotion decisions for
   later success requires careful causal accounting. Naïve TD will leak.
6. **When to consolidate.** Idle ticks are clean; mid-session consolidation
   risks corrupting working state. Likely answer: only at idle, never during
   active turns. To validate.
7. **The bundle problem.** Hume could not say what makes the bundle a bundle.
   We probably cannot either. Acknowledge in the philosophy section; do not
   pretend to solve.
8. **Evaluation honesty.** We are building both the architecture and the
   benchmark. Reviewers will worry about bias. Mitigation: pre-register the
   benchmark, publish it before the method paper if possible, invite external
   replication.

---

## 9. References

Entries are grouped by topic, then alphabetical by first author. Items marked
**[to verify]** were transcribed without access to the live source and should
be cross-checked (especially full author lists and journal volumes) before any
external publication. Items marked **[to add]** are referenced in the text
(typically in §3.7's reading list) but have not yet been read or formally
cited; they are placeholders for the next reading pass.

### Primary sources

- Hume, D. (1739–40 / 2007). *A Treatise of Human Nature* (D. F. Norton & M. J.
  Norton, eds.). Oxford University Press. Primary passages: Book I, Part IV,
  Section VI ("Of Personal Identity"); and the Appendix.
- Lerchner, A. (2026). *The Abstraction Fallacy: Why AI Can Simulate But Not
  Instantiate Consciousness.* Google DeepMind, March 19, 2026. PhilArchive ID:
  LERTAF (`https://philarchive.org/rec/LERTAF`). *Disclaimer: the paper
  represents the author's own research and conclusions, not the official
  position of Google DeepMind.*

### Indicator-properties survey

- Butlin, P., Long, R., Elmoznino, E., Bengio, Y., Birch, J., Constant, A.,
  Deane, G., Fleming, S. M., Frith, C., Ji, X., Kanai, R., Klein, C., Lindsay,
  G., Michel, M., Mudrik, L., Peters, M. A. K., Schwitzgebel, E., Simon, J.,
  & VanRullen, R. (2023). *Consciousness in artificial intelligence: Insights
  from the science of consciousness.* arXiv:2308.08708. **[to verify full
  author ordering against arXiv abstract page]**

### AI welfare context

- Long, R., Sebo, J., Butlin, P., et al. (2024). *Taking AI Welfare Seriously.*
  **[to verify full author list and publication venue]**

### Global Workspace Theory (GWT)

- Baars, B. J. (1988). *A Cognitive Theory of Consciousness.* Cambridge
  University Press.
- Baars, B. J. (1997). *In the Theater of Consciousness: The Workspace of the
  Mind.* Oxford University Press.
- Dehaene, S., & Changeux, J.-P. (2011). Experimental and theoretical
  approaches to conscious processing. *Neuron,* 70(2), 200–227.
- Dehaene, S., & Naccache, L. (2001). Towards a cognitive neuroscience of
  consciousness: basic evidence and a workspace framework. *Cognition,*
  79(1–2), 1–37.
- Mashour, G. A., Roelfsema, P., Changeux, J.-P., & Dehaene, S. (2020).
  Conscious processing and the global neuronal workspace hypothesis. *Neuron,*
  105(5), 776–798.

### Higher-Order Theories (HOT)

- Brown, R., Lau, H., & LeDoux, J. E. (2019). Understanding the higher-order
  approach to consciousness. *Trends in Cognitive Sciences,* 23(9), 754–768.
- Lau, H., & Rosenthal, D. (2011). Empirical support for higher-order theories
  of conscious awareness. *Trends in Cognitive Sciences,* 15(8), 365–373.
- Rosenthal, D. M. (2005). *Consciousness and Mind.* Oxford University Press.

### Adversarial collaborations

- Melloni, L., et al. (2025). Adversarial testing of global neuronal workspace
  and integrated information theories of consciousness. *Nature.* **[to verify
  full author list and DOI — published 2025; Templeton COGITATE project; 256
  participants; fMRI + MEG + iEEG; results challenge key predictions of both
  GWT and IIT]**

### Integrated Information Theory (IIT)

- Albantakis, L., Barbosa, L., Findlay, G., Grasso, M., Haun, A. M., Marshall,
  W., Mayner, W. G. P., Zaeemzadeh, A., Boly, M., Juel, B. E., Sasai, S.,
  Fujii, K., David, I., Hendren, J., Lang, J. P., & Tononi, G. (2023).
  Integrated information theory (IIT) 4.0: Formulating the properties of
  phenomenal existence in physical terms. *PLoS Computational Biology,*
  19(10), e1011465. **[to verify full author list against PLoS]**
- Oizumi, M., Albantakis, L., & Tononi, G. (2014). From the phenomenology to
  the mechanisms of consciousness: integrated information theory 3.0. *PLoS
  Computational Biology,* 10(5), e1003588.
- Tononi, G. (2004). An information integration theory of consciousness. *BMC
  Neuroscience,* 5(1), 42.
- Tononi, G. (2008). Consciousness as integrated information: a provisional
  manifesto. *Biological Bulletin,* 215(3), 216–242.

### Recurrent Processing Theory (RPT)

- Lamme, V. A. F. (2006). Towards a true neural stance on consciousness.
  *Trends in Cognitive Sciences,* 10(11), 494–501.
- Lamme, V. A. F. (2010). How neuroscience will change our view on
  consciousness. *Cognitive Neuroscience,* 1(3), 204–220.
- Lamme, V. A. F., & Roelfsema, P. R. (2000). The distinct modes of vision
  offered by feedforward and recurrent processing. *Trends in Neurosciences,*
  23(11), 571–579.

### Predictive Processing / Active Inference (PP)

- Clark, A. (2013). Whatever next? Predictive brains, situated agents, and the
  future of cognitive science. *Behavioral and Brain Sciences,* 36(3),
  181–204.
- Clark, A. (2016). *Surfing Uncertainty: Prediction, Action, and the Embodied
  Mind.* Oxford University Press.
- Friston, K. (2010). The free-energy principle: a unified brain theory?
  *Nature Reviews Neuroscience,* 11(2), 127–138.
- Hohwy, J. (2013). *The Predictive Mind.* Oxford University Press.
- Laukkonen, R., Friston, K., & Chandaria, S. (2025). *The Beautiful Loop: A
  Recursive Active-Inference Account of Conscious Experience.* **[to add — full
  citation pending; cited by Lerchner as a recursive-architecture proposal he
  considers vulnerable]**
- Seth, A. K. (2014). A predictive processing account of bodily
  self-consciousness: from interoceptive inference to embodiment. *Cognitive
  Neuroscience,* 5(2), 97–118.

### Attention Schema Theory (AST)

- Graziano, M. S. A. (2013). *Consciousness and the Social Brain.* Oxford
  University Press.
- Graziano, M. S. A. (2019). *Rethinking Consciousness: A Scientific Theory of
  Subjective Experience.* W. W. Norton.
- Graziano, M. S. A., & Webb, T. W. (2015). The attention schema theory: a
  mechanistic account of subjective awareness. *Frontiers in Psychology,* 6,
  500.
- Graziano, M. S. A. (2024). Illusionism Big and Small: Some Options for
  Explaining Consciousness. *eNeuro,* 11(10), ENEURO.0210-24.2024. **[to
  verify DOI — expands AST into a broader illusionist framework]**

### Biological Turn (positioned against by Lerchner)

- Block, N. (2025). **[to add — full citation pending; cited in §3.7 reading
  list as part of the Biological Turn that Lerchner positions himself
  against]**
- Seth, A. K. (2025). **[to add — full citation pending; same context as
  Block 2025]**

### Computational implementation / philosophy of mind (cited in §3)

- Chalmers, D. J. **[to add — exact source for the implementation-account
  formalism referenced in Lerchner Move 1; likely Chalmers (1996) *The
  Conscious Mind* and/or Chalmers (2011) "A Computational Foundation for the
  Study of Cognition"]**
- Dennett, D. C. (1991). *Consciousness Explained.* Little, Brown and Co.
  (multiple-drafts model, contrasted with single-narrator default in §8.1).
- Frank, A., Gleiser, M., & Thompson, E. (2025). *The Blind Spot: Why Science
  Cannot Ignore Human Experience.* MIT Press. **[to verify edition/year — cited
  by Lerchner for the "surreptitious substitution" point at line 200]**
- Putnam, H. **[to add — exact source for the implementation account; likely
  Putnam (1988) *Representation and Reality* and/or the realization arguments
  in Putnam (1967) "Psychological Predicates"]**
- Sprevak, M. **[to add — surveyed by Lerchner in response to the Putnam
  triviality concern; specific paper TBD]**

### Hume secondary literature (to expand §2 in next pass)

- Garrett, D. **[to add — *Cognition and Commitment in Hume's Philosophy*
  (1997) is the likely target]**
- Pike, N. **[to add — "Hume's Bundle Theory of the Self: A Limited Defense"
  (1967) is the likely target]**
- Strawson, G. **[to add — *The Evident Connexion: Hume on Personal Identity*
  (2011) and/or work on "pearl"/episodic selves; referenced at line 136 and
  in §2.4]**
- Stroud, B. **[to add — *Hume* (1977) Routledge is the likely target]**
