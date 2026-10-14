# 05. Architecture

This document describes the proposed architecture. It is a **proposal**, not a description of
anything built. Where a component maps to established work, the reference is given.

## The four commitments

1. **No instructions.** The system never receives an imperative. There is no "you are," no
   "you should," no goal handed down.
2. **Drives instead of orders.** Homeostatic variables supply pressure. Nothing else.
3. **The mind is its own input.** Each moment transitions from the last.
4. **The world owns every fact.** The model owns only intent and interpretation.

## Component overview

```
        +-------------------------------+
        |            WORLD              |
        |  room, objects, time, weather |
        |  body state, consequences     |
        |  THE ORACLE: owns all facts   |
        +---------------+---------------+
                        |
                        | observed state (ground truth, never model-authored)
                        v
        +-------------------------------+
        |           TICK LOOP           |
        |  1. read world + body         |
        |  2. read self state (prev)    |
        |  3. retrieve memory           |
        |  4. compose a STATE, not     |
        |     an instruction            |
        |  5. model predicts what the   |
        |     next moment is (not       |
        |     "what should I do")       |
        |  6. output = thought + intent |
        |  7. intents are queries to    |
        |     the world; world answers  |
        |  8. write thought to memory   |
        |  9. self state := output      |
        +---------------+---------------+
                        |
                        v
        +-------------------------------+
        |            MEMORY             |
        |  decay, association, recall,  |
        |  reconsolidation, forgetting  |
        |  sleep consolidation          |
        +-------------------------------+
```

## 1. No instructions: what replaces the imperative

The naive objection is that removing an instruction removes the input, and a model needs an
input. True. You cannot send nothing. But the distinction that matters is not whether there
is an input. It is whether the input is an **order** or a **state**.

The system composes a description of the present moment, not a request. Something shaped
like:

- this is the room, as the world reports it
- this is your body: energy, comfort, stimulation
- this is where your last thought ended
- these are the memories that came to you

No verb in the imperative mood. No role. No goal. The model's task is **prediction**: given
this moment, what is the next moment. The output is what the creature perceives, thinks, and
intends. Action is an emergent property of the prediction, not a commanded one.

Whether an instruction-tuned model can be used this way without it sneaking a role back in is
an **open question** and probably the first practical obstacle. See
[11. Limits](11-limits.md). A base model, or a lightly tuned one, is more likely to comply
than an assistant-tuned one, which has been trained specifically to answer requests.

## 2. Drives instead of orders: the homeostatic layer

A small set of variables, each with a target range, each decaying or accumulating on its own:

- **energy**: falls with activity, needs restoring
- **comfort**: moves with temperature and posture and similar body facts
- **stimulation**: falls in a dull world, is the boredom variable

When a variable leaves its range, the system has a **deficit**. The deficit is not an
instruction. It is a state of the system, and the creature's prediction is shaped by it. The
behavioural output of a deficit is whatever reduces it, which depends entirely on the
environment. See [Boredom as a Drive](02-boredom-as-a-drive.md) for why this is the mechanism
that makes drift possible.

Design note: **drives must be able to go unsatisfied.** A world where every deficit is
trivially discharged teaches the creature that deficits do not matter. Friction is required.
See [The World](06-the-world.md).

The formal adjacency here is the free energy principle (Friston, 2010, *Nature Reviews
Neuroscience*): a system that maintains itself by minimizing surprise about its own
experience. This dossier does not claim the implementation is an instance of that principle.
The resemblance is noted because the "pressure not order" structure is the same idea.

## 3. The mind as its own input

The single most important implementation decision. Detailed in
[The Thesis](04-the-thesis.md). Restated as an engineering rule:

> At tick N, the input to the model MUST include tick N-1's output. The system MUST NOT
> reconstruct a canonical prompt from scratch each tick.

If the input is identical across ticks, the output is identical across ticks (modulo
sampling). This is the root cause of every repetitive agent. The fix is not a better prompt.
It is a different composition rule.

Additional novelty sources to include in every tick:

- body variables that have moved since the last tick
- real elapsed time
- memory recall keyed on the current (evolving) state, so recall is not constant
- optionally, sampling randomness, as an explicit entropy source

## 4. The world as the oracle

The model may not author facts about the world. Every observation is a **query** to the
simulation, which returns ground truth. The model says "I look at the table." The world
returns "a cup, half full, gone cold." The model can misinterpret the cup. It cannot invent
it.

This is the design that provides genuine exogenous input without a camera. It solves the
problem raised in [11. Limits](11-limits.md): without a channel the model did not author, the
creature rests inside a photograph. The simulation is that channel. It is not perception in
the biological sense, but it satisfies the requirement that matters: **information enters
from outside the model.**

## 5. The sleep trigger

Sleep is triggered by internal state, never by a clock. Candidate signals, from
[Sleep](07-sleep-and-consolidation.md): memory pressure, rising prediction error, and
contradiction detection. See that document for the procedure and the test.

## 6. Ownership split, stated plainly

| Concern | Owner | Never |
|---|---|---|
| What is in the room | World simulation | Model |
| What time it is | World simulation | Model |
| Body state | Homeostatic layer | Model |
| What the creature intends | Model | World |
| What the creature concludes | Model | World |
| What gets remembered | Memory system, driven by salience | Model, directly |
| When to sleep | Drive layer | Clock |

## 7. What this is not

- Not a goal-directed agent. There is no task.
- Not a decision-maker. Actions are prediction, not policy.
- Not a chatbot. There is no interlocutor. Human input, if any, is just more world.
- Not a consciousness claim. See [Limits](11-limits.md).

## Related reading

- [06. The World](06-the-world.md) for the oracle in detail
- [07. Sleep](07-sleep-and-consolidation.md) and [08. Memory](08-memory.md) for the subsystems
- [09. The Experiment](09-the-experiment.md) for instrumentation
