# Crucible

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-skill-orange)](https://claude.ai/code)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](./CONTRIBUTING.md)
[![GitHub stars](https://img.shields.io/github/stars/mshadmanrahman/crucible?style=social)](https://github.com/mshadmanrahman/crucible/stargazers)

A council of 16 personas that stress-tests one idea under structured conflict. Built as a [Claude Code](https://claude.ai/code) skill. Useful when you are a product manager, founder, or builder staring at a decision that does not have a right answer.

> Wicked problems need multiple lenses. This is sixteen of them.

Full story: [claudecodeguide.dev/crucible](https://claudecodeguide.dev/crucible)

## What it does

You give Crucible a question. The chair selects 6 to 10 personas from the roster of 16 based on the problem type. Each persona writes an opening take in parallel, blind to the others. Selected pairs then rebut each other. The chair synthesises the transcript and issues a verdict of `PROCEED`, `REFRAME`, or `KILL`, plus a mandatory minority report.

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

Four modes depending on the weight of the question.

### Quick mode (daily default, ~30 seconds)

```
/crucible --quick should I split this feature into two releases?
```

Three personas in parallel. Contrarian, Steelman, Reframer. Three short challenges plus a one-line call. Use this between meetings.

### PM Daily mode (bounded ideation, ~60 to 90 seconds)

```
/crucible --pm what is the sharpest first experiment for this discovery?
```

Four personas with a short verdict and one concrete next action. Use for feature direction, user research design, ideation-grade questions.

### Decision mode (full council, ~30 to 60 seconds per phase)

```
/crucible should I keep working on X vs pivot to Y?
```

Six to ten personas. Full duels. Written transcript. Use for anything with more than a week of time, real money, or reputation at stake.

### Existential mode (full roster of 16)

```
/crucible --existential should I leave my job to ship this full time?
```

All 16 personas. Extended verdict. Guardrailed: if you invoke this more than once a quarter, the chair warns you that life decisions do not usually move that fast.

## The 16 personas

**Core 4 (always convened):**
- Contrarian — attacks the premise
- Steelman — argues the strongest case for
- Pre-mortem Pessimist — names the failure mechanism
- Operator — Monday-morning reality check

**Personal bench (career, moonlight, identity):**
- Archivist — cites what you have already proven
- Values Compass — checks against your stated goals
- Success Vision — paints the 12-month win as a scene
- Downside Floor — quantifies the failure cost

**Strategic bench (business, product, market):**
- Economist — follows the money
- Competitor / Mirror — how the market responds
- Second-Order Thinker — chains of consequence
- Historian — who has tried this

**Creative bench (reframing, ambition):**
- Ambition Stretcher — what if this were 10x bigger
- Reframer — is this the right question

**Conditional:**
- Naive Outsider — activated for user-facing product calls
- Regulator — activated for money, data, or platform ToS

## The three phases

1. **Intake.** Chair restates the question in one sentence and asks you to confirm before spending subagents on it.
2. **Parallel openings.** Every selected persona writes its take in parallel. Personas are blind to each other.
3. **Serial duels in pairs.** Steelman vs Contrarian. Ambition Stretcher vs Pre-mortem. Success Vision vs Downside Floor. Economist vs Second-Order Thinker. Archivist vs Reframer.
4. **Verdict.** Chair reads all openings and rebuttals, writes the transcript, issues an inline verdict with a mandatory minority report.

## Model routing

Each persona runs on the model tier that matches its job.

- **Opus:** Contrarian, Second-Order Thinker, Chair
- **Sonnet:** Steelman, Economist, Reframer, Pre-mortem, Success Vision, Ambition Stretcher, Archivist, Competitor, Regulator
- **Haiku:** Operator, Values Compass, Downside Floor, Historian, Naive Outsider

Why this matters: running 16 large-model agents in parallel collapses context and burns budget. Routing by role keeps a full run cheap enough to use twice a week.

## Guardrails

Two are worth calling out.

**Motivated convening check.** Before firing Decision or Existential mode, the chair asks: has a similar question been convened in the last 30 days? What concrete action will a verdict produce in the next 14 days? What new information has arrived since the last time you thought about this? Honest answers keep the skill from becoming a sophisticated form of procrastination.

**Archivist ownership classification.** The Archivist has to label every cited project as personal moonlight, client work, collaborative work, or day-job work before drawing any pattern claim across them. This exists because the first run confused a piece of client work for a personal choice, and the correction is now structural.

## Files

- [`SKILL.md`](SKILL.md) — orchestration spec
- [`personas.md`](personas.md) — all 16 rubrics
- [`benches.md`](benches.md) — problem-type to bench mapping

## Contributing

This is version one. The roster is deliberately small so each voice has space. But I will add personas that earn their seat.

If you are a PM, founder, or builder with a blind spot you wish a council would catch, write a persona for it and open a pull request. The bar is a concrete rubric. "Name the hidden assumption and attack it with one argument" is a rubric. "Be contrarian" is not.

Issues and PRs welcome at [github.com/mshadmanrahman/crucible](https://github.com/mshadmanrahman/crucible).

## License

MIT.

---

Built by [Shadman Rahman](https://www.linkedin.com/in/shadmanrahman/). More on the backstory at [claudecodeguide.dev/crucible](https://claudecodeguide.dev/crucible).
