# Compressed derivation

- **Scope:** global
- **Surfaced in:** theoretical RL
- **Protocol version:** v0.5 and v0.6
- **Rule it motivated:** Preserve the checkable chain in calculations and derivations.

## What happened

The model put a complete parameter calculation into one display. It moved from the target to a
convenient sufficient condition, introduced abbreviations, chose a squared-log candidate, asserted
two logarithmic bounds, and substituted the result back into the original notation without pausing
between those jobs.

One asserted bound used the generic step
\(\log(t+1) \leq \log(2t)\). The required fact \(t \geq 1\) was not shown. Establishing it was not a
single cosmetic line: it required assumptions on the original parameters and a short chain of
lower bounds. The compact display hid both the missing condition and where it entered.

A milder instance later appeared in an otherwise clear derivation. After verifying that a tuned
parameter was admissible under one condition, the proof moved to "the complementary regime" and
used a trivial fallback bound. The conclusion was correct, but the transition did not restate the
negated condition or its equivalent form needed by the next inequality. The user had to return to
the earlier condition, negate it, and perform the rearrangement before the final implication was
clear.

## Why it was a problem

The output was short but not cheap to verify. The user had to unpack the display, identify which
lines were reductions and which were definitions, infer why the candidate had its chosen shape,
and reconstruct the omitted side argument. Elementary algebra was doing logical work, so skipping
it transferred the cost of finding the gap to the user.

The issue was not length alone. A longer revision became easier to check because it separated the
subgoal, assumptions, definitions, candidate motivation, local bounds, verification, and final
substitution.

## Fix

State the subgoal and any sufficient reduction in prose. Separate conceptual moves instead of
placing the whole derivation in one display. Give each non-obvious inequality its reason where it
first appears, and retain any intermediate chain that establishes a condition needed later.

Routine algebra may still be compressed, but only with a short local note saying what was omitted
and why the stated assumptions make the compression valid. When choosing a candidate bound or
ansatz, explain its shape briefly and verify the inequality it must satisfy before using it.

At a case or regime transition, restate the condition defining the new branch. If a fallback bound
combines with a rearrangement of that condition, show the rearrangement or name it next to the
conclusion it supports.
