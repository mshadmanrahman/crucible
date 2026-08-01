> ## This repo has moved
>
> It now ships as the **`crucible`** plugin inside [**PM Pilot**](https://github.com/mshadmanrahman/pm-pilot):
>
> ```bash
> claude plugin marketplace add mshadmanrahman/pm-pilot
> claude plugin install crucible@pm-pilot
> ```
>
> One install surface instead of a repo per skill. The move also fixed dead file paths that broke this skill's bundled reference files for anyone who followed the old install instructions.
>
> This repo is archived and read-only. The content below is preserved for reference.

---

# Crucible

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-skill-orange)](https://claude.ai/code)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](./CONTRIBUTING.md)
[![GitHub stars](https://img.shields.io/github/stars/mshadmanrahman/crucible?style=social)](https://github.com/mshadmanrahman/crucible/stargazers)

A council of 17 personas that stress-tests one idea under structured conflict. Built as a [Claude Code](https://claude.ai/code) skill. Useful when you are a product manager, founder, or builder staring at a decision that does not have a right answer.

> Wicked problems need multiple lenses. This is seventeen of them, with First Principles and Expansionist as the spine.

Full story: [claudecodeguide.dev/crucible](https://claudecodeguide.dev/crucible)

## What it does

You give Crucible a question. The chair selects a bench from the roster of 17 based on the problem type. Each persona writes an opening take in parallel, blind to the others. Selected pairs then rebut each other (with the opposing argument anonymized so they attack the argument, not the persona). The chair synthesises the transcript and issues a verdict of `PROCEED`, `REFRAME`, or `KILL`, plus a mandatory minority report.

The central tension of every Decision-mode run is **First Principles vs Expansionist**: strip the problem to its irreducible facts, then ask what the 100x version looks like with AI-era leverage. The verdict must address where the two landed. This is the friction PMs and builders most often skip and most need.

Transcripts persist to disk so your main thread stays clean.

## Install

### As a Claude Code skill

```bash
# Clone into your Claude Code skills folder
git clone https://github.com/mshadmanrahman/crucible ~/.claude/skills/crucible
```

That is the whole install. Open Claude Code and type `/crucible` followed by a question.

### Without Claude Code

The persona files in `personas.md` are portable. You can paste a single persona prompt into ChatGPT or Claude.ai and run that lens manually. You lose the orchestration; you keep the rubrics.

## Usage

Five modes depending on the weight of the question.

### Solo mode (single-lens exploration, ~10-15 seconds)

```
/crucible --solo first-principles should we ship this feature now or after the refactor?
```

Or natural language:

```
give me the Expansionist read on the morning digest cron
```

One persona, no debate, just the rubric output. Use for exploration, not decisions.

### Council mode (daily reflex, ~30-45 seconds)

```
/crucible --council should I split this feature into two releases?
```

Five personas in parallel: Contrarian, First Principles, Expansionist, Outsider, Operator. Five short challenges plus a one-line call. Modelled on Andrej Karpathy's LLM Council. Use this between meetings.

### PM Daily mode (bounded ideation, ~60 to 90 seconds)

```
/crucible --pm what is the sharpest first experiment for this discovery?
```

Four personas with a short verdict and one concrete next action. Use for feature direction, user research design, ideation-grade questions.

### Decision mode (full council, ~30 to 60 seconds per phase)

```
/crucible should I keep working on X vs pivot to Y?
```

Nine to eleven personas (Core 6 plus bench specialty). Full duels including the mandatory First Principles vs Expansionist showdown. Written transcript. Use for anything with more than a week of time, real money, or reputation at stake.

### Existential mode (full roster of 17)

```
/crucible --existential should I leave my job to ship this full time?
```

All 17 personas. Extended verdict. Guardrailed: if you invoke this more than once a quarter, the chair warns you that life decisions do not usually move that fast.

## The 17 personas

**Core 6 (always convened in Decision and Existential modes):**
- **Contrarian** attacks the premise
- **Advocate** argues the strongest case for
- **Pre-mortem** names the failure mechanism
- **Operator** Monday-morning reality check
- **First Principles** strips the problem to bedrock facts
- **Expansionist** finds the 100x version with AI-era leverage

**Personal bench (career, moonlight, identity):**
- **Track Record** cites what you have already proven
- **Goals Check** checks against your stated goals
- **Success Vision** paints the 12-month win as a scene
- **Worst Case Cost** quantifies the failure cost

**Strategic bench (business, product, market):**
- **Economist** follows the money
- **Competitor** how the market responds
- **Consequences** chains of causation
- **Historian** who has tried this

**Creative bench:**
- **Reframer** is this the right question

**Conditional:**
- **Outsider** activated for user-facing product calls
- **Regulator** activated for money, data, or platform ToS

## The four phases

1. **Intake.** Chair restates the question in one sentence and asks you to confirm before spending subagents on it.
2. **Parallel openings.** Every selected persona writes its take in parallel. Personas are blind to each other.
3. **Serial duels with anonymized rebuttals.** First Principles vs Expansionist always fires in Decision and Existential modes. Other duels: Advocate vs Contrarian, Success Vision vs Worst Case Cost, Economist vs Consequences, Operator vs Historian, Track Record vs Reframer. The opposing argument is shown without persona labels so the rebuttal attacks the argument, not the persona.
4. **Verdict.** Chair reads all openings and rebuttals, writes the transcript, issues an inline verdict with a mandatory minority report and a mandatory First Principles vs Expansionist resolution line.

## Model routing

Each persona runs on the model tier that matches its job.

- **Opus:** Contrarian, First Principles, Consequences, Chair
- **Sonnet:** Advocate, Expansionist, Economist, Reframer, Pre-mortem, Success Vision, Track Record, Competitor, Regulator
- **Haiku:** Operator, Goals Check, Worst Case Cost, Historian, Outsider

Why this matters: running 17 large-model agents in parallel collapses context and burns budget. Routing by role keeps a full run cheap enough to use twice a week, and Council mode cheap enough to use daily.

## Guardrails

Three are worth calling out.

**Motivated convening check.** Before firing Decision or Existential mode, the chair asks: has a similar question been convened in the last 30 days? What concrete action will a verdict produce in the next 14 days? What new information has arrived since the last time you thought about this? Honest answers keep the skill from becoming a sophisticated form of procrastination.

**Track Record ownership classification.** Track Record has to label every cited project as personal moonlight, client work, collaborative work, or day-job work before drawing any pattern claim across them. This exists because the first run confused a piece of client work for a personal choice, and the correction is now structural.

**Anonymized rebuttals.** In duels, the opposing argument is shown without persona labels. Prevents deference to dramatic framings (Pre-mortem's failure scenes tend to win on tone if you know who wrote them) and forces argumentative honesty.

## Files

- [`SKILL.md`](SKILL.md), orchestration spec
- [`personas.md`](personas.md), all 17 rubrics
- [`benches.md`](benches.md), problem-type to bench mapping plus all 5 modes

## Contributing

The roster is deliberately small so each voice has space. But I will add personas that earn their seat.

If you are a PM, founder, or builder with a blind spot you wish a council would catch, write a persona for it and open a pull request. The bar is a concrete rubric. "Name the hidden assumption and attack it with one argument" is a rubric. "Be contrarian" is not.

Issues and PRs welcome at [github.com/mshadmanrahman/crucible](https://github.com/mshadmanrahman/crucible).

## Acknowledgements

The Council mode and the First-Principles-vs-Expansionist spine are inspired by Andrej Karpathy's LLM Council pattern (five lenses: contrarian, first principles, expansionist, outsider, executor). Crucible extends it with bench-typed Decision mode, mandatory minority reports, and anonymized rebuttals.

## License

MIT.

---

Built by [Shadman Rahman](https://www.linkedin.com/in/shadmanrahman/). More on the backstory at [claudecodeguide.dev/crucible](https://claudecodeguide.dev/crucible).
