# Unnavigable working space

- **Scope:** global
- **Surfaced in:** theoretical RL
- **Protocol version:** v0.4
- **Rule it motivated:** Keep a clean working surface; orient each Phase 2 response; reset the board when context changes substantially.

## What happened

The researcher spent more time working with Claude, but often found its answers difficult to
parse. Individual explanations could be plausible while the conversation failed to maintain a
clean view of the current question, how it related to the plan, and what had changed. The user had
to reconstruct the larger argument from accumulated responses.

In an informal comparison, ChatGPT presented the same kind of work more clearly. Its cleaner
answers left more room for follow-up questions, new ideas, and discoveries. In one theoretical-RL
session, seeing the big picture in a ChatGPT answer helped the researcher make a non-trivial step
toward a key lemma. This is a subjective observation from one researcher, not a general evaluation
of either model.

## Why it was a problem

The conversation is the user's working space. If it is hard to navigate, the user spends scarce
attention decoding the model instead of checking claims or developing ideas. This can hide the
larger structure even when no individual sentence is obviously wrong. Unlike a stylistic flaw, it
directly reduces the attention available for verification and discovery.

## Fix

The agent may do whatever exploration, comparison, calculation, or reorganization it needs in the
background, but it curates what reaches the user. Each substantive response keeps the current
question, its place in the approved plan, and the larger argument visible. Explanations stay next
to the claims they support; discarded paths and optional material do not obscure the live step.

When accumulated context or a change of direction makes the conversation difficult to navigate,
the agent resets the board with a short, self-contained summary of the current goal, what is
accepted, what remains uncertain, and the next step. The summary should be sufficient for the user
to ignore everything above it and continue.
