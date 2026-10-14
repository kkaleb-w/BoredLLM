# 08. Memory

## Keep only what is real about human memory

Most agent memory is a flat vector store with retrieval on top. That is a filing cabinet, not
a memory. Human memory has structure, and most of that structure is functional. This document
keeps the parts with a real function and cuts the rest.

## The five properties worth keeping

### 1. Multiple stores with different decay rates

Human memory is not one thing. At minimum:

- **working**: seconds, tiny, holds what is immediately relevant
- **episodic**: events, dated, decays relatively fast
- **semantic**: facts and generalities, decays slowly
- **procedural**: skills and routines, decays very slowly

The decay rate is the point. What is forgotten quickly is what is not reinforced. The
differential rates are how importance gets decided over time, without a hand-tuned rule.

### 2. Decay (use it or lose it)

Traces weaken without reinforcement, following a forgetting curve shape (associated with
Ebbinghaus). Retrieval strengthens. This is not a limitation to engineer around. It is the
mechanism by which the memory system decides what matters, by exposure rather than by an
explicit importance field alone.

### 3. Reconsolidation (recall rewrites the memory)

This is the strange and important one. In humans, recalling a memory makes it temporarily
labile and it is restabilized with modifications. **Remembering changes what you remember.**

Machine implication: retrieval should not be a pure read. It should write back a slightly
revised trace. This is both more faithful to the human system and, in a mind whose input is
its own output, a genuine source of drift. A memory read twice is not the same memory the
second time.

Caution: reconsolidation done carelessly is corruption. It should be bounded, and original
traces should be preserved (accumulate, do not overwrite).

### 4. Association (spreading activation)

Memories are linked by co-occurrence, by temporal proximity, and by causal relation. Activating
one activates neighbors. This is the mechanism that makes recall cue-dependent and produces
chains rather than lookups.

Machine implication: a graph, not a table. Edges strengthen with co-activation (a Hebbian
rule). Retrieval is multi-probe: a cue activates several seeds, and activation spreads.

### 5. Forgetting as a feature

A system that keeps everything retrieves nothing. Pruning is not data loss to be avoided, it
is the mechanism that keeps retrieval sharp. It runs during sleep (see
[Sleep](07-sleep-and-consolidation.md)).

## What to cut

For a small system on modest hardware:

- **Cross-encoder reranking.** Accurate, and heavy. For a small store, multi-probe retrieval
  with a light reranking heuristic is enough.
- **Large embedding models.** A small text embedding model (for example, a 33M-parameter
  sentence encoder in the BGE family) is sufficient for a single-creature world. The
  vocabulary is tiny and the store is small.
- **Heavy graph infrastructure.** A sparse adjacency structure in memory, persisted to a
  small database, is enough. No graph database needed.
- **Any cloud dependency.** The whole point is a self-contained creature.

Storage: a single small embedded database (SQLite is sufficient). Total footprint, including
a small embedding model, should be on the order of tens to a couple of hundred megabytes.
This fits comfortably even on a modest server. See [Hosting](13-hosting.md).

## Consistency with the world

The world ([06](06-the-world.md)) owns facts. Memory owns the creature's *experience* of
those facts. These must not be allowed to diverge silently:

- A world fact is ground truth. If memory conflicts with the world, the world wins, and the
  conflict is a **contradiction event**, which is a sleep trigger ([07](07-sleep-and-consolidation.md))
  and a reconsolidation event.
- The creature's *interpretations* of world facts are memory, not world, and may be wrong.
  This is correct and desirable. It can be wrong about what the cold cup means. It cannot be
  wrong that the cup is cold.

## Salience: what triggers a write

Not every moment is worth remembering. A write should be gated by salience, which is a
combination of:

- novelty (the moment differed substantially from expectation)
- intensity (a drive variable moved sharply)
- recurrence (this has been seen before, so strengthen rather than add)
- explicit marking (for a human researcher probing the system, an override)

This gate is what keeps the store from drowning in the creature's own dull moments, which,
in a boring world, will be the overwhelming majority of moments. Getting this gate right is
also how the creature's days get *thinner* over a run, feeding the rising-boredom prediction.

## Related reading

- [07. Sleep and Consolidation](07-sleep-and-consolidation.md) for what operates on memory
- [05. Architecture](05-architecture.md) for how memory feeds the tick loop
- [10. Prior Art](10-prior-art.md) for existing implementations to study or adapt
