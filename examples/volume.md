# Volume

- **Scope:** global
- **Surfaced in:** theoretical RL
- **Harness version:** v0.1
- **Rule it motivated:** Phase gates with hard length budgets; one delta per session; no document before Phase 3.

## What happened
287 lines, two theorems, six lemmas, a general template, an optional variant, and a preview of two
further stages, produced before any of it had been checked. The first error appeared early.
Everything after it was wasted.

## Why it was a problem
The cost of verification is superlinear in length, and the expected value of the tail of a long
document is near zero, because it is conditional on every preceding step being right.

## Fix
Phase gates with hard length budgets; one delta per session; no document written before Phase 3 is
reached and requested.
