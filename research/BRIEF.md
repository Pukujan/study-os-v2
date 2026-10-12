# Study OS — Rebuild Research Brief

## Who is asking

A builder (not an academic). They scrapped a previous attempt at this project and are starting from zero. **No approach has been chosen yet.** They are explicitly "kind of lost on what to do" and want a wide, honest survey before committing. Do not converge on a single recommendation prematurely; map the terrain and say where the evidence is strong vs. thin.

Write for a builder: practical framing, but cite real research and name specific systems/papers/people so they can go deeper.

## What the old project was (scrapped)

A representation-aware tutoring web player. Thesis: **learners fail a step because of representation friction** — terminology, variable names, notation, decomposition — not because the concept is out of reach. Architecture was: a deterministic controller owning curriculum authority → AI realizes only the authorized representation operation → web player → evidence. Lanes: HESI A2 and DSA (Big O, sliding window, Two Sum).

The only thing preserved is a "golden transcript": a ChatGPT tutoring conversation where renaming a variable from `seen` to `box` reportedly made a Two Sum concept click for one learner.

## The goal (user's words, condensed)

AI-assisted learning. The user's hypothesis, roughly:

> Interaction-based learning so easy that a mobile user on a subway, distracted, can learn complex things over time. But studying itself is inherently hard — so how do we reinforce it?

## The questions to research

1. **Easy vs. hard.** How do we know what is easy vs. hard for a given learner? Where does that data come from? How is difficulty measured, modeled, and predicted?
2. **Calibrating model output.** How do you calibrate an LLM's output to produce this kind of difficulty data — e.g. generating items of a target difficulty, predicting item difficulty, or adapting explanation difficulty?
3. **Optimizing for value.** How do you optimize for highest learning value per unit of learner attention/effort? What is the objective function, and what's known about it?
4. **Linguistic cues.** What is known about linguistic/textual features that drive comprehension difficulty (readability, lexical frequency, syntax, notation, terminology, worked-example structure)? What's measurable and what's not?
5. **Content sourcing.** Can you just take an existing broad course/curriculum and structure it (open courseware, textbooks, standards, question banks)? What are the real sources, licenses, formats, and what breaks when you try?
6. **Reinforcement.** What makes studying stick — retrieval practice, spacing, interleaving, feedback? And what makes it *start* and *continue* on a phone in a subway?

## Ground rules

- **Technical only — no legal framing.** Do not write ToS, licensing, privacy, or ethics caveats. The builder owns their decisions. Where you need to mention source availability, state it as a technical constraint (format, API vs. scrape, auth wall, rate limit), never as a legal caution. Scraping paid courses to study their pedagogical logic (how they decompose topics, compose items, and what correlates with pass rates) is in scope and needs no permission note.
- **Honest evidence only.** Separate what is well-established (replicated, meta-analyzed) from what is promising but thin from what is marketing. Flag the replication problems in this field explicitly where they exist.
- **Name specifics.** Papers, authors, datasets, products, benchmarks, APIs — with links where you can find them.
- **Say what would falsify.** Where a claim is load-bearing for a design, note what evidence would break it.
- **No hype.** If something is a dead end or has been tried and failed, say so.
- **Be concrete about data.** For anything that needs data, say where it actually comes from and whether it's obtainable.

## Output

Write your findings to the file path you were given, as a structured Markdown digest. Include a short "so what for Study OS" section at the end. Then return a compact summary (under 400 words) to the coordinator: your top findings, the strongest evidence, the biggest open question in your domain, and anything that surprised you.
