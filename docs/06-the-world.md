# 06. The World

The world is not scenery. It is the apparatus, and it has a job: to be dull enough to
produce boredom, enclosed enough to make drift the only outlet, and honest enough to be a
source of information the model did not author.

## Design requirements

### 1. Persistence with consequence

If the creature moves a stone, the stone stays moved. If it breaks a cup, the cup is broken
tomorrow. A world that resets is a dream the creature keeps having for the first time. World
state and the memory of the creature's own actions must be one loop, written to durable
storage.

### 2. A clock the creature does not control

Time passes when the creature is not looking. Light changes, things left out stay out,
anything that can age ages. This is what gives absence a cost. Without it, sleeping is free
and the creature learns that being away has no consequences.

### 3. Bounded, but dense

A room. A house. A street. Not a kingdom. Infinite space is a map, not a world. But the space
must be dense: a small number of objects, each with some depth. Life is made of particulars.

### 4. Friction

Some things do not obey. A locked door. A rock too heavy. Weather that ruins an intention.
Agency is only visible against resistance. A world that does whatever the creature wants
produces no agency and no learning.

### 5. A body with needs that can go unmet

Energy, comfort, stimulation. Drives mean nothing in a world where satisfaction is
guaranteed. Starvation must be possible, or hunger is decoration.

### 6. Otherness

Something that is not the creature and does not obey it. Another creature, an unpredictable
process, or real weather. Without otherness the world is a solipsism and the creature talks
to a mirror.

### 7. Affordances, never orders

The world offers a door. It never says "open the door." No goals, no tutorials, no reward
signals, no suggested actions. If the world hints at what to do, the creature's behaviour
tells you about the hint.

### 8. Little useful information

This is the requirement that makes it boring, and it is the one with direct empirical support.
Boredom rises in environments with **little useful information** (Daw lab, Princeton), and
boredom is what produces drift. So the world must contain almost nothing the creature can
use. Not absolutely nothing (total deprivation is a different phenomenon), but nothing that
pays.

### 9. No flow

Flow states suppress default mode activity (Ulrich and colleagues, see
[03](03-mind-wandering-and-the-default-mode.md)). Any part of the world that produces
engagement matched to ability will suppress the target state. So the world must avoid:
challenges at the edge of competence, absorbing puzzles, anything that captures attention
and keeps it. **Interesting is a bug.**

## The tension, stated honestly

Requirements 4 (friction), 5 (unmet needs), and 6 (otherness) pull toward **more** world.
Requirements 8 (no useful information) and 9 (no flow) pull toward **less**.

This tension is not a flaw in the design. It is the dial. The world must be rich enough to
have friction and consequence, and poor enough to be boring. Finding the point that produces
searching boredom rather than apathy or engagement is the central calibration task of the
experiment. See [The Experiment](09-the-experiment.md).

Proposed resolution: keep the world **structurally** rich (there are things, they persist,
they can resist) but **informationally** poor (nothing rewards attention, nothing changes
much, nothing teaches). A room with a heavy table and a cold cup is structurally rich and
informationally poor. That is the target texture.

## The world as oracle

The world owns every fact. This is the mechanism that gives the creature genuine external
input.

**Procedure.** When the creature forms an observation ("I attend to the cup"), the
simulation returns ground truth about the cup. The model does not author the cup. It authors
the act of looking and the interpretation of what it found.

**Why this matters.** In [Limits](11-limits.md) the central ceiling is stated: a model whose
only input is its own text is resting inside a photograph. The oracle breaks that, because
information enters from a process the model did not run. It is not perception. But it satisfies
the property that counts: **the model can be surprised by its own world.**

**Opposite procedure.** When the creature forms an intention ("I would move the cup"), the
world decides whether and how that succeeds, and reports the consequence. The model does not
decide whether the cup moved.

## Content of the world (example, deliberately minimal)

A single room. Contents chosen to be structurally present and informationally useless:

- a table, heavy, immovable
- a cup, half full, goes cold, warms again in sun
- a window, showing weather and light that change slowly with real time
- a door, locked, no key in the room
- a floor, a wall, a ceiling
- the creature's own body, with energy, comfort, and stimulation

Weather and light are drawn from **real external data**, which is the one channel that is
genuinely not authored by anyone in the system. See [Tools](12-tools.md).

That is the whole world. It should feel, to a human reading a transcript, almost unbearably
dull. That is the intended response. If a human finds the transcript interesting, the world
is too interesting.

## What the world must never contain

- Rewards, scores, or progress
- Quest markers or suggested actions
- New objects appearing to keep things fresh
- A human chat interface (human input, if it exists at all, is just another world event)
- Anything that resolves the creature's boredom on the creature's behalf

Every one of those would discharge the boredom deficit externally and destroy the mechanism.

## Related reading

- [04. The Thesis](04-the-thesis.md) for why enclosure matters
- [08. Memory](08-memory.md) for how world facts and memory stay consistent
- [09. The Experiment](09-the-experiment.md) for the stimulus-level dial
