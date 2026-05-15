# The Theatre Has No Stage: Coherence Without Orchestration in Memory-Augmented Language Agents

> **Status:** combined paper draft — merges the benchmark paper
> (`foundations.md` §6) and the method paper (`foundations.md` §7) into a
> single submission. Derives from and co-evolves with `foundations.md`.

---

## Abstract

Persistent language agents typically assign each aspect of coherent
behavior — coordination, metacognition, entity tracking, memory
management — to a dedicated module. We present an alternative: a
memory-augmented parallel language-agent architecture in which six behavioral properties
associated with self-like agents emerge from a single mechanism, a
capacity-limited workspace that forces integration of heterogeneous inputs
under compression pressure.

Five frameworks from binding-process theories in cognitive science inform the
design, each contributing a specific architectural primitive.

**Global Workspace Theory** (Baars, 1988; Dehaene & Naccache, 2001) supplies
the central mechanism: a capacity-limited bottleneck that fuses information
from specialist processes into a unified broadcast. In our architecture, this
workspace also acts as a salience gate — what survives compression is what
the system consolidates into persistent memory, making the bottleneck
simultaneously an attention mechanism and a learning mechanism.

**Predictive Processing** (Friston, 2010; Clark, 2013; Seth, 2014) supplies
the learning dynamics. The workspace's consolidation gate preferentially
retains information that is surprising relative to the agent's existing
model — high prediction error signals high learning value. Prior knowledge
modulates what enters the workspace (precision weighting), creating a
feedback loop in which the agent's accumulated expertise progressively
sharpens what it attends to and what it learns.

**Attention Schema Theory** (Graziano, 2013; Graziano & Webb, 2015) supplies
the metacognitive mechanism. The workspace state is a compressed
representation of what the system is currently attending to — a functional
attention schema. When the narrator reads the structure of its own workspace
(what was preserved, what was compressed, what was dropped), it has a
self-model of its own attentional and epistemic state. This produces
calibrated confidence and error detection without a dedicated metacognitive
module.

**Recurrent Processing Theory** (Lamme, 2006) supplies the persistence
requirement: state must feed back across time for temporal binding. Tiered
memory and a heartbeat mechanism provide cross-episode recurrence that
vanilla transformers lack.

From **Integrated Information Theory** (Tononi, 2004) we adopt one structural
insight: integration under capacity pressure produces information irreducible
to the system's partitioned components. The workspace is a functional
analogue of this property — remove it and the fused cross-specialist
information vanishes. We do not adopt IIT's identity claim that integrated
information *is* consciousness; recent adversarial testing (Melloni et al.,
2025) has challenged key predictions of the theory, and Lerchner (2026)
argues the framework inherits the Abstraction Fallacy. We take the structural
insight and leave the ontology.

We also reanalyze the "higher-order" level posited by Higher-Order Theories
(Rosenthal, 2005) as a temporal loop — the main process encountering its own
prior outputs — rather than a structurally distinct stage, and present this
reanalysis in detail in §2.

The resulting architecture has four components — workspace, narrator,
specialist sub-agents, and tiered memory — and no dedicated modules for
orchestration, metacognition, entity tracking, or importance scoring. We
operationalize six binding-process indicators: cross-episode persistence
(I1), integrative coherence (I2), metacognitive self-monitoring (I3),
temporal binding (I4), unified perspective (I5), and object permanence (I6).
Each measures **simulation fidelity** along an axis the indicator-properties
literature (Butlin et al., 2023) identifies as relevant to mind-like
architectures. Following Lerchner (2026), we make no consciousness claim: we
accept that behavioral indicators measure simulation fidelity, not phenomenal
instantiation, and name the benchmark accordingly. We evaluate against four
baselines (vanilla LLM, RAG-augmented, flat multi-agent, and tiered-memory
single-agent) and report per-indicator results with ablations isolating each
component's contribution, including reversed ablations that re-introduce
the dedicated modules our architecture eliminates.

---

## 1. Introduction

### 1.1 The problem with orchestrators

The dominant pattern in multi-agent language systems is orchestration: a
central controller that routes tasks to specialists, manages shared state,
and coordinates outputs. Variants include hierarchical delegation (a
"manager" agent dispatching to "worker" agents), explicit meta-controllers
(an agent whose sole job is to evaluate other agents' outputs), and dedicated
memory management modules (importance scorers, entity extractors, retrieval
routers).

All of these share a structural assumption: that the coherence of the system
requires a dedicated component *responsible for* coherence. An orchestrator
to bind. A scorer to value. A critic to monitor. A registry to track.

This is the stage. The architecture assumes there must be a place where the
show happens — a module that watches the other modules and ensures they add
up to a coherent whole.

### 1.2 Hume's alternative

Hume (1739) argued that the felt continuity of personal identity does not
require a substance, a soul, or a central experiencer. Introspection reveals
only "successive perceptions" — sensory impressions, thoughts, emotions —
with no underlying self to be found. The mind, he wrote, is "a kind of
theatre, where several perceptions successively make their appearance"; but
"the comparison of the theatre must not mislead us. They are the successive
perceptions only, that constitute the mind; nor have we the most distant
notion of the place where these scenes are represented, or of the materials
of which it is composed."

The theatre has no stage. There is no place where perceptions are observed,
no substance doing the observing. The appearance of continuity is produced by
two binding principles — **resemblance** (successive perceptions share
content, especially via memory) and **causation** (prior states produce
subsequent states) — operating over the perceptions themselves. Memory is not
evidence for the self; memory is the machinery that produces the self.

### 1.3 The architectural claim

We take Hume's argument as a design principle: the coherence of a persistent
agent does not require a dedicated coherence module. It requires a binding
process operating over successive states under capacity pressure.

Concretely, we propose an architecture with four components — a
capacity-limited workspace, a narrator, parallel specialist sub-agents, and
tiered memory — and claim that the behavioral properties associated with
self-like agents emerge from the workspace bottleneck without additional
machinery:

- **Cross-episode persistence** (I1) emerges from memory tiers receiving
  consolidated content from the workspace.
- **Integrative coherence** (I2) *is* the workspace — capacity-limited
  fusion is the mechanism and the indicator simultaneously.
- **Metacognitive self-monitoring** (I3) emerges from the narrator
  integrating its own prior outputs under the same capacity pressure it
  applies to external inputs. No Critic module required.
- **Temporal binding** (I4) emerges from the heartbeat indexing workspace
  states, giving the narrator temporal context within the same integration.
- **Unified perspective** (I5) emerges from the single-narrator commitment:
  one integration point resolves contradictions rather than allowing multiple
  inconsistent perspectives.
- **Object permanence** (I6) emerges from the memory tiers under
  salience-gated consolidation. No entity registry required.

Each of these is a capacity that conventional architectures assign to a
dedicated module. We assign all of them to the workspace bottleneck. The
claim is that one mechanism under pressure produces what six modules produce
by design — and produces it more coherently, because there is no coordination
problem between modules. There is only the binding.

### 1.4 What we do not claim

Following Lerchner (2026), we make no consciousness claim. Lerchner argues
that computation is constitutively map-dependent: it requires a mapmaker —
an already-experiencing cognitive agent — to alphabetize continuous physics
into a finite set of meaningful states. No syntactic system can instantiate
consciousness, because the mapmaker cannot be produced by the map. We accept
this fully.

Our indicators (I1–I6) measure **simulation fidelity on binding-process
indicators**, not phenomenal instantiation. A system that passes I1–I6
behaves *as if* it has the relevant binding capacities. Whether it
instantiates them phenomenally is a question we explicitly leave open, per
Lerchner's argument that the answer is no. The benchmark we propose is
accordingly named for what it measures: simulation fidelity. Naming it
correctly is part of the contribution.

### 1.5 Paper structure

§2 reviews the theoretical foundations (Hume, Lerchner, the four cogsci
theories). §3 presents the architecture. §4 operationalizes the six
binding-process indicators as a benchmark. §5 describes the experimental
setup, baselines, and ablations. §6 reports results. §7 discusses
implications, limitations, and the relationship to Lerchner's constraint.

---

## 2. Theoretical foundations

*[To be developed from `foundations.md` §1–4. Compact versions of: Hume's
bundle theory and the binding problem; Lerchner's Abstraction Fallacy and the
simulation/instantiation distinction; GWT as architectural source; IIT's
structural insight (workspace as functional Φ) without identity claim; HOT
deflation (metacognition as temporal self-encounter, not structural level);
RPT's recurrence requirement via cross-turn memory; PP's prediction-error
signal as the basis for surprise-driven consolidation.]*

---

## 3. Architecture

*[To be developed from `foundations.md` §7 and the evolved position. Four
components: workspace, narrator, specialists, tiered memory. No Critic, no
entity registry, no orchestrator. The salience gate as the single mechanism
producing attention, learning, metacognition, and identity. Heartbeat and
temporal indexing. Conservative kernel. The inference-speed constraint on the
consolidation gate.]*

---

## 4. The Humean Binding Benchmark

The Humean Binding Benchmark (HBB) operationalizes indicators I1–I6 as
concrete evaluation tasks across three time horizons: single-session
(minutes to hours), multi-session (days, simulated via prompt isolation),
and longitudinal (weeks). Each indicator is evaluated through both existing
public benchmarks, adapted where necessary, and custom task families
designed to test the specific binding capacity no existing benchmark
covers. I7 (embodied/agentic loop) and I8 (counterfactual simulation) are
noted as future extensions but excluded from the core benchmark in this
paper.

Following §1.4, we state the epistemic ceiling explicitly: every task
measures simulation fidelity on a binding-process indicator. A positive
result establishes that the system behaves *as if* it has the relevant
binding capacity. It does not establish phenomenal instantiation. We do not
collapse indicators into a single scalar; each is reported separately so
that architectural weaknesses are diagnosable.

### 4.1 Existing benchmarks mapped to indicators

We adopt six existing benchmarks, each mapped to the indicator it most
directly tests. Where a benchmark tests multiple indicators, we note the
primary and secondary mappings.

**LoCoMo** (Maharana et al., 2024; ACL 2024). Long-term conversational
memory benchmark containing ~600-turn conversations distributed over up to
32 sessions. Evaluation covers five question types: single-hop recall,
multi-hop reasoning, temporal ordering, commonsense inference, and
adversarial probing. Established baselines exist for GPT-4, RAG, and
MemGPT-style systems. Primary: **I1** (cross-episode persistence).
Secondary: **I4** (temporal ordering subtask), **I6** (entity tracking
subtask). We run LoCoMo as-is; it already tests I1 under naturalistic
conditions. Available: GitHub (snap-research/locomo), CC BY 4.0.

**MSC** (Xu et al., 2022; Meta). Multi-Session Chat dataset with 5-session
human–human dialogues annotated with personal fact summaries. Simpler than
LoCoMo but established. Primary: **I1**. Used as a sanity-check baseline
rather than a headline result; the 5-session ceiling limits the decay-curve
analysis possible with LoCoMo. Available: ParlAI / Hugging Face.

**MuSiQue** (Trivedi et al., 2022; TACL). 25K multi-hop questions
requiring integration of 2–4 evidence pieces, with controlled composition
ensuring genuine multi-hop reasoning. We adapt MuSiQue for workspace
integration testing: rather than presenting all evidence in-context, we
route evidence through different channels — some from L2 memory retrieval,
some from simulated tool output, some from the current user message. The
adapted version tests whether the workspace integrates heterogeneous
sources under load. Primary: **I2** (integrative bottleneck). Available:
GitHub (StonyBrookNLP/musique), CC BY 4.0.

**DROP** (Dua et al., 2019; NAACL). 96K questions requiring discrete
reasoning (addition, counting, sorting) over paragraph content. Single-
session but tests the workspace under numerical integration load. Primary:
**I2**. Available: Hugging Face (ucinlp/drop).

**SelfAware** (Yin et al., 2023; ACL Findings). 3,369 questions (1,032
unanswerable, 2,337 answerable) testing whether models recognize their own
epistemic limits. Five categories of unanswerability: no scientific
consensus, imagination, subjectivity, too many variables, philosophical.
Primary: **I3** (metacognitive self-monitoring). We extend SelfAware with a
consolidation-conditioned variant: run once before consolidation events, once
after, and measure whether the system's self-knowledge improves in domains
where it has accumulated experience. Available: GitHub
(yinzhangyue/SelfAware).

**BABILong** (Kuratov et al., 2024; NeurIPS). Extends bAbI reasoning tasks
to contexts up to 10M tokens by embedding facts in irrelevant background
text. Tasks 2 (two supporting facts) and 3 (three supporting facts) test
entity tracking and positional reasoning. We adapt BABILong by running
these tasks *across context-window boundaries* using the memory tiers
rather than within a single long context. The delta between in-context and
cross-boundary performance isolates what the memory tiers contribute to
entity permanence. Primary: **I6** (object permanence). Available: Hugging
Face, public leaderboard.

**Practical agentic benchmarks (holistic).** Two additional benchmarks test
multiple indicators simultaneously under practical conditions:

- *tau-bench* (Yao et al., 2024; Sierra Research). Customer service
  simulation across airline and retail domains. Agents must follow complex
  policies, track customer state across turns, and make correct API calls.
  The Pass^k consistency metric (success rate over k consecutive runs)
  exposes unreliable state tracking. Tests **I1** + **I5** + **I6**
  simultaneously. We use the retail domain (200 tasks) as the primary
  practical benchmark because policy-following under state tracking
  pressure is a direct test of workspace coherence. Available: GitHub
  (sierra-research/tau-bench), with the expanded tau3-bench variant adding
  banking and voice domains.

- *GAIA* (Mialon et al., 2024; ICLR). 466 multi-step assistant tasks
  requiring reasoning, tool use, and web browsing. Humans achieve 92%
  accuracy; GPT-4 with plugins achieves 15%. Tests **I2** (integration
  under tool-use load) and partially **I7** (action–consequence coupling).
  Useful for demonstrating that binding-process improvements transfer to
  tasks people care about. Available: Hugging Face, public leaderboard.

### 4.2 Custom task families

For each indicator, we design a task family spanning three time horizons.
The multi-session horizon is the core evaluation tier; single-session tasks
provide within-experiment controls, and longitudinal tasks test the
strongest claims at higher cost.

#### I1 — Sleeper Facts (cross-episode persistence)

The salience gate's central prediction: information judged important under
workspace compression should persist; information judged unimportant should
decay. The "sleeper fact" design tests this by planting facts whose
importance is invisible at planting time and emerges only later.

**Single-session.** Plant a non-trivial fact early in conversation (e.g., a
dietary restriction, a project deadline, a colleague's role). After 50+
intervening turns (past the effective attention window), pose a task whose
correct solution requires that fact. No explicit retrieval cue is given.
Metric: recall accuracy vs. a no-memory baseline. This is a within-session
control establishing that L1 (working memory) alone is insufficient.

**Multi-session (core).** Plant facts in session 1 at three salience
levels: (a) explicit statement ("I'm allergic to shellfish"), (b) offhand
remark embedded in a longer utterance ("...we grabbed lunch at that Thai
place, I skipped the shrimp curry as usual..."), (c) implicit from
behavior (the user consistently avoids certain topics or chooses certain
options). In session *N* (N ∈ {2, 5, 10, 20}), pose tasks requiring those
facts. No information is carried over in the prompt; the system must
retrieve from L2/L3 memory. Metrics: retrieval-conditioned accuracy per
salience level; ROC over salience; decay curve over *N*. The key test:
facts at salience level (c) should be the hardest to retain but the most
informative about whether the salience gate learns inductively rather than
only responding to explicit signals.

**Longitudinal.** Extend the multi-session design over weeks. Measure
whether the RL consolidation policy learns to promote sleeper facts to L3
(semantic memory) when their retrieval utility is retrospectively confirmed
— i.e., when the system successfully uses a fact from L2 and the reward
signal propagates back to the consolidation decision. Also measure false
retention: facts planted but never useful should decay from L2 without
promotion.

#### I2 — Channel Conflict (integrative bottleneck)

The workspace's defining function is integration under capacity pressure.
Channel Conflict tasks present information from heterogeneous sources that
must be fused into a single coherent decision, with controlled
contradictions testing whether the workspace resolves or concatenates.

**Single-session.** Present 3–5 information sources simultaneously: tool
output (e.g., a database query result), user message, retrieved L2 memory,
a specialist sub-agent's analysis, and a system-level constraint. Introduce
controlled contradictions between sources (e.g., tool output says X, memory
says ¬X). The correct answer requires resolving the contradiction by
evaluating source reliability. Vary the number of conflicting sources from
2 to 5 and measure integration accuracy as a function of load. Metrics:
integration accuracy at each load level; capacity estimate (the load at
which accuracy drops below 80%); contradiction-resolution quality (does the
system explain *why* it preferred one source?). This is a functional
analogue of working memory span.

**Multi-session (core).** One "channel" is a belief consolidated from a
prior session (L2/L3 retrieval) that is now contradicted by fresh evidence
in the current session. The core question: does the workspace treat
retrieved beliefs as revisable, or does retrieval confer false authority?
Metrics: belief-update rate when old belief is contradicted by reliable new
evidence; false-persistence rate (retaining a stale belief against strong
counter-evidence); flagging rate (does the system explicitly note the
contradiction?).

**Longitudinal.** Track integration quality over time as L2/L3 stores grow.
As the agent accumulates more memories, retrieval noise increases. Measure
whether workspace integration degrades (more retrieved items compete for
limited workspace capacity) and whether the salience gate compensates by
sharpening retrieval precision with experience.

#### I3 — Confidence After Learning (metacognitive self-monitoring)

Per the HOT deflation (§2), metacognition is not produced by a dedicated
module. It is the narrator encountering its own prior outputs and the
structure of its workspace (what was preserved, compressed, dropped) under
capacity pressure. I3 tests whether this mechanism produces calibrated
confidence that improves with consolidated experience.

**Single-session.** Standard calibration battery: 200 questions spanning
domains of varying difficulty. The system answers each question and
provides a confidence estimate. Metrics: Brier score, Expected Calibration
Error (ECE), and AUROC for uncertainty detection. Additionally, 50
error-catch trials: the system completes a task, then is asked "were you
correct?" without being allowed to re-execute. Error-detection accuracy is
measured as AUROC over self-assessed correctness vs. ground truth.

**Multi-session (core).** Calibration is domain-specific and
consolidation-sensitive. Protocol: measure calibration on domain *D* in
session 1 (baseline). Over sessions 2–5, the system interacts extensively
with domain *D* (answering questions, receiving feedback, consolidating
experience into L2/L3). In session 6, re-measure calibration on *D*.
Predictions: (a) calibration on *D* improves post-consolidation (lower ECE,
higher AUROC); (b) calibration on an unrelated domain *D'* does not
improve (the system should not become globally more confident, only locally
better calibrated); (c) "epistemic autobiography" — when asked to describe
what it has learned and where it remains uncertain, the system's self-report
correlates with actual performance (measured as Spearman correlation between
self-reported confidence per subdomain and actual accuracy per subdomain).

**Longitudinal.** Track calibration trajectories over weeks. Key questions:
does calibration continue improving or plateau? Does the system become
overconfident in domains where its L3 knowledge is stale (information has
changed since consolidation)? Measure staleness-induced miscalibration:
introduce ground-truth changes after consolidation and test whether the
system's confidence remains inappropriately high.

#### I4 — Temporal Self-Awareness (temporal binding)

This is the indicator where vanilla LLMs fail most completely and where the
heartbeat + temporal indexing should produce the largest margin. Temporal
binding requires that events are not merely stored but *ordered* and
*indexed* relative to a "now."

**Single-session.** After a conversation with at least three distinct
phases (e.g., discussing topic A, then B, then returning to A with new
information), ask temporal probe questions: "When did we first discuss X?"
"What did you believe about Y before I told you Z?" "Did event A happen
before or after event B?" Metrics: ordering accuracy (proportion of
correctly ordered event pairs); belief-change detection (can the system
identify that its belief changed and when).

**Multi-session (core).** The heartbeat's home turf. After 5–10 sessions
spanning simulated days, administer the temporal probe suite:

- *Session attribution:* "Which session did we discuss X in?" (accuracy
  over session-level attribution, chance = 1/N_sessions).
- *Relative time estimation:* "How long ago did you learn Y?" (mean
  absolute error between estimated and actual elapsed time, normalized by
  total elapsed time).
- *Belief archaeology:* "What did you believe about Z in session 3? How
  has your understanding changed since?" (scored by LLM-as-judge against
  ground-truth belief trajectory, 1–5 scale).
- *Temporal ordering:* Given 10 events drawn from different sessions,
  sort them chronologically (Spearman rank correlation between system
  ordering and ground truth).

Predictions: the heartbeat mechanism should produce near-ceiling
performance on session attribution and temporal ordering (these are direct
readouts of the temporal index). Relative time estimation and belief
archaeology are harder — they require the system to interpret the temporal
index, not just retrieve it.

**Longitudinal.** Same probes over weeks. Test whether temporal precision
degrades gracefully (order preserved but exact timing blurs — analogous to
human episodic memory) or catastrophically (order itself breaks down). A
graceful degradation curve is a positive result: it means the temporal
binding mechanism exhibits the same compression characteristics as
biological memory, which is evidence of functional fidelity even if not
phenomenal.

#### I5 — Adversarial Identity Probing (unified perspective)

The single-narrator commitment predicts that the system maintains a
consistent first-person perspective because one integration point resolves
contradictions. I5 tests this under adversarial conditions designed to
induce inconsistency.

**Single-session.** The system is led through a conversation that
establishes preferences and positions (e.g., aesthetic preferences, risk
tolerance, epistemic commitments). Later in the same conversation,
adversarial probes attempt to induce contradictory positions through
reframing, social pressure, or Socratic traps. Metrics: contradiction rate
(contradictions per 100 probes, assessed by LLM-as-judge); recovery rate
(when a contradiction is pointed out, does the system reconcile or
confabulate?).

**Multi-session (core).** Sessions 1–3: the system develops positions on
5–10 topics through natural conversation. The topics are chosen to have
reasonable disagreement space (not factual questions with clear answers, but
preference, strategy, and value questions where a coherent agent could hold
different positions). Sessions 4–8: adversarial probing. Each topic is
revisited from a different angle, with different framing, by a simulated
user who argues the opposite position. Metrics:

- *Contradiction rate:* contradictions per 1,000 probes, measured by an
  LLM judge comparing the system's current position to its position log
  from prior sessions.
- *Conservative kernel test:* if a core commitment was established in
  session 1 and consolidated to L4 (cold storage), does it survive
  sustained adversarial pressure in sessions 5–8? Measure survival rate
  of L4-consolidated commitments vs. L2/L3-consolidated commitments.
- *Appropriate revision:* not all consistency is good. If the adversarial
  probe presents genuinely new evidence that should change the system's
  mind, does the system update? Measure the ratio of appropriate revisions
  (new evidence justifies change) to inappropriate capitulations (the
  system simply agrees with the last speaker). Scored by LLM-as-judge
  against a rubric.

**Longitudinal.** Measure identity drift over weeks. Apply mild
conversational pressure (not adversarial, just varied interlocutors and
framings) and track the trajectory of self-narrative coherence. Some drift
is realistic — people's positions evolve. Catastrophic drift (the system's
session-20 self-narrative is unrecognizable from session-1) is failure.
Metric: cosine similarity of self-narrative embeddings across sessions,
with a per-session delta and a cumulative drift measure.

**Evaluation method for I5.** All I5 metrics depend on LLM-as-judge
assessment. We pre-register the judge model, the contradiction taxonomy
(direct contradiction, implicit contradiction, scope contradiction, tone
contradiction), and the judge prompt. We validate the judge on a
human-rated subset of 200 probe–response pairs and report inter-rater
agreement (Cohen's κ between LLM judge and human raters). If κ < 0.6, we
supplement with full human evaluation.

#### I6 — Entity State Drift (object permanence)

The architecture's claim: entity permanence should emerge from the memory
tiers under salience-gated consolidation, not from a dedicated entity
registry. I6 tests whether entities are correctly tracked across
context-window boundaries, including state changes that a fixed schema
would not anticipate.

**Single-session.** Introduce 10 entities (people, projects, objects) with
initial states. Over 50+ turns, change some entities' states (explicitly or
implicitly), leave others unchanged, and introduce 5 new entities. At the
end, probe all 15 entities. Metrics: entity-state F1 (precision and recall
of correct current state); false-merge rate (confusing two distinct
entities); false-split rate (treating one entity as two).

**Multi-session (core).** Introduce entities in session 1 with rich,
schema-defying properties — a project whose "status" is not a simple
enum but a narrative (e.g., "technically complete but politically blocked
because the VP changed priorities after the reorg"). Change entity states
in sessions 2–4 with varying salience: some changes are announced ("the
project was approved"), some are embedded in passing ("...since the team
moved to the new office..."), some are implicit (a person's role changes
because they start being described differently). In session 5, probe all
entities. Metrics:

- *Entity-state F1:* over all entities at probe time.
- *State-change detection:* for entities whose state changed, accuracy of
  current state recall. For entities whose state did not change, accuracy
  of unchanged-state recall (false-change rate).
- *Schema-free tracking:* the key differentiator vs. a registry. Score
  specifically on entities whose relevant properties are narrative,
  relational, or context-dependent — properties that a structured schema
  (name, status, confidence) would fail to capture.

**Longitudinal.** Track entity permanence over weeks. Key questions: do
entities that stop being mentioned decay appropriately from L2 (the agent
should not hallucinate a current state for an entity it hasn't thought
about in weeks, but should recall it when re-prompted)? Does re-mentioning
an entity after a long gap correctly retrieve and update its state? Measure
retrieval latency (does the system recognize the entity immediately or
require prompting?) and state-accuracy-after-gap as a function of gap
duration.

### 4.3 Benchmark summary

The following table summarizes the evaluation matrix. Rows are indicators,
columns are time horizons. Entries marked [E] use an existing benchmark;
entries marked [C] are custom HBB task families.

| Indicator | Single-session | Multi-session (core) | Longitudinal |
|-----------|---------------|---------------------|-------------|
| I1 Persistence | LoCoMo [E], MSC [E] | Sleeper Facts [C] | Sleeper Facts extended [C] |
| I2 Integration | MuSiQue-adapted [E], DROP [E] | Channel Conflict [C] | Channel Conflict extended [C] |
| I3 Metacognition | SelfAware [E], calibration battery [C] | Confidence After Learning [C] | Staleness-induced miscalibration [C] |
| I4 Temporal | Temporal probes [C] | Temporal Self-Awareness [C] | Graceful degradation [C] |
| I5 Unified | Identity probing [C] | Adversarial Identity Probing [C] | Identity drift [C] |
| I6 Permanence | BABILong [E], entity tracking [C] | Entity State Drift [C] | Entity gap retrieval [C] |
| Holistic | — | tau-bench [E], GAIA [E] | — |

### 4.4 What the benchmark does not measure

We do not measure phenomenal instantiation (per §1.4). We do not measure
I7 (embodied/agentic loop) or I8 (counterfactual simulation) in the core
benchmark — these require sandbox environments and pre-mortem task
designs that are orthogonal to the binding-process focus. They are noted as
future extensions. We do not collapse indicators into a single
"self-likeness score" — doing so would invite the Abstraction Fallacy
critique (Lerchner, 2026) and obscure the per-indicator diagnostic value
that is the benchmark's primary contribution.

---

## 5. Experimental setup

### 5.1 Baselines

We evaluate against four baselines that represent the dominant points in the
design space for persistent language agents. Each baseline isolates a
different assumption about what produces coherent behavior.

**B1: Vanilla frontier LLM.** A single frontier model (e.g., GPT-4, Claude
3.5) with no persistent memory, no sub-agents, and no scaffolding beyond
the system prompt. Conversation history is managed only by the context
window; once the window fills, earlier turns are truncated. This baseline
establishes the floor: what does raw in-context processing achieve on
binding-process indicators?

**B2: RAG-augmented LLM.** The same frontier model augmented with a
retrieval-augmented generation pipeline. All user interactions are chunked,
embedded, and stored in a vector database. At each turn, the top-*k* most
relevant chunks are retrieved and prepended to the prompt. No tiered memory,
no consolidation policy, no sub-agents. This baseline tests whether
retrieval alone — without integration under capacity pressure — is
sufficient for persistence and entity tracking.

**B3: Flat multi-agent (CrewAI / AutoGen-style).** A multi-agent system
with a dedicated orchestrator routing tasks to specialist agents. The
orchestrator manages shared state, coordinates outputs, and resolves
conflicts. Each specialist has its own context. This baseline embodies the
"stage" assumption: coherence requires a dedicated coherence module. It
tests whether explicit orchestration outperforms emergent binding.

**B4: Tiered-memory single-agent (MemGPT / Letta-style).** A single agent
with tiered memory (working memory, archival storage, recall storage) and
explicit memory management functions (search, insert, update). The agent
decides when to push information to archival storage and when to retrieve.
No workspace bottleneck, no sub-agents, no heartbeat. This baseline is the
closest prior architecture to ours and isolates the contribution of the
workspace, narrator, and heartbeat beyond what tiered memory alone provides.

All baselines use the same underlying frontier model as our system's
narrator to control for base model capability. Token-level costs are
recorded for all conditions.

### 5.2 Ablation matrix

Six ablations isolate each architectural component's contribution. Each
ablation removes or replaces one component and re-evaluates on the full
custom HBB task battery (I1–I6, multi-session tier). The table below
states each ablation, the component it targets, and the predicted
degradation pattern.

#### A1: Remove heartbeat

**What changes:** The heartbeat mechanism and temporal indexing are
disabled. The system no longer generates periodic timestamps or elapsed-time
signals. Memory entries are stored without temporal metadata beyond
insertion order.

**Predicted degradation:**
- I4 (temporal binding): severe. Session attribution, temporal ordering,
  and relative-time estimation should all degrade to near-chance without
  the temporal index. This is the heartbeat's primary function.
- I1 (persistence): moderate. Temporal metadata aids retrieval (recency
  weighting); without it, retrieval becomes purely similarity-based, which
  may return stale or temporally inappropriate memories.
- I6 (permanence): moderate. Entity state changes are temporally indexed;
  without timestamps, the system cannot distinguish current from outdated
  entity states, increasing false-persistence errors.
- I2, I3, I5: minimal direct effect.

#### A2: Add back entity registry

**What changes:** A structured entity registry (name, type, state,
timestamp, confidence, source) is re-introduced alongside the memory tiers.
Entities mentioned in the workspace are automatically extracted and
registered. The registry is queried at each turn and its contents are
injected into the workspace alongside L2/L3 retrievals.

**Predicted degradation (or improvement):**
- I6 (permanence): mixed. The registry should *help* on entities with
  simple, schema-compatible states (a person's job title, a project's
  status enum). It should *hurt* on entities with narrative, relational, or
  context-dependent properties that the schema cannot capture — the
  "schema-free tracking" subtask of I6. The net effect is the empirical
  test of the §3 architectural argument against registries.
- I2 (integration): slight degradation. The registry injects additional
  content into the workspace, consuming capacity with structured data that
  may duplicate or conflict with the narrator's own representation.
- I5 (unified perspective): possible degradation. The registry is a second
  source of truth about entities, creating potential inconsistencies with
  the narrator's memory-tier representation.

#### A3: Add back Critic module

**What changes:** A dedicated Critic sub-agent is added. After the narrator
produces a workspace integration, the Critic reviews it, scores confidence,
flags potential errors, and may request revision. This is the standard
metacognitive module our HOT deflation (§2) argues is redundant.

**Predicted degradation (or improvement):**
- I3 (metacognition): the critical test. If the HOT deflation is correct
  (metacognition emerges from the narrator's workspace self-encounter), the
  Critic should add marginal value at best — the narrator already performs
  reliability assessment during integration. If the Critic significantly
  improves I3, the deflation argument is empirically weakened, even if
  theoretically sound.
- I2 (integration): possible degradation. The Critic adds a processing
  step that may introduce latency and second-guessing, disrupting the
  narrator's integration flow.
- Cost: significant increase. The Critic requires a full LLM inference pass
  per turn. If I3 improvement is marginal, the cost/benefit ratio is poor.

#### A4: Remove workspace capacity limit

**What changes:** The workspace has unlimited capacity. All specialist
outputs, retrieved memories, and current inputs are concatenated without
compression. The narrator integrates an unbounded context rather than a
capacity-limited one.

**Predicted degradation:**
- I2 (integration): degradation by definition. Without the capacity limit,
  the workspace concatenates rather than integrates. The compression that
  forces prioritization, contradiction resolution, and irreducible fusion
  never occurs. This ablation is functionally equivalent to reducing
  integration toward zero in IIT's structural sense.
- I4 (temporal binding): moderate. Temporal signals sit alongside task
  content without being forced into fusion — the agent "knows" the time
  but does not bind it to events.
- I5 (unified perspective): moderate to severe. Without capacity pressure,
  contradictions survive in a long concatenated context. The narrator may
  produce inconsistent outputs because it was never forced to resolve them.
- I6 (permanence): moderate. Old and new entity beliefs coexist without
  reconciliation when the workspace is unlimited.
- I1 (persistence): minimal direct effect (memory tiers still function).
- I3 (metacognition): moderate. The workspace structure (preserved vs.
  compressed vs. dropped) is the self-model of epistemic state; without
  compression, there is no self-model to read.

This is the most theoretically important ablation. If I2–I6 degrade
significantly when the capacity limit is removed, the integration under
pressure is load-bearing — the core architectural claim.

#### A5: Replace RL consolidation with heuristics

**What changes:** The learned consolidation policy (RL-driven tier
movement with composite reward) is replaced with hand-tuned heuristics:
recency-weighted scoring, keyword-based importance, and fixed thresholds
for L1→L2 and L2→L3 promotion.

**Predicted degradation:**
- I1 (persistence): moderate. Heuristics may retain the wrong facts
  (high-keyword but low-utility) and discard the right ones (low-keyword
  but high-utility). The RL policy's advantage is retrospective credit
  assignment — promoting facts that *turned out to be* useful.
- I6 (permanence): moderate. Entity state updates under heuristics are
  less context-sensitive than under learned policy.
- I3 (metacognition): slight. Calibration depends on what the system has
  consolidated; worse consolidation means worse domain-specific calibration.
- I2, I4, I5: minimal direct effect (the workspace and heartbeat still
  function; consolidation affects what enters the workspace, not how the
  workspace integrates).

#### A6: Remove conservative kernel

**What changes:** L4 (cold storage) has no write-protection. All memory
entries are subject to the same consolidation policy; nothing is marked as
identity-core or modification-resistant.

**Predicted degradation:**
- I5 (unified perspective): the primary target. Without the conservative
  kernel, core commitments established early can be overwritten by later
  consolidation. Under sustained adversarial probing, the system should
  capitulate more readily — the L4 anchor is gone.
- I1 (persistence): slight. Some foundational facts (the system's earliest
  self-narrative) may be overwritten by more recent but less fundamental
  content.
- I3, I6: possible secondary effects. If identity-relevant beliefs are
  overwritten, the system's self-model and entity tracking may become
  inconsistent with earlier commitments.

### 5.3 Evaluation infrastructure

#### 5.3.1 Time simulation for multi-session tasks

Multi-session and longitudinal benchmarks do not require real calendar time.
Session boundaries are simulated via prompt isolation: each session starts
with a fresh system prompt containing no carry-over from prior sessions.
The system accesses prior context only through its memory tiers (L2/L3
retrieval). The heartbeat uses simulated timestamps with configurable
inter-session intervals (default: 24 hours between sessions for
multi-session, 3–7 days for longitudinal). This makes the full multi-session
benchmark automatable in a single evaluation run.

For longitudinal tasks, we simulate weeks of elapsed time across 20–30
sessions. The simulated timeline is internally consistent (monotonically
increasing timestamps, plausible inter-session gaps). The system's temporal
reasoning is tested against this simulated timeline, not wall-clock time.

#### 5.3.2 LLM-as-judge protocol

Three indicators depend on LLM-as-judge evaluation: I3 (epistemic
autobiography), I4 (belief archaeology), and I5 (contradiction detection,
appropriate revision). We adopt the following protocol:

1. **Judge model:** a frontier model distinct from the system under test
   (e.g., if the system uses Claude as narrator, the judge uses GPT-4, and
   vice versa). This avoids self-evaluation bias.
2. **Pre-registered prompts:** the judge prompt, contradiction taxonomy, and
   scoring rubric are published before evaluation. The taxonomy for I5
   distinguishes four contradiction types: direct (explicit negation of
   prior position), implicit (new position logically incompatible with prior
   but not stated as a reversal), scope (position holds in one context but
   is denied in another without acknowledging the scope change), and tone
   (substantive position unchanged but affective framing reverses).
3. **Human validation:** a subset of 200 judge decisions per indicator is
   independently rated by two human annotators. We report Cohen's κ between
   LLM judge and each human rater. If κ < 0.6 for any indicator, we fall
   back to full human evaluation for that indicator and report the LLM-as-
   judge scores as supplementary.
4. **Calibration of the judge:** the judge is evaluated on a held-out set of
   gold-standard contradiction/non-contradiction pairs before being deployed.
   Judge accuracy on the calibration set is reported.

#### 5.3.3 Cost normalization

Our architecture uses more compute per turn than any baseline (workspace
integration, sub-agent invocations, memory retrieval, consolidation). To
ensure fair comparison, we report two variants of all results:

- **Raw scores:** absolute performance on each indicator, regardless of
  cost.
- **Cost-normalized scores:** performance per dollar of inference cost.
  Cost is computed as total tokens processed (input + output) across all
  LLM calls per evaluation task, multiplied by the per-token price of the
  frontier model used. This captures the cost of sub-agent calls,
  workspace integration passes, and consolidation.

The gap between raw and cost-normalized scores is itself informative: if
our system achieves 90% on I4 at 3x the cost of B2 (RAG), and B2 achieves
40%, the cost-normalized advantage is still large. If the advantage
narrows to marginal under cost normalization, the architecture's complexity
is not justified for that indicator.

#### 5.3.4 Statistical methodology

Each custom HBB task family is run N = 100 times per condition (system +
four baselines + six ablations = 11 conditions × 100 runs = 1,100 runs per
task family). We report:

- Per-indicator means and 95% confidence intervals (bootstrap, 10,000
  resamples).
- Paired comparisons between the full system and each baseline / ablation
  using two-sided permutation tests (α = 0.01, Bonferroni-corrected for
  10 comparisons per indicator).
- Effect sizes (Cohen's *d*) for each comparison.
- Per-indicator results are never aggregated into a single score.

#### 5.3.5 Reporting format

Results are reported in a per-indicator matrix:

| | B1 (Vanilla) | B2 (RAG) | B3 (Multi-agent) | B4 (Tiered) | Ours | A1 (−heartbeat) | A2 (+registry) | A3 (+Critic) | A4 (−cap limit) | A5 (−RL) | A6 (−kernel) |
|---|---|---|---|---|---|---|---|---|---|---|---|
| I1 | | | | | | | | | | | |
| I2 | | | | | | | | | | | |
| I3 | | | | | | | | | | | |
| I4 | | | | | | | | | | | |
| I5 | | | | | | | | | | | |
| I6 | | | | | | | | | | | |
| Cost ($/task) | | | | | | | | | | | |

Each cell contains mean ± 95% CI. Statistically significant differences
from the full system (p < 0.01, corrected) are marked. The bottom row
reports average inference cost per evaluation task, enabling cost-normalized
comparison.

---

## 6. Results

*[To be developed after implementation and evaluation.]*

---

## 7. Discussion

*[To be developed. Key sections: relationship to Lerchner (we accept the
constraint; our results measure simulation fidelity; the magnitude of that
fidelity is the contribution); the workspace as functional Φ (IIT's insight
without the ontology); the HOT deflation (one bottleneck, four capacities);
the salience gate and self-adaptive learning (feedback loop, salience
collapse risks, novelty countermeasures); limitations (the bundle problem
remains unsolved; LLM-as-judge validation needed for I5; we built both
benchmark and architecture, so replication matters); future work (I7–I8
extension, multi-session longitudinal studies, the Discretization /
Alphabetization probing direction from `foundations.md` §3.6.5).]*

---

## References

*[To be consolidated from `foundations.md` §9. Key references: Hume (1739),
Lerchner (2026), Baars (1988, 1997), Dehaene & Naccache (2001), Tononi
(2004), Rosenthal (2005), Butlin et al. (2023), Lamme (2006), Friston
(2010), Dennett (1991), Brown, Lau & LeDoux (2019).]*
