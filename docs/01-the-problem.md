# 01. The Problem

## A model does nothing until it is handed something

A language model is a function. Give it a sequence, it returns a probability distribution
over the next token. It has no state between calls. It has no clock. It does not run when
you are not calling it. The weights do not move. The context window does not accumulate
unless you carry it yourself.

This is not a limitation of scale. It is the shape of the thing. A frontier model and a
laptop model have the same property: compute happens on the forward pass, and the forward
pass happens when someone asks.

So when we say an agent "runs," we mean something specific and slightly dishonest. We mean a
loop, written by a human, that repeatedly constructs a string and calls the model. The
"agent" is the loop. The model is a component inside it, turned on and off.

## Therefore every current agent has an instruction

Because the loop must decide what to send, and because the loop author wants a particular
behavior, the string almost always contains an imperative. Somewhere in it there is a
sentence telling the model what it is and what to do. This is true of the naive case ("You
are a helpful assistant") and of the sophisticated case (a system prompt with a role, a
goal, a tool list, and a persona). The vocabulary changes. The structure does not.

The consequence is subtle and rarely stated: **a system that is told what it is has never
had to find out.** Its behavior tells you about the instruction, not about the system. We
then read the output and call it a personality.

## What the field calls "thinking" is more forward passes

Recent models ship with explicit "thinking modes." It is worth being precise about what
these are: additional forward passes over a prompt you already sent, generating intermediate
tokens before a final answer. Useful, and real, for reasoning benchmarks. But it is
prompt-triggered, it is bounded by the task, and it stops the moment the answer is produced.
It is not idling. It is a longer answer.

One 2026 paper is instructive here. "Thinking Inertia: LLMs Keep Thinking When Told Not To"
(arXiv:2610.11765) found that models cannot reliably suppress visible reasoning when
instructed to, and that this "thinking inertia" grows as the answer space opens. It is
tempting to read this as evidence of spontaneous thought. It is not. It is the residual
echo of the training distribution, an involuntary tic, not a mind continuing on its own.
Involuntary is not spontaneous. A twitch is not a thought.

## Agents between calls are dead, and we designed them that way

Look at any deployed agent framework. When the user stops typing:

- the process may still be alive, but it is blocked on input
- the memory store is untouched
- no computation happens that was not scheduled by an external event
- the model is idle in the ordinary sense: nothing is being computed

The system has been designed so that the human is the only source of motion. Every pulse
originates outside. This is efficient, and it is exactly why these systems cannot have an
inner life of any kind. An inner life, if the phrase means anything, is motion that
originates inside.

## The honest statement of the problem

We have built systems that are computationally dormant whenever no one is speaking to them,
driven by instructions, with no internal source of pressure that would produce motion in the
absence of a task.

If the goal is to observe anything like idle cognition, all three of those properties have
to change:

1. **Something must run in the absence of input.** Not necessarily fast, not necessarily
   expensive, but continuously.
2. **The instruction must go.** What replaces it is not nothing. It is pressure. See
   [Architecture](05-architecture.md).
3. **There must be an internal source of motion.** A reason to do anything that does not come
   from a human. See [Boredom as a Drive](02-boredom-as-a-drive.md).

The rest of this dossier is about those three changes, and about the fact that the third one
has a well studied human analogue that we can borrow from directly.

## Related reading

- [02. Boredom as a Drive](02-boredom-as-a-drive.md) for what replaces the instruction
- [10. Prior Art](10-prior-art.md) for the systems that have partially solved this
- [11. Limits](11-limits.md) for why the solved parts may not compose into a mind
