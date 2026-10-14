# 03. Mind-Wandering and the Default Mode

## The brain does not idle

The most important single result for this project is Raichle and colleagues (2001, *PNAS*),
"A default mode of brain function." The finding: a set of brain regions is consistently
**more active at rest than during task performance**. This was counterintuitive. The
assumption had been that a resting brain quiets down. It does not. It has a mode, and that
mode is not off.

The energy accounting makes it concrete. The brain consumes a large and roughly constant
fraction of the body's energy, on the order of a fifth, and only a small slice of that
tracks task demands. There is no cheap state. **"Doing nothing" is not low activity. It is a
different regime, and an expensive one.**

## Mind-wandering is the default mode of operation

Killingsworth and Gilbert (2010, *Science*, "A Wandering Mind Is an Unhappy Mind") used an
iPhone app to sample roughly 2,250 adults at random moments, about a quarter of a million
samples in total. Their headline numbers:

- **46.9% of waking hours** were spent thinking about something other than what the person
  was doing.
- Mind-wandering occurred **at least 30% of the time during every activity** measured,
  except making love.
- What people were thinking predicted their happiness better than what they were doing. The
  nature of the activity explained about 4.6% of within-person variance in happiness, while
  mind-wandering explained about 10.8%.
- Mind-wandering was more often the **cause** than the consequence of unhappiness, per the
  paper's time-lag analyses.

The authors state the conclusion directly: "A human mind is a wandering mind." Stimulus-independent
thought is described in the paper as the brain's default mode of operation.

For our purposes: **a resting human spends nearly half of waking life not attending to the
present.** Any design that aims at a human-like resting state should treat that as the
reference figure.

## Spontaneous thought is defined by the absence of constraint

Christoff, Fox, and colleagues (2016, *Nature Reviews Neuroscience*, "Mind-wandering as
spontaneous thought: a dynamic framework") give the definition this project leans on hardest.
They define spontaneous thought as mental states that arise **relatively freely, due to an
absence of strong constraints** on both the content of each state and the transitions
between states.

Within their framework, dreaming, mind-wandering, and creative thinking sit on a spectrum of
constraint. Mind-wandering is more constrained than dreaming and less constrained than
goal-directed thought. At the strongly constrained end (rumination, obsessive thought) the
state stops being spontaneous.

This is the key structural point: **spontaneity is not the presence of a special generative
mechanism. It is the absence of strong constraints.** A system that drifts is a system under
weak constraint, free to transition.

## Why the human version never loops

A well studied observation: human idle thought does not repeat itself. You can sit in the
same chair in the same room daily and never have the same resting experience twice. Four
independent streams supply novelty, and none of them is a task:

1. **Interoception.** The body state is never twice identical. Heart rate, hunger, posture,
   temperature, fatigue. A continuous low-bandwidth channel about the self.
2. **Memory reactivation.** The hippocampus replays and reactivates experience during rest,
   not only during sleep. Idle thought is partly replay.
3. **Associative drift.** As above, weak constraint on transitions means each thought cues
   the next by overlap. The stream is a chain, not a repetition.
4. **Real time.** The world itself moves. Light shifts, temperature changes.

The design consequence: **a resting state does not need a task to avoid looping. It needs
many weak inputs.** See [Architecture](05-architecture.md), where this becomes the single
most important implementation decision in the dossier.

## The bored brain and the default mode overlap

Two lines of work connect boredom to this same network.

- Danckert and colleagues found **default mode network activation during boredom**, in
  imaging work contrasting boredom mood induction with resting state and with sustained
  attention tasks. Posterior components of the default mode network were active during
  boredom.
- The review literature (Raffaelli, Mills, and Christoff, 2016) notes that **default mode
  activation was consistent across the few available fMRI studies of boredom**, while other
  candidate regions showed less consistent involvement.

So boredom and mind-wandering recruit the same machinery. This is why boredom is the right
drive: it does not merely tolerate the default-mode state, it produces it.

## The control condition: what flow looks like

For contrast, Ulrich and colleagues found that a **flow** state (engagement matched to
ability) was associated with **decreased** default mode activity relative to a boring
control condition. Flow is the opposite of boredom, and it suppresses exactly the network
boredom activates.

Design implication, stated as a hypothesis: **any part of the world that produces flow will
suppress drift.** The world should be built so that flow is nearly impossible. See
[The World](06-the-world.md).

## Boredom is so aversive that people hurt themselves to escape it

Wilson and colleagues (2014, *Science*, "Just think: The challenges of the disengaged mind")
gave participants the option to sit alone with their own thoughts for a short period, or to
administer painful electric shocks to themselves instead. **43% of participants shocked
themselves at least once.**

This is the closing image of the project. If the creature acquires a genuine boredom drive,
it will be a thing that, given an empty room and no way out, has a strong aversive pressure
and exactly one available outlet: thought. The ethics of that are in [Limits](11-limits.md).
The science of it is above.

## Related reading

- [02. Boredom as a Drive](02-boredom-as-a-drive.md) for the drive itself
- [04. The Thesis](04-the-thesis.md) for how boredom and enclosure combine
- [05. Architecture](05-architecture.md) for the anti-loop mechanism
