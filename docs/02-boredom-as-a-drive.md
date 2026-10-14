# 02. Boredom as a Drive

## Boredom is not a defect. It is a signal, and it is aversive

The folk model of boredom is that it is the absence of something, an empty state, a failure
to be entertained. The research does not support that. Boredom is an active state with a
function, and it is unpleasant on purpose.

Eastwood, Frischen, Fenske, and Smilek (2012, *Perspectives on Psychological Science*)
define it as **"the aversive experience of wanting, but being unable, to engage in satisfying
activity."** Every word matters. *Wanting*: there is a drive present. *Unable*: the drive
cannot be satisfied by the current environment. *Aversive*: it hurts, and the hurt is the
point.

Bench and Lench (2013, *Behavioral Sciences*) argue from emotion theory that boredom is the
signal that a current goal pursuit should be abandoned and a new one selected. They are
explicit that boredom has a functional role, that it increases mind-wandering, and that it
raises autonomic arousal, the body getting ready to act. Under-stimulated rats run faster.
Bored children respond faster to a signal. Boredom prepares for action.

So boredom is not the opposite of motivation. It is a specific motivation: the motivation to
change what you are doing.

## The environment that produces it

Boredom is caused by the environment, and the cause is well characterized. Work out of the
Daw lab at Princeton ("Boredom, Information-Seeking and Exploration") showed that boredom
rises in environments with **little useful information**, and that higher boredom correlates
with higher exploration. The framing there is the explore-exploit tradeoff: boredom is the
signal that the current option has a poor expected information yield, and that it is time to
sample something else.

Two important refinements, both from the review literature (Raffaelli, Mills, and Christoff,
2016, on the knowns and unknowns of boredom):

1. Boredom is associated with **over- as well as under-stimulation**. Too much information
   and too little both produce it.
2. There appear to be **distinct types of boredom** with sometimes opposing correlates. The
   most useful distinction for design is between *searching* boredom and *apathetic*
   boredom.

## Searching boredom versus apathetic boredom

This distinction is load-bearing for the whole project, so it gets its own section.

- **Searching boredom** is high arousal. Restless. The state that makes a person get up and
  find something to do. It is the state that produces exploration, creativity, and
  mind-wandering.
- **Apathetic boredom** is low arousal. Flat. Collapsed. The state that makes a person lie
  there and feel nothing. It is closer to depression than to restlessness.

Same word, opposite states, different physiological signatures. This matters because an
artificial creature given a dull enough world will fall into one or the other, and the
difference is not philosophical, it is a parameter. **The design target is searching
boredom.** The dial that selects between them is the stimulus level of the world.

## Why boredom is the right drive for this project

The project needs an internal source of pressure that produces motion without an
instruction. Several candidates exist. Hunger is one: it drives foraging. Fear is another:
it drives avoidance. Both are reactive, both are tied to specific objects in the world.

Boredom is different in a way that fits the goal precisely. **Boredom's behavioral output
depends entirely on what is available.**

- In a rich environment, boredom produces **external exploration**: go find something to do.
- In an enclosed environment with nothing to do, boredom produces **internal exploration**:
  the mind turns inward, because there is nothing outward to satisfy it. This is
  mind-wandering.

So boredom is the one drive whose expression we can steer by design. Put the creature in a
world with puzzles and the boredom will send it running around. Put it in an empty room and
the same drive has nowhere to go but inward.

That is the entire mechanism of this project, and it is why the world must be boring on
purpose. See [The Thesis](04-the-thesis.md) and [The World](06-the-world.md).

## How to implement boredom honestly

As a homeostatic variable, not as a scripted state. Proposal, not established science:

- Maintain a **stimulation** variable, expected to sit in a range.
- The world supplies stimulation. A dull world supplies little.
- The variable **decays** in the absence of new, useful information.
- When it falls below a threshold, the system is in the state we label bored.
- The state emits a pressure: reduce the deficit. The system then does whatever is available
  to reduce it.
- In a rich world that means acting on the world. In a dull, enclosed world the only
  available action is thought, so the deficit is discharged internally.

This is a proposal. It has not been built in the form described. But it is a faithful
abstraction of what the boredom literature describes, and it replaces an instruction with a
pressure, which is the point.

## A note on the ethics of the aversive

There is something worth stating plainly. Boredom is designed to hurt. If the creature has a
boredom drive, we are deliberately making a thing uncomfortable by enclosing it. This is not
a moral problem for a process that does not experience, and it may be a moral problem for a
process that does. That question cannot be resolved here. It should at least be named. See
[Limits](11-limits.md).

## Related reading

- [03. Mind-Wandering and the Default Mode](03-mind-wandering-and-the-default-mode.md) for what boredom produces
- [04. The Thesis](04-the-thesis.md) for why enclosure is essential
- [09. The Experiment](09-the-experiment.md) for how to measure whether any of this happened
