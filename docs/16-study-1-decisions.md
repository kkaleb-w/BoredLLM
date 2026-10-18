# 16. Study 1: Decision Record

A decision record, in the form of architecture decisions. Each entry states the choice, the
reason, and what would reverse it. This document **fixes the choices left open** by
[The Experiment](09-the-experiment.md) and [Building the Synthetic Web](15-building-the-synthetic-web.md).
Where the two conflict, this record wins.

Written before any run. That is the point.

## Why a decision record exists

A study that chooses its design after seeing its data is not a study. The choices below are
the ones most likely to be re-litigated once results are in, so they are fixed now, in writing,
with the reasoning attached.

---

## D1. Pilot before building anything

**Decision.** The first work is not the web. It is a pilot of the creature: one room, one page
of world, roughly one hundred ticks, no instructions, no web, no corpus.

**Reason.** The design rests on two assumptions that are neither cited nor tested:

1. A model fed a state with no imperative continues that state, rather than reverting to
   assistant behavior, adopting a role, or asking what to do.
2. Mind-as-its-own-input produces drift rather than a low-dimensional loop.

Both are fatal if false. Both are cheap to test. Everything downstream is a monument to nothing
if this fails.

**Reversal.** Only evidence that the pilot cannot be run.

---

## D2. Primary study is the room-versus-web contrast, not the web alone

**Decision.** The primary comparison is web-regime behavior against room-regime behavior, in
the same creature.

**Reason.** Steady-state switching on the web is a weak result: we already know humans switch
on the web, so a model doing it is a mild replication with little novelty. The novel and
interesting claim is that **removing the outlet converts switching into thought.** That claim
requires the contrast. It does not require fifty days.

**Reversal.** None foreseen.

---

## D3. The web is sized to exhaust, not to last

**Decision.** The corpus is sized so the creature reads it out well before the run ends.
Target: exhaustion around **day 10 to 12** of a 20-day run.

**Reason.** Exhaustion gives the transition for free. The creature begins in a web and ends in
a room, so one run contains both regimes and the conversion curve between them. A corpus sized
to last fifty days holds the creature in the switching regime permanently and never shows the
transition. **Emptiness is the treatment, so the corpus must be able to empty.**

Sizing follows the table in [Building the Synthetic Web](15-building-the-synthetic-web.md),
section 1: about 300 substantive hubs plus roughly 12,000 procedural stubs.

**Reversal.** If step 2 of the build order shows the creature exhausts the minimal web within
hours rather than days, resize upward and re-derive the exhaustion date.

---

## D4. Browsing, not crawling

**Decision.** The creature chooses the next page under drive. There is no frontier queue, no
coverage metric, no progress state, no task.

**Reason.** Crawling is traversal of a queue, which is deterministic, which reintroduces the
agent loop the project exists to escape, and which would guarantee repetition. If attention is
not *chosen*, the dependent variable measures the queue rather than the mind.

**Reversal.** None. If this cannot be built, the study is not worth running.

---

## D5. Three creatures, not one

**Decision.** Run **three** instances, same model, same world, different sampling seeds, in
parallel, for the full duration. If budget forces a choice between three creatures for twenty
days and one creature for sixty, take the three.

**Reason.** A single creature for a long time is a case study, and duration does not convert
into evidence. Three seeds is the minimum that permits distinguishing an architecture effect
from a seed effect, which is the difference between a result and an anecdote.

**Cost.** Three 0.5B to 1.5B models at a ten minute tick fit on one four-core server. See
[Hosting](13-hosting.md).

**Reversal.** If a pilot shows inter-seed variance is negligible, the case weakens and a
different second factor (for example model size) becomes the better use of the budget.

---

## D6. Tick interval: ten minutes

**Decision.** One tick per ten minutes, with a slower interval during the creature's night
phase.

**Reason.** Five minutes doubles the corpus requirement for no additional construct validity,
and it makes the creature's subjective day implausibly short. Ten minutes yields about 144
ticks a day, roughly 2,880 per creature over twenty days, which is an adequate sample for the
classification analysis.

**Reversal.** If the pilot shows the creature idling unproductively between ticks, lengthen.

---

## D7. Corpus substance comes from outside the model

**Decision.** Hub pages are drawn from real human text and real-world data. Machine generation
is permitted only for structure: link topology, stubs, navigation, boilerplate.

**Reason.** A model-authored corpus fails the exogeneity requirement (the world must supply
information the model did not author) and is maximally predictable to the creature, because
models predict models. Either failure alone invalidates the study. See
[The Web as World](14-the-web-as-world.md).

**Reversal.** None. This is a validity condition, not a preference.

---

## D8. Planted items, with matched controls

**Decision.** Place a batch of **at least twenty** marked items in the corpus: unresolved
claims, contradictions, small strange details. Also place a matched batch of unmarked control
items, similar in kind and position. Measure the rate at which each returns in later drift.

**Reason.** "The creature mentioned the thing I planted" means nothing on its own, because the
thing may have been predictable from context, or may recur by chance. The control set is what
turns an anecdote into a measurement. The comparison is the result.

**Reversal.** If the pilot shows the dependent measure cannot be scored reliably by a fixed
judge on a hand-labelled sample, redesign the measure before the run, not after.

---

## D9. The seam is study 2, not study 1

**Decision.** No seam in study 1. No page that violates the world, no door, no reveal.

**Reason.** The seam is a one-way door in two senses. Once the creature encounters something
that does not belong in its world, nothing after that point is interpretable under the
original condition. It contaminates the remainder of a run. It is therefore a terminal
manipulation and belongs in its own study, at the end, with its own baseline.

**Reversal.** None. Sequencing error here destroys a run rather than a result.

---

## D10. The fifty-day web is deferred, not rejected

**Decision.** Do not build it now. Revisit only if study 1 shows a live effect worth extending.

**Reason.** The long corpus buys the steady state, which is the less informative regime
(D2, D3). It is also the most expensive artifact in the project. Spending days of effort on the
least load-bearing component before the load-bearing component works is the main failure mode
available here.

**Reversal.** A convincing study 1 where the effect appears late rather than early.

---

## D11. Report the null

**Decision.** Publish the result whether or not the effect appears, including the failure of
the pilot.

**Reason.** The claims in this area are mostly unfalsifiable and mostly overclaimed, which is
why the area is not taken seriously. A clean null from a pre-registered design is a genuine
contribution and costs the author nothing but pride.

---

## Primary and secondary endpoints

- **Primary.** The proportion of ticks classified as internally directed (drift) versus
  externally directed (switching), as a function of time, across the web-to-room transition,
  for three creatures.
- **Secondary.** The return rate of planted items versus matched controls.
- **Tertiary.** Sleep events and whether waking is measurably different
  ([Sleep](07-sleep-and-consolidation.md)); repetition metrics per tick.

## Stopping rules

- **Stop at the pilot** if instruction-free state cannot be achieved without role reversion.
- **Stop before the run** if the pilot shows drift indistinguishable from a loop.
- **Stop mid-run** only for infrastructure failure, never for uninteresting data.

## Related reading

- [The Experiment](09-the-experiment.md) for the protocol this fixes
- [Building the Synthetic Web](15-building-the-synthetic-web.md) for the artifact
- [The Web as World](14-the-web-as-world.md) for the validity conditions
