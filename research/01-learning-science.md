# 01 — The Core Science of What Makes Studying Stick (and What Makes It Hard)

*Research digest for Study OS. Domain: retrieval practice, spacing, interleaving, desirable difficulties, cognitive load, feedback, transfer, effort/motivation, and micro-sessions. Evidence graded: **Solid** (replicated + meta-analyzed), **Promising** (real but thin/bounded), **Thin/contested**, **Debunked**.*

---

## TL;DR — the hierarchy of evidence

| Claim | Status | Headline number |
|---|---|---|
| Retrieval practice (testing effect) beats restudy | **Solid** | meta-analytic *g* ≈ 0.40–0.60 (Adesope 2017; Visible Learning MetaX 0.49 over 744 studies) |
| Spaced/distributed practice beats massed | **Solid** | optimal gap ≈ 10–20% of the retention interval (Cepeda 2006/2008) |
| Interleaving beats blocking | **Promising** | *g* ≈ 0.42 overall, ~0.34 for math (Brunmair & Richter 2019) |
| Worked examples beat problem-solving for novices | **Solid (with reversal)** | strong, but reverses with expertise (Kalyuga) |
| Feedback helps — but only some kinds | **Solid, type-dependent** | *d* ≈ 0.48 overall; near-zero for grades/praise (Wisniewski 2020) |
| Abstract representations transfer better than concrete | **Contested** | Kaminski 2008 vs. De Bock 2011 vs. Trninic 2020 |
| Desirable difficulty raises *durable* learning even when it lowers *perceived* learning | **Solid as a principle; dangerous as a UX** | fluency illusion is real and robust |
| Learning styles (visual/auditory/kinesthetic matching) | **Debunked** | no credible support for the "meshing hypothesis" |
| Microlearning apps teach in minutes a day | **Thin** | positive reviews, weak designs, one recent retraction |

---

## 1. Retrieval practice — the strongest single lever

The testing effect is the best-supported finding in the field. **Dunlosky et al. (2013)**, the standard map of study techniques, rated **practice testing** and **distributed practice** as the only two techniques of *high utility* out of ten, with interleaving, elaborative interrogation and self-explanation as *moderate*, and rereading, highlighting, summarization, keyword mnemonics and imagery as *low*. ([PDF](https://www.whz.de/fileadmin/lehre/hochschuldidaktik/docs/dunloskiimprovingstudentlearning.pdf) · [AFT summary](https://www.aft.org/ae/fall2013/dunlosky) · [SAGE](https://journals.sagepub.com/doi/abs/10.1177/1529100612453266))

**Adesope, Trevisan & Sundararajan (2017)**, the largest retrieval-practice meta-analysis, pooled ~272 independent effects: retrieval beat restudy at roughly *g* = 0.51, and beat filler/no-activity at *g* ≈ 0.93. Multiple-choice practice tests produced larger effects (*g* = 0.70) than short-answer (*g* = 0.48) — a counter-intuitive result the authors themselves flag. Effects were **largest for secondary students**, held in real classrooms, and were stronger when the study–test gap exceeded one day. ([SAGE](https://journals.sagepub.com/doi/abs/10.3102/0034654316689306) · [summary](https://theeconomyofmeaning.com/2017/03/21/important-new-meta-analysis-on-the-testing-effect-with-some-surprises/))

Visible Learning MetaX aggregates 5 meta-analyses (744 studies, 222,708 students) at a weighted mean of **0.49** — but individual meta-analyses range from 0.16 (Fong 2021) to 0.74 (Hattie & Donoghue 2016), which is itself the honest headline: **the effect is real and moderate, but its size swings hard with how retrieval is operationalized.** ([MetaX](https://www.visiblelearningmetax.com/influences/view/practice_testing)) **Agarwal et al. (2021)** review the classroom evidence and conclude retrieval practice consistently benefits learning across ages, subjects and formats ([PDF](https://pdf.poojaagarwal.com/Agarwal_etal_2021_EDPR.pdf)).

**Where it breaks down (2024–2026):** Sigayret, Parmentier & Silvestre (2026) ran two experiments on Prolific and found **no testing effect at all**, despite prescreening and above-minimum pay. They attribute the nulls to engagement/attrition in crowdsourced samples, not to a failure of the effect, and cite **Redifer et al. (2026)**, who found no testing advantage with a long, conceptually demanding text — high cognitive load suppressing delayed recall. Treat this as a **boundary condition, not a refutation**: retrieval practice works when the retrieval is effortful *and* the learner is actually engaged. ([Frontiers](https://www.frontiersin.org/journals/psychology/articles/10.3389/fpsyg.2026.1727423/full))

---

## 2. Spacing — the most quantitatively useful lever

**Cepeda et al. (2006)** synthesized 839 assessments from 317 experiments and found the interval between study episodes and the final test "operate jointly": the optimal gap grows with the retention interval. **Cepeda et al. (2008)** turned this into a usable rule — **the best gap is roughly 10–20% of the retention interval**. For a test one week away, optimal gap ≈ 1–2 days; for retention over a year, optimal gap ≈ 3–5 weeks. Cepeda's follow-up work found optimal spacing improved final recall by **up to 150%** versus massed practice, and that *over*-spacing causes only a "quite modest" decline. ([Cepeda 2006 PDF](https://augmentingcognition.com/assets/Cepeda2006.pdf) · [Cepeda 2008](https://pubmed.ncbi.nlm.nih.gov/19076480/) · [APS Observer](https://www.psychologicalscience.org/observer/temporal-spacing-and-learning))

Practical consequence: **cramming isn't just "less good" — it produces performance that decays fast, while spacing produces durable retention at the same total time.** The failure mode is motivational, not cognitive: spaced study feels worse (see §5).

---

## 3. Interleaving — real, but the weakest and most conditional of the big three

**Brunmair & Richter (2019)** meta-analyzed interleaved learning: overall *g* ≈ 0.42, but only **g ≈ 0.34 for mathematical tasks**, with ambiguous results for expository text. Their title says it: **"Similarity matters."** Interleaving helps most when the to-be-discriminated categories are *confusable* — it forces learners to practice *selecting* the right strategy, not just executing it. ([PubMed](https://pubmed.ncbi.nlm.nih.gov/31556629/) · [MetaX](https://www.visiblelearningmetax.com/influences/view/interleaved_practice))

The most important recent result is a **null**: a year-long RCT of interleaved vs. blocked math practice in 62 Nigerian classrooms (NBER w31853) found interleaving raised **short-term** retention by 0.29 SD but showed **no evidence of improvement on longer-term outcomes**. That is exactly the outcome that matters, and it did not replicate. ([NBER](https://doi.org/10.3386/w31853))

Boundary conditions worth carrying into design: interleaving helps little when learners lack the prior knowledge to tell the categories apart (Bjork & Bjork 2020), and it reliably **hurts immediate performance** while helping delayed retention.

---

## 4. Cognitive load theory, worked examples, and expertise reversal

**Worked examples beat problem-solving for novices** — the "worked example effect" is one of CLT's most replicated findings: studying a full solution reduces element interactivity versus searching for one. **Sweller (2023)** traces the theory's development ([Springer](https://link.springer.com/article/10.1007/s10648-023-09817-2)); Chen et al. (2023) confirm the effect on solution-step learning ([T&F](https://www.tandfonline.com/doi/full/10.1080/01443410.2023.2273762)).

The critical corollary is the **expertise reversal effect** (Kalyuga): guidance that scaffolds novices becomes *redundant and harmful* for experts, who must reconcile external explanation with existing schemas — adding load instead of reducing it. The evidence includes electrical apprentices (text+diagram helps novices, hurts after training) and writing prompts that help early-semester students but demotivate experienced ones. The design implication is **adaptive fading** — remove support as competence grows. ([Chartered College](https://my.chartered.college/impact_article/expertise-reversal-effect-and-its-instructional-implications/) · [Cambridge Handbook ch.40](https://www.cambridge.org/core/books/cambridge-handbook-of-expertise-and-expert-performance/cognitive-load-and-expertise-reversal/03F656FD334F23214426ACB4118FEBF9))

**Honest caveat:** CLT is a framework with genuine measurement problems. "Cognitive load" is inferred from single-item mental-effort scales and dual-task proxies; element interactivity is hard to quantify independently of outcomes, which makes some claims close to unfalsifiable. Treat CLT as a **useful design heuristic, not a precise measurement model**. ([Sweller 2024](https://www.sciencedirect.com/science/article/pii/S1041608024000165))

---

## 5. Desirable difficulties, the fluency illusion, and why studying feels bad

Bjork's **desirable difficulties** framework says conditions that make practice *harder* (spacing, interleaving, retrieval, varied conditions, reduced feedback) lower *performance during learning* while raising *long-term retention*. The learning-vs-performance distinction is the load-bearing idea: **what looks like learning in the moment is often just fluency.** ([Bjork & Bjork 2011 PDF](https://bjorklab.psych.ucla.edu/wp-content/uploads/sites/13/2016/04/EBjork_RBjork_2011.pdf) · [UNH](https://www.unh.edu/teaching-learning-resource-hub/sites/default/files/media/2023-06/itow-introducing-desirable-difficulties-into-practice-and-instruction-bjork-and-bjork.pdf))

The **fluency illusion** is the mechanism: learners mistake ease of processing for mastery. Koriat & Bjork's (2005) "foresight bias" and Benjamin, Bjork & Schwartz's (1998) "mismeasure of memory" show judgments of learning track *encoding fluency*, not future recall. Karpicke & Blunt (2011) found students *believed* rereading worked better while retrieval practice actually worked far better. 84% of surveyed students at an elite university still study by rereading. ([Structural Learning](https://www.structural-learning.com/post/fluency-illusions-students-think-they-know))

**The UX trap:** because desirable difficulties feel worse, learners avoid them — and an app that optimizes for "feels easy and productive" will systematically push users *away* from what works. **de Bruin (2023)**'s Start-and-Stick-to-Desirable-Difficulties (S2D2) framework models this as the interplay of *perceived effort* and *perceived learning*: students disengage when effort rises without visible payoff. **Zepeda et al. (2020)** catalog motivational strategies to keep learners on effortful methods. ([de Bruin](https://link.springer.com/article/10.1007/s10648-023-09766-w) · [Zepeda](https://www.sciencedirect.com/science/article/abs/pii/S2211368120300681))

**One live disagreement:** Kirschner (2025) argues the supposed tension between "desirable difficulties" and cognitive load theory is a **non-existent problem** — the two are complementary (make low-complexity tasks harder; scaffold high-complexity tasks), and papers claiming to reconcile them misrepresent both. Practically: **difficulty must be *desirable*, i.e. within working-memory reach.** Difficulty that overloads is not a desirable difficulty; it is just failure. ([Kirschner](https://www.kirschnered.nl/2025/01/02/desirable-difficulties-and-cognitive-load-three-authors-solve-a-non-existent-problem/))

---

## 6. Feedback — real but oversold, and type-dependent

Hattie & Timperley's (2007) famous *d* ≈ 0.73 for feedback is **inflated**. **Wisniewski, Zierer & Hattie (2020)** re-meta-analyzed and found a medium effect of **d ≈ 0.48** with enormous heterogeneity — and crucially, that feedback **without information value** (grades, praise, generic comments) has near-zero effect, while **elaborated, task-focused feedback** carries the benefit. ([Frontiers](https://www.frontiersin.org/journals/psychology/articles/10.3389/fpsyg.2019.03087/full))

**Timing is genuinely unresolved.** The classic generalization (immediate for procedural/simple, delayed for retention/complex/transfer) is unstable in recent work: Ryan et al. (2024) found immediate-post-item and delayed-end-of-test feedback broadly equivalent ([ASME](https://asmepublications.onlinelibrary.wiley.com/doi/full/10.1111/medu.15287)). Adesope et al. found feedback added only a *slight* boost on top of retrieval practice. **"How much is too much":** guidance beyond what the learner needs becomes redundant load (the expertise-reversal logic). The safe design stance: **specific, correct-answer-plus-why feedback, matched to the learner's current knowledge, and faded as competence grows.**

---

## 7. Transfer and representation — the founding story, honestly assessed

Study OS's origin story is that renaming a variable (`seen` → `box`) made Two Sum click. The research gives this **a real mechanism but a warning**.

- **Kaminski, Sloutsky & Heckler (2008, *Science*)** found learners taught with **abstract symbols transferred far better** than those taught with concrete examples, who "did no better than chance" on a novel game; concrete-then-abstract sequencing backfired. ([OSU](https://news.osu.edu/concrete-examples-dont-help-students-learn-math-study-finds/))
- **De Bock et al. (2011)** partially replicated but argued the transfer task itself was "abstract," so the conclusion overreaches ([ERIC](https://eric.ed.gov/?id=EJ918251)).
- **Trninic et al. (2020, *Cognitive Science*)** titled their paper "**The Disappearing Advantage of Abstract Examples**": the abstract advantage is **fragile and test-design-dependent**, not a law. ([Wiley](https://onlinelibrary.wiley.com/doi/full/10.1111/cogs.12851))

The deeper, well-replicated finding is **surface vs. deep structure** (Gick & Holyoak): learners latch onto surface features (variable names, story context, notation) and fail to transfer the underlying structure unless they are pushed to compare across examples. **This is the honest framing for the golden transcript:** a variable rename is a *surface-feature* change. It can help when the old name carried misleading connotations (the CLT story: it reduced extraneous load / improved grounding) — but **a single anecdote is not evidence that representation changes reliably drive conceptual transfer.** The falsifiable version: learners given misleading variable names should underperform matched learners given neutral names on a delayed transfer item, and the effect should shrink for higher-expertise learners (expertise reversal). ([Gick & Holyoak lineage](https://pressbooks.pub/learningenvironmentsdesign/chapter/jonassen-toward-a-design-theory-of-problem-solving/))

---

## 8. Micro-sessions — the weakest evidence, and the project's biggest bet

Three separate claims must be kept apart:

1. **Distributed > massed at any scale** — Solid. The spacing effect appears even with short gaps (the "lag effect"); you do not need long sessions to benefit from spacing.
2. **Learning happens in very short sessions** — Plausible; each retrieval episode is minutes-scale by nature.
3. **A minutes-a-day mobile cadence produces durable, complex-skill learning** — **Thin.** The microlearning literature is dominated by corporate-training reviews with weak designs. Monib et al. (2025) find "positive effects" but heterogeneous, low-quality evidence ([Heliyon](https://www.cell.com/heliyon/fulltext/S2405-8440(24)17444-0)); higher-ed meta-analyses report gains but rest on quasi-experiments. A recent *Scientific Reports* microlearning paper was **retracted**, a good caution flag ([retraction](https://www.nature.com/articles/s41598-025-29769-7)).

**There is no rigorous RCT demonstrating that a "minutes a day on a phone, distracted" cadence yields transfer to complex skills.** The closest positive signal is that *frequent, low-stakes retrieval with spacing* is exactly what the well-established findings prescribe — so a micro-session app is a reasonable bet built on solid mechanisms, but its core value proposition is **untested**. Falsification test: a controlled trial comparing a minutes-a-day spaced-retrieval app against the same total time in longer blocks, on a delayed transfer measure.

---

## 9. Replication problems to flag explicitly

- **Learning styles: Debunked.** Pashler et al. (2008) found no credible support for the "meshing hypothesis" (matching instruction to style); Newton (2017) and later reviews concur. It persists in teacher training anyway. Do not build on it. ([Newton 2017](https://pmc.ncbi.nlm.nih.gov/articles/PMC5366351/) · [Education Next](https://www.educationnext.org/stubborn-myth-learning-styles-state-teacher-license-prep-materials-debunked-theory/))
- **Cognitive load theory:** strong effects, but measurement and falsifiability are contested (see §4).
- **Transfer:** effect sizes are wildly design-sensitive (Kaminski vs. De Bock vs. Trninic).
- **Retrieval practice:** robust, but the Sigayret (2026) nulls and Redifer (2026) load finding mark real boundary conditions.
- **Interleaving:** the one large field RCT showed the effect on *short-term* outcomes only.

---

## So what for Study OS

1. **Build the core on retrieval + spacing, not on representation tricks.** These are the two high-utility, replicated levers. A session should be a *retrieval event* (answer before reveal), scheduled at ~10–20% of the target retention interval, not a re-explanation.
2. **Make difficulty "desirable" = calibrate it.** Use CLT + expertise reversal as the difficulty controller: heavy scaffolding/worked examples for novices, faded for experts. The old project's "controller owns curriculum authority" instinct is well-founded — but the signal that should drive it is *demonstrated mastery*, not a fixed script.
3. **Design against the fluency illusion.** Because users will prefer what feels easy, the product must *make productive difficulty feel legible* — show progress from *retrieval success over time*, not from content consumed. This is where S2D2 and Zepeda's motivation work applies directly.
4. **Feedback: specific and task-focused, not grades or praise.** d ≈ 0.48 overall, but the information-value-free forms are near-zero. Timing is unsettled — default to immediate correctness + brief why, and let it fade.
5. **Treat the variable-rename story as a hypothesis, not a thesis.** The transfer literature says surface features matter but that their effects are fragile and expertise-dependent. Test it: misleading vs. neutral naming, delayed transfer item, novice vs. expert. If it doesn't replicate, the founding story is a design intuition, not a principle.
6. **The micro-session bet is unproven.** The mechanisms (spacing, retrieval) are solid; the *cadence-and-complexity* claim is not. Build the fastest falsification test you can: minutes-a-day vs. equal-time longer blocks, delayed transfer outcome.
7. **What would break this plan:** if retrieval practice fails to produce transfer in your specific complex-skill domain (cf. Sigayret/Redifer), or if measured "desirable difficulty" turns out to just be cognitive overload for your users. Both are testable early.

---

### Key sources
Dunlosky et al. 2013 · Adesope et al. 2017 · Cepeda et al. 2006/2008 · Brunmair & Richter 2019 · Sweller 2023/2024 · Kalyuga (expertise reversal) · Bjork & Bjork 2011 · de Bruin 2023 (S2D2) · Zepeda et al. 2020 · Wisniewski et al. 2020 · Kaminski et al. 2008 · De Bock et al. 2011 · Trninic et al. 2020 · Pashler et al. 2008 · Monib et al. 2025 · Sigayret et al. 2026 · NBER w31853.
