# An attention-budget protocol for AI-assisted reasoning

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

The final goal is a useful and truthful research product. The protocol's immediate operating goal
is narrower: produce **the smallest object the researcher can check**, then stop and wait. This
also leaves room for exploration. A useful intermediate object may be a proof step, a
counterexample, a short list of genuinely different routes, or a diagnosis of why the current
route is stuck. What matters is that it advances a research decision without hiding its evidential
status or consuming more verification attention than necessary.

## What the protocol does

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
- **A clean working surface.** The model may reason freely in the background, but what it presents
  must keep the current question and larger plan easy to see. When the context changes substantially,
  it gives a self-contained summary that lets the user ignore the earlier conversation and continue
  from a clear board.
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
  digging into steps and weaknesses, and never consolidated a finished write-up through the protocol.
- **Verification split with a different model:** untested. That a model cannot reliably check its
  own work still holds; carrying a result to a *different* model is a recommendation I have not run.
- **Optional cheap filters (symbolic checks, stress tests, simulation):** untested.

Two informal comparisons suggest different strengths. On the same math problem, both models
reached the same conclusion, but ChatGPT 5.6 sol max explained it more clearly than Claude Fable 5
max. On an unrelated health-planning task, Claude pushed back more readily. ChatGPT became critical
only when asked, then found holes in Claude's arguments that Claude accepted, while the reverse did
not happen. These are subjective observations, not an evaluation.

Clarity matters beyond style. The model's output becomes the working space that the user reads,
revisits, questions, and builds on throughout the session. If that space is hard to parse, attention
is spent navigating the answer instead of reasoning. I have spent more time working with Claude,
but often find its answers harder to parse. ChatGPT's cleaner presentation has left more room for
follow-up questions, new ideas, and discoveries. In one case, seeing the big picture clearly in a
ChatGPT answer helped me make a non-trivial step toward a key lemma. What the model presents should
therefore be organized carefully and remain easy to understand and navigate.

Treat the untested parts as hypotheses, not established practice.

## Files

**You only need one file: [`PROTOCOL.md`](PROTOCOL.md).** Everything else is optional.

| File | What it is |
|---|---|
| [`PROTOCOL.md`](PROTOCOL.md) | The protocol. Self-contained: paste it into project instructions or a system prompt. |
| [`examples/`](examples/) | Optional, searchable archive of the failures each rule exists to prevent, one per file. |
| [`CHANGELOG.md`](CHANGELOG.md) | Version history and the reason for each change. |

## Using it

Paste `PROTOCOL.md` into your project instructions or system prompt, and keep a copy with your work
so you can diff versions as you tune it. Then work one phase at a time: name what you are building
on and the single thing to improve, and let the model stop at each phase boundary until you tell it
to go on. `PROTOCOL.md` ends with a short opener template for starting a session.

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

The process for updating this protocol is not rigorous, and you should know that before relying on
it. It is maintained mostly by one person: a researcher on a theoretical reinforcement-learning
problem who uses the protocol as the instructions for a Claude Project. Changes are made largely on
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
verification split used by Bolzano (arXiv 2604.16989).

These projects address related problems at different layers:

- **MathResearchPrompts** provides task-specific prompts for research exploration, theorem proving,
  construction, numerical screening, and production of mathematical artifacts. Its associated work
  describes a human--AI interactive workflow. This protocol instead governs the interaction itself:
  what may be produced at each stage, when the human must approve progress, and how unsupported
  steps are exposed before they expand into a long argument.
- **QED** uses a multi-agent proof pipeline with decomposition, proof generation, regulation, and
  separate structural and detailed verification. This protocol assumes that no automatic or
  model-based verifier is trustworthy enough to remove the human from the critical path. It uses
  phase gates, verbatim sources, and precondition checks to make that human verification cheaper.
- **Bolzano** uses parallel model generation, verification, summarization, and a persistent store of
  findings that survive verification. This protocol targets the smaller setting of one human and
  one model, and keeps the corresponding state in a compact standing log and clean working surface.

The approaches are complementary. A QED- or Bolzano-like system could supply candidate arguments
or automated checks inside this protocol, while MathResearchPrompts-style task prompts could be
used within a phase. The contribution here is the human-facing control layer for settings where no
verifier can be treated as an oracle and the researcher's attention is the binding constraint.

## Contributing

Most valuable: a failure the protocol did not catch, added as a case under [`examples/`](examples/)
(copy [`examples/_template.md`](examples/_template.md)) with the rule you think would have caught
it. Second: a port to a non-mathematical domain.

Redact anything unpublished; the shape of the error matters more than the mathematics. Rules are
added reluctantly. A rule earns its place only if it forces an unsupported step to surface as an
explicit gap or makes an error cheaper to spot. Confidence scores, status tags, and self-assessments
do not qualify: a model generates them as fluently as the errors they are meant to flag.

## License

MIT. Use it, change it, redistribute it freely, no attribution required. See [`LICENSE`](LICENSE).
