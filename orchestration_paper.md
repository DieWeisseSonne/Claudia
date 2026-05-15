# The Theatre Has No Stage: Binding Without Orchestration in Memory-Augmented Parallel Language Agents

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

Five frameworks from the science of consciousness inform the design, each
contributing a specific architectural primitive.

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

*[To be developed from `foundations.md` §5–6. Operationalization of I1–I6 as
concrete task families with metrics. Explicit framing: simulation fidelity,
not consciousness. What a pass shows and does not show. I7 and I8 noted as
future extensions but not included in the core benchmark for this paper.]*

---

## 5. Experimental setup

*[To be developed. Four baselines: vanilla frontier LLM, RAG-augmented,
flat multi-agent (CrewAI/AutoGen-style), tiered-memory single-agent
(MemGPT/Letta-style). Ablations: remove heartbeat; add back entity registry;
add back Critic; replace RL consolidation with heuristics; remove
conservative kernel; remove workspace capacity limit. Cost-normalized
variants.]*

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
