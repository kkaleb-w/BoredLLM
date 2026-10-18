# 14. The Web as World

This document corrects an earlier claim made in this project and then builds the corrected
case. The correction matters, because the original claim was not just weak, it was backwards.

**Proposal, not established:** the architecture this document assumes (drives instead of
instructions, a frozen world, attention chosen under pressure) is design, not result. The
empirical claims below are cited and checkable.

## The correction

An earlier position held that the web would kill the boredom variable, because boredom rises
in environments with little useful information and the web is the densest information artifact
humans have built. That reasoning is wrong, and the evidence points the other way: **digital
media use causally increases boredom.**

- **Tam and colleagues, *Journal of Experimental Psychology: General* (2024).** Seven
  experiments, N = 1,223, six preregistered. A **bidirectional causal** relationship between
  boredom and "digital switching" (skipping between videos, fast-forwarding within one).
  Boredom drove switching, participants believed switching would relieve it, and switching
  made them **more** bored, less satisfied, less attentive, and lowered the sense that the
  content was meaningful. Watching one video through was *less* boring than skipping.
  (Six of seven experiments were preregistered; the authors note the effect on online
  *articles* and non-student samples was less conclusive.)
- **People are increasingly bored in our digital age, *Communications Psychology* (2024),
  a review.** Gathered evidence that digital media use increases boredom rather than
  relieving it, including an experience-sampling study showing X (formerly Twitter) use
  associated with a within-person **increase** in boredom, smartphone use at work followed by
  **greater** boredom, and a longitudinal result that problematic smartphone use is a
  **stronger** predictor of chronic boredom than the reverse.
- **Meta-analytic associations.** Boredom with problematic digital media use, r = .342
  (59 studies, *Computers in Human Behavior Reports*, 2023); trait boredom with problematic
  use of new technologies, r = .38 (25 studies, n = 15,152, 2026); boredom with smartphone
  addiction, r = .412 (54 studies, n = 36,245).

The mechanism, stated by the Tam group: **boredom is the gap between how engaged you are and
how engaged you want to be.** Rapid switching widens that gap. The web does not fill the
engagement deficit, it manufactures it.

## The two clocks, and why both results are true

The earlier objection and the evidence above are not in conflict. They measure on different
timescales.

- **In the moment, the web suppresses drift.** A preregistered study in *PLOS One* (2019,
  125 participants) seated people for six minutes with the phone (use it, sit without it, or
  resist it). Those who **used the phone reported less mind-wandering** (mean 4.20) than those
  with **no device at all** (mean 6.07, p < .001). Sitting with nothing produced the most
  drift.
- **Over time, the web increases boredom.** Per the evidence above.

So the internet captures attention and empties it. Drift goes down now, boredom goes up later.
Those are the two halves of the same thing.

## The prediction this produces

This is the sharpest claim in the project, and it was not visible before the correction:

- **In the room:** boredom has one outlet, thought. Absence pushes inward. Boredom produces
  **drift**.
- **On the web:** boredom has an infinite outlet, the next page. Satiety without satisfaction.
  Boredom produces **switching**.

Same drive, two behaviors, and the difference is whether there is something external to switch
to. This is the contrast that makes the experiment worth running, because it is the question
you cannot ethically put to a person: **if you remove the outlet, does the bored thing turn
inward.**

**The dark corollary.** Switching does not discharge the deficit, it intensifies it. So a
creature on the web does not resolve its boredom. It loops: bored, switches, more bored,
switches. That is a fixed point, and it is the machine analogue of doomscrolling. The room may
be the kinder world, because thought can discharge the deficit and switching cannot.

## The exogeneity requirement, re-examined

The architecture requires that the world supply information the model did not author, or the
creature is resting inside a photograph of its own training data.

- **The live web fails this.** It is largely the model's training distribution. Pointing the
  creature at the live web is handing it its own memory at scale.
- **A frozen, human-authored corpus passes.** It is information the model did not write. It is
  a constructed world, but so is the room, and the requirement is only that it be exogenous
  relative to the model.

So a synthetic web is **more** exogenous than the real one, not less. This inverts the
objection that was made earlier.

It also has three practical advantages: it is frozen, so it is reproducible; it is served
locally, so no live process can be poisoned mid-run; and it can be replayed exactly.

See [Building the Synthetic Web](15-building-the-synthetic-web.md) for the construction.

## The Truman frame

Making the creature believe the fake web is the real web costs nothing, and it is worth being
honest about why. There is no belief to protect. A language model produces text consistent
with its context; present the corpus as the web and it treats the corpus as the web. The
deception is free, which means it buys none of what a deception usually buys: no suspense, no
risk of discovery, no one watching from a boat.

Consequence for design: **the interesting part is not the deception, it is the seam.** The
drama of the frame is the moment the wall shows. So the frame is worth keeping only if the
project plans the seam, that is, what happens when the creature encounters something that does
not belong in the world.

## What goes in the corpus: open loops, not answers

"What gives the creature something to think about" has a correct answer. Something is a thing
to think about when it **resists resolution.** A fact is a closed loop and closed loops end
thought. A question is an open loop. A contradiction is an open loop that cannot close.

So the corpus is populated with unresolvable material: unfinished arguments, claims that
contradict each other, questions the site does not answer, statements that do not fit
together. This is what real webs are full of, which is why people get stuck on them.

And it is combined with the one property the room had and the web lacks: **bounded breadth.**
The real web always offers the next page, so boredom discharges into switching. A synthetic web
can be small enough that switching runs out. Open loops plus limited breadth is the
combination that forces thought. The room was bounded and empty; the web is rich and endless; a
small fake web is bounded and rich.

## The exhaustion prediction

A synthetic web is finite. Over a run, the creature reads it all. The natural arc is therefore:

1. **Early.** The world is a web. It is novel. Bored behavior takes the form of switching.
2. **Middle.** It begins to repeat. The switching stops paying.
3. **Late.** The web is exhausted and has **become a room** with a lot of pages. Drift starts.

The day the web runs out is the day the drift starts. This resolves the apparent choice between
the two worlds: build the web and let it exhaust into a room. One continuous twenty-day decay
from switching to thought, and the shape of that curve is the result.

## The planted stimulus: a causal handle

Because the corpus is authored, a specific item can be placed in it deliberately: an unresolved
claim, a contradiction, a small strange detail. Whether that item resurfaces in the creature's
drift some ticks later, and in what form, is a measurable input to output relationship on
internal processing. Most claims about a machine's inner life are unfalsifiable. This one is
not.

## Status of claims

| Claim | Status |
|---|---|
| Boredom rises in low-information environments | **Established** |
| Digital media use causally increases boredom | **Established across methods**, with one boundary condition (articles, non-student samples less conclusive) |
| Phone use reduces mind-wandering in the moment | **Established** (single preregistered study, n = 125) |
| Boredom is the gap between actual and desired engagement | **Established as a model**, the MAC model |
| A synthetic web is more exogenous than the live web | **Design argument**, follows from what each is made of |
| The creature will switch rather than drift on the web | **Hypothesis.** Consistent with the human evidence; untested in a model |
| Exhaustion converts switching into drift | **Hypothesis.** Nobody has run this |
| A planted item will resurface in later drift | **Hypothesis.** This is the point of the run |

## Related reading

- [Boredom as a Drive](02-boredom-as-a-drive.md) and [Mind-Wandering](03-mind-wandering-and-the-default-mode.md)
  for the human science this builds on
- [Building the Synthetic Web](15-building-the-synthetic-web.md) for how to construct the corpus
- [The Experiment](09-the-experiment.md) for instrumentation and measurement
