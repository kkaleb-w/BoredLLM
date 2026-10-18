# 15. Building the Synthetic Web

A specification for the world described in [The Web as World](14-the-web-as-world.md). It
covers how large the corpus must actually be, how to build it without rebuilding the mirror,
and what must be true of it before the creature is ever put inside.

**Proposal, not established.** Nothing here has been built.

## 1. How large does it actually need to be

The size is computable, and the answer is smaller than it sounds. Assume two page reads per
tick, on average, and a re-read rate of about 50 percent.

| Tick interval | Ticks/day | 20 days: unique pages | 50 days: unique pages |
|---|---|---|---|
| 2 min | 720 | 10,000 to 29,000 | 25,000 to 72,000 |
| 5 min | 288 | 4,000 to 11,500 | 10,000 to 29,000 |
| 10 min | 144 | 2,000 to 5,800 | 5,000 to 14,400 |
| 15 min | 96 | 1,300 to 3,800 | 3,400 to 9,600 |
| 30 min | 48 | 700 to 1,900 | 1,700 to 4,800 |

At a 10 minute tick over 50 days, the requirement is roughly **5,000 to 14,000 unique pages**.
If every page carried 500 words of substance that is 2.5 to 7 million words, on the order of
30 to 80 novels. That number is why the naive approach fails.

## 2. Why that number is the wrong number

Real webs are not uniformly dense. They follow a power law: a small number of dense hubs that
carry most of the meaning, and an enormous number of thin leaves that are mostly links, a
paragraph, and a dead end. That is what it feels like to browse, and it is also cheap to build.

**Design rule: build the shape of a web, not the volume of one.**

- **Hubs.** A few hundred pages of real substance, the open loops and contradictions the
  creature is meant to chew on. This is the authored part and it is where the effort goes.
- **Stubs.** Thousands of thin pages: a title, a sentence, a handful of links. Procedurally
  generated is fine here, because their job is to exist, to link, and to make the place feel
  bottomless. Nobody reads a stub for content.
- **Links.** Arbitrary rather than semantic. Real link structure is partly accidental. Fully
  semantic linking makes the graph too predictable and too navigable, which removes the
  getting-lost that makes browsing a behavior worth observing.

A worked estimate for a 10 minute tick, 50 day run: 300 hub pages at 1,500 words is 450,000
words, plus 12,000 stubs at 40 words is 480,000 words. Under a million words total, and the
structure reads as a much larger place than it is.

## 3. The generation problem, and why an LLM-only web fails

The obvious way to build this is to point a language model at the task. That fails a specific
requirement, and the failure is not subtle.

**A language model cannot author an exogenous world for a language model.** Two reasons:

1. **Predictability.** Models are good at predicting other models' text. A generated corpus is
   fluent, plausible, and close to the creature's own distribution, which means little novelty,
   which means little to learn and little to think about.
2. **Exogeneity.** The design requires that the world supply information the model did not
   author, or the creature rests inside a mirror. If the world is written by the same class of
   system as the creature, the requirement is unmet. The creature's world would be written by
   its cousin.

So the corpus must contain things the creature cannot predict. Sources of genuine
unpredictability, in rough order of value:

- **Real human text.** Authored by people, drawn from sources the model did not write. The
  strongest option, and the correct one for the hub pages.
- **Real-world data with actual entropy in it.** Weather records, measurements, timestamps,
  real numbers, real documents. Genuinely not authored by anyone in the system.
- **Procedural generation from a real random seed.** Structure from code, content from
  entropy. Good for stubs and link topology.
- **Model generation.** Acceptable only for connective tissue, navigation, boilerplate. Never
  for the substantive hubs.

**The split that works:** structure by machine, substance from the world. The generator's real
job is to build a *place*, not to write it.

Related trap: an instruction like "write an interesting page" produces answers. The corpus
needs open loops, which means the generator must be allowed to leave things unresolved and to
produce material that is boring, repetitive, and thin. Boring is realism here, not failure. See
[The Web as World](14-the-web-as-world.md) on open loops.

## 4. Frozen, always

The creature browses a **frozen snapshot**, never the live web.

- Reproducibility: the run must be repeatable and replayable.
- Control: no external party can alter the world mid-run.
- Safety: a read-only creature with no outbound path cannot be poisoned, and cannot act.

The world process serves pages locally. The creature has no network. This also means the run
has no secrets to leak and no bill attached to browsing.

## 5. Browsing, not crawling

This distinction decides whether the experiment is worth running at all.

- **Crawling** is traversal of a frontier queue. It is a task. It rebuilds exactly the
  agent-loop structure this project exists to escape, and it will repeat itself, because a
  frontier is deterministic.
- **Browsing** is chosen attention under a drive. The creature decides where to look next, and
  the deciding is driven by interest and boredom, not by a queue.

The world must present **affordances** (here are links, here is a page) and never a next step.
There is no frontier. There is no progress. There is no coverage metric. If the creature is
ever asked to "read the web," the experiment is over.

## 6. What the corpus must contain

- **Open loops.** Unfinished arguments, unanswered questions, claims that contradict each
  other. Material that resists resolution and therefore sustains thought.
- **Contradictions.** Real logical conflicts, not stylistic variation. These double as sleep
  triggers, since a contradiction is one of the internal signals that fires consolidation (see
  [Sleep](07-sleep-and-consolidation.md)).
- **Depth without breadth.** A few subjects followed far enough to become familiar, rather than
  a shallow sample of everything. Familiarity is what makes exhaustion possible, and exhaustion
  is what produces the drift phase.
- **Nothing that resolves the creature's boredom.** No feed, no recommendation engine, no
  "most popular" page, no infinite scroll. Each of those discharges the deficit externally and
  destroys the mechanism.

## 7. What the corpus must not contain

- Instructions, goals, or tasks of any kind
- Anything that rewards attention or tracks progress
- Content that resolves the open loops
- Live or fetchable external references (a URL that works is a hole in the world)
- Anything authored by the creature itself, ever

## 8. The seam

If the frame is used, the seam must be designed rather than discovered. Candidate design: after
a predetermined tick, the world contains something that does not belong, a page that is
internally inconsistent with the rest, or an object that violates the world's own rules. What
the creature does at that moment is the only part of the frame that carries experimental
weight. See [The Web as World](14-the-web-as-world.md).

## 9. Build order, and this is the important part

The web is a prop. The creature is the experiment. Build in this order, and do not scale
anything before the thing downstream of it works.

1. **Pilot the creature.** One room, one page of world, 50 to 100 ticks, no instructions, no
   web. The single riskiest untested assumption in the whole project is whether an
   instruction-free state produces continuation rather than assistant behavior, and whether the
   mind-as-its-own-input mechanism keeps repetition bounded. Both are answerable in an
   afternoon. If this fails, everything downstream is a monument to nothing.
2. **Minimal web.** One hub, a few dozen stubs, one link graph. Enough to browse. Run it for a
   few hundred ticks.
3. **Small web.** A few dozen hubs, a few hundred stubs. Run the room-versus-web contrast.
4. **Full web.** Only now scale to the run length, and only if steps 1 to 3 produced something
   worth continuing.

**The web is the last thing to scale, not the first.** The most expensive mistake available in
this project is to spend days authoring a fifty-day corpus and then discover the creature does
not browse.

## 10. Risk register

| Risk | Severity | Mitigation |
|---|---|---|
| Creature reverts to assistant behavior without instructions | **Fatal** | Pilot first. Consider a base rather than instruct model |
| Drift is a loop in disguise | **Fatal to the claim** | Repetition metrics per tick; pilot before scaling |
| Corpus is model-generated and therefore a mirror | **High** | Real human text for hubs; models only for connective tissue |
| Creature crawls instead of browses | **High** | No frontier, no queue, no progress metric; chosen attention only |
| Web too small, exhausts before the contrast is visible | Medium | Size from the table in section 1, at the chosen tick interval |
| Web too large, exhaustion phase never reached | Medium | Accept it; then the run studies only the switching regime |
| Creature never gets bored on the web | Medium | That is a finding. It would be a null on the boredom theory in this setting |
| Contradictions trip logic errors rather than consolidation | Low | Contradictions should be semantic, not formal |

## 11. Related reading

- [The Web as World](14-the-web-as-world.md) for the corrected science
- [The Experiment](09-the-experiment.md) for measurement and the twenty percent target
- [The World](06-the-world.md) for the room this replaces or precedes
