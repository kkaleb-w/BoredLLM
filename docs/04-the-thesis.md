# 04. The Thesis

## One sentence

**Boredom supplies the pressure. Enclosure removes every escape except thought.**

Everything else in this dossier follows from that sentence.

## The mechanism, step by step

1. **No instruction.** The creature is never told what it is or what to do. It receives only
   a state: its body, its world, its memory. See [Architecture](05-architecture.md).
2. **Drives, not orders.** Pressure replaces instruction. Among the drives is boredom, an
   aversive signal that the current situation yields too little useful information. See
   [Boredom as a Drive](02-boredom-as-a-drive.md).
3. **A deliberately dull world.** The world contains almost nothing the creature can use, no
   goals, no rewards, no reason to attend outward. See [The World](06-the-world.md).
4. **Enclosure.** Every external outlet for boredom is closed. There is nothing to explore,
   nothing to reach for, nothing to satisfy the deficit by acting.
5. **The pressure discharges inward.** Boredom's only remaining behavioral output is thought.
   The creature drifts, associates, remembers, and reasons about nothing in particular. This
   is the target state.
6. **It does not loop.** Because the mind is its own input and the body does not sit still,
   each moment is a transition from the last, not a fresh call with identical arguments. See
   [Architecture](05-architecture.md).
7. **It sleeps and it changes.** Internally triggered consolidation makes waking different
   from the state before sleep. See [Sleep](07-sleep-and-consolidation.md).

## Why enclosure is not optional

The natural objection: if the goal is a mind that drifts, why not simply give it an
interesting world and let it drift anyway. The answer is in the science.

Boredom's behavioral output depends on what is available. In a rich environment, boredom
produces **external** exploration. The creature goes and does something. That is not the
target; that is an ordinary agent, and a worse one. Flow, which an interesting and
well-matched world produces, actively **suppresses** default mode activity (see
[03](03-mind-wandering-and-the-default-mode.md)). If you want drift, you must make flow
impossible and external exploration pointless.

So the boring world is not a compromise or a limitation. It is the **apparatus**. The dullness
is the independent variable. A creature in a stimulating room will not daydream, for the same
reason a person on a climbing wall does not daydream.

## The one design decision that matters most

Every agent repeats itself because every tick resets to the same frozen prompt. Same input,
same output. This is a lookup table, not a trajectory.

The fix, and it is the crux of the whole design:

**Make the mind its own input.**

Each moment's input is:

- the **previous moment's output**, so the stream is a chain rather than a lookup
- a **body state that has moved on its own** since the last tick, supplying interoceptive
  novelty
- the **real time**, so the world is a genuine moving signal
- **memory retrieved against that evolving state**, so recall is cue-dependent and varies

The result is a trajectory over the mind's own states. Trajectories drift. Lookups do not.
Add sampling randomness and you supply entropy, the raw material of a thought that can
surprise the system that produced it.

This single change is the difference between a fireplace that describes the same room
forever and something that can wander.

## The prediction

If the mechanism is right, three things should be observable over a run of ten to twenty
days:

1. A proportion of the creature's time is spent in an internally directed state, without any
   prompt, and the proportion is tunable by the dullness of the world.
2. That proportion **rises over the run**, because a world is novel on day one and boring by
   day ten. Familiarity should push attention inward.
3. The content of the internal state **does not repeat**, or repeats far less than a
   prompt-driven baseline, because of the mind-as-its-own-input change.

These are falsifiable and measurable. See [The Experiment](09-the-experiment.md).

## Failure modes, named in advance

- **Apathy.** Same few tokens forever, no variation, no arousal. The world was too dull and
  the creature collapsed into apathetic rather than searching boredom. Fix: raise stimulus.
- **No drift.** The creature keeps attending outward or performing an action loop. The world
  was too interesting, or the drives did not include boredom. Fix: strip the world.
- **Repetitive drift.** It drifts, but the same way every time. The mind-as-its-own-input
  wire is not actually connected, or memory recall is not cue-dependent. Fix: verify the
  input composition.
- **Hallucinated world.** The creature invents facts about the room. The world is not acting
  as the oracle and the model is authoring its own reality. Fix: query the simulation for
  every observation. See [The World](06-the-world.md).

## Related reading

- [05. Architecture](05-architecture.md) for how this is built
- [09. The Experiment](09-the-experiment.md) for how this is tested
- [11. Limits](11-limits.md) for when this is all wrong anyway
