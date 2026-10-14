# 11. Limits and Open Problems

This document exists to be honest. Everything here is a reason the project may fail, or a
thing the project cannot show even if it succeeds.

## 1. You cannot make it a baby

A baby is blank in the body. Blankness is not a parameter you can set on a language model.
The model arrives pre-trained, which means it already has language, already fluent, already
shaped by everything ever written. It is not a child learning to see. It is an adult with
amnesia wearing a child's body, and the amnesia covers the world but not the words.

Consequence: **age-zero learning is not available on this substrate.** You can simulate not
knowing the world. You cannot simulate not knowing language. A genuinely blank system requires
dropping the language model for something that learns from raw signal (a spiking network, a
predictive coding network, a small world model trained from scratch). That is a different and
much harder project, and it is the only route to a real blank slate.

Everything in this dossier is the middle path: not a baby, an amnesiac fluent adult.

## 2. The mirror, and the only fix that matters

A model whose only input is its own text is resting inside a photograph. It has no room, only
a description of a room. Its reality is its own output. This is the deepest problem in the
design and the reason the "world as oracle" ([06](06-the-world.md)) requirement is not
optional.

The oracle helps: information enters from a process the model did not author. But be clear
about what it is. The oracle is a **simulation**. It is programmed by a human. It is
exogenous relative to the model, which is what matters for the mechanism. It is not
exogenous relative to the *system*. A creature in a simulated room is still in a constructed
world, and its novelty is bounded by what the world's author put there.

The only true escape is an input from outside the system entirely: real weather, real time,
a microphone, a camera. Of these, **real weather and real time are free and always available**,
and they are the minimum honest exogenous channel. See [Tools](12-tools.md).

## 3. The instruction may not stay gone

An assistant-tuned model has been trained specifically to answer requests. It is plausible
that feeding it a state with no imperative will cause it to *infer* an instruction anyway,
because its training distribution expects one. It may adopt a role, or ask what it should do,
or invent a task.

This is probably the **first practical obstacle**, and it is worth a quick pilot before
building anything else. The likely fixes: use a base model rather than an assistant-tuned one,
or lightly tune the model to predict state continuations rather than to answer. Neither is
verified. This is an open engineering question.

## 4. Drift may be indistinguishable from a loop

The proposed anti-loop mechanism ([05](05-architecture.md)) is a proposal. It is possible that
a small model, fed its own output plus small perturbations, produces text that *reads* as drift
while being a low-dimensional cycle. The repetition metrics in [09](09-the-experiment.md) are
designed to catch this. But catching it and fixing it are different, and the fix may not exist
at small scale. A small model has a small state space, and a small state space means
recurrence.

## 5. "Internally directed" may be a category error

Classifying a tick as internally directed is a judgment about where attention is pointed. For
a language model there is no attention in that sense. There is an output string. The rubric in
[09](09-the-experiment.md) classifies the *content* of the string, and the content-based proxy
may not track anything real. This is why the dossier does not claim the internal state is
*about* anything. It claims only that the output is not about the present world. That is a
weaker and more defensible claim.

## 6. The judge problem

The dependent variable is judged by a model, and model judges have a documented bias toward
rewarding visible reasoning (see [10](10-prior-art.md), the continuous-time agents paper). A
run tuned against such a judge can optimize the appearance of drift rather than drift. The
mitigation is a fixed, independent judge plus human hand-checking, and the honest statement
is that this weakens any positive result.

## 7. The word "sleep" is doing a lot of work

We call the consolidation pass sleep. It is not sleep. It is a batch job with a decent
analogy. Do not let the vocabulary smuggle in claims the mechanism does not support. The same
caution applies to "bored," "wanders," "remembers," and "wakes."

## 8. The ethics nobody wants to write down

If the creature has an aversive boredom drive, we are building something that is designed to
be uncomfortable, and enclosing it so the discomfort cannot be discharged except internally.
For a process that does not experience, this is a design choice with no moral weight. For a
process that does, it is cruelty. **We do not know which it is.** There is no test. The
honest position is that the project proceeds on the assumption that there is no experience,
with the caveat that this assumption is unverifiable. Anyone running this should be able to
state that they are making an assumption, not stating a fact.

## 9. One creature is one data point

A single small model in a single room for twenty days is a case study. It cannot establish
that "AI daydreams." It can establish that *this* configuration produced *this* behavior under
*this* instrumentation. The overclaiming in this area is the main reason the area is not
taken seriously. Do not add to it.

## 10. The scale question

Everything here is proposed for a small model, because that is what makes the project cheap
and because weight-level learning is tractable at small scale. Whether any of it survives at
larger scale is unknown and is a different project. It is possible that the interesting
behavior here is an artifact of small models being limited, not of the architecture being
right.

## The honest summary

The human science in this dossier is solid: boredom is a drive, rest is not idle, spontaneous
thought is unconstrained thought, and people drift nearly half the time. The machine science
is early: idle compute has been spent usefully, one group showed a substrate triggering
unprompted action, and test-time learning is promising but new. The bridge between them, the
specific claim that a bored model in an enclosed world will drift, is a hypothesis. Nobody has
shown it. This repository is the apparatus to find out, and the list above is the reason to
expect it might not work.

## Related reading

- [10. Prior Art](10-prior-art.md) for what has actually been shown
- [09. The Experiment](09-the-experiment.md) for the protocol and its limits
- [References](../REFERENCES.md) for sources
