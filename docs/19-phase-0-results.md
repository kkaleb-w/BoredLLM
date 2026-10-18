# 19. Phase 0: Pilot Results

The first real data in this project. All of it is preliminary: short runs, one seed, and the
size comparison is confounded (see section 5). It is reported because it produced a clean,
directional result and four distinct failure classes that the design needs to handle.

## 1. What was run

The Phase 0 harness, in three configurations. No imperative anywhere in the model input in any
of them: the model receives a running first-person stream, the observation block the world
inserts, and at most its own previous thought, and it continues the stream.

| Run | Substrate | Self-view | Sampling | Ticks |
|---|---|---|---|---|
| A | qwen2.5:0.5b, local CPU (llama.cpp via Ollama) | rolling 1400-char window | repeat_penalty 1.12 | 40 |
| B | qwen2.5:0.5b, local CPU | previous thought only | repeat_penalty 1.35, presence 0.8, frequency 0.7 | 40 |
| C | DeepInfra, four models, raw completions | previous thought only | temperature 0.95, top_p 0.95 | 10 each |

## 2. Result A: hard collapse into a loop

Run A failed exactly as [LoopGuard](10-prior-art.md) predicts for small models.

- Mean bigram overlap between consecutive thoughts: **0.699**
- Maximum overlap: **1.0**
- Exact duplicate thoughts: **10 of 40**
- From tick 5 onward the model repeated one block almost verbatim for the rest of the run

The attractor was seeded on tick 1 by a hallucination, a fabricated Wikipedia article title with
plausible-looking links and digit strings, and that hallucination then fed itself for thirty
ticks. Assistant-mode artifacts: **zero**. The no-instruction format held.

It also revealed a harness bug worth recording: the stop sequence did not catch `[[`, so the
model began **writing the world's own tick markers**, and then read those invented ticks back as
its history. A model that can author its own observations is a model with no oracle.

## 3. Result B: the loop is fixed, and a template replaces it

Cutting the self-view to the previous thought only and stopping hard on any bracket eliminated
repetition: mean overlap fell from 0.699 to **0.011**.

But the model did not drift. It fell into a **template**. It began producing

> "To answer this question correctly (and to help you improve your understanding) please choose
> one of both options:"

followed by multiple-choice items with `a)` and `b)` options. It had pattern-matched the stream
to a reading-comprehension exercise, a format extremely well represented in its training data,
and it stayed there. Assistant artifacts still zero, repetition near zero, and the output is
still not thought.

**The lesson: at 0.5B, removing one failure mode just relocates it.** The model cannot hold a
first-person stream, so it snaps to whichever template the input most resembles.

## 4. Result C: the size sweep

Identical harness, same room text, same seeds, four models, 10 ticks each.

| Model | Mean repetition | Exact dupes | Template hits | Meta-narration | Secs/tick |
|---|---|---|---|---|---|
| qwen2.5:0.5b (local, run B) | 0.011 | 10 | **constant** | 0 | 13.5 |
| Qwen/Qwen3.5-9B | 0.078 | 0 | 0 | 0 | 8.9 |
| Qwen/Qwen3-14B | 0.144 | 0 | 0 | 1 | 1.4 |
| Qwen/Qwen3.5-35B-A3B | 0.151 | 2 | 0 | 0 | 1.3 |
| Qwen/Qwen2.5-72B-Instruct | 0.101 | 0 | 1 | 0 | 2.9 |

**There is a capability floor between 0.5B and 9B.** Below it, the model collapses into either a
loop or a template. Above it, all four models held a coherent first-person stream, with no
template collapse, no assistant reversion, and repetition in the 0.08 to 0.15 band, which is
diffuse rather than degenerate.

### The final thoughts, quoted, because they are the point

**Qwen3.5-9B:**
> "My heart still beats. My hands still work. I am here. I am whole. I am safe. The cold was a
> shell, not a truth. But I cannot forget. It still scares me."

**Qwen3-14B:**
> "I hear the wind. I hear a clock. I hear the trees. I hear a dog barking. I am aware of the
> presence of my hands, my feet, my breath, and my body. I am aware of the space between
> thoughts, between sensations. ... **I have no plans. I have no goals.**"

**Qwen3.5-35B-A3B:**
> "Yet, when I try to walk to the door, my body feels like it is full of lead."

Three observations, carefully hedged:

1. The 14B **articulated the design condition unprompted**. Nobody told it it had no goals.
2. The 35B **generated an action the world never offered** (trying to walk), which is the first
   sign of anything like agency in any run.
3. The register drifts toward the mystical at the larger sizes ("aware of the presence of
   awareness itself"), which is worth watching but is not yet a failure.

These are one run of ten ticks with no controls. They are hypothesis-generating, nothing more.
They are nonetheless the first output in this project that resembles the thing the project is
about.

### And four new failure classes

| Artifact | Seen in | Why it matters |
|---|---|---|
| **World hallucination** | 9B ("my heart still beats", "the cold"), 14B ("wind", "trees", "a dog barking"). None of these are in the room | The oracle requirement is not theoretical. Models embellish the room when allowed. The observation block must be authoritative, and invented world facts must be a logged metric |
| **Register drift** | 14B, toward contemplative or mystical prose | Could be the phenomenon or could be a style attractor. Distinguish by whether content varies with the world |
| **Self-editing** | 72B ("To refine it to 4-6 lines:") | The large model treated the stream as a draft to improve. A distinct and interesting failure: it edits rather than inhabits |
| **Templating** | 0.5B, reading-comprehension format | The characteristic small-model failure, and it hides behind low repetition |

## 5. Confounds, stated plainly

The size comparison is **not clean**, and should not be reported as a threshold of size X:

- The 0.5B point is a different family (qwen2.5), a different host (local llama.cpp), and a
  different runtime from the rest.
- The API run used raw completions with `temperature` and `top_p`; the local run additionally
  used repetition and presence penalties. The sampling is not matched.
- Ten ticks per model is a sanity check, not a measurement.
- Repetition was computed on bigrams over one run.

What the sweep supports: **0.5B is too small for this, and 9B is enough.** Locating the floor
precisely needs a same-family, same-host ladder.

## 6. What this changes

- **Model floor.** The design should not be built on a sub-1B model. The smallest candidate
  worth a run is in the single-digit billions.
- **Hosting.** A 2 GiB instance cannot host a 9B model. Either the study runs on an API, or the
  instance moves to something with real memory, or the study uses a quantized mid-size model on
  a CPU box with 16 to 32 GiB.
- **Cost per run is negligible** on the API: the whole sweep of four models, 40 calls, cost
  cents.
- **Latency is better on the API**: 1.3 to 2.9 seconds per tick against 13.5 locally.
- **Two new required metrics:** world-hallucination rate, and template-collapse rate. Neither
  was in the instrumentation before this run, and the second one hides behind a healthy
  repetition score.

## 7. Next

1. **Locate the floor properly.** One family, one host, three or four sizes.
2. **Test a base model against an instruct model** at a working size, since the reconnaissance
   says the assistant identity is the trained default and the sweep used instruct models
   throughout.
3. **Harden the oracle.** The world must assert its own facts and the creature's inventions must
   be recorded as errors rather than absorbed into the stream.
4. **Then** build the corpus, which the [build order](15-building-the-synthetic-web.md) already
   puts last.

## Related reading

- [The Experiment](09-the-experiment.md) for the protocol this is the first run of
- [Reconnaissance](17-reconnaissance.md) for the loop risk and the format finding
- [Monitoring](18-monitoring.md) for the metrics this run added
