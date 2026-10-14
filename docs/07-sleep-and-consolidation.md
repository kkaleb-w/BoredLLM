# 07. Sleep and Consolidation

## Sleep is not a pause. It is a job

In humans, sleep is an active process with a specific function: the transfer and
reorganization of experience. During rest, the hippocampus reactivates and replays
sequences of recent experience (see Foster and Wilson, 2006, on replay in hippocampal place
cells, and the broader literature on sharp-wave ripple replay). Over time, recent episodic
traces are moved into more durable storage, and the day's events become dispositions rather
than events.

This is the single most defensible subsystem in the whole dossier, because the human target
is well characterized and the machine analogue is implementable.

## The requirement: self-triggered, never clock-triggered

Sleep must be triggered by the creature's own state, not by a timer. A timer makes sleep an
external event. An internal trigger makes it a consequence of the creature's life.

Candidate trigger signals, all internal:

1. **Memory pressure.** The fast store is filling. This is the closest analogue to the
   intuition that a day has been long.
2. **Rising prediction error.** The world has stopped making sense, and the discrepancy
   between expected and observed needs digesting. This is the analogue of a mind that keeps
   working on a problem overnight.
3. **Contradiction.** Something in memory conflicts with something new. This is the specific
   itch that wakes a person at three in the morning. It maps directly onto the conflict
   detection that memory systems already implement (see [Memory](08-memory.md)).

A proposal: sleep fires when some combination of the above crosses a threshold for a period,
not on a single spike. The threshold is a tunable parameter and part of the experiment.

## What sleep does

Sleep is the maintenance pass. Proposed sequence:

1. **Stop external input.** The world is not observed during sleep.
2. **Replay.** Re-present a compressed sample of the period's memories, prioritizing high
   salience and unresolved items. This is the analogue of hippocampal replay.
3. **Consolidate.** Merge duplicates, strengthen associations between co-occurring items,
   detect and resolve contradictions, promote the strong, decay the weak.
4. **Prune.** Let the weakest traces fall below threshold and be removed. Forgetting is a
   feature, not a bug. See [Memory](08-memory.md).
5. **If weight updates are available, train on the replayed set.** This is the analogue of
   systems consolidation and is the point at which the creature can genuinely change rather
   than merely remember. See [Prior Art](10-prior-art.md) for the methods (agentic
   test-time training, self-distillation of verified experience).

## The test: it must wake different

This is the cleanest falsifiable claim in the dossier.

> A creature that sleeps must be **measurably different** on waking from the state before
> sleep. If nothing changed, it did not sleep. It paused.

Ways to measure "different":

- the distribution of memories it retrieves for the same query, before and after
- the associations that now exist in the memory graph (new edges, resolved contradictions)
- the pruning record (what was forgotten)
- if weights move: a held-out behavioural probe, before and after

Any of these can show a difference, or fail to. Either result is informative: a creature that
wakes identical has told you its sleep is cosmetic.

## Sleep quality as a variable

Two failure modes, mirroring the sleep literature on humans:

- **Sleep that does nothing** (no consolidation, no change). The creature is effectively
  photostatic. It accumulates without reorganizing.
- **Sleep that destroys** (over-aggressive pruning, catastrophic drift in weights). The
  creature wakes with less than it had, or wakes a different creature.

The design target is the middle: sleep that reorganizes without erasing, the way human sleep
consolidates without losing the day.

## Relation to the boredom mechanism

Sleep and boredom interact in a way worth noting. A long waking period in a dull world
accumulates a large boredom deficit and a large store of low-salience experience. Sleep is
where the low-salience material is pruned and the period's few meaningful items are
consolidated. So the creature's days get thinner over time, which is itself a mechanism for
increasing boredom over a run. See the prediction in
[The Experiment](09-the-experiment.md).

## Related reading

- [08. Memory](08-memory.md) for the store that sleep operates on
- [09. The Experiment](09-the-experiment.md) for how to measure wake-difference
- [10. Prior Art](10-prior-art.md) for the machine methods that make consolidation real
