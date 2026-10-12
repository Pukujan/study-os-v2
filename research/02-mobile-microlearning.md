# Mobile-first microlearning and engagement mechanics

**Can a distracted person on a subway actually learn complex things this way?**

*Research digest 02. Compiled 2026-10-12. Evidence tags: **[ESTABLISHED]** = replicated/meta-analysed; **[MIXED]** = real but contested or moderated; **[THIN]** = small-n, single-domain, or self-reported; **[MARKETING]** = company claim with no independent verification.*

---

## 0. The verdict up front

The honest answer to the domain question splits in two, and the split is the most important thing in this document:

- **Retaining discrete items** (vocabulary, facts, definitions, "which of these is Big O of n log n") in 1–5 minute fragmented mobile sessions is **well-supported**. The mechanisms — spacing, retrieval practice, adaptive scheduling — are among the most replicated findings in learning science.
- **Building complex, transferable skill** (producing an argument, writing working code, decomposing a novel problem) from fragmented mobile sessions is **not demonstrated anywhere I could find**. Every strong effect below is measured on *recognition or recall of discrete items*. The closest thing to a real-world test (Duolingo's own A2 study) shows the ceiling clearly: ~125 hours of app use produced *Intermediate Low reading* but only *Novice-high listening*.

So the defensible design thesis is not "make hard things easy on a phone." It is **"use fragmented mobile attention to build and maintain the declarative substrate and the habit, and reserve longer/desktop sessions for the transfer work."** Whether tap-based micro-interactions can *teach* complex skill is an **untested hypothesis a product would be running an experiment on**, not a settled fact.

**Compact effect-size table (the falsification-friendly artifact):**

| Intervention | Outcome | Effect | Rigour note |
|---|---|---|---|
| Spaced repetition (broad) | Objective test scores | SMD **0.78** [0.56–0.99], 21,415 learners (Maye & Hurley 2026) | **[ESTABLISHED]** |
| Retrieval practice (classroom) | Learning | 57% of 49 effects medium/large, n=5,374 (Agarwal et al. 2021) | **[ESTABLISHED]** |
| Gamification | Cognitive | Hedges **g = .49** [.30–.69] | Survives rigor filter (g = .42) |
| Gamification | Motivational | g = .36 [.18–.54] | **Drops to ns** (g=.22) under strict designs |
| Gamification | Behavioural | g = .25 [.04–.46] | **Drops to ns** under strict designs |
| Spacing, simple psychomotor | Retention | **d = 0.97** (Donovan & Radosevich 1999) | **[ESTABLISHED]** |
| Spacing, **complex** tasks | Retention | **d = 0.07–0.42** | Same review — the key caveat |

The last two rows are the whole game: **the spacing benefit collapses as task complexity rises.**

---

## 1. What session-length and cadence evidence actually says

Duolingo is the most transparent operator here, and its engineering blog is unusually candid.

- **They abandoned raw session counts and raw time-on-task** and now optimise a metric they call **Time Spent Learning Well (TSLW) = minutes on the "path" + 0.5 × minutes elsewhere**. Rationale: with 15 min/day a learner might do 5 easy sessions or 3 hard ones, so "fewer sessions" isn't worse ([blog](https://blog.duolingo.com/time-spent-learning-well/)).
- **The target is 15 min/day**, explicitly because "5 minutes a day would take much longer to reach goals." Their own data: *shortening* lessons did **not** raise TSLW and ultimately hurt it; *adding content* always helped.
- **Scheduling is model-driven.** Duolingo's **half-life regression** (Settles & Meeder, [ACL 2016](https://aclanthology.org/P16-1174/)) estimates each word's half-life in memory (p = 2^(−Δ/h)) and reviews "on the verge of forgetting." It cut prediction error ~45% vs. the Leitner baseline (mean absolute error 0.13, i.e. a 50% recall prediction really spans 37–63%) and lifted daily engagement 9.5–12% in A/B tests. Code/data are public ([github](https://github.com/duolingo/halflife-regression)).
- **Design implication that runs against intuition:** daily cadence beats session length; variety beats intensity; and "they can't learn if they churn" is stated company doctrine.

**Microlearning meta-analysis:** Jainuri et al. 2025 ([MATHEMA](https://publikasi.teknokrat.ac.id/index.php/jurnalmathema/article/view/517)) pooled 42 studies / 15,673 participants: retention **OR = 1.87** [1.45–2.41], learning outcomes **SMD = 0.74** [0.58–0.90], heterogeneity I²=67–71%. Strongest in STEM + mobile. **Caveat:** it does not isolate "short session" as the causal ingredient — the effective packages bundle spacing, feedback, and engagement.

---

## 2. The engagement ≠ learning problem

This is the central tension and where most product claims die.

- **The "34 hours = one semester" claim is real but narrower than the headline.** Vesselinov & Grego (2012, Duolingo-funded) reported a *range* of **26–49 hours** to hit a **WebCAPE placement cutoff** (270) — a placement threshold that "weights recognition heavily," not a proficiency measure. Krashen's [critique](http://sdkrashen.com/content/articles/krashen-does-duolingo-trump.pdf) adds the killer detail: participants averaged 35 years old, 69% college graduates, and **only 90 of 156 finished**. Mean gain 8.1 points/hr, but **median 3.9** and SD 12.1 — the headline number is an outlier-driven mean.
- **Duolingo's own efficacy page** ([blog](https://blog.duolingo.com/results-duolingo-efficacy-studies/)) reports "five sections ≈ five university semesters" but gives **no sample sizes, no effect sizes, no hours**. Treat as **[MARKETING]** with a kernel of truth.
- **The most rigorous internal study** is the A2 whitepaper (Jiang, Rollinson, Plonsky & Pajak, [DRR-20-04](http://duolingo-papers.s3.amazonaws.com/reports/Duolingo_whitepaper_language_read_listen_2020.pdf), n=225): median **112–125 hours** → Intermediate Low *reading*, only approaching/at Novice High *listening*. The listening gap is the honest headline.
- **Time-on-task correlates only weakly with learning** — Godwin et al. 2021 ([Educational Psychology](https://doi.org/10.1080/01443410.2021.1894324)) found a positive but weak association and large variability. Engagement metrics are a *proxy* for learning, not a measure of it, and Duolingo admits this explicitly.

**Biggest single counterpoint to "notifications build habits":** the Aliakbari et al. 2026 RCT ([KMANPUB](https://doi.org/10.61838/kman.aitech.4724), n=150, 9 weeks) found algorithmic reminders produced **82% vs 67% vs 49%** daily compliance — but consistency **collapsed once reminders stopped**, which the authors call *extrinsic dependency*. **Caveat:** low-tier journal; use it as directionally consistent with Duolingo's *own* independent finding of a novelty effect (§4), not as a load-bearing citation.

---

## 3. Interruption, resumption, and the subway constraint

The cognitive-science core here is solid; the *commuter-specific* literature is thin and mostly lab/AR, not transit.

- **Resumption lag is real and roughly doubles your action time.** Altmann & Trafton ([CogSci 2004](https://www.interruptions.net/literature/Altmann-CogSci04.pdf)): after an interruption the first action took **3.8 s vs 1.9 s** between normal actions. Longer interruptions → longer lags; **cues available *before* the interruption reduce the lag** (people spontaneously "prepare to resume").
- **The "Memory for Goals" account:** interruptions disrupt task memory, so resumption is a *memory retrieval* problem — you need a cue that reactivates the goal ([Brumby, Janssen & Mark 2018](https://cpjanssen.nl/Publications/Janssen_brumby_mark_InterruptionsChapter.pdf)).
- **Which cue works — the actionable finding.** Chen et al. 2025 ([Scientific Reports](https://pmc.ncbi.nlm.nih.gov/articles/PMC12271330/)) tested retrieval cues ("here's where you were") vs **assistant cues ("here's what's next")**. Assistant cues won, especially **under fatigue**; retrieval cues were no better than nothing. Design implication: **re-entry screens should state the next action, not re-narrate history.**
- **Goal gradient / progress illusion.** Kivetz, Urminsky & Zheng ([JMR 2006](https://home.uchicago.edu/ourminsky/Goal-Gradient_Illusionary_Goal_Progress.pdf)): café customers accelerated ~20% toward a free coffee, and a **12-stamp card with 2 pre-given "bonus" stamps was completed faster than a plain 10-stamp card** — *illusionary* progress motivates. Acceleration predicted retention. This is the empirical backbone for progress bars and for Duolingo's streak math.
- **Commuter specifics (offline, one-hand, no-audio) have almost no peer-reviewed design literature.** What exists: mobile-typing research shows text entry is a measurable sensorimotor + cognitive load ([Viviani 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12563232/)), and small screens + typing raise extraneous load ([Faudzi et al. 2024](https://www.sciencedirect.com/science/article/pii/S2405844024135256)). Treat one-handed, offline, silent operation as **engineering constraints derived from first principles**, not as evidence-backed design rules.

---

## 4. Notifications, streaks, loss aversion

- **The best primary source is Duolingo's own KDD paper.** Yancey & Settles ([KDD 2020](https://dl.acm.org/doi/10.1145/3394486.3403351)) built a "sleeping, recovering bandit" over ~200M reminders and got **+0.5% DAU and +2% new-user retention** over a strong baseline. Two transferable findings: (1) **novelty effect is real** — new templates work better then fade, so the algorithm must *rotate*, not converge; (2) **eligibility bias** — you must compare a template only against others shown to the same kind of learner or you mis-rank it. Dataset is public.
- **Streak mechanics, with numbers** ([blog](https://blog.duolingo.com/how-duolingo-streak-builds-habit/)): a 7-day streak → **3.6× more likely to complete the course**; streak animations → **+1.7%** week-1 retention; allowing two **Streak Freezes** → **+0.38% DAU** *without* increasing missed days. The loss-aversion framing ("protect your streak") is company narrative; the *numbers* are A/B-tested, the *psychology story* is not.
- **A cautionary note from their own design:** Duolingo explicitly moved Monthly Challenges from XP-based to quest-based because XP let users game the system late in the month. If your objective function is points, users optimise points.
- **Fogg's model (B=MAP)** remains the standard practitioner framing — behaviour needs Motivation × Ability × Prompt converging at one moment ([behaviormodel.org](https://www.behaviormodel.org/)). It's a useful design checklist, **[THIN]** as predictive science.

---

## 5. Gamification: what the meta-analysis actually says

The best single source is **Sailer & Homner 2020**, *Educational Psychology Review* ([link](https://link.springer.com/article/10.1007/s10648-019-09498-w)), 38 publications / 40 experiments:

- Cognitive **g = .49**, motivational **g = .36**, behavioural **g = .25** — all small-to-moderate, all significant, but **I² = 64–75%** (unexplained heterogeneity).
- **Under a high-rigour subsplit, only the cognitive effect survived** (g = .42). Motivational and behavioural effects went **non-significant**. That is the finding to internalise: gamification's *learning* benefit is the robust one; its *motivation* benefit is the fragile one.
- **Moderator:** competition **combined with collaboration** beat competition alone — "mere competition might be suboptimal." School settings > higher-ed/informal for cognitive outcomes. No delayed-posttest data, so **long-term persistence is unknown**.

**Overjustification / undermining.** The classic is Deci, Koestner & Ryan 1999 ([PubMed](https://pubmed.ncbi.nlm.nih.gov/10589297/)), 128 experiments: **expected, contingent rewards** (engagement- and completion-contingent) *undermine* intrinsic motivation; positive feedback and unexpected rewards do not. This is genuinely contested (Cameron & Pierce, Eisenberger et al. dispute effect sizes), so mark **[MIXED]** — but the practical reading is safe: **reward the behaviour you want to persist, and prefer informational feedback (competence signals) over controlling, contingent payouts.** The Aliakbari post-intervention collapse is the applied echo.

---

## 6. Input modality and cognitive load

Weakest evidence section; keep claims modest.

- **Touchscreen typing is costly.** Mobile text entry is a dual sensorimotor+cognitive task with measurable error and load ([Viviani 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12563232/)); small screens make typing/searching harder and raise extraneous load ([Faudzi 2024](https://www.sciencedirect.com/science/article/pii/S2405844024135256)).
- **Multimodal can help or hurt.** Deng 2026 ([Frontiers](https://www.frontiersin.org/journals/psychology/articles/10.3389/fpsyg.2026.1758104/full)) reviews 31 studies: multimodal support (glosses, captions) helps when split attention is minimised, but dense simultaneous channels "exceed capacity," and **lower working-memory learners are hit hardest**. One cited study found an **inverted-U with an optimum near 3–4 channels**; another found sensory overload correlated negatively with retention (β = −0.53). **No-audio-on-a-subway is therefore defensible:** audio is one more channel competing for a noise-stressed, one-hand-occupied learner.
- **Practical reading:** tap/multiple-choice ≈ low extraneous load (good for the subway); micro-typing (one word, one number) is acceptable in small doses; anything requiring composing sentences on a phone is high-friction. Recognition formats are cheap to produce and score — but see §0: they test recognition, not production.

---

## 7. Matuschak and the "mnemonic medium"

Quantum Country ([quantum.country](https://quantum.country/)) embeds spaced-repetition prompts directly in a textbook. Matuschak's own evidence ratings ([notes](https://notes.andymatuschak.org/Effects_of_the_mnemonic_medium_on_reader_memory)):

- **Strong** evidence that completing reviews produces reliable recall; **moderate** that the time cost is acceptable (**35–50% reading-time overhead**); **emerging** that readers would remember much less without reviews; **no** evidence that it generalises across domains.
- **Only 29%** of readers who finished the in-text level went on to the 1-month review level. Retention of the *willing* is not retention of the *many*.
- His framing essay *[Why Books Don't Work](https://andymatuschak.org/books/)* argues the medium rests on "transmissionism" — that reading transfers understanding — which is false; the fix is to make the *medium's grain* force retrieval.

**Do not over-claim Quantum Country**: tiny, self-selected, one domain. It is a **design existence-proof** (you can interleave prompts into prose), not effectiveness evidence.

---

## 8. So what for Study OS

**Design implications the evidence supports**

1. **Split the job by material type.** Micro-sessions for the declarative substrate (terms, notation, signatures, definitions, "what does this operator do") and for habit; longer sessions for transfer work. Do not promise that subway taps teach DSA.
2. **Make the daily unit ~5–15 min and make it daily.** Cadence beats length; variety beats intensity; a 30-second "streak-saver" is better than a missed day (Duolingo's own doctrine).
3. **Schedule with a forgetting model, not fixed intervals.** HLR is public and open-source; a half-life/FSRS-style scheduler is the highest-leverage, lowest-risk engine to copy.
4. **Design re-entry for resumption, not recap.** On reopening after an interruption, show the **next action** (assistant cue), not a summary of what happened (retrieval cue). Add a brief "collect your thoughts" beat before the first interaction.
5. **Use illusionary progress deliberately** (a pre-granted head start on a progress bar) — Kivetz's bonus-stamp result is directly transferable.
6. **Reward with competence information, not contingent payouts.** Prefer "you're now at 80% on this concept" over "you earned 50 XP." Gamification's *motivational* effect is the fragile one; don't build retention on it.
7. **Default to no-audio, one-hand, offline-capable, tap-first.** Justified by load theory and the commute context, not by transit-specific studies.
8. **Instrument for transfer, not just engagement.** Log not only completion but *delayed* production/application items — because that is the only outcome that can falsify the product thesis.

**What would falsify the load-bearing claims (the brief asks for this)**

- *"Micro-sessions build complex skill"* → a **delayed posttest of transfer/production with an active control** showing no advantage, or an advantage that disappears at 1 month. This is the single most important experiment Study OS could run; nothing in this literature has run it.
- *"Notifications build durable habit"* → measure consistency **after** removing reminders. Aliakbari's collapse suggests the habit may be the notification's, not the learner's.
- *"Streaks drive learning"* → the 3.6× figure is *correlational*; a streak A/B with a learning-outcome (not retention) endpoint would test it. If streak-driven learners complete more but test no better, streaks are engagement theatre.
- *"Gamification helps"* → a design where the motivational effect is measured under strict controls; Sailer & Homner predict it may vanish.

**Biggest open question:** *Does retrieval-based micro-session learning on mobile produce **transfer**, or only retention of isolated items?* Every strong effect in this digest — spacing (g≈0.78), retrieval practice, HLR — is measured on recognition/recall of discrete items, and Donovan & Radosevich show the spacing benefit shrinking to d≈0.07–0.42 as tasks get complex. A builder should treat "tap-based micro-interactions can teach complex things" as **the hypothesis their product exists to test**, not as a premise.

**Sources** (primary-first): [Duolingo TSLW](https://blog.duolingo.com/time-spent-learning-well/) · [Duolingo efficacy](https://blog.duolingo.com/results-duolingo-efficacy-studies/) · [Duolingo streaks](https://blog.duolingo.com/how-duolingo-streak-builds-habit/) · [Settles & Meeder ACL 2016](https://aclanthology.org/P16-1174/) · [Yancey & Settles KDD 2020](https://dl.acm.org/doi/10.1145/3394486.3403351) · [Jiang et al. A2 whitepaper](http://duolingo-papers.s3.amazonaws.com/reports/Duolingo_whitepaper_language_read_listen_2020.pdf) · [Krashen critique](http://sdkrashen.com/content/articles/krashen-does-duolingo-trump.pdf) · [Sailer & Homner 2020](https://link.springer.com/article/10.1007/s10648-019-09498-w) · [Deci, Koestner & Ryan 1999](https://pubmed.ncbi.nlm.nih.gov/10589297/) · [Altmann & Trafton 2004](https://www.interruptions.net/literature/Altmann-CogSci04.pdf) · [Chen et al. 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC12271330/) · [Kivetz et al. 2006](https://home.uchicago.edu/ourminsky/Goal-Gradient_Illusionary_Goal_Progress.pdf) · [Matuschak, mnemonic medium](https://notes.andymatuschak.org/Effects_of_the_mnemonic_medium_on_reader_memory) · [Matuschak, Why Books Don't Work](https://andymatuschak.org/books/) · [Deng 2026, WM review](https://www.frontiersin.org/journals/psychology/articles/10.3389/fpsyg.2026.1758104/full) · [Jainuri et al. 2025 microlearning](https://publikasi.teknokrat.ac.id/index.php/jurnalmathema/article/view/517) · [Aliakbari et al. 2026 RCT](https://doi.org/10.61838/kman.aitech.4724) · [Agarwal et al. 2021 retrieval practice](https://link.springer.com/article/10.1007/s10648-021-09595-9) · [Cepeda et al. 2006](https://doi.org/10.1037/0033-2909.132.3.354) · [Dunlosky et al. 2013](https://doi.org/10.1177/1529100612453266)
