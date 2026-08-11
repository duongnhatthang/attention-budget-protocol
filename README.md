# A harness for AI-assisted reasoning in domains with no verifier

Formal mathematics has Lean. Software has tests. Empirical machine learning has benchmarks. Large
parts of research have none of these.

If you are proving a regret bound, deriving an identification result, or checking a
mechanism-design argument, **you are the verifier.** There is no oracle, and nothing fails loudly.
A wrong argument looks exactly like a right one until an expert reads it carefully.

This is the setting language models are worst suited to and most eagerly used in. The dominant
failure is not incoherence; it is *fluent, plausible, and wrong*, and the cost is paid entirely out
of the researcher's attention.

## Why attention is the constraint

Three properties of expert verification drive the whole design.

1. **It is expensive, and only an expert can do it.** The work cannot be batched, delegated, or
   automated.
2. **It is noisy.** Experts miss things, especially in long, confident prose with unfamiliar
   notation.
3. **Its cost grows faster than length.** A 300-line argument is far more than ten times harder to
   check than a 30-line one: one bad step at line 40 invalidates everything after it, but you do
   not find that out until you reach line 40. And an argument long enough to tire the reader gets
   skimmed, which is how a wrong result gets believed.

So the goal is not to produce a proof. It is to produce **the smallest object the researcher can
check**, and then stop and wait.

## What the harness does

It does not make the model more accurate. It forces every unsupported step to surface as an
explicit gap instead of hiding in prose, so your attention goes to catching errors rather than
hunting for them. The rules, roughly in order of how much they matter:

- **Phase gates.** Work advances only when you say so, one phase at a time. Each phase has a hard
  length budget, and exceeding it is itself a failure.
- **Verbatim quoting.** Every external result is quoted in its source's own notation and numbering.
  Anything that cannot be quoted is marked as such and blocks the step. No paraphrase, no renaming.
- **A precondition ledger.** Before reusing a result you rely on, its conditions are listed one per
  row, and each is matched to the exact line that satisfies it or flagged as unmet. One unmet
  condition stops the step, and talking around it in prose is banned. This is the single most
  common way a wrong step survives.
- **A verification split.** A model cannot reliably check work it just produced; its confidence in
  its own output is meaningless here. So a finished result is meant to be carried into a fresh
  conversation, ideally a different model, with only its statement and proof, for an adversarial
  prove-or-disprove pass. (The cross-model part is a recommendation I have not yet tested; see
  Status.)
- **A type table.** Every object gets a row saying what kind of thing it is and what values it may
  take. Mechanical and boring, and it catches whole classes of confident error before any reasoning
  starts.
- **A closing block.** Every substantive message ends by naming its own weakest point and the one
  thing worth checking.

## What it is not

It is not a proof assistant and produces nothing machine-checkable. It is a discipline for a
collaboration where one side generates cheaply and the other verifies expensively, arranged so the
generating side cannot quietly spend the verifier's budget.

It has been used seriously in one project by one researcher. There is no controlled evaluation and
no claim that it generalizes. It is published because the failure mode is common and the fix was
not obvious.

## Status

This has been used in one setting, a theoretical RL proof effort, and much of it is untested.
Concretely:

- **Phases 0-2 (Ground, Plan, Execute):** used regularly. This is the tested core.
- **Phase 3 (Assemble the final artifact):** rarely reached. In practice the work stayed in Phase 2,
  digging into steps and weaknesses, and never consolidated a finished write-up through the harness.
- **Verification split with a different model:** untested. That a model cannot reliably check its
  own work still holds; carrying a result to a *different* model is a recommendation I have not run.
- **Optional cheap filters (symbolic checks, stress tests, simulation):** untested.

Treat the untested parts as hypotheses, not established practice.

## Files

**You only need one file: [`HARNESS.md`](HARNESS.md).** Everything else is optional.

| File | What it is |
|---|---|
| [`HARNESS.md`](HARNESS.md) | The harness. Self-contained: paste it into project instructions or a system prompt. |
| [`examples/`](examples/) | Optional, searchable archive of the failures each rule exists to prevent, one per file. |
| [`CHANGELOG.md`](CHANGELOG.md) | Version history and the reason for each change. |

## Using it

Paste `HARNESS.md` into your project instructions or system prompt, and keep a copy with your work
so you can diff versions as you tune it. Then work one phase at a time: name what you are building
on and the single thing to improve, and let the model stop at each phase boundary until you tell it
to go on. `HARNESS.md` ends with a short opener template for starting a session.

## Adapting it to another domain

Nothing in the structure is specific to mathematics. It should fit any area where the same three
conditions hold: no automatic verifier, only an expert can check the work, and that expert's
attention is the bottleneck. I have only used it in mathematics, so treat other domains as
untested.

To port it, swap the mathematical specifics for your domain's equivalents: the prior result you are
modifying, the source text you quote, the check on what each object is and what values it can take,
and the conditions under which each borrowed result holds. Keep the machinery unchanged.

**The core mechanism, changed only conservatively.** The phase gates, the length budgets, the
one-change-per-session rule, and the ban on arguing around an unmet condition are not
domain-specific; they are the whole point. They are not frozen, but they are what has made the
process work so far, so I change them only after testing a modification in my own workflow, never
speculatively.

## How this is maintained

The process for updating this harness is not rigorous, and you should know that before relying on
it. It is maintained mostly by one person: a researcher on a theoretical reinforcement-learning
problem who uses the harness as the instructions for a Claude Project. Changes are made largely on
vibe, a subjective sense of whether they improve the work in front of me. There is no controlled
evaluation behind any rule.

I hope it generalizes; I do not know that it does. The asymmetry that follows: failure cases are
cheap to add and I take them readily, but changing a global rule is a higher bar and happens only
after I have tested it, because a bad rule is expensive to discover.

## Related work

The design draws on published analyses of how LLM mathematics fails: taxonomies of citation
fabrication and premise smuggling (arXiv 2606.24902), the seven failure modes behind the QED
multi-agent system (arXiv 2604.24021), the human-in-the-loop theorem-proving workflow of Li et al.
(arXiv 2512.09443, prompts at `github.com/optsuite/MathResearchPrompts`), and the multi-model
verification split used by Bolzano (arXiv 2604.16989). The contribution here is narrower: a
single-file discipline for a one-human, one-model collaboration where no verifier exists and the
human's attention is the binding constraint.

## Contributing

Most valuable: a failure the harness did not catch, added as a case under [`examples/`](examples/)
(copy [`examples/_template.md`](examples/_template.md)) with the rule you think would have caught
it. Second: a port to a non-mathematical domain.

Redact anything unpublished; the shape of the error matters more than the mathematics. Rules are
added reluctantly. A rule earns its place only if it forces an unsupported step to surface as an
explicit gap or makes an error cheaper to spot. Confidence scores, status tags, and self-assessments
do not qualify: a model generates them as fluently as the errors they are meant to flag.

## License

MIT. Use it, change it, redistribute it freely, no attribution required. See [`LICENSE`](LICENSE).
