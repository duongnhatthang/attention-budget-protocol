# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is **not a software project**. There is no code, no build, no test suite, no lint, and no
dependencies to install. The repository ships a single prompt-engineering artifact, a *harness*
(`HARNESS.md`), as plain Markdown, together with the docs that explain, adapt, and version it.

The harness is a discipline for a one-human, one-model collaboration in a domain with **no
automatic verifier** (e.g. proving a bound, deriving an identification result). The human is the
only verifier and their attention is the scarce resource. The harness exists to make the model's
errors *cheap to find*, not to make the model more accurate. That goal is prerequisite to editing
anything here: every rule must make unverifiable output harder to produce or easier to spot.

## The single-file principle

`HARNESS.md` is THE product: the one self-contained file a user pastes into project instructions.
It must work with **zero** other files present, which is why it keeps a compact failure table
inline. Everything else in the repo is optional support. Never introduce a dependency from
`HARNESS.md` onto another file in the repo.

## File roles

| File | Role |
|---|---|
| `HARNESS.md` | **The product.** The pasteable, self-contained prompt. Versioned (currently v0.3). |
| `README.md` | Motivation, the economics of verification, how to use it, adapting to other domains, the (non-rigorous) maintenance process, contributing, license. |
| `LICENSE` | MIT. |
| `CHANGELOG.md` | Harness version history plus repository-level notes. Every rule change records *why*, keyed to an observed failure. |
| `examples/` | Optional, searchable failure archive: one case per file, each named after the failure (no numbering), plus a `_template.md` and a `README.md` that doubles as the contributing guide. |
| `CLAUDE.md` | This file. |

## How the harness is structured (edit with this in mind)

`HARNESS.md` encodes a fixed mechanism. Its load-bearing parts:

- **Phase gates (0→3):** Ground / Target / Delta / Proof. Advance only on the user typing `GO`.
  Each phase has a hard line budget; exceeding it is itself a failure.
- **Three mandatory artifacts:** Quote block (verbatim sources), Type table (ontology check),
  Precondition ledger (cited-result hypotheses, each `discharged by` a line or `NOT DISCHARGED`).
- **Banned moves, one-delta rule, standing log, and the closing block** (`WEAKEST LINK /
  IF YOU CHECK ONE THING / GAPS`, 70-word cap).
- **Failure table (F1–F6):** each numbered failure is a real observed incident. The compact table
  lives inline in `HARNESS.md` (its F-numbers are a curated in-prompt list, not a file-naming
  convention); the full write-up of each lives in `examples/`, named by the failure (e.g.
  `examples/type-error.md`).

**Invariants (do not weaken without an explicit user decision):** the phase gates, the length
budgets, the one-delta rule, and the ban on arguing around an undischarged precondition. Per the
README's "Adapting" section, these *are* the mechanism; the mathematical vocabulary around them is
not.

## Conventions when changing the harness

- **Rules are added reluctantly.** A rule earns its place only by making unverifiable output
  harder to produce or easier to spot. Reject additions that merely make output *look* rigorous
  (confidence scores, status tags, self-assessments): they are generated as fluently as the errors
  they flag.
- **Every rule ties to a failure.** A new or changed rule should reference the failure it prevents
  (an existing case or a new `examples/` case), with the reasoning recorded in `CHANGELOG.md`. When
  a new failure case is added to `examples/`, keep the inline table in `HARNESS.md` in sync if the
  case is sharp enough to belong there. Adding a case is cheap and encouraged; changing a global
  rule in `HARNESS.md` is a higher bar and is done conservatively, only after the maintainer has
  tested the change in their own workflow.
- **Keep `HARNESS.md` de-domained and self-contained.** Domain-specific guidance belongs in the
  README's "Adapting" section, not in `HARNESS.md`.
- **Bump the version** in `HARNESS.md`'s title and add a `CHANGELOG.md` entry for any behavioral
  change to the harness.

## Writing style

Prose here is dense and deliberately plain: front-loaded, no filler, no manufactured rigor. Match
it. Per the user's global rule, **never use em dashes**: use commas, colons, parentheses, or
separate sentences (en dashes `--` for ranges are fine). Note some pre-existing files still contain
em dashes; do not mass-edit them unless asked, but keep new prose free of them.
