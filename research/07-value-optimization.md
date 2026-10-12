# Study OS Research Digest 07 — Value Optimization: Measuring Learning Gain and Maximizing Learning per Unit of Attention

*Domain: the objective function, the measurement, and the decision algorithms. Web research, technical framing, 2015–2026 preference. See BRIEF.md for project context and ground rules.*

---

## TL;DR for the builder

- You cannot directly observe durable learning in a live product. The field's honest position: **immediate in-session performance is a proxy that can diverge from delayed retention** (Bjork's performance/learning distinction). Any product optimizing accuracy, speed, or hint-reduction is optimizing a proxy.
- **Usable live signals**: normalized gain on a repeated instrument (with caveats), item-level mastery via knowledge tracing, learning-curve slope, retention-probability models (half-life regression / FSRS). The gold standard — delayed, transfer tests — is rarely collectible.
- **Adaptive algorithms**: multi-armed bandits work at scale with many samples per action; contextual bandits rarely beat a well-tuned population-level policy; RL for instructional sequencing gives modest, theory-constrained wins.
- **Experiment design**: peeking inflates false positives; use sequential/always-valid methods. Realistic field effect sizes are small (~0.2 SD is "large" per Kraft).
- **Attention economy**: time-on-task correlates only weakly with learning; "learning efficiency" (gain per unit time/effort) is the right lens but weakly operationalized.
- **North star**: engagement metrics (streaks, DAU, session length) are the standard trap; learning products that care track mastery counts / retention.

---

## 1. Measuring learning gain

### 1.1 Normalized gain (Hake's g)

Canonical pre/post measure from physics education: `g = (post − pre) / (max − pre)`, i.e. `(post − pre) / (100 − pre)` for percent scores. Hake (1998) used it to separate interactive-engagement courses (g ≈ 0.48) from traditional (g ≈ 0.23); the <0.3 / 0.3–0.6 / >0.7 bands are still widely quoted.

Caveats that matter for a product:
- **Prescore bias**: Coletta & Phillips (2005) found class-average gain correlated with class-average pretest; Von Korff et al. (2016) meta-analysis did not replicate it. Unresolved; Coletta (2020) argues g should still be used.
- **Ignores drop rates** (a big deal for a mobile app with churn) and **ignores losses** (post < pre is common; g assumes no decline).
- **Definition drift**: "gain of averages" vs "average of gains" differ by up to ~10%.
- PhysPort's practical guidance: use **matched** data (only learners who took both), pick one definition deliberately, and for small/diverse groups prefer **effect size** or **normalized change** (Marx & Cummings 2007), since g swings wildly in small n.

Links: PhysPort https://www.physport.org/expert/gain/ ; Coletta 2020 https://link.aps.org/doi/10.1103/PhysRevPhysEducRes.16.010108

**So what:** normalized gain needs a fixed instrument administered twice. In a subway app you won't get clean pre/post from most users. Usable for a small motivated cohort or a paid study, not as a live metric.

### 1.2 Cohen's d on pre/post, and effect-size reality

Kraft (2020, *Educational Researcher*, ~1,250 cites) is the reference: Cohen's 0.2/0.5/0.8 benchmarks mislead for field interventions. He proposes **<0.05 SD = small, 0.05–0.20 SD = medium, ≥0.20 SD = large** for standardized achievement outcomes, combined with cost and scalability. High-dosage tutoring lands around **0.23 SD**. So "0.2 SD" is a strong field effect, not a small one. (https://www.matthewakraft.com/s/Kraft-2020-Interpreting-Effect-Sizes-ER.pdf)

Hattie's ".40 hinge point" is heavily criticized — Bergeron & Rivard (2017, *McGill J. Education*) call the meta-meta-analytic method a misuse of Cohen's d; Slavin and others note few real interventions reach .40. Don't anchor on .40.

### 1.3 Learning curves / power law of practice

Newell & Rosenbloom (1981) formalized the **power law of practice**: performance improves as a power function of practice trials — sharp early gains, diminishing later gains. Alternatives fit better in some data — Heathcote et al. (2000) argue for an **exponential** law; Donner & Hardy (2015) find **piecewise** power laws in individual learning curves. Fitting a curve to per-skill accuracy yields a **learning rate**, a per-learner quantity.

The big empirical result: Koedinger et al. (2023, PNAS, "An astonishing regularity in student learning rate"). Across 27 datasets and 1.3M observations, students vary a lot in **initial** accuracy (~55% lower half, ~75% upper half) but learn at **remarkably similar rates** (~0.1 log-odds / ~2.5% accuracy per practice opportunity). Implication: achievement gaps are largely opportunity gaps; personalize *how much practice*, not *how fast*.

**Important counter-evidence:** a 2024 EDM replication (Simpson et al.) reproduced it on 15k+ MATHia students, BUT arXiv 2605.01690 ("Revisiting the Regularity of Student Learning Rate") shows the estimated student-slope variance **inflates by ~118%** when each student–skill pair is truncated to 10 attempts, and depends on which observations enter the fit. The "astonishing regularity" is fragile to modeling choices. Treat "everyone learns at the same rate" as a hypothesis, not a law.

### 1.4 Item-level mastery thresholds

BKT (Corbett & Anderson) is the workhorse; the classic mastery criterion is **P(mastery) ≥ 0.95**. Two recent results complicate it:
- **EDM 2025, "How Much Mastery is Enough Mastery?"** (Rori WhatsApp math tutor, 543 students, 49,612 problems): the 0.98+ band outperformed every lower band on next-lesson accuracy and learning; authors argue much "over-practice" may not be over-practice. Correlational only.
- **arXiv 2610.00095, "One Mastery Threshold Does Not Fit All Knowledge Tracing Models"**: 6 KT models, 12 thresholds (0.50–0.99). BKT barely responds to threshold; neural models become far more selective; the *same cutoff does not mean the same mastery* across models. Stricter thresholds can restrict advancement inequitably (stronger-prior students advanced up to 3.26× as often).

**Practical:** 0.95 is a default, not a validated optimum; recalibrate per model and per skill, and judge thresholds on performance + practice burden + advancement fairness together.

### 1.5 Transfer and delayed retention tests

- **Delayed retention** is the gold standard for "did it stick." Bjork's framing: performance during training ≠ learning.
- **Transfer**: near transfer (similar context) is common; **far transfer is rare** — Sala et al. (2019, *Collabra*, second-order meta-analysis of cognitive training) find near transfer consistent and far transfer dubious. Don't build a value proposition that depends on far transfer.
- **Overlearning** (practice past criterion): Driskell et al. (1992) meta found a moderate retention benefit; Rohrer & Taylor found **limited** long-term benefit for extra study time — diminishing returns. This is the empirical basis for not grinding past mastery.

---

## 2. Proxies and their validity

Load-bearing section: the product will optimize a proxy.

### 2.1 The performance/learning gap

Bjork & Bjork: conditions that make practice *feel* harder and perform worse short-term (spacing, interleaving, testing, varied practice) often produce *better* long-term retention. Classic demonstration: **massed practice beats spaced practice on immediate tests but loses on delayed tests** (Karpicke & Roediger 2007). So immediate accuracy can rank methods *backwards*.

### 2.2 Metacognitive / fluency illusions

Learners (and naive product metrics) mistake fluency for mastery. Judgments of learning (JOLs) are systematically overconfident; delaying JOLs and reducing fluency corrects the illusion (Wang et al. 2019; Pieger et al. 2017; "Mending metacognitive illusions in JOLs," 2026). A product measuring "how confident/fast the learner feels" is measuring fluency, not learning.

### 2.3 Behavioral proxies that DO carry signal

- **Gaming the system** (Baker et al. 2004): rapid guessing + excessive hint use. Students who gamed learned **~38% less** than students who never gamed; gaming frequency correlates with post-test as strongly as prior knowledge. Hint-abuse and rapid-guessing detectors are *negatively* predictive — useful as data-quality gates. (https://learninganalytics.upenn.edu/ryanbaker/p383-baker-rev.pdf)
- **Response time + correctness (fluency)**: used as a mastery signal in ITS and in the CK-12 bandit reward (below). Speed alone is ambiguous (fast-but-wrong = guessing; slow-but-right = effortful).
- **Time on-task**: Godwin et al. (2021, *Educational Psychology*): on-task behavior is **positively but weakly** correlated with learning, and correlation strength is **highly variable** across studies — "not simply an issue of measurement." Time on task is a weak proxy.

### 2.4 What practitioners do instead of delayed tests

- **Spaced-repetition scheduling models** infer retention probability from the review history itself: Duolingo's **half-life regression** (Settles & Meeder, ACL 2016) predicts recall probability and half-life; **FSRS** (Free Spaced Repetition Scheduler, now default in Anki) uses a three-component memory model (Difficulty, Stability, Retrievability) fit per user. These give a *predicted* retention curve without a delayed test.
- **Knowledge tracing** (BKT, DKT, AKT, …) infers latent mastery from in-session responses.
- **Learning-curve slope** as a per-skill learning-rate estimate.

All three are **model-based proxies** — they predict delayed performance but are validated on the same in-session data, which is circular unless occasionally checked against a held-out delayed measure.

**Falsification note:** the experiment that breaks "proxies track durable learning" is a held-out delayed test showing in-session gains don't survive. Build in *one* delayed checkpoint (e.g., a 1–2 week re-test of sampled items) even if only a fraction of users take it.

---

## 3. Adaptive decision algorithms

### 3.1 Bandits in edtech — the best-evidenced modern result

**Schmucker et al. 2025 (arXiv 2508.00270, CMU + CK-12)**, "Insights from Multi-Armed and Contextual Bandits in Large-Scale Education":
- ~1M students, 23.8M assistance requests, 10,210 questions, 43,355 candidate actions (hints, paragraph, vocab, remove distractor, first letter, none).
- **Reward**: `R = 0.4·(reattempt correct) + 0.6·(student ability, 3PL-IRT at session end)`. The low-variance reattempt-correctness signal trained better than noisy end-of-session ability.
- **MAB beat random** by 5.2–15.0% reattempt correctness in A/B tests over 166k sessions; biology failure-to-complete dropped ~11% relative.
- **Contextual bandits did NOT clearly beat a well-tuned MAB** — heterogeneous treatment effects exist (by question position, IRT ability) but are "often too small for CB policies to provide significant improvements."
- **Scale matters**: hundreds of samples/action here vs ~6.5 in Prihar et al. 2022.
- Many expert-written assistance actions performed **no better than no assistance**.

**Prihar et al. 2023, "A Bandit You Can Trust" (ASSISTments)**: Dynamic Linear Epsilon-Greedy contextual bandit that keeps unbiased A/B analysis possible; slightly increased learning vs A/B testing; high- and low-knowledge students benefited from different content. Data at osf.io/zuwf7.

**Takeaway:** bandits are the most practical adaptive layer, but (a) you need many samples per arm, (b) reward design dominates, (c) personalization often adds little over a good population policy, (d) non-stationarity (the learner changes; rewards shift) breaks i.i.d. assumptions — see Luo et al. 2018 (non-stationary contextual bandits), Wu et al. 2018 (change detection).

### 3.2 RL for instructional sequencing

**Doroudi, Aleven & Brunskill (2019, IJAIED 29(4):568–620), "Where's the Reward? A Review of Reinforcement Learning for Instructional Sequencing"**: three historical waves; over half of empirical studies found RL-derived policies significantly beat baselines; **best results come when RL is constrained by cognitive/learning-science theory**, and they urge offline analyses robust to any single student model.

Related: **Chi et al. (2011)** RL tutorial tactics — "content exposure and practice opportunities can help students learn even when tutors have poor pedagogical tactics" (practice volume may matter more than tactic cleverness). **Ausin et al. (2021)** deep RL + credit assignment improved learning in two classroom studies.

**Caveat:** these are ITS/classroom settings with large n and a fixed curriculum; gains are modest and sensitive to the student model. Don't expect a from-scratch RL agent on a subway app to beat simple spaced repetition.

### 3.3 Bayesian optimization / active learning for item selection

Bayesian optimization and Bayesian active learning (BALD / prediction-oriented BALD) select the most informative items to test or label; used for **comparative judgement** in education (Gray 2024, *Computers & Education: AI*) and for choosing which items to pretest/calibrate. Practical use: item calibration (IRT parameters) with few responses, and adaptive-testing item selection. Not a reward optimizer — an *information* optimizer. Useful for cold-start.

---

## 4. Experiment design at small scale

- **The peeking problem**: standard frequentist A/B testing assumes a single look. Checking continuously and stopping when significant inflates the false-positive rate far above 5%. Fixes: **sequential testing / always-valid inference / alpha-spending**, or Evan Miller's "Simple Sequential A/B Testing." (https://www.evanmiller.org/sequential-ab-testing.html ; GrowthBook guidance https://www.growthbook.io/blog/what-is-a-b-testing)
- **Low traffic**: with few users you can't detect small effects. Options: (a) accept only large effects, (b) use **within-subject / crossover** designs (more power per user, but order/carryover effects must be handled), (c) use **single-case / N-of-1 / alternating-treatments** designs borrowed from behavior analysis, (d) pool across users with hierarchical models.
- **Effect-size reality**: plan around **0.1–0.3 SD** for a real intervention; anything you can detect with 20 users is probably huge or spurious. Kraft's thresholds (above) are the right calibration.
- **Unit of randomization**: randomize by learner, item, or session — each has different contamination and power properties. In-session A/B on the *same* learner (alternating items) can be far more sensitive than between-learner, which is why bandit-style within-session comparisons are attractive.
- **What would falsify**: a "win" from a small within-subject test that reverses on a delayed or between-subject test. Always ask whether the effect survives a change of design.

---

## 5. Attention-economy framing: learning per unit of attention

- **Time-on-task hypothesis** is weak: Godwin et al. 2021 (above) — positive but weak and variable correlation. So "more minutes" ≠ "more learning."
- **Cognitive efficiency** (Hoffman 2012, *Learning & Instruction*): "qualitative increases in knowledge gained in relation to the time and effort invested." Directly the objective function you're after — but a conceptual/methodological construct, not a turnkey metric.
- **Instructional efficiency** (Paas & Van Merriënboer tradition; e.g., "Instructional efficiency: the role of prior knowledge and cognitive load," 2023): efficiency = performance relative to invested mental effort (often subjective effort scales). Used to compare instructional designs.
- **Spacing / diminishing returns**: Cepeda et al. (2008, *Psych Science*): there is an **optimal inter-study gap** for a given retention interval, and the interaction implies "many educational practices are highly inefficient." Practically, schedule review gaps at ~10–20% of the target retention interval (≈1 day for a 1-week test, ≈3 weeks for a 1-year test). Closest thing to a principled "value per unit attention" rule.
- **Session length**: the massed-vs-distributed literature (e.g., motor learning, Kwon 2015) shows distributed practice is more efficient than massed for durable learning; long single sessions hit diminishing returns. There is **no strong universal "optimal session length"** — it depends on task and retention target. Treat session length as an empirical knob.
- **Beyond Time on Task** (JLA 2026) argues digital environments need richer engagement/attention measures than raw time.

---

## 6. North-star metric design

- **Generic NSM literature** (Amplitude, Mixpanel, GoPractice, Product School): a good NSM reflects *customer value*, not activity. The repeated warning: **engagement is the easiest trap** — DAU, session length, logins are "simple to measure" and "≠ value." (https://amplitude.com/blog/product-north-star-metric)
- **Learning products specifically** track a mix: completion/progress/mastery counts (LMS-style: "mastery measured by assessment, not time spent"), pass rates, streaks (Duolingo), skill mastery (Khan Academy's mastery levels). Fora Soft's learning-metrics writeup is a concrete list.
- **Duolingo's own efficacy framing**: "lessons mattered more than time spent learning" — they explicitly de-emphasize time-on-task. (https://www.duolingo.com/efficacy/studies ; research hub https://research.duolingo.com/)
- **Gamification ≠ learning, but can raise durable engagement**: a 2026 RCT (*Quantitative Marketing & Economics*), 10,000 students on an Indian English app, found a contest raised stories completed by 45% during and **76% twelve weeks later**, with effects concentrated in the already-engaged upper tail. Incentives can change the *distribution of engagement*, not just transient activity.
- **UNESCO "An Ed-Tech Tragedy?"** (2023): a blunt critique that time spent on education is not of equal value — a caution against equating screen time with learning.
- **Postmortem-style lesson**: a metric that rewards engagement will drift the product toward easy, high-frequency content (streaks, short lessons) and away from hard, durable learning — exactly Bjork's performance/learning trap at the product level.

---

## So what for Study OS

1. **Pick a proxy, and be honest that it's a proxy.** Optimize something like *predicted retention probability at a target delay* (half-life-regression / FSRS-style) or *learning-curve slope per skill*, not raw accuracy or time. Accuracy is the fluency trap.

2. **Instrument one delayed checkpoint.** Even a low-compliance delayed re-test on a sampled subset of items is the only way to validate your proxy. Without it you are optimizing in the dark. This is the single highest-value measurement decision.

3. **Mastery thresholds: don't hardcode 0.95.** Recent work suggests higher thresholds (0.98) may help future learning, that the threshold interacts with the KT model, and that aggressive thresholds can hurt advancement equity. Make it a tunable per-skill parameter and monitor practice burden.

4. **Adaptive layer: start with a simple bandit on a low-variance reward, not RL.** The CK-12 result is the strongest practical evidence: MAB > random, CB adds little over a good MAB, reward variance dominates. RL for sequencing gives modest gains and needs theory constraints. Use Bayesian active learning only for cold-start item calibration.

5. **Design experiments for small n deliberately.** Sequential/always-valid tests to avoid peeking inflation; prefer within-subject or N-of-1 where contamination allows; calibrate expectations to 0.1–0.3 SD. Assume you can only detect large effects.

6. **North star = mastery/retention counts, not streaks.** If the metric is engagement, the optimizer will find the fluency illusion and exploit it. Watch the "learning per unit attention" ratio (cognitive-efficiency framing) as a secondary guardrail, and use spacing (gap ≈ 10–20% of retention target) as the principled scheduler.

7. **Watch for gaming.** Rapid guessing and hint-abuse are negatively predictive of learning (Baker). Use them as data-quality gates so your proxy isn't poisoned.

**Open question / falsifier:** There is no well-validated, low-cost, live measure of *durable* learning. Every live signal is a proxy. The load-bearing assumption for Study OS is that in-session proxies (retention-probability models, learning-curve slope, spacing-scheduled success) predict delayed transfer. The evidence for that is *model-based and largely circular*; the evidence against it (fluency illusion, massed-vs-spaced reversal, far-transfer failures) is strong and old. Build the delayed check, or you are betting the product on an unvalidated proxy.

---

## Key links

- Hake g / PhysPort: https://www.physport.org/expert/gain/
- Coletta 2020 (normalized gain defense): https://link.aps.org/doi/10.1103/PhysRevPhysEducRes.16.010108
- Kraft 2020 (effect sizes): https://www.matthewakraft.com/s/Kraft-2020-Interpreting-Effect-Sizes-ER.pdf
- Koedinger 2023 (learning rate): https://www.pnas.org/doi/10.1073/pnas.2221311120
- Revisiting the regularity: https://arxiv.org/html/2605.01690v2
- EDM 2024 replication: https://educationaldatamining.org/edm2024/proceedings/2024.EDM-short-papers.40/index.html
- EDM 2025 mastery: https://educationaldatamining.org/EDM2025/proceedings/2025.EDM.short-papers.4/index.html
- One Mastery Threshold Does Not Fit All: https://arxiv.org/pdf/2610.00095
- Bjork & Bjork, desirable difficulties: https://bjorklab.psych.ucla.edu/wp-content/uploads/sites/13/2016/04/EBjork_RBjork_2011.pdf
- Baker, gaming the system: https://learninganalytics.upenn.edu/ryanbaker/p383-baker-rev.pdf
- Godwin time-on-task: https://www.tandfonline.com/doi/abs/10.1080/01443410.2021.1894324
- Doroudi/Aleven/Brunskill RL review (ERIC): https://eric.ed.gov/?id=EJ1235264
- CK-12 bandits: https://arxiv.org/html/2508.00270v1
- A Bandit You Can Trust (ASSISTments): https://doi.org/10.1145/3565472.3592955
- Duolingo half-life regression: https://research.duolingo.com/papers/settles.acl16.pdf
- FSRS: https://github.com/open-spaced-repetition/fsrs4anki
- Cepeda spacing: https://journals.sagepub.com/doi/10.1111/j.1467-9280.2008.02209.x
- Brunmair & Richter interleaving: https://pubmed.ncbi.nlm.nih.gov/31556629/
- Sequential A/B testing: https://www.evanmiller.org/sequential-ab-testing.html
- Hattie critique: https://mje.mcgill.ca/article/view/9475/7229
- Cognitive efficiency: https://www.sciencedirect.com/science/article/abs/pii/S095947521100079X
- Duolingo efficacy: https://www.duolingo.com/efficacy/studies
- Contests/gamification RCT: https://doi.org/10.1007/s11129-026-09311-3
