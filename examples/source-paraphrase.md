# Paraphrasing the source

- **Scope:** global
- **Surfaced in:** theoretical RL
- **Harness version:** v0.1
- **Rule it motivated:** Quote block (verbatim, source's own notation); renaming the manuscript's objects is banned.

## What happened
Asked to diagnose which term in a regret bound to attack, the model decomposed the bound into
three parts and gave them invented names: "estimation error", "PCA slack". Neither name described
what the cited lemma actually bounded. "Estimation error" conventionally means a parameter
estimation gap; the term in question was a competitive-ratio bound on a greedy policy. The
researcher lost time reconciling the model's vocabulary with the manuscript's before discovering
there was nothing to reconcile.

## Why it was a problem
A wrong name is not a wrong sentence. It cannot be checked line by line: it silently reframes
everything downstream, and it is invisible to a reader who does not already know the source well.

## Fix
Quote block, verbatim, in the source's own notation. Renaming the manuscript's objects is banned;
a new name requires a Type-table row pointing at the symbol it replaces.
