<!-- Save this as <slug>.md, named after the failure (e.g. type-error.md). Do not number it. -->
# <Short name of the failure>

- **Scope:** global | project: <slug>   <!-- "global" if the failure mode is domain-general; otherwise name the domain it is specific to -->
- **Surfaced in:** <domain where it was observed, e.g. theoretical RL>
- **Harness version:** <version in use when it happened, e.g. v0.3>
- **Rule it motivated:** <the HARNESS.md rule that now guards against it, or "proposed">

## What happened
<The thing the model produced. Concrete. Quote the offending output if short.>

## Why it was a problem
<The cost to the verifier: why this is expensive to catch, or how it corrupts everything downstream.>

## Fix
<The rule that was added or changed in response, or the rule you propose.>
