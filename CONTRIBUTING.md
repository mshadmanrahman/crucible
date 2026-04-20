# Contributing to Crucible

Thanks for opening the repo. This is how to contribute without either of us wasting time.

## What this project accepts

Crucible is deliberately small. The 16 personas, three benches, and four modes are the full surface area. New contributions are welcome if they earn their seat. Specifically:

- **New personas** with a concrete rubric (see bar below)
- **Bench additions** for problem types the current benches do not cover
- **Guardrail improvements** that address observed failure modes, backed by a real transcript
- **Mode refinements** that make Crucible easier to use daily
- **Documentation fixes** and clarifications
- **Bug reports** with a reproducible example

## The rubric bar

A persona proposal has to clear this bar or I will close it with a pointer to this file:

1. **Name the job in one sentence.** Not "be thoughtful." Something like "name the failure mechanism and the earliest warning sign."
2. **Provide a concrete rubric with 3 to 5 deliverables.** Each deliverable should force a specific answer, not a general take. "Cite one historical analog by name" is a rubric. "Give historical context" is not.
3. **Specify the output format.** Word count range, bulleted vs prose, numbered lists, whatever keeps the persona honest.
4. **Specify the model tier** (Opus, Sonnet, or Haiku) and justify it.
5. **Name what this persona does NOT do** so it does not collide with an existing one.

Open a [persona proposal issue](../../issues/new?template=persona_proposal.yml) first. Once the rubric is discussed, send a pull request against `personas.md`.

## Pull request workflow

1. Fork the repo.
2. Create a branch named `persona/<name>`, `bench/<name>`, `guardrail/<topic>`, or `docs/<topic>`.
3. Make the change. Keep diffs small and focused.
4. Run the persona against a real question if you can and paste the transcript in the PR description. This is the strongest form of evidence.
5. Open the PR using the template.

## What I will not merge

- Personas without rubrics (vibes, not deliverables).
- Personas that duplicate an existing voice with a new name.
- Bench creep — adding personas to existing benches without a clear triggering problem type.
- Behaviour that removes the minority report. The minority report is load-bearing.
- Auto-triggering logic. Crucible fires on explicit invocation only.

## Style

- No em dashes in any contributed prose.
- Short declarative sentences.
- Specific examples over abstract claims.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
