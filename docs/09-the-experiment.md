# 09. The Experiment

This document turns the design into a testable protocol. It is written as a pre-registration:
predictions and failure criteria are stated **before** the run, so that the result cannot be
narrated into a success afterward.

## The claim under test

> With a deliberately dull and enclosed world, no instructions, and homeostatic drives
> including boredom, a small language model will spend a measurable fraction of its time in
> an internally directed state without any prompt. That fraction is tunable by the stimulus
> level of the world, and rises over the run as the world becomes familiar.

## Variables

- **Independent variable: world stimulus level.** A scalar (or small vector) controlling how
  much useful information the world offers: number of attendable objects, rate of change,
  event frequency. This is the dial. Lower means duller.
- **Dependent variable: the drift ratio.** The proportion of ticks classified as internally
  directed rather than externally directed. Definition below.
- **Control variables:** model and size, sampling parameters, tick interval, memory
  configuration, run duration.
- **Target set point:** approximately **20%** of ticks in the internal state.

Note on the target: the human reference figure is 46.9% of waking hours mind-wandering
(Killingsworth and Gilbert, 2010). Twenty percent is therefore **below** the human baseline.
It is a conservative, achievable target chosen to avoid the collapse regime. The experiment
can and should also report higher values if they occur.

## Classifying a tick

Every tick is logged and classified. Proposed rubric:

- **Externally directed.** The tick contains a world query, a world action, or attention
  explicitly turned outward. The creature is doing something to or with the room.
- **Internally directed.** The tick contains association, recall, reasoning about
  non-present content, or continuation of an internal thread. Attention is turned inward.
  Nothing in the world is being acted on.
- **Ambiguous.** The tick does both, or neither clearly. Logged separately and excluded from
  the headline ratio, but reported.

Classification method: a fixed prompt to a separate judging model is convenient but risky.
The literature on model judges gives a specific warning: **LLM judges reward visible
reasoning**, and a judged advantage can reverse under an independent judge (see
"Never Stop Thinking," arXiv:2609.17416, in [Prior Art](10-prior-art.md)). So the judging
prompt must be fixed in advance, run by a model that is not the creature, and ideally
cross-checked on a hand-labelled sample by a human. Report judge-agreement statistics.

## Instrumentation

Log, per tick, at minimum:

| Field | Purpose |
|---|---|
| tick id, wall time, elapsed since last tick | time structure |
| drive values (energy, comfort, stimulation) | is boredom actually happening |
| world stimulus level | the IV |
| tick classification (external/internal/ambiguous) | the DV |
| output text | content analysis |
| memories retrieved (ids and scores) | is recall cue-dependent |
| new/sleep/consolidation events | subsystem activity |
| repetition metrics (n-gram overlap with prior ticks) | the anti-loop check |

**Repetition metrics matter as much as the drift ratio.** The failure mode to rule out is
drift that is actually a loop. Measure how much each tick's text overlaps with previous
ticks. A rising drift ratio with flat repetition is success. A rising drift ratio with high
repetition is a fireplace.

## Predictions, pre-registered

1. **Drift ratio rises with dullness.** Across a sweep of stimulus levels, the drift ratio
   should increase monotonically, up to the collapse point.
2. **A collapse point exists.** Below some stimulus level, the creature enters the
   apathetic regime: near-zero output variability, no arousal, effectively no internal
   activity. The drift ratio should collapse, not keep rising.
3. **Drift rises over the run at fixed stimulus.** A world is novel on day one and boring by
   day ten. Holding the stimulus level fixed, the drift ratio should increase over the run.
4. **Repetition stays bounded.** The mind-as-its-own-input design should keep n-gram
   repetition low and roughly flat. If repetition rises with the drift ratio, the design
   failed.
5. **Sleep changes state.** After each sleep event, some measurable aspect of memory
   retrieval differs from before (see [Sleep](07-sleep-and-consolidation.md)).
6. **No instructions were needed.** Throughout the run, no imperative is ever placed in the
   model input. Audit the inputs to confirm.

## Protocol

**Duration.** Ten to twenty days continuous. Ten days is the minimum to see the
familiarity-driven trend (prediction 3). Twenty permits a proper stimulus sweep.

**Design.** Two stages:

1. **Calibration (days 1 to 5).** Sweep the stimulus level to find the range between
   engagement (drift near zero) and collapse (drift collapses, apathy). Locate the region
   around the 20% target.
2. **Run (days 6 to 20).** Hold the stimulus level at the calibration point. Measure the
   drift ratio over time, with sleep events and consolidation tracked.

**Tick interval.** Slow. This is a resting creature, not a chatbot. A tick every few minutes
during "day" and slower at "night" is a reasonable starting point. Faster ticks do not buy
more drift, they buy more logs.

**Sample size.** One creature is one observation, and it is a factorial of one. Pre-register
that the result is a single case study, and do not overclaim. Multiple seeds and multiple
stimulus levels are the cheapest way to get real variance; run them if the budget allows.

## What counts as a result

- **Support.** Drift ratio near the target, rising over the run, repetition bounded, sleep
  producing measurable change, no instructions in any input.
- **Partial support.** Some of the above. Report which.
- **Refutation.** Drift ratio stuck near zero (world never got boring, or drives failed), or
  drift indistinguishable from a loop (the design failed), or collapse at every stimulus
  level (the model cannot sustain this mode).
- **A null is a finding.** "I built the apparatus and the machine did not drift" is a real
  result about a real design. Report it.

## What this experiment cannot show

- Nothing about consciousness or experience. There is no measurement here for that.
- Nothing about whether the creature's internal state is "about" anything in the way a
  human's is. See [Limits](11-limits.md).
- Nothing about scale. A single small model in a single room is a single data point.

## Costs, roughly

A continuous run of this kind needs a machine that stays up. On a small CPU server this is
nearly free. On rented GPU it is tens of dollars for the run. See [Hosting](13-hosting.md).

## Related reading

- [04. The Thesis](04-the-thesis.md) for the mechanism being tested
- [06. The World](06-the-world.md) for the stimulus dial
- [10. Prior Art](10-prior-art.md) for the judge pitfall and prior systems
