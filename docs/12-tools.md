# 12. Tools

This document describes the stack. It contains no code, by design. Where a tool is named it
is because it is a reasonable fit for a small, self-contained, always-on creature, not
because it has been tested in this configuration.

## Inference: a small local model

The creature needs a model it can call dozens of times an hour, forever, on cheap hardware.

- **Runtime: Ollama or llama.cpp.** Both run quantized models on CPU and GPU, fully offline,
  with a simple local API. Ollama is easier to operate (model management, a single service).
  llama.cpp gives more control and is lighter. Either works.
- **Model: small and quantized.** The 0.5B to 1.5B class is the right target: for example, a
  Qwen2.5 0.5B or 1.5B, a Llama 3.2 1B, or TinyLlama 1.1B, at 4-bit quantization. The reason
  to go this small is not only cost. It is that weight-level learning during sleep
  ([07](07-sleep-and-consolidation.md)) is tractable at this size, and it is the whole point
  of the memory-versus-change distinction.
- **Base versus instruct.** Prefer a base or lightly tuned model. An assistant-tuned model
  has been trained to answer requests and may try to reintroduce an instruction even when
  none is given. See [Limits](11-limits.md), item 3.
- **Expected throughput.** On a 2 to 4 core CPU server, a 0.5B to 1B model at 4-bit generates
  on the order of ten to twenty tokens per second. For a creature ticking every few minutes,
  this is more than enough. On a rented GPU it is effectively instantaneous.

## Embeddings and the memory store

- **Embeddings.** A small sentence encoder, in the 30M to 140M parameter range (a
  BGE-family small model is the obvious default). The vocabulary of a single-creature world
  is tiny, so a large embedder buys nothing.
- **Store.** SQLite. One file, no service, trivially durable and backed up by copying.
- **Memory engine.** Either adopt **CortiLoop** (`github.com/shenchengtsi/CortiLoop`) for its
  attention gate, consolidation, association graph, decay, and reconsolidation, or implement
  the thin version in [Memory](08-memory.md). CortiLoop is closer to a turnkey fit than most,
  but it is small and new; read it before depending on it. See [Prior Art](10-prior-art.md).

## The exogenous channel

This is the part that must not be skipped. The creature needs at least one input from outside
the model, ideally outside the system.

- **Real time.** Free, always available, genuinely not authored by anyone in the system.
- **Real weather.** The obvious free source is **Open-Meteo**, which offers a no-key API for
  current conditions and forecasts. Weather in the room changes with real weather outside it.
  This is a cheap way to get genuine exogenous novelty into a dull world without making the
  world interesting.
- **Optional, later: audio or video.** A microphone or camera is the strongest version of the
  exogenous channel, and the only route to anything resembling real perception. Out of scope
  for a first build.

## The world simulation

- Anything is fine as long as it owns every fact. A single Python process with in-memory
  state persisted to SQLite is sufficient. No engine, no graphics. The creature's "vision" is
  the simulation's textual report of the room.
- Event randomness should be seeded and logged, so a run is reproducible.
- The simulation's clock must advance when the creature is asleep. See
  [The World](06-the-world.md), requirement 2.

## Scheduling and the loop

- A simple in-process scheduler that fires a tick at a fixed interval, plus sleep logic
  triggered by drive thresholds rather than by the clock.
- The tick must compose the input from the previous output. This is the crux. See
  [Architecture](05-architecture.md).

## Deployment

- **Docker.** One container for the creature, one volume for its state (world, memory
  database, logs). A restart policy so it survives reboots. A supervisor inside the container:
  on start, run a wake routine; on termination, run the sleep routine so state is written
  cleanly. See [Hosting](13-hosting.md).
- **Logs are the experiment.** Every tick, its classification, drive values, retrieved
  memories, and repetition metrics are the data. See [The Experiment](09-the-experiment.md).
  Log to a durable volume from the first tick, not from the first interesting tick.

## The judging model

A separate model instance, run with a **fixed** prompt, that classifies ticks as internally or
externally directed. It must not be the creature. Cross-check a human-labelled sample and
report agreement. See the judge pitfall in [The Experiment](09-the-experiment.md).

## What to avoid

- Cloud LLM APIs. They make the creature dependent on a network and a bill, and they cannot
  be updated locally during sleep.
- Heavy agent frameworks. They bring instructions, which is the one thing that must not enter
  the system.
- Vector databases as a service. A single creature does not need them.
- Anything that supplies a goal. That includes reward signals, scoring, and progress tracking.

## Cost summary

A small CPU server (see [Hosting](13-hosting.md)) runs this for between free and about ten
dollars a month. A rented GPU runs it for tens of dollars over a twenty day experiment. There
is no reason this project needs to be expensive.

## Related reading

- [06. The World](06-the-world.md) for the oracle requirement
- [13. Hosting](13-hosting.md) for where to run it
- [10. Prior Art](10-prior-art.md) for the memory engines
