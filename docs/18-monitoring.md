# 18. Monitoring

How to watch the creature without ruining the experiment. This is a design document, not
code.

## Three principles

1. **The log is the artifact.** Whatever the website shows is a convenience. The durable,
   append-only record on the study machine is the data, and it is what analysis reads.
2. **The observer must not perturb the creature.** A view that can write back, or that the
   creature can see, is a loop and possibly a seam. Monitoring is one direction only.
3. **Alerts beat dashboards.** Over twenty unattended days, a page you have to remember to open
   will not get opened. The failure that costs you the run is the silent one at three in the
   morning. The dashboard serves curiosity; the alerts serve the science.

## Layer 1: the record

Append only. Never mutate. One line per tick, on the durable volume. Minimum fields:

| Field | Why |
|---|---|
| tick id, wall clock, elapsed since previous tick | time structure, and gaps reveal restarts |
| drive values (energy, comfort, stimulation) | shows whether boredom is actually rising |
| world stimulus level, and web-versus-room phase | the independent variable |
| tick classification, judge score, judge identity | the dependent variable |
| output text | content analysis and the repetition measure |
| memories retrieved, with ids and scores | is recall cue-dependent |
| repetition metrics (n-gram overlap with prior ticks) | the anti-loop check |
| planted-item hits | the causal handle |
| sleep events, consolidation summaries | subsystem activity |
| errors, retries, model identity and parameters | provenance |

The whole study is one file plus its indices. It should be replayable.

## Layer 2: the view

### Recommended architecture

**The box writes. The site reads. Nothing goes the other way.**

1. The study machine appends to the record, and on a schedule (every 30 to 60 seconds) derives
   a **compact snapshot**: current drives, last N ticks, the running series behind each chart,
   counters, health.
2. The snapshot is pushed **outbound** to static hosting. On AWS that is S3, or Cloudflare R2
   since `kkaleb.com` is already served by Cloudflare Pages.
3. The website fetches the snapshot client side and renders it. Static, cacheable, no server,
   effectively free.

**Cadence.** The creature ticks every ten minutes, so a one minute snapshot is already
effectively real time. There is no reason to want anything faster, and wanting it faster is a
sign the design has drifted.

### Why this shape

- **No inbound ports.** The study machine opens no listener. There is nothing to attack and
  nothing to misconfigure, which matters for a box that runs unattended for weeks.
- **The view cannot perturb the creature.** Enforced by architecture rather than by discipline.
- **The view is not part of the world.** The creature never sees it. If it ever did, the
  observer would have become a world event.

A read-only endpoint tunneled from the box is possible and is deliberately not recommended. It
adds an attack surface to buy live-ness that a ten minute tick does not need.

## What the view should show

Ordered by how much each one contributes to understanding.

1. **The drift ratio over time.** The primary endpoint, forming live. Plot it against two
   reference lines: the 20 percent target, and the human figure of 46.9 percent of waking hours
   spent mind-wandering. This is the money chart.
2. **The repetition metric.** The alarm that matters most. If n-gram overlap between ticks
   climbs, the creature has collapsed into a loop and the experiment is failing. You want to
   know that on day two rather than discovering it on day twenty.
3. **The stimulation variable.** The headline number. When stimulation falls, the tick should
   turn inward. That relationship on screen is the phenomenon.
4. **Phase and exhaustion countdown.** Web regime versus room regime, and how much corpus
   remains. The predicted transition is the interesting event, so it should be visible coming.
5. **The stream.** Newest first: time, the thought, an internal or external badge, and the
   drives inline. This is the part that is simply interesting to read.
6. **The planted-item tracker.** Twenty planted items against their matched controls, and
   whether each has resurfaced. The causal handle, made visible.
7. **Sleep events.** As markers, plus the wake-difference summary. A creature that wakes
   different is the cleanest falsifiable claim in the study.
8. **Health.** Last tick age, restart count, disk, error count. Unglamorous and necessary.

## Alerts, which matter more than the view

Push these, and do not wait to be looked for. Telegram is the natural channel for this
project.

| Trigger | Why it matters |
|---|---|
| No tick for N intervals | The run may be silently dead. This is the most likely way to lose twenty days |
| Repetition above threshold | The predicted failure mode, and it is recoverable early and not late |
| Drift ratio out of band for a sustained period | Either the mechanism is working or the world is wrong; both need a decision |
| Disk or volume above threshold | The record is the experiment. Running out of disk destroys it |
| Model load or inference errors | Usually a config drift, best caught in minutes |
| Wake-difference absent after sleep | Consolidation is not doing anything, which invalidates a secondary endpoint |

**The test of the alerting.** If the run dies at 3am, you should know before morning, without
having opened a page.

## Design: it belongs on the desk

`kkaleb.com` is the `ode` portfolio, served by Cloudflare Pages: the desk at 1am, one night
photograph, manila folders you hover and open, no scrolling, everything else drawn. A creature
monitor should be **a folder on that desk**, not a separate dashboard.

Which means it should follow the same rule every round of that site has followed: less text and
less chrome. Concretely, resist the dashboard:

- **One number.** The current drift ratio.
- **One line.** The ratio over time, with the two reference lines.
- **One column.** The stream.
- Everything else behind a fold, for when you actually want it.

The temptation will be to build a control room. A control room will not be opened. A folder
looks like something he already opens.

## Safety and publication

- The view is public, so nothing sensitive may be on the machine. The design already requires
  this: local model, local corpus, at most a keyless weather API, no secrets on the box.
- The snapshot is derived data only. Never export model weights, credentials, or anything with
  a path to the host.
- If the study ever becomes sensitive, move the view behind Cloudflare Access rather than
  hardening the machine.

## The one rule that matters

**Read-only, one direction, enforced by architecture.** The moment the view can write to the
creature, or the creature can see the view, the observation is part of the world and both the
experiment and the interpretation are compromised.

## Related reading

- [The Experiment](09-the-experiment.md) for what the record must contain
- [Architecture](05-architecture.md) for the tick loop that produces it
- [Hosting](13-hosting.md) for the machine, and for why the box opens no ports
