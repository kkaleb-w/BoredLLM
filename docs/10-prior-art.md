# 10. Prior Art and Evidence

Two bodies of work bear on this project: the machine side (systems that idle, think
continuously, or learn at test time) and the human side (already covered in
[02](02-boredom-as-a-drive.md) and [03](03-mind-wandering-and-the-default-mode.md)). This
document covers the machine side, and is explicit about what is established versus what is
one group's report.

A general caution that applies to everything below: several of these are recent preprints.
Preprints are claims, not settled results. Treat benchmarks and headline numbers as reported,
not verified.

## Systems that think on their own

### Continuous-time language agents

"Never Stop Thinking: Continuous-Time Language Agents" (arXiv:2609.17416) shows that
continuous-time cognition, thinking while listening and thinking while speaking, emerges from
an unmodified text model under a lightweight interrupt-and-resume orchestrator. It reduces
latency in a voice pipeline. More important for us is a methodological finding buried in the
paper: **LLM judges reward visible reasoning, and a large judged advantage can reverse sign
under an independent judge.** Models trained against such a judge objectively completed fewer
requirements when they thought.

**Why it matters here:** our dependent variable is to be judged by a model. This paper is the
warning label. Fix the judging prompt, use an independent judge, and hand-check a sample.

### Idle-time compute

"Anticipate and Learn: Unleashing Idle-Time Compute in Proactive Agents" (ProAct,
arXiv:2605.25971) treats the time between user interactions as a resource. The agent predicts
likely upcoming needs and gathers information in the background. Reported effects: task
completion faster by about 14.8% in turns, user effort down 11.7%, hallucination rate down
28.1%, and about 42.8% of predictable needs anticipated before being asked.

**Why it matters here:** this is the closest established thing to "useful idle time," and it
is a *reactive* use of idle compute (preparing for a user). Our project is the opposite:
idle compute with no user at all. ProAct is evidence that idle compute can be spent
productively; it is not evidence of anything like daydreaming.

### Spontaneous action from internal dynamics (EMBER)

"EMBER: Experience-Modulated Biologically-inspired Emergent Reasoning" (arXiv:2604.12167)
places a language model as a replaceable reasoning engine inside a spiking neural network
substrate with spike-timing-dependent plasticity. Associative memory lives in the network;
the model reasons over what it surfaces. The reported result: during an idle period, lateral
propagation in the network triggered the model to act without any external prompt. In one
case, after seven conversational exchanges separated by an eight-hour designed idle period,
the system autonomously chose to send an unprompted message to a user. Over a three-day
scripted baseline it produced 23 impulse-driven actions (1 reach-out, 22 journal entries).

**Why it matters here:** this is a working demonstration that idle internal dynamics can
trigger unprompted action. Read the caveats: it is one group's report, the sample is small,
and the mechanism (a spiking substrate, not the language model) is doing the work. It does
not show an LLM daydreaming. It shows a network using an LLM as a mouth.

### Reasoning in latent space

"Training Large Language Models to Reason in a Continuous Latent Space" (Coconut, Hao and
colleagues, arXiv:2412.06769, Meta FAIR and UCSD) feeds the model's last hidden state back as
the next input embedding, so it reasons in continuous space rather than in language. The
emergent behavior: a continuous thought can encode multiple alternative next steps at once,
supporting a breadth-first-like search rather than a single committed path.

Follow-on theory ("Reasoning by Superposition") proves that a two-layer transformer with D
steps of continuous thought can solve directed graph reachability for a graph of diameter D,
while the best known constant-depth discrete chain-of-thought result needs O(n^2) steps. The
interpretation: each continuous thought is a superposition of multiple search frontiers.

**Why it matters here:** this is the nearest mechanical cousin to "thinking without words."
It is not idle thought; it is task reasoning. But if a future version of this project wants
the creature's drift to be genuinely non-linguistic, this is the direction the field is
moving.

## Systems that learn after deployment

The unsolved layer, and the one that decides whether the creature can *change* rather than
merely *remember*.

- **Test-time training for multi-turn agents** ("No Time Like the Present," aTTT,
  arXiv:2607.03441) updates an adapter during an agent episode, with a token-level reweighting
  that downweights repeated n-grams to prevent a self-training death spiral. Reported gains
  up to 5.0 points on ALFWorld and 4.9 on SWE-bench Lite, concentrated where the model
  already has competence but drifts over long trajectories.
- **AdaMEM** (arXiv:2606.05684) adapts agent behavior at test time via a hybrid memory
  architecture, without updating parameters online. Reported relative gains up to 13% on
  ALFWorld and 11% on WebShop.
- **ASCENT** (arXiv:2610.05303) self-distills verified experience into persistent weights
  (LoRA) during deployment, using a frozen copy of the initial model as a privileged teacher.
- **StepLearn** ("Learn Now, Use Next, Trust Later," arXiv:2609.35911) separates immediate use
  of a new hypothesis from persistent trust, requiring prospective validation before reuse.
- **The field's own framing:** the NeurIPS 2026 workshop "Towards Test-Time Continual Learning
  Agents" states plainly that today's agents remain largely static after deployment, cannot
  continually learn at test time, and suffer catastrophic forgetting when updated naively.

**Why it matters here:** weight-level learning is what would make sleep ([07](07-sleep-and-consolidation.md))
truly consolidate rather than merely reorganize memory. At a small model size this is
tractable on a laptop or a cheap server in a way it is not at frontier scale. **This is the
one part of the dossier where the project's constraints make an unsolved problem solvable.**

## Memory systems to study (or adapt)

- **CortiLoop** (`github.com/shenchengtsi/CortiLoop`, also forked at `github.com/AlleinLin/CortiLoop`).
  A bioinspired memory engine with an attention gate, encoder, consolidation, a Hebbian
  association graph, multi-route retrieval with rank fusion, decay, and reconsolidation.
  SQLite storage, works with any LLM exposing a completion method, native Ollama support, and
  a background consolidation worker. Reports 92% on LongMemEval. **Caveat:** both
  repositories are small and new. Treat the benchmark as their claim, and note that the
  seven "brain-labelled" layers are an analogy, not evidence that the components interact the
  way the named brain regions do.
- **LOOP** (`github.com/andrewowest/loop`) uses a three-tier memory hierarchy with Bayesian
  importance tracking. Useful as a contrast: it is more principled about importance than most.
- **cortex-mem** (`github.com/sopaco/cortex-mem`) and **corti**
  (`github.com/fgm-builds/corti`) are larger, self-hosted memory layers with graph and
  vector search. Probably too heavy for a single-creature world, but worth reading for design.

For the small custom system this project proposes, the recommendation is to take the *ideas*
(salience gating, decay, reconsolidation, association, sleep pruning) and implement the thin
version described in [Memory](08-memory.md), rather than adopting a full framework.

## What the evidence does and does not support

| Claim | Status |
|---|---|
| Boredom is an aversive, functional drive | **Established** (multiple papers, replicated correlates) |
| Boredom increases mind-wandering | **Established as a correlate**, with some debate on causal direction |
| A human resting brain does not idle | **Established** (Raichle 2001 and the default mode literature) |
| Humans mind-wander most of waking life | **Established** (Killingsworth and Gilbert, 46.9%) |
| An LLM can be made to idle on its own | **Partially demonstrated** (idle-time compute; EMBER's substrate-triggered action) |
| An LLM can be made to daydream | **Not demonstrated.** No system in this list shows a language model producing spontaneous, non-task, non-repetitive internal thought |
| Weight-level learning at test time works at small scale | **Promising, recent, unverified.** Several methods, no long-duration evidence |
| The proposed architecture produces drift | **Hypothesis.** Not built. This project exists to test it |

The last row is the honest summary of this entire repository. The human science is solid. The
machine science is early. The bridge between them is a proposal.

## Related reading

- [11. Limits](11-limits.md) for the ceiling
- [12. Tools](12-tools.md) for what to actually use
- [References](../REFERENCES.md) for links
