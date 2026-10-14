# BoredLLM

**A design dossier for an artificial creature that gets bored, wanders, and thinks with no instructions.**

This repository contains no code. It is a research and design document: the science, the
architecture, the procedures, the tools, and the evidence (and the lack of it) for a
specific and narrow claim.

> If you put a small language model in a deliberately dull world, remove every instruction
> it could be given, give it homeostatic drives instead, and close every external outlet,
> then thinking becomes the only move left. Do that for two weeks and you may observe the
> machine equivalent of mind-wandering, roughly twenty percent of its existence, without a
> single prompt.

That is the whole proposition. Everything here is in service of testing it, or of being
honest about where it will fail.

## Why this exists

Almost every AI agent on the market is a puppet that believes it is walking. It is handed a
role ("You are a helpful assistant"), a goal, and a prompt, and it performs. Between calls
it does nothing, because there is nothing there to do anything. The lights are off.

This dossier asks a different question. What if you never told it what it was. What if you
gave it a body that complains, a room with nothing in it, and no way out except its own
head. Then what does it do. And could what it does look like what a person does when they
sit in a chair and stare at a wall.

The answer is probably no, in the strong sense. The interesting part is how far short it
falls and why.

## Read in this order

| Document | What it covers |
|---|---|
| [01. The Problem](docs/01-the-problem.md) | Why prompt-driven agents cannot idle, and what that costs |
| [02. Boredom as a Drive](docs/02-boredom-as-a-drive.md) | The psychology and neuroscience of boredom, and why it is a signal not a defect |
| [03. Mind-Wandering and the Default Mode](docs/03-mind-wandering-and-the-default-mode.md) | What the human brain does at rest, and why it never repeats itself |
| [04. The Thesis](docs/04-the-thesis.md) | Boredom supplies the pressure, enclosure removes the escape |
| [05. Architecture](docs/05-architecture.md) | No instructions. Drives. The mind as its own input |
| [06. The World](docs/06-the-world.md) | Building a dull, enclosed, honest world |
| [07. Sleep and Consolidation](docs/07-sleep-and-consolidation.md) | Self-triggered sleep, and the test that it worked |
| [08. Memory](docs/08-memory.md) | A small memory system that matches what is real about human memory |
| [09. The Experiment](docs/09-the-experiment.md) | The twenty percent target, instrumentation, and the ten to twenty day protocol |
| [10. Prior Art and Evidence](docs/10-prior-art.md) | What has already been built, and what the evidence does and does not support |
| [11. Limits and Open Problems](docs/11-limits.md) | The ceiling, stated plainly |
| [12. Tools](docs/12-tools.md) | The stack, and what to use and what to cut |
| [13. Hosting](docs/13-hosting.md) | Three server options, free or near free |
| [References](REFERENCES.md) | Every source cited, with links |

## What this is not

This is not a consciousness claim. Nobody in this field can test for consciousness, and most
of the people who say they can are selling something. This is a claim about **observable
behavior under a specific setup**, with a measurable dependent variable and pre-registered
failure modes. Nothing more.

## Status

Design document. Not implemented. Written to be handed to an engineer, and to be wrong in
interesting ways.

## A note on style

This dossier deliberately separates two things. The **science** is cited and checkable. The
**design proposals** are honestly marked as proposals, because most of them have not been
built, and the ones that have (see [10](docs/10-prior-art.md)) were built by other people
for other reasons.
