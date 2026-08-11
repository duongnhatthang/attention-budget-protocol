# A harness for AI-assisted reasoning in domains with no verifier

Formal mathematics has Lean. Software has tests. Empirical machine learning has benchmarks.
Large parts of research have none of these.

If you are proving a regret bound, deriving an identification result, checking a mechanism-design
argument, or reasoning about a model whose consequences cannot be simulated, then **you are the
verifier.** There is no oracle. Nothing fails loudly. An argument that is wrong looks exactly
like an argument that is right until an expert reads it carefully.

This is the setting large language models are worst suited to and most eagerly used in. The
dominant failure is not incoherence. It is *fluent, plausible, and wrong*, and the cost of that
failure is paid entirely out of the researcher's attention.

## The economics that motivate the design

Three properties of human verification drive everything here.

**1. It is expensive, and the expense is expert time.** Nobody else can check the work. It cannot
be batched, delegated, or automated.

**2. It is noisy.** Experts miss things, especially in long documents, especially when the prose
is confident and the notation is unfamiliar.

**3. Its cost is superlinear in output length.** This is the property that matters most and the
one that is usually ignored. A 300-line document is not ten times more expensive to check than a
30-line one. Errors compound: one bad step at line 40 invalidates everything downstream, but the
reader does not know that until they reach line 40, and by then they have paid for the whole
thing. Worse, a document long enough to be tiring gets skimmed, and skimming is how a wrong
result gets believed.

Together these imply that the objective is **not** to produce a proof. The objective is to
produce **the smallest object the researcher can check**: and then stop, and wait.

## What the harness does

It does not make the model more accurate. It makes the model's errors cheap to find.

- **Phase gates.** Work advances only when the researcher explicitly says so. Each phase has a
  hard length budget. Exceeding the budget is treated as a failure in itself.
- **Verbatim quoting.** External results are quoted in the source's own notation, with exact
  numbering. Anything that cannot be quoted is marked `CANNOT QUOTE` and blocks the step. No
  paraphrase, no renaming, no "essentially says".
- **A type table.** Every object gets a row: space, shape, constraints, what it is measurable
  with respect to, who chooses it. Mechanical, boring, and it catches dimension errors in
  seconds.
- **A precondition ledger.** Every cited result has its hypotheses listed one per row, verbatim,
  each mapped either to the line of the new construction that establishes it or to
  `NOT DISCHARGED`. One undischarged row kills the proposal. Arguing around it in prose is
  banned; this is the single most common way a bad step survives.
- **A closing block.** Every substantive message ends by naming its own weakest point and the one
  thing worth checking.
- **A verification split.** The model is not a reliable checker of work produced in the same
  conversation. Completed results go into a fresh context (ideally a different model) carrying
  only the statement and proof, for an adversarial prove-or-disprove pass.

## What it is not

It is not a proof assistant and it produces nothing machine-checkable. It is a discipline for a
collaboration in which one party generates cheaply and the other verifies expensively, designed
so that the generating party cannot quietly spend the verifier's budget.

It has been used seriously in one research project by one researcher. There is no controlled
evaluation, no benchmark, and no claim that it generalizes. It is published because the failure
mode it addresses is common and the fix was not obvious.

## Files

**You only need one file: [`HARNESS.md`](HARNESS.md).** Everything else is optional support.

| File | What it is |
|---|---|
| [`HARNESS.md`](HARNESS.md) | The harness. The single self-contained file: paste it into project instructions or a system prompt. |
| [`examples/`](examples/) | Optional archive of the observed failures each rule exists to prevent, one per file. Searchable; contribute by adding a file. |
| [`CHANGELOG.md`](CHANGELOG.md) | Version history and the reason for each change. |

## Using it

Paste `HARNESS.md` into the project instructions of a Claude Project, a custom GPT, or the system
prompt of whatever you use. Keep a copy in the project's files so you can diff versions as you
tune it.

Then open each session with:

```
Baseline: <exact numbered result you are building on>
Term to attack: <one term>
Phase: 0
```

The gates only bind if you enforce them. If the model runs past a phase boundary without your
GO, that is the signal that the harness has failed and needs another rule, or that the rule it
broke was unrealistic. Both are useful information; please open an issue.

## Adapting it to another domain

The harness is written in the vocabulary of mathematical proof, but nothing in its structure is
specific to mathematics. It applies wherever three conditions hold: no automatic verifier exists
for the claims; only a domain expert can check the work; and the expert's attention is the binding
constraint on progress. That covers theoretical economics, mechanism design, statistical
methodology, parts of theoretical biology and physics, formal policy analysis, and legal argument.

**What to change** is only the domain-specific surface:

| Component | Mathematics | General form |
|---|---|---|
| Baseline | a numbered theorem | the specific prior result, model, or claim being modified |
| Quote block | lemma statements, verbatim | the source text of every result, definition, or datum relied on |
| Type table | space, shape, measurability | what kind of thing each object is: its units, domain, admissible values, who fixes it |
| Precondition ledger | hypotheses of cited lemmas | the conditions under which each borrowed result is valid |
| Phase 1 target | one term in a bound | the single quantity, mechanism, or claim under attack |
| Cheap filters | symbolic algebra, stress tests | whatever partial check the domain admits: dimensional analysis, a unit test, a toy simulation, a limiting case |

**The core mechanism, changed only conservatively.** The phase gates, the length budgets, the
one-delta rule, and the ban on arguing around an undischarged precondition are not domain-specific;
they are the whole point. They are not frozen: I am willing to revise even these. But they are what
has made the process work so far, so I change them only after testing a modification in my own
workflow first, never speculatively. The type table is the component most often assumed to be
mathematics-only. It is not: in any domain, "what kind of object is this and what are its
admissible values" is a mechanical check that catches a large class of confident errors before any
reasoning happens.

The one thing to get right: the harness works by making it expensive to produce unverifiable
output and cheap for you to notice when it has. Every rule you add should serve one of those two
goals. Rules that merely make output look more rigorous (status tags, confidence scores,
self-assessments) do not work; they are generated as fluently as the error they are meant to flag.

## How this is maintained

The process for updating this harness is not rigorous, and you should know that before you rely on
it. It is maintained mostly by one person: a researcher working on a theoretical
reinforcement-learning problem, who uses the harness as the project instructions for a Claude
Project. Changes are made largely on vibe, a subjective sense of whether they improve the working
process on the particular problem in front of me. There is no controlled evaluation, no benchmark,
and no A/B test behind any rule.

I hope it generalizes to other domains. I do not know that it does. If you find it useful, the two
contributions I would most value are more failure cases and more rules, each grounded in something
that actually went wrong. Note the asymmetry: failure cases are easy to take on board and cost
nothing to add, so I am quick to accept them. Changes to the global rules in `HARNESS.md` are a
higher bar, and I promote something to a rule only after testing it in my own workflow, because the
current rules are what make the process work and a bad rule is expensive to discover. See below.

## Related work

The design draws on published analyses of how LLM mathematics fails: taxonomies of
citation fabrication and premise smuggling (arXiv 2606.24902), the seven failure modes behind the
QED multi-agent system (arXiv 2604.24021), the human-in-the-loop theorem-proving workflow of Li
et al. (arXiv 2512.09443, prompts at `github.com/optsuite/MathResearchPrompts`), and the
multi-model verification split used by Bolzano (arXiv 2604.16989). The contribution here is
narrower than any of those: a single-file discipline for a one-human, one-model collaboration
where no verifier exists and the human's attention is the binding constraint.

## Contributing

Most valuable: a report of a failure the harness did not catch, added as a case under
[`examples/`](examples/) (copy [`examples/_template.md`](examples/_template.md)), with the rule you
think would have caught it. Second most valuable: a port to a non-mathematical domain, as a change
to the "Adapting" section above or a new set of example cases.

Redact anything unpublished; the shape of the error is more useful than the mathematics itself.
Rules are added reluctantly: a rule earns its place only if it makes unverifiable output harder to
produce or easier to spot. Confidence scores, status tags, and self-assessments have been tried
and do not work, because they are generated as fluently as the errors they are meant to flag.

## License

MIT. Use it, change it, redistribute it freely, no attribution required. See [`LICENSE`](LICENSE).
