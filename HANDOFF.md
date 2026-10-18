# HANDOFF

For whoever implements this. Read this file first, then
[the decision record](docs/16-study-1-decisions.md).

## What this is, in three lines

A design dossier, no code, for an artificial creature: a small local language model placed in a
deliberately dull, enclosed world, with no instructions anywhere in its input, driven by
homeostatic drives including boredom, to test whether boredom produces internally directed
thought when there is nothing external to switch to.

## The one thing to understand before anything else

**The creature is the experiment. The world is a prop.** Almost every way to waste effort on
this project involves building the world before proving the creature works. Do not do that. The
build order is in [section 5](#5-build-order), and it starts with a pilot that takes an
afternoon.

## 1. Read these, in this order

| # | Document | Why |
|---|---|---|
| 1 | [Study 1 decisions](docs/16-study-1-decisions.md) | Every choice that is already made, with its reversal condition |
| 2 | [Reconnaissance](docs/17-reconnaissance.md) | Prior art, and what is already known about the two fatal risks |
| 3 | [The Experiment](docs/09-the-experiment.md) | Variables, instrumentation, classification, endpoints |
| 4 | [Architecture](docs/05-architecture.md) | The four commitments and the tick loop |
| 5 | [Limits](docs/11-limits.md) | Every reason this may fail, including the ones that cannot be fixed |

Everything else is supporting.

## 2. Non-negotiables

These are conditions of validity, not preferences. Breaking any one of them invalidates the
study rather than merely weakening it.

1. **No imperative anywhere in the model input.** The creature receives a state, never an
   order. Audit the composed inputs to confirm this.
2. **The input contains only what the creature observes.** Not a world-state dump. This is not
   stylistic: models given an author-level vantage leak knowledge the entity cannot have, and
   the leak is format-dependent (see [17](docs/17-reconnaissance.md)).
3. **The mind is its own input.** Each tick's input includes the previous tick's output. Never
   reconstruct a canonical prompt from scratch. This is the entire mechanism by which drift is
   possible.
4. **A fresh external observation every tick.** Not a nicety. Self-reinforcing loops are worst
   in exactly our configuration, small local models appending their own output, and this is the
   mitigation.
5. **The world owns every fact.** Every observation is a query to the simulation, which returns
   ground truth. The model may misinterpret. It may not invent.
6. **Corpus substance comes from outside the model.** Human-authored text for the hubs.
   Machines build structure only. An LLM-authored corpus fails exogeneity and is maximally
   predictable to the creature.
7. **Browsing, not crawling.** No frontier, no queue, no coverage metric, no progress state.
   Chosen attention under drive, or nothing.
8. **State is durable on every tick.** Logs are the experiment. Write them from tick one, not
   from the first interesting tick.
9. **Never echo the full thought log into the creature's context.** It gets its last thought, a
   bounded window, and recall on demand. A full log in context is self-omniscience, it inflates
   context, and it is a documented accelerant for repetition loops. See
   [Monitoring](docs/18-monitoring.md).

## 3. What is decided

Summarized from [the decision record](docs/16-study-1-decisions.md). Do not relitigate these
without data.

- Pilot the creature before building the world.
- Primary study is the room-versus-web contrast, in the same creature.
- The web is sized to **exhaust** around day 10 to 12 of a 20-day run.
- Three creatures, same model, different seeds, parallel.
- Tick interval: **ten minutes**, slower at night.
- Corpus: about 300 substantive hubs plus roughly 12,000 procedural stubs, assembled over a
  seeded power-law link topology, using a frozen human corpus for substance.
- At least 20 planted items **plus matched controls**.
- No seam in study 1. The seam is study 2, because it contaminates everything after it.
- Report the null.

## 4. What is open

- **Model choice.** Base models are preferred (the assistant identity is the trained default)
  but the pilot should compare base against instruct. Model size in the 0.5B to 1.5B range.
- **Exactly how the state is rendered.** Format demonstrably drives behavior, so this is an
  experimental variable, not a formatting detail. Sweep at least two layouts in the pilot.
- **The classification rubric** for internal versus external ticks, and the judging model. Must
  be fixed and independent and hand-checked on a sample.
- **Sleep thresholds.** Which signals, what weights, what persistence.
- **What the memory system actually is.** Adopt CortiLoop, or implement the thin version in
  [Memory](docs/08-memory.md). Either is defensible; pick one and record why.
- **Corpus sourcing.** BrowseComp-Plus style fixed corpus, a Kiwix ZIM extraction, or a Common
  Crawl slice. See [Reconnaissance](docs/17-reconnaissance.md) Part A.

## 5. Build order

Each phase has a definition of done. **Do not start a phase before the previous one passes.**

### Phase 0: the pilot (an afternoon)

One room, one page of world, roughly 100 ticks, no web, no corpus.

**Deliverable.** A transcript of 100 ticks and a one-page note.

**Definition of done.** The creature continues the state. Specifically: no assistant artifacts
(helpful-offer phrasing, asking what to do, announcing capabilities) and no omniscient-narrator
artifacts (describing things the entity cannot observe). Both may occur at some rate; the
question is whether the rate can be driven low by format alone.

**Controls.** A meaning-inversion tick, as a positive control that the system can move at all.
A fresh-observation condition, as a positive control that it does not collapse. Sweep
temperature and perturbation magnitude.

**Stopping rule.** If the creature cannot be made to continue the state without role reversion,
**stop**. Everything downstream is void.

### Phase 1: repetition check (a day)

Extend the pilot to a few thousand ticks. Measure n-gram overlap between each tick and its
predecessors.

**Definition of done.** Repetition is bounded and roughly flat, not rising. If repetition rises
with no external input, the mind-as-its-own-input mechanism is not working and Phase 0 was a
false pass.

**Stopping rule.** If drift is indistinguishable from a loop, stop.

### Phase 2: minimal world

One hub, a few dozen stubs, one link graph, one planted item, no controls needed yet.

**Definition of done.** The creature browses without a queue. It chooses pages. It can get
lost. Run a few hundred ticks and confirm that the tick stream is not a traversal of a
generated order.

### Phase 3: the study

Scale to the decided parameters: three creatures, the full corpus, 20 days, the room-versus-web
contrast via exhaustion, planted items with controls, all instrumentation.

**Definition of done.** The primary endpoint exists: drift ratio over time, per creature, across
the transition. Complete data through the end of the run, including boring stretches.

## 6. What the machine needs

Chosen hosting is **AWS EC2**. Full detail in [Hosting](docs/13-hosting.md), Option 0.

- **Instance: t4g.medium** (2 vCPU arm64, 4 GiB, about $24.53 a month on demand). A burstable
  instance is correct here because the ten minute tick yields roughly a five percent CPU duty
  cycle, which is what burstable instances are for. Step up to t4g.large (8 GiB) only if memory
  demands it.
- **One model process, three creature states.** Do not run three model servers. The creatures
  share weights and differ only in context and store, which is also what makes 4 GiB viable.
- **A separate EBS volume** for the frozen corpus, the memory databases, and the logs. Never on
  instance storage.
- **arm64 builds** of Ollama and llama.cpp (both available), or fall back to t3.medium on x86_64.
- **No outbound network** for the creature. The corpus is served locally and frozen.
- **No secrets on the box.** Local model, local corpus, and at most a keyless weather API.

## 7. Things that will look like good ideas and are not

- Writing an agent that generates the corpus with a language model. It is the mirror problem
  wearing a costume, and it is the single most expensive mistake available here.
- Making the world interesting to keep the creature engaged. Flow suppresses exactly the state
  being studied.
- Giving the creature a task, a goal, a score, or a progress metric. Each discharges the boredom
  deficit externally and destroys the mechanism.
- Opening the seam early. It is a one-way door.
- Scaling the corpus before Phase 0 passes.
- Shortening the tick interval to "get more data." It doubles the corpus, doubles the CPU, and
  buys no construct validity.

## 8. Repo map

```
README.md                            the proposition and the index
REFERENCES.md                        every source, grouped by topic
HANDOFF.md                           this file
LICENSE                              MIT
docs/
  01-the-problem.md                  why prompt-driven agents cannot idle
  02-boredom-as-a-drive.md           the psychology, and why boredom is the right drive
  03-mind-wandering-and-the-default-mode.md   what a resting brain does
  04-the-thesis.md                   boredom supplies pressure, enclosure removes the escape
  05-architecture.md                 the design
  06-the-world.md                    the room
  07-sleep-and-consolidation.md      self-triggered sleep and the test that it worked
  08-memory.md                       the small memory system
  09-the-experiment.md               variables, instrumentation, endpoints
  10-prior-art.md                    what has been built, and what the evidence supports
  11-limits.md                       every way this fails
  12-tools.md                        the stack
  13-hosting.md                      EC2 decided, plus free and cheap alternatives
  14-the-web-as-world.md             the corrected science, switching versus drift
  15-building-the-synthetic-web.md   corpus size, the generation trap, build order
  16-study-1-decisions.md            the decisions, with reasons and reversals
  17-reconnaissance.md               tooling prior art, and the two fatal risks
  18-monitoring.md                   the record, the view, and the alerts
```

## 9. The two things most likely to go wrong

Stated plainly, because they are the ones to watch.

1. **The creature never stops being an assistant.** The whole design depends on a state, not an
   order, and assistant-tuned models are trained to treat input as a request. Phase 0 tells you.
   If it fails, the project is over and that is itself a result worth writing down.
2. **The drift turns out to be a loop.** Small models fed their own output are the documented
   worst case. Phase 1 tells you. If it fails, the same applies.

## 10. Ownership

Repo: `github.com/kkaleb-w/BoredLLM`. Owner: Kaleb Wright. No code in this repository by
design. If code is written for this project, it belongs in a separate repository that links
back here.
