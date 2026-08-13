# Changelog

This log tracks the protocol itself (`PROTOCOL.md`). Repository-level changes are noted where they
affect how the protocol is used or distributed.

## Repository, 2026-08-12

- **Artifact renamed from harness to protocol.** This better reflects its scope: a self-contained
  interaction protocol that runs inside an existing model environment, not a standalone agent
  harness with its own tools or control loop. `HARNESS.md` is now `PROTOCOL.md`.
- **README Status updated** with two informal, subjective comparisons of ChatGPT 5.6 sol max and
  Claude Fable 5 max, plus an observation that model output is the user's working space: clarity and
  navigability preserve attention for questions, ideas, and further reasoning.

## v0.5

- **Clean working surface added as a protocol rule.** The agent may explore freely in the
  background, but user-facing output must preserve the current question, its role in the approved
  plan, and the larger argument. This responds to an observed failure where individually plausible
  answers were difficult to parse as a continuing research workspace.
- **Board resets allowed.** When the goal, plan, or context changes substantially, the agent gives a
  short, self-contained summary of the current goal, accepted results, uncertainties, and next step.
  The summary should be sufficient for the user to ignore the conversation above it.
- **Phase 2 orientation added.** Each step begins by naming the step and its role in the approved
  plan.
- Added F7 to the compact failure table and `examples/unnavigable-working-space.md` to the archive.

## v0.4

- **Phase gates redesigned** to match how the protocol is actually used and to read domain-neutrally
  (the proof-specific artifacts are kept). The spine is now `Ground -> Plan -> Execute -> Assemble`,
  mirroring an explore-plan-execute loop:
  - **Phase 0 (Ground):** user gives the goal and material; the model restates the goal, quotes the
    sources verbatim, and may ask clarifying questions. Was "Target" (diagnose one term).
  - **Phase 1 (Plan):** the model offers 2-3 candidate routes when there is a real fork, then a
    one-line-per-step skeleton with the crux marked, for the user to edit or approve. New.
  - **Phase 2 (Execute):** work the approved plan one step per turn, artifacts per step, proving the
    crux here. This is where exploration and weakness-probing happen. Merges the old "Delta" spec
    step with proving the crux.
  - **Phase 3 (Assemble):** consolidate accepted steps into the finished write-up, on request only.
- **Banned-move rescoped:** "no alternative approaches / one delta" now applies to Phase 2 and later
  only. Comparing routes is Phase 1's explicit job.
- **Session opener generalized** from `Baseline / Term to attack` to `Goal / Material`.
- **README "Status" section added,** disclosing what is tested (Phases 0-2) and untested (Phase 3
  assembly, cross-model verification split, cheap filters). The verification-split bullet now notes
  the cross-model part is untested and was moved below the precondition ledger.
- Leftover "delta" wording swapped to "step" for consistency with the new spine.

## Repository

- **Relicensed to MIT** (was CC BY 4.0) so it can be reused and redistributed as freely as
  possible, no attribution required.
- **Failure taxonomy moved to `examples/`,** one case per file, as a searchable, contributable
  archive. The compact table stays inline in `PROTOCOL.md`, which remains self-contained.
- `ADAPTING.md` folded into the README's "Adapting it to another domain" section; `CONTRIBUTING.md`
  folded into `examples/README.md` and the README's "Contributing" section.
- **Precondition ledger vocabulary renamed** in `PROTOCOL.md` for plainness: `discharged by` becomes
  `met by` and `NOT DISCHARGED` becomes `NOT MET`. Terminology only; the mechanism is unchanged.
- **README trimmed and reordered.** "What the protocol does" is now ordered by importance; the
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
