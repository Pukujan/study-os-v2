# Study OS v2

A learner can understand an idea and still be stopped by the words it is written in.

![The Study OS learner and helper standing beside a row of labelled boxes on a whiteboard](.content-system/characters/refs/pair-key-art.webp)

## Why this exists

In a recorded tutoring session, a learner working through Two Sum said the code confused her. She could already say which number left the window and which number entered — the arithmetic was fine. What stopped her was one variable name. She had learned `seen` as a set, so when the same word held a dictionary, the mapping broke. The tutor renamed the variable to `box`, and the problem clicked.

That session is the founding evidence for this project, and it is thin: one learner, one session, no controlled comparison. It still points somewhere specific and testable. **Learners are frequently blocked by representation rather than by concept** — by terminology, variable names, notation, decomposition, or the amount of context shown at once. Study OS exists to find out where that block sits, change one thing, and measure whether the change helped.

## What Study OS is

Study OS is an instrumented learning procedure, not a course and not a chatbot. It keeps a deterministic controller in charge of what is being learned, what assistance is allowed, when the learner may advance, and how evidence is read. An AI layer realizes the representations the controller authorizes; it does not decide the pedagogy.

The working thesis is narrower than "AI tutor." If a learner is stuck, the system should be able to name *which* representation failed — a symbol binding, a variable name, a missing intermediate step — change that one thing, and then check whether the learner can now do the step unaided. Every change is an intervention with a before and an after, recorded as evidence.

The current version is a rebuild. The previous repository's architecture and application code were scrapped; what carried forward is the evidence — the recorded sessions, the curated DSA content, the learner record, and the documents that define the project. Those are kept verbatim in [`sessions/`](sessions/), [`domains/`](domains/), [`subjects/`](subjects/) and [`legacy/`](legacy/), and the new work starts from research rather than from the old code.

## How it works

The system separates four things that are easy to blur together.

**Evidence classes stay distinct.** Observed behavior (what the learner actually did), self-reported understanding (what the learner said), and derived state (what the system inferred) are recorded as different kinds of claim. A correct answer today is not mastery, and a learner saying "I get it" is not evidence that they do.

**The controller authorizes, the model realizes.** The deterministic layer holds the lesson state and decides what operation is allowed next. The AI layer produces the explanation, diagram, or example for that operation. This keeps pedagogy in code and state, not only in a prompt.

**One relation changes at a time.** When a learner is stuck, the system introduces a single new relation, keeps the current representation stable until it holds, exercises it, explains with the same representation, and only advances after a correct retry is confirmed by a second one.

**Claims are graded, not scored.** Findings carry a status — observed, self-reported, derived, or established — and a source. The project does not collapse them into a single confidence number.

## What the research says

A seven-part research pass ([`research/`](research/)) produced a synthesis ([`research/SYNTHESIS.md`](research/SYNTHESIS.md)) that sharpens the thesis in three ways.

**The subway idea splits in two.** Fragmented short sessions are strongly supported for *retaining and drilling* material the learner has already met — spacing and retrieval practice are among the best-replicated findings in education. There is no evidence anywhere that minutes-a-day sessions *build* complex new skill. So the honest design has two modes: acquisition in longer, higher-attention moments, and retention on the subway. Routing material between those two modes is the product's job.

**Difficulty is a joint property, not a content label.** How hard an item is depends on the content, the learner, and the time since they last saw it. Text-side judgments of difficulty — including LLM ratings — track the curriculum, not the learner, and miss the misconception-driven difficulty that actually blocks people. Only learner responses see it.

**The measurable proxy is memory over time.** The strongest copyable engine is predicting recall probability — half-life regression and related scheduling models — validated against a delayed test rather than an in-session score. In-session performance can rank methods backwards, so a delayed checkpoint is designed in from the start, not added later.

## Where the project stops

Study OS has one longitudinal learner, `subject-001`, and no efficacy claim. It does not claim that visual instruction is better, that a learner has a fixed learning style, or that a single good explanation caused a gain. Findings from one learner are labeled as such until they replicate.

The hardest open problem is acquisition under fragmented attention. Nobody has published a working mechanism for it, and this project has not built one. It is stated as a research question, not a feature.

## Next

The immediate work is the minimal loop that tests the thesis: one lane of DSA items, sessions scheduled by a memory model, every response instrumented as (item, learner, time, correct, latency, hint use), and a delayed checkpoint that breaks the circularity of in-session metrics.

The first experiment is cheap and specific. The founding anecdote has two competing explanations — that concrete names like `box` help, or that descriptive names help. The code-comprehension literature supports the second only for experienced developers, and the concreteness explanation has no direct evidence. Study OS's own response data can separate them.

Read [`research/SYNTHESIS.md`](research/SYNTHESIS.md) for the map and the citations, [`sessions/`](sessions/) and [`legacy/`](legacy/) for the carried evidence, and [`POLICY.md`](POLICY.md) for how work here is run.
