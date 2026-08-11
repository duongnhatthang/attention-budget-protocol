# Changelog

This log tracks the harness itself (`HARNESS.md`). Repository-level changes are noted where they
affect how the harness is used or distributed.

## Repository

- **Relicensed to MIT** (was CC BY 4.0) so it can be reused and redistributed as freely as
  possible, no attribution required.
- **Failure taxonomy moved to `examples/`,** one case per file, as a searchable, contributable
  archive. The compact table stays inline in `HARNESS.md`, which remains self-contained.
- `ADAPTING.md` folded into the README's "Adapting it to another domain" section; `CONTRIBUTING.md`
  folded into `examples/README.md` and the README's "Contributing" section.
- **Precondition ledger vocabulary renamed** in `HARNESS.md` for plainness: `discharged by` becomes
  `met by` and `NOT DISCHARGED` becomes `NOT MET`. Terminology only; the mechanism is unchanged.
- **README trimmed and reordered.** "What the harness does" is now ordered by importance; the
  domain-generalization claims were softened; duplicated statements of the rule-admission criterion
  were removed.

## v0.3

- **Closing block: purpose stated.** It is primarily a durable log for the assistant and for
  later sessions; secondarily a five-second scan in which the user checks whether anything is
  worth asking about immediately. Moving on without acting is the expected case. Added rules that
  follow from this: front-load the signal, prefer a question the user could simply ask, and keep
  completeness in the standing log rather than the block.
- Clarified that the block is never a task assignment.

## v0.2

- **Closing block rewritten.** Fields are now WEAKEST LINK / IF YOU CHECK ONE THING / GAPS, capped
  at 70 words. The previous version accumulated session-local references and replayed the full
  list of open items, which made it undecodable to the researcher returning after a few days
  (failure F6). NOT CHECKED and FAILS HERE FIRST were merged: in practice they were answering
  the same question from two angles, and the distinction cost more than it returned.
- **Standing log separated.** Open items live in one maintained file. Messages report a count and
  a delta rather than restating the list.
- **F6 added** to the failure table: session-local notation in summaries.
- **De-domained.** Manuscript-specific references removed so the file can be used as-is in other
  projects; ADAPTING.md added.

## v0.1

Initial version. Phase gates, quote block, type table, precondition ledger, banned moves,
closing block, verification split. Written after a review session in which a 287-line generated
document was found to contain a type error, an invalid lemma reuse defended in prose, and
invented terminology for the manuscript's own objects.
