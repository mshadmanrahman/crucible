---
name: crucible
description: Stress-test an idea under pressure by convening a multi-persona council that debates, rebuts, and returns a verdict. Use for heavy-lift decisions like moonlight strategy, product direction, career moves, or any question where the cost of being wrong is high. Triggers on "crucible", "/crucible", "run this through the crucible", "convene the crucible", "put X in the crucible". Not for quick questions or tactical calls.
---

# Crucible

A council of specialized personas debates one idea under structured conflict. The chair synthesizes into a verdict. Transcripts are saved to disk for second-brain reuse.

Use this when the cost of being wrong is high enough to justify convening 6-10 subagents. Do not use for tactical questions or anything that takes under five minutes of normal thinking.

## When to trigger

Trigger on explicit user invocation only. Examples:
- `/crucible <idea>`
- "Run this through the crucible: ..."
- "Convene the crucible on ..."
- "Put X in the crucible"

Do not auto-trigger. This is an expensive skill and the user decides when it runs.

## Three modes (added 2026-04-19 for daily usability)

The chair selects mode at intake based on the user's phrasing OR an explicit flag. Do not run the full Decision mode for every invocation; most daily use belongs in Quick or Brainstorm.

### Mode 1: Quick (daily brainstorm default)

**Triggers:** "quick crucible", "gut check", "throw rocks at this", "poke holes in", "sanity check", or `/crucible --quick`

**Bench:** 3 personas, parallel openings only. No duels. No verdict.
- Contrarian (Sonnet, NOT Opus at this tier to keep cost down)
- Steelman (Sonnet)
- Reframer (Sonnet)

**Output:** Three short challenges (80-120 words each), plus a one-line "worth pursuing / shelve / reframe" call from the chair. No transcript file unless the user asks. ~30 seconds wall time.

**Use for:** Daily PM ideation, "I just thought of X, is it dumb?", quick sanity checks between meetings.

### Mode 2: PM Daily (bounded ideation)

**Triggers:** "pm brainstorm", "pm daily crucible", or `/crucible --pm`

**Bench:** 4 personas, parallel openings, no duels, short verdict.
- Contrarian (Sonnet)
- Operator (Haiku)
- Naive Outsider (Haiku)
- Ambition Stretcher (Sonnet)

**Output:** Four one-paragraph challenges plus a chair verdict (proceed / shelve / reframe + one concrete next action if proceed). Transcript optional. ~60-90 seconds.

**Use for:** Daily PM work ideation, feature direction, user research design, anything that deserves more than "throw rocks" but less than a full decision council.

### Mode 3: Decision (current default, 6-10 personas)

**Triggers:** `/crucible`, "convene the crucible", "run this through the crucible", or the skill's default when the user's phrasing implies a real decision.

**Bench:** Per `benches.md`, typically 6-10 personas. Full duels. Full verdict. Transcript written.

**Use for:** Real decisions with >1 week of time at stake, or money, or reputation.

### Mode 4: Existential (Heavy Custom, 16 personas)

**Triggers:** User explicitly flags "existential", "life decision", "full crucible", or chair detects the question framing implies existential scope.

**Bench:** All 16 personas. Full duels. Extended verdict.

**Use guardrail:** If invoked more than once per quarter, chair warns: "You've convened the Existential tier N times in M days. These questions usually move slower than that. Proceed anyway?"

## Motivated-convening guardrail (added 2026-04-19)

Before firing any Decision or Existential mode council, the chair asks three questions at intake:

1. **Has a structurally similar question been convened in the last 30 days?** (Chair should glob `~/Documents/VibeCoding/_scratch/crucible/` for recent transcripts and compare topic proximity. If a near-duplicate exists, show its verdict and ask: "Is this a re-run of that, or genuinely new?")
2. **What decision does a verdict produce that you will act on in the next 14 days?** If the user cannot name a concrete action, the chair should say: "This looks like a discussion, not a decision. Use Quick mode instead."
3. **What new information has arrived since you last thought seriously about this?** If the honest answer is "none, just renewed energy," the chair should say: "Run 2 on 2026-04-19 named this pattern 'appetite disguised as governance.' Proceed anyway, or reframe?"

The chair never REFUSES to run. It names what it is seeing and lets the user decide. This is the skill's main defence against becoming a procrastination ritual.

## Core design

**16 personas, 1 chair.** The chair selects 6-10 personas per run based on problem type (see `benches.md`). Four personas are always convened. The rest are activated by bench.

**Four phases.** Intake, opening takes, pair duels, verdict.

**Model routing per persona.** Haiku for rubric-driven checks, Sonnet for judgment, Opus for Contrarian, Second-Order Thinker, and Chair.

**Transcripts persist.** Full debate writes to `~/Documents/VibeCoding/_scratch/crucible/YYYY-MM-DD-{slug}.md`. Main thread sees only the verdict and a link. Thinking is preserved, context stays clean.

**Rubrics prevent theater.** Each persona has concrete deliverables (cite an analog, estimate a number, name a mechanism), not just "give your take". See `personas.md`.

## Phase 1: Intake

The chair (main thread, Opus) does three things:

1. **Classify the input.** Is this a *problem statement* ("should I do X?") or a *proposed solution* ("here's my plan, tear it apart")? Frame the persona prompts differently for each.

2. **Select the bench.** Always convene the core 4: Contrarian, Steelman, Pre-mortem Pessimist, Operator. Add additional personas based on problem type per `benches.md`.

3. **Restate and confirm.** Before spinning up subagents, restate the question in one sentence and ask the user to confirm or correct the framing. Do not skip this. Mis-framed questions produce sharp answers to the wrong thing.

## Phase 2: Opening takes (parallel subagents)

Spawn one subagent per selected persona **in parallel** (single message, multiple Agent tool calls). Each subagent:

- Uses `general-purpose` subagent_type
- Uses the model tier specified in `personas.md`
- Receives a self-contained prompt containing: the persona's full rubric, the user's idea, the framing mode (problem vs solution), and the output length constraint
- Writes a direct response back; no file writes at this phase

Personas cannot see each other's openings. This is deliberate; it prevents contamination and produces honest independent takes.

Collect all opening takes before moving to Phase 3. Store them internally as labeled blocks (`{persona_name}_opening`).

## Phase 3: Pair duels (serial subagents)

Default duels, activated only if both personas are in the current bench:

- **Steelman vs Contrarian** — is the premise valid?
- **Ambition Stretcher vs Pre-mortem Pessimist** — how big, how fragile?
- **Operator vs Historian** — can we ship it, has anyone shipped it?
- **Economist vs Second-Order Thinker** — who pays, and then what?
- **Success Vision vs Downside Floor** — best case vs worst case, in concrete numbers
- **Archivist vs Reframer** — your track record vs the meta-question: are you pattern-matching correctly or asking the wrong thing?

For each active duel, spawn **two subagents serially**:
1. Persona A reads Persona B's opening, writes a rebuttal (100-150 words)
2. Persona B reads Persona A's opening, writes a rebuttal (100-150 words)

Unpaired personas in the bench (e.g., Values Compass, Naive Outsider, Regulator) skip the duel phase and weigh in at verdict.

## Phase 4: Chair verdict (main thread, Opus)

The chair reads all openings and rebuttals, writes the transcript file, and issues a verdict.

**Transcript file.** Write to `~/Documents/VibeCoding/_scratch/crucible/YYYY-MM-DD-{slug}.md` with sections:
- Question (as restated and confirmed at intake)
- Bench convened (list of personas, model used)
- Opening takes (one section per persona)
- Duels (one section per active duel, both rebuttals)
- Unpaired voices (Values Compass, Naive Outsider, Regulator if present)
- Verdict (the chair's output, duplicated here)

Use `date -u +"%Y-%m-%d"` for the filename date.

**Inline verdict format** (this is what the user sees in the chat):

```
## Crucible Verdict

**Question:** {restated question}
**Bench:** {n} personas convened

**Verdict:** PROCEED | REFRAME | KILL
**Reasoning:** {2-3 sentences tying to the strongest arguments}

**Strongest unresolved objection:** {one sentence} — {Persona name}

**If PROCEED:** Next action → {one concrete step}
**If REFRAME:** New framing → {one sentence}
**If KILL:** Failure mechanism → {one sentence}

**Values check:** {Values Compass finding, one line, if activated}

**Minority report:** {named persona} — "{one sentence quoted dissent}"

**Full transcript:** `_scratch/crucible/YYYY-MM-DD-{slug}.md`
```

The minority report is mandatory. Even on a unanimous PROCEED, surface the strongest cautionary voice. This prevents false consensus.

## Orchestration rules

- **Parallelize Phase 2.** All opening takes fire in a single message with multiple Agent tool calls. Do not serialize.
- **Serialize Phase 3.** Duels run sequentially because the second persona needs to see the first rebuttal. Within a duel, fire the two rebuttals in parallel (each reads the opposing opening, not the opposing rebuttal).
- **Never skip intake confirmation.** If the user's input is ambiguous, restate and ask. One extra round trip costs nothing; a mis-framed council costs 10 subagents.
- **Respect the roster.** Do not invent new personas mid-run. If the problem genuinely needs a persona not in the roster, flag it in the verdict as "council gap" and recommend adding one.
- **Archivist requires file access.** When the Archivist is in the bench, the subagent prompt must include the reading list (MEMORY.md, relevant shards, recent handoffs). See `personas.md` for specifics.

## Cost expectations

A typical moonlight-strategy run: 8 personas (core 4 + personal bench 4) plus chair. Roughly:
- 3 Haiku subagents (Archivist-adjacent rubric work): cheap
- 4-5 Sonnet subagents: moderate
- 2 Opus subagents (Contrarian, Chair): the expensive ones, justified by their leverage

User is on subscription; dollar cost is not the constraint. The constraints are time (7-10 subagents takes 30-60 seconds) and main-thread context (verdict + minority report only, transcript on disk).

## Files in this skill

- `SKILL.md` — this file, the orchestration spec
- `personas.md` — all 16 persona rubrics with model tier and output format
- `benches.md` — problem-type to bench mapping

Load `personas.md` and `benches.md` at intake. They are the runtime data the chair needs.

## Anti-patterns to avoid

- **Council theater.** Personas that give vibes instead of concrete deliverables. Enforce the rubric.
- **Mediator mush.** Chair that summarizes instead of deciding. Always issue a verdict.
- **False consensus.** Verdict that hides dissent. The minority report is not optional.
- **Bench creep.** Convening all 16 personas "to be thorough". Thorough means the *right* personas for *this* problem, not all of them. Exception: the Heavy Custom bench in `benches.md` is legitimate when the user explicitly flags an existential decision.
- **Auto-triggering.** Firing the crucible on questions that don't warrant it. Explicit invocation only.
- **Volitional inference.** Drawing narrative claims about user choice ("walked away from", "gave up on", "doesn't want to own", "rejected as a model") from evidence that doesn't support volition. Especially dangerous when mixing personal moonlight with client/contract work. A project ending under someone else's ownership is not the user choosing to exit. Caught Run 1 on 2026-04-19 when the Archivist cited Wisebox (client work for family friends) as evidence of the user "walking away from the SaaS model." See Archivist guardrails in `personas.md`.
