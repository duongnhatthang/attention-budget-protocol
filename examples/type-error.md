# Type error

- **Scope:** global
- **Surfaced in:** theoretical RL
- **Harness version:** v0.1
- **Rule it motivated:** Type table, filled before any proof is written.

## What happened
The model wrote `\hat B_n := x_n`, where `\hat B_n` is a `d x m` matrix representing an estimated
shared subspace and `x_n` is a vector.

## Why it was a problem
Trivial to catch when looked for, invisible when embedded in three pages of confident prose. It
invalidated the construction it appeared in.

## Fix
Type table, filled before any proof is written: every object gets a row for its space/shape,
constraints, what it is measurable with respect to, and who chooses it. A mechanical check that
catches this class of error in seconds.
