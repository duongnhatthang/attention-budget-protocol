# Failure archive

This is the optional memory behind the protocol. Every rule in [`PROTOCOL.md`](../PROTOCOL.md)
exists because of a specific failure in a real session; this directory records those failures in
full, one per file, so a rule is never removed without knowing what it was for.

**You do not need this directory to use the protocol.** `PROTOCOL.md` is self-contained and carries
a compact table of these same failures inline. This archive is for the things a single pasted
file is bad at: holding more cases than fit in a prompt, being searched or grepped across tasks,
and letting people add cases without editing a shared file.

## Using it

- **Reading:** browse the case files; each is a self-contained failure, named after what it is.
- **In a tool-driven agent:** grep the directory for the failure shape you are worried about
  (`type`, `precondition`, `notation`, `working space`, ...) and read the matching case.
- **In a Claude Project or custom GPT:** attach the specific cases relevant to your task, or the
  whole directory, as project knowledge alongside `PROTOCOL.md`.

## Adding a case (this is the most valuable contribution)

Copy [`_template.md`](_template.md) to a new file named after the failure (e.g. `type-error.md`,
`precondition-smuggling.md`) and fill it in. Do not number the files: names describe, numbers just
force coordination. A good case answers:

1. **What happened**: what the model produced that was wrong.
2. **Why it was a problem**: the cost to the verifier, or how it corrupted what came after.
3. **Fix**: the rule that should have caught it: an existing `PROTOCOL.md` rule, or one you propose.

Guidance:

- **Redact anything unpublished.** The *shape* of the error is what matters, not your mathematics.
  A case stripped to its structure is more useful, not less.
- **Set `Scope` honestly.** `global` if the failure mode is domain-general; otherwise name the
  domain. The public archive ships global cases; keep project-specific ones with your own project.
- **Rules are added reluctantly.** A rule earns its place only by making unverifiable output
  harder to produce or easier to spot. Rules that merely make output *look* rigorous (confidence
  scores, status tags, self-assessments) have been tried and do not work: they are generated as
  fluently as the errors they are meant to flag. If your case motivates such a rule, it probably
  is not the right rule yet.

A port of the protocol to a domain other than mathematics is the second most valuable
contribution. See "Adapting it to another domain" in the [top-level README](../README.md).
