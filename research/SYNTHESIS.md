# Study OS Rebuild — Research Synthesis

*Cross-cutting read of digests 01–07. Read the individual digests for citations and detail; this file is the map. Written 2026-10-12.*

---

## The one-sentence state of knowledge

**Micro-retrieval on mobile is one of the best-evidenced things in education; micro-instruction of complex material has no supporting evidence anywhere; difficulty is a joint property of content × learner × time that no text-side heuristic can substitute for; and the closed loop of own-items + real-learner-responses + a calibrated difficulty model is the actual moat.**

Everything below follows from that.

---

## 1. The convergence — three independent domains hit the same wall

Agents researching learning science (01), mobile microlearning (02), and value measurement (07) independently converged on the same boundary, from three different directions:

- **What's well-supported:** fragmented short sessions for *retaining discrete items* — spacing (SMD ≈ 0.78), retrieval practice (57% of effects medium/large; classroom meta-analysis g ≈ 0.5), adaptive review scheduling. This is bedrock, replicated across decades.
- **What has no evidence:** fragmented short sessions for *building complex, transferable skill* (micro-instruction). No study anywhere demonstrates minutes-a-day cadence can deliver high-element-interactivity new schemas.
- **The load-bearing caveat:** Donovan & Radosevich — the spacing benefit *collapses as task complexity rises* (d ≈ 0.97 simple → 0.07–0.42 complex). Spaced micro-sessions may actively underperform on the hardest material.

**Implication for the subway thesis.** The thesis isn't wrong; it's split in two:
1. *Retention engine* — subway-sized sessions reviewing and drilling material already met. Fully evidence-backed. This is where the product's reliable value lives.
2. *Acquisition engine* — how complex material first gets built under a low-attention budget. This is the open, risky, potentially novel part. Duolingo's own data is a warning: shorter lessons *hurt* learning value (they abandoned lesson-count metrics for time-in-path, 15 min/day target).

A honest v1 thesis: **"acquisition happens in slightly-longer, higher-attention moments; retention and drilling happen on the subway."** The product's scheduling brain routes material between the two modes. That is a defensible, evidence-aligned design — and *the acquisition-side design is where Study OS can differentiate*, because nobody has published a working acquisition-under-fragmented-attention mechanism.

## 2. The proxy problem — the deepest technical hazard

Digest 07's core finding: **there is no cheap, validated, live measure of durable learning.** Every in-session signal is a proxy, and Bjork's performance/learning distinction means proxies can rank methods *backwards* (massed practice wins immediate tests, loses delayed ones — Karpicke & Roediger 2007).

The known-good proxies, all from one family: **predicting recall probability over time** — half-life regression (Settles & Meeder 2016) and FSRS. These surfaced independently in three digests (02, 03, 07) as the strongest copyable engine. Note the folklore correction: HLR's +12% engagement came from *removing* text features — per-user response history carried the signal. History-first design is validated.

But all of these are validated against in-session data — circular unless checked against a held-out **delayed test**. The single highest-leverage measurement investment: **one low-compliance delayed checkpoint** ("you did this yesterday — 3 quick ones"). Almost nothing else gives ground truth. Design for the checkpoint from day one; make it content, not a survey (a "yesterday lane" users *want* to do).

## 3. Difficulty — what "easy vs. hard" actually means

Digest 03's reframe: difficulty is **content × learner × time**. Four model families, one per axis:

| Axis | Family | Production-ready pick |
|---|---|---|
| Content | IRT | 2PL (needs response data) |
| Content × learner, online | Elo | Elo-with-uncertainty |
| Learner state | Knowledge tracing | extended BKT |
| Time/memory | memory models | FSRS or HLR |

Simple beats deep at small scale — the DKT edge over BKT was ~32% evaluation artifact (Khajah 2016). Deep sequence models pay off only at volumes a small product never reaches.

**The central trap:** under *adaptive* item selection, proportion-correct difficulty estimates are biased **and do not improve with more students** (Pelánek 2016). "Collect responses, average accuracy" silently breaks. Elo-with-uncertainty or JMLE-class estimation is mandatory, not optional.

**Cold start:** Duolingo's BERT-LLTM generalizes difficulty to unseen items — but every published pipeline leaned on 100k+ live sessions. **Nobody has published the "LLM generates + small-scale recalibrates" loop for a product with hundreds of users.** This is open territory; Study OS would be doing research, not engineering folklore.

**And the calibration folklore, corrected twice over:**
- Fine-tuned BERT (RMSE 0.29) beat GPT-4+ICL (0.36) at difficulty prediction; GPT-4 rationales added nothing (BEA-2024).
- "The Easy Trap" (arXiv:2607.26067): LLM difficulty ratings are *curricular, not learner-relative* — ρ 0.52–0.70 vs. 770 real students, and LLMs miss misconception-driven difficulty entirely ("100 : 1/2" is 34%-correct for students but rated easy by LLMs). Single study, one subject — but it's the sharpest caution for the representation-friction thesis: **the difficulty that matters lives in the learner's misconceptions, which are invisible to text-side judgment.** Only response data sees them.

## 4. The `seen` → `box` story — two better explanations than "descriptive names"

Digest 05's strongest surprise: the code-comprehension literature says descriptive naming helps *experienced* devs (~14–19% faster) but **vanishes for novices**, and single-letter names showed no deficit. So "descriptive names are better" does not explain the founding anecdote.

The better fit: **concreteness/imageability.** `box` is a concrete, imageable noun; `seen` is an abstract participle colliding with the learner's prior "sets have `seen`" association. This is coherent with psycholinguistics (word frequency, concreteness are among the most replicated effects in the field) but has **zero direct code-comprehension evidence**.

Which makes it the **highest-value cheap experiment available**: Study OS's own data can cross concreteness × descriptiveness × length in renames and find out which dimension actually moves outcomes. The founding story becomes a testable product feature instead of a creation myth. (Digest 01's honest verdict on the anecdote itself: consistent with evidence, not established by it — n=1.)

Related representation findings: the transfer literature (01) supports *representation–learner alignment*, multiple varied examples, and concreteness fading (Fyfe 2014) as the routes to deep structure. LLM readability control is unreliable (grade targets missed, misinformation introduced while simplifying — Newsela pairs are only ~16% error-free). Any simplification pipeline needs its own factuality gate; never trust surface output.

## 5. Content — the skeleton is free, the leaves are yours

Digest 06's answer to "do we copy a broad study": **copy the skeleton, generate the leaves.**

- Standards documents publish the decomposition for free: AP CS A CED = 4 units → 53 topics → **223 Essential Knowledge statements** + 12 skills + exam weights + exclusion statements. The CED itself says its sequence came from analyzing effective teachers' syllabi — it's pre-aggregated consensus ordering.
- Item composition is formulaic: ~1–3 generated items per EK, weighted by unit/skill share. OpenStax's section template (Example → Solution → Try-It, exercises grouped Verbal/Numeric/Algebraic/Real-World) is itself a difficulty ladder, reusable as a generation template.
- Eedi's misconception-tagged distractors are the cleanest public model for diagnostic wrong answers — directly relevant given §3's point that misconceptions carry the difficulty signal.
- Commercial platforms hide their inner structure; the useful logic is in standards docs and open datasets, not scraped catalogs.
- Verification beats generation as the differentiator (04): the closed loop — own items + real learner responses feeding a calibrated difficulty model — is the moat, not the generator. LLM judges buy consistency, not validity.
- **Solving ≠ teaching** (MathTutorBench): a model's benchmark accuracy does not tell you it can sequence pedagogy. Measure pedagogy separately.
- **The tutoring trap is causal** (Bastani et al., PNAS 2025): unguarded GPT-4 raised practice +48% but left students 17% *below* controls after removal; guardrails largely eliminated the harm. If a chat helper exists in-product, it needs answer-guarding from day one — this is the strongest causal finding in the entire research set.

## 6. Engagement — the fragile part is motivation, not learning

Digest 02's counterintuitive split: gamification's *cognitive* benefit survives rigor filtering (g ≈ 0.42–0.49); the *motivational and behavioral* benefits are the fragile ones (drop to non-significant). And reminder-driven consistency **collapses when reminders stop** (independent RCT) — streaks create extrinsic dependency, not habit.

What survives for a subway user:
- **"Here's what's next" beats "here's where you were"** for resumption after interruption (interruptions ~double first-action latency) — a concrete, cheap design rule.
- **Goal-gradient pre-granted progress** ("bonus stamps") accelerates completion.
- **Cepeda's optimal-gap rule**: spacing ≈ 10–20% of the retention horizon — the closest thing to a principled value-per-attention scheduler, and it composes cleanly with FSRS/HLR.
- The delayed checkpoint (§2) doubles as an engagement mechanism if framed as a challenge rather than a test.

## 7. What to build first — the minimal loop that tests the thesis

Synthesizing across all seven digests, the smallest honest version:

1. **One lane** (DSA/Two Sum — it matches the golden transcript and the AP CED gives the skeleton). Parse the standards doc into units → topics → EK statements. Generate 2–3 items per EK with a misconception-tagged distractor each.
2. **Session = retrieval-first.** Subway mode: 1–3 minute review/drill sessions scheduled by FSRS/HLR. Acquisition mode: a single longer, high-attention session type for new material (length is an experiment, not a decision).
3. **Instrument from item #1:** every response is (item, learner, time, correct, latency, hint-use). Elo-with-uncertainty for difficulty, BKT for state, FSRS for memory. No accuracy-counting — the Pelánek trap.
4. **The delayed checkpoint as a feature.** The proxy-circularity breaker. Without it, all difficulty calibration is circular.
5. **The founding experiment:** concreteness × descriptiveness × length in identifier renames, with response data as the readout. This is Study OS's own contribution to the literature — cheap, novel, and it tests the actual thesis.
6. **Guardrails before the chat helper.** If AI assistance exists, it ships answer-guarded or not at all (Bastani).

## 8. The honest risk register

| Risk | Severity | Mitigation from the research |
|---|---|---|
| Proxy circularity — optimizing in-session metrics that don't predict durable learning | **Highest** | Delayed checkpoint from day one; treat in-session metrics as hypotheses |
| Micro-instruction bet — assuming short sessions can teach complex material | **High** | Split acquisition/retention modes; the split *is* the product design |
| LLM difficulty ratings are curricular, not misconception-aware | High | Calibrate against live responses; never trust text-side judgment alone |
| Adaptive-selection bias corrupting difficulty estimates | Medium | Elo-with-uncertainty from the start; never proportion-correct |
| Simplification introduces factual errors | Medium | Factuality gate on any generated simplification |
| Engagement mechanics decay (reminders stop → usage stops) | Medium | Build intrinsic scheduling pull (checkpoint-as-challenge); use goal-gradient, not streaks |
| Cold start with no 100k sessions | Medium | Feature-based difficulty priors (BERT-LLTM pattern); accept wide uncertainty bands |

## 9. The three questions the research could not answer

1. **Does micro-session learning transfer?** Nothing in the literature has run the delayed production posttest vs. active control. If Study OS runs it on its own data, that's a publishable null or result — either way it's the thesis test.
2. **Can the "LLM-generates + small-scale-recalibrates" difficulty loop work at hundreds of users?** Nobody has published it. It's the product's core engine and an open research question.
3. **What actually caused the `seen` → `box` click?** Concreteness is the best hypothesis; the founding experiment (§7.5) is the way to find out.
