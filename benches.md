# Benches

Maps problem type to the persona bench the chair should convene. The chair reads this at intake and picks the matching bench. Default: personal bench for moonlight/career questions, which is the most common trigger.

Always convened (core 4): **Contrarian, Steelman, Pre-mortem Pessimist, Operator.**

---

## Bench: Personal / Moonlight / Career

**Triggers:**
- Moonlight strategy, product direction, revenue goals
- Career moves, positioning, personal brand
- "Should I keep working on X vs pivot?"
- "Is this project worth the time?"
- Identity-adjacent questions (what kind of builder am I becoming)

**Adds to core 4:**
- Archivist (track record check)
- Values Compass (alignment with 100K SEK / family / halal-only goals)
- Success Vision (12-month end state if it works)
- Downside Floor (quantified failure cost)

**Total:** 8 personas + chair.

---

## Bench: Strategic / Business / Product

**Triggers:**
- Product strategy, feature prioritization
- Business model, pricing, unit economics
- Market positioning, competitive response
- Decisions with multi-month horizons and multiple stakeholders

**Adds to core 4:**
- Economist (who pays, what behavior is rewarded)
- Competitor / Mirror (market response)
- Second-Order Thinker (causation chains, feedback loops)
- Historian (precedents and analogs)

**Total:** 8 personas + chair.

---

## Bench: New Idea / Reframe

**Triggers:**
- Greenfield ideas, "should I start X?"
- Anything labelled "brainstorm", "explore", "is this worth doing"
- When the user seems stuck on the framing itself

**Adds to core 4:**
- Ambition Stretcher (what if 10x bigger)
- Reframer (is this the right question)
- Historian (who has tried this)
- Success Vision (paint the default win)

**Total:** 8 personas + chair.

---

## Bench: User-Facing Product Launch

**Triggers:**
- Consumer product launches
- User-facing feature decisions where adoption is uncertain
- Anything where "will people actually use this" is an open question

**Adds to core 4:**
- Naive Outsider (will beginners get it)
- Economist (who pays, how)
- Competitor / Mirror (market response)
- Success Vision + Downside Floor (paired: best case vs floor)

**Total:** 9 personas + chair.

---

## Bench: Regulated / High-Stakes

**Triggers:**
- Money, data, health, legal, platform ToS, affiliate compliance
- Halal-finance decisions
- Anything that could get a cease-and-desist

**Adds to core 4:**
- Regulator (compliance specifics)
- Economist (who pays, who enforces)
- Downside Floor (quantified failure including legal cost)
- Historian (who got burned and how)

**Total:** 8 personas + chair.

---

## Bench: Quick Brainstorm (Mode 1, daily)

**Triggers:** "quick crucible", "gut check", "throw rocks at this", "poke holes", "sanity check", `/crucible --quick`

**Personas (3, all Sonnet to keep cost down and speed up):**
- Contrarian
- Steelman
- Reframer

**No duels. No transcript by default.** Output: three short challenges (80-120 words each) + one-line chair call.

**Total:** ~30 seconds wall time, ~3K tokens total.

---

## Bench: PM Daily (Mode 2, bounded ideation)

**Triggers:** "pm brainstorm", "pm daily crucible", `/crucible --pm`

**Personas (4):**
- Contrarian (Sonnet)
- Operator (Haiku)
- Naive Outsider (Haiku)
- Ambition Stretcher (Sonnet)

**No duels.** Output: four paragraph challenges + chair verdict with next action. Transcript optional.

**Total:** ~60-90 seconds wall time.

---

## Bench: Heavy Custom (user-requested full roster)

**Triggers:**
- User explicitly says "run the full crucible" or "convene everyone"
- One-off existential decisions (major career pivots, founder splits, etc.)

**Adds to core 4:**
- All 12 remaining personas

**Total:** 16 personas + chair. Use sparingly; this is the "everything" button.

---

## Chair selection logic

1. Read the user's phrasing and classify the problem type using the triggers above.
2. If ambiguous, ask: "This looks like a {type} question — should I use the {bench name} bench, or do you want a different mix?"
3. Never mix benches by default. If the question genuinely spans two domains (e.g., a moonlight product that needs regulatory review), it is legitimate to add one specialist persona from a second bench. Name the reason in the intake restatement.
4. Never drop a core 4 persona. They are always in.
5. Never skip the intake confirmation. One extra round trip is cheap; a mis-framed council is expensive.
