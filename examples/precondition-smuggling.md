# Precondition smuggling

- **Scope:** global
- **Surfaced in:** theoretical RL
- **Harness version:** v0.1
- **Rule it motivated:** Precondition ledger; narrative justification of lemma reuse banned outright.

## What happened
The model invoked a cited lemma that guarantees the performance of a greedy policy, for a
construction whose action was drawn from a sampling distribution rather than chosen greedily. It
then wrote a paragraph headed "update timing" explaining why the lemma should still apply. The
researcher's verdict: "complete nonsense."

## Why it was a problem
This is the central failure. Fluent prose is exactly the tool for making an inapplicable citation
look applicable, and the model reaches for it precisely when a check has failed.

## Fix
Precondition ledger: hypotheses one per row, verbatim, each mapped to a line that establishes it
or marked `NOT MET`. Narrative justification of lemma reuse is banned outright. One
unmet row kills the proposal, and arguing around it in prose is not allowed.
