# Calibrating LLM Output for Pedagogy

*Research digest for Study OS. Domain: generating explanations, questions, and items at a **target difficulty** and **verified quality** — and knowing when the generation is actually calibrated. Written for a builder; evidence graded **[strong]** (replicated / meta-analyzed / multiple independent confirmations), **[promising]** (real but thin, few replications, or single-lab), **[thin/marketing]**.*

---

## 0. The framing to carry throughout

"Calibrating an LLM for pedagogy" decomposes into three *separate* engineering problems that are constantly conflated:

1. **Generation** — make the model produce an item/explanation *at* a target difficulty.
2. **Prediction** — make the model *estimate* the difficulty of existing text.
3. **Verification** — make the model *check* that a generated item is answerable, unambiguous, and at the claimed difficulty.

The single most important framing result: **an LLM's ability to *solve* a problem is not the same as its ability to *teach* or *calibrate* it.** MathTutorBench ([arXiv:2502.18940](https://arxiv.org/abs/2502.18940), EMNLP 2025) shows a pedagogy-vs-expertise trade-off *on its benchmark* — strongest solvers are not strongest tutors, and longer dialogues get harder for every model. So benchmark accuracy does not transfer to "good explanation" or "right difficulty"; you need a separate measurement loop.

The second result: **LLMs rate difficulty only moderately well, and there is evidence they conflate two kinds of "hard."** "The Easy Trap" ([arXiv:2607.26067](https://arxiv.org/abs/2607.26067), EDM 2026) compared LLM difficulty ratings against 770 elementary-math students and found Spearman ρ ≈ 0.52–0.70 — read that honestly: the *upper* end is respectable, the problem is the spread and the failure mode. LLMs confuse *curricular* difficulty (topic is advanced) with *misconception-driven* difficulty (topic is basic but the item trips a misconception): "100 : 1/2" is only 34% correct among students, yet LLMs rate it easy. **Caveat: one study, one subject, one population — treat "LLMs systematically conflate the two" as a hypothesis to test on your own items, not a law.** It is nonetheless the load-bearing caution for Study OS: an LLM's difficulty estimate leans curricular, and the case where it fails is exactly representation/misconception friction.

---

## 1. Automatic Item Generation (AIG) with LLMs

**[strong] The generation part works. The difficulty-control part works partly.** LLMs generate plausible multiple-choice and open items across domains cheaply; that is no longer in question. What is in question is whether the *difficulty knob* is real or cosmetic.

**Controlled-difficulty generation — the verified line of work:**
- **Cloze/distractor difficulty control [promising].** Zhang, Xie & Qiu train PLM *surrogate* models that predict IRT parameters for cloze items and use them to steer gap and distractor difficulty ([arXiv:2403.01456](https://arxiv.org/abs/2403.01456)). Kang et al. do the same for distractors via data augmentation + multitask learning ([arXiv:2511.01526](https://arxiv.org/abs/2511.01526)). Both report that naive generation "lacks control over difficulty levels" and that a difficulty-aware objective fixes part of it. The surrogate-model approach is the pattern worth stealing: **generate → score with a difficulty model → regenerate if off-target.**
- **Multi-agent generation beats single-pass [promising].** ReQUESTA (Tian et al., McNamara group, [arXiv:2602.03704](https://arxiv.org/abs/2602.03704)) uses a hybrid multi-agent pipeline to produce MCQs targeting text-based vs. inferential vs. main-idea comprehension, with expert raters judging distractor quality — multi-agent output was preferred to single-pass GPT-class generation on difficulty and discrimination. Concept-map–grounded MCQ generation hit **75.2% expert approval vs ~37% for a baseline** ([arXiv:2505.02850](https://arxiv.org/abs/2505.02850), ECAI 2025) — grounding in an explicit concept structure, not a bigger model, moved the number. KNIGHT ([arXiv:2602.20135](https://arxiv.org/abs/2602.20135)) does the same over a knowledge graph but validates only *fluency, unambiguity, answerability* — it does not claim the difficulty target is hit psychometrically.

**The honest caveat on AIG:** almost every "controlled difficulty" paper validates with *either* expert ratings *or* a model-based difficulty proxy, rarely both, and almost never with a **pretest on real learners**. "Difficulty-controllable" in the literature usually means "controllable with respect to a model of difficulty," not measured human difficulty. Wang et al. ([arXiv:2607.28634](https://arxiv.org/abs/2607.28634)) make this explicit: GPT-4.1 zero-shot difficulty classification reaches QWK ≈ 0.578, *below* a fine-tuned ConvBERT baseline (≈ 0.625); LLMs underestimate hard items, and their embeddings **mix difficulty levels** rather than separating them.

**Human vs. LLM rating agreement [promising].** Razavi & Powers ([arXiv:2504.08804](https://arxiv.org/abs/2504.08804), IJAIED) find direct LLM ratings moderate-to-strong, and **feature-extraction + tree ensembles reach r ≈ 0.87** — a cheap supervised model on embeddings can beat a prompted LLM. Kolesnikova et al. ([arXiv:2605.18562](https://arxiv.org/abs/2605.18562)) find **pairwise comparisons beat absolute ratings**. BEA 2025 (Nebhi et al., EF Education First) reports few-shot GPT-4o writing-item generation correlating **0.9 with experts** and a **majority-vote LLM ensemble at Kappa ≈ 0.89** — but judges agreeing with each other is a *consistency* claim, not validity (see §2).

---

## 2. Calibration and verification — knowing the item is actually good

**[strong] LLM-as-judge is usable but biased, and its biases are documented.** Two surveys ([arXiv:2411.15594](https://arxiv.org/abs/2411.15594), [arXiv:2411.16594](https://arxiv.org/abs/2411.16594)) catalog position bias, verbosity bias, self-preference (models favor their own outputs), and sensitivity to prompt format. The Judge's Verdict ([arXiv:2510.09738](https://arxiv.org/abs/2510.09738)) is the sharpest result: across 54 judges, the ones that look most "human-like" are *less* self-consistent, and the hyper-consistent judges are consistent because they've collapsed onto a degenerate rule. **A judge with high inter-run agreement is not necessarily a good judge.**

**Item-Writing Flaws (IWF) [strong].** The classic 19-criteria IWF rubric was operationalized at scale: 7,126 MCQs from 448k students ([arXiv:2503.10533](https://arxiv.org/abs/2503.10533)). Flaw frequency correlates **negatively with item discrimination (r ≈ −0.17 to −0.28)** — items with flaws like "all of the above," "longest option is correct," or grammatical-cue mismatches tend to be low-discrimination, low-difficulty items. IWF features have *moderate* predictive power and are best used as a **screening filter for obviously-broken items**, not a difficulty predictor (SAQUET automates IWF detection). **Run a cheap rule-based + small-model IWF pass before any expensive difficulty modeling.**

**Automated item evaluation at scale [promising].** A 2026 study ([arXiv:2608.06609](https://arxiv.org/abs/2608.06609)) fuses a fine-tuned **DeBERTaV3-large** with **Qwen3 critique** over 52,759 ELA/math items, reaching Accuracy 0.75 / F1 0.64 / AUC 0.80 overall (math F1 0.73 / AUC 0.86; ELA F1 0.51 / AUC 0.72). Note the spread by subject and dimension: "is this item good?" is several different questions with very different achievable accuracies, and math is far easier to judge than ELA.

**Simulated students [promising].** SMART ([arXiv:2507.05129](https://arxiv.org/abs/2507.05129), EMNLP 2025) DPO-aligns a simulated student so its LLM-scored responses produce IRT curves that fit real data — the closest thing to an automated pretest. "Take Out Your Calculators" ([arXiv:2601.09953](https://arxiv.org/abs/2601.09953)) simulates whole classrooms and reports **NAEP correlations of 0.75 / 0.76 / 0.82 (grades 4/8/12)**; a *weaker* math model (Gemma) predicted *better*, suggesting the simulation captures item structure rather than raw model skill. **Read the number carefully: those are correlations against aggregate test-level performance, not per-item difficulty recovery** — which is what an item bank actually needs. Single-lab, and unproven at per-item resolution.

**Two dimensions the AIG literature under-measures [promising, thin].**
- **Differential item functioning (DIF) / fairness.** The automated-evaluation study above is weakest precisely on bias/fairness (ELA F1 0.51), and a "good" item can still be unfair across subgroups. Fairness is not captured by difficulty or IWF. If Study OS ever compares learners, per-item fairness screening (DIF across prior-knowledge or language subgroups) is a real gap — and one of the least-solved in this domain.
- **Distractor quality.** For MCQs, the *wrong answers* are the pedagogy: a distractor that is implausible (never chosen) or that is actually also correct (ambiguous) silently breaks the item. ReQUESTA and the distractor-control papers ([arXiv:2511.01526](https://arxiv.org/abs/2511.01526)) treat distractor quality as a first-class target; a difficulty-only metric does not.

**Adversarial robustness of the verifier [thin].** The verification loop assumes the judged content is honest input. It is not, once learners or scraped content can reach it: a prompt-injection inside a generated explanation, or a learner gaming an LLM-judged metric, can flip a "verified" item. Assume the judge is attackable and hold out human labels as the backstop.

**Falsifier for this section:** a head-to-head showing a prompted frontier LLM judge matches a fine-tuned small model + IWF filter + simulated-student pretest ensemble on held-out human-pretested items. Nobody has shown that; the small-model-plus-checks pipeline currently wins on cost and often on accuracy.

---

## 3. Predicting difficulty from text alone

**[strong] Text-only difficulty prediction is real but bounded, and the ceiling is set by "difficulty is a triangle."** (See digest 03 and 05.) Text features predict *average* difficulty; they cannot predict difficulty *for a specific learner*, because the missing term is prior knowledge. What works:

- **Surface readability (Flesch-Kincaid etc.) [strong but weak]** — correlates with comprehension in aggregate, standard error ~1.8–2.3 grade levels, ignores semantics. Fine as a first-pass feature, useless as a target.
- **Embedding + supervised regression [promising]** — the r ≈ 0.87 result above (Razavi & Powers) and the difficulty-classification QWK ≈ 0.58–0.63 range (Wang et al.) bracket the practical accuracy: **good enough to rank items, not good enough to hit a target level precisely.**
- **LLM zero-shot/prompted [promising]** — moderate (ρ ≈ 0.5–0.7) and biased toward curricular difficulty (Easy Trap, above). Use it as *one* signal, and calibrate it against your own pretested items rather than trusting the prompt.
- **Item statistics as ground truth [strong]** — where you have real response data, IRT difficulty (b) and discrimination (a) are the only *valid* targets. NAEP releases item-level statistics publicly; that is the cleanest large-scale ground truth available (see digest 03).

**The falsifiable claim to watch:** if someone shows a text-only model predicting *learner-relative* difficulty (i.e. per-student correctness) at useful accuracy from item text + a short knowledge profile, that would break the "difficulty is a triangle" consensus. As of 2026, no one has.

---

## 4. LLM tutoring quality — and the "tutoring trap"

**[strong, with a large RCT] The answer-leakage problem is real and it *harms learning*.** Bastani et al. (SSRN 4895486; *PNAS* 2025, [DOI 10.1073/pnas.2422633122](https://doi.org/10.1073/pnas.2422633122)): ~1,000 Turkish high-school math students. A GPT-4 base assistant raised practice performance **+48%**; a guarded "GPT Tutor" **+127%** — but when access was removed, **GPT Base students scored ~17% below** students who never had access, and the guarded tutor **largely eliminated the harm.** The mechanism is the representation-friction thesis inverted: the base model hands over the answer, practice looks great, nothing is learned. **Caveats:** one country, one subject, one short-horizon post-test; students in the unguarded arm may have practiced less independently. Replication outside Turkey and in a second subject would strengthen it — but it is the closest thing to causal evidence in the field. (A PNAS correction, [DOI 10.1073/pnas.2518204122](https://doi.org/10.1073/pnas.2518204122), concerns an affiliation, not the numbers.)

**[thin — practitioner report, not peer-validated] Guardrails measurably reduce leakage.** Khan Academy's AIED 2026 paper ([arXiv:2608.11259](https://arxiv.org/abs/2608.11259)) is the most concrete engineering account — but grade it correctly: an **internal, industry-authored report with no external validation and no control group, whose outcome metrics (answer-giving F1 0.77, cognitive engagement F1 0.83, math error F1 0.61) are themselves LLM-judged proxies**, not learning measures; the "next-item correctness +10%" is a proxy, not a delayed posttest. Read it as a *playbook*, not evidence. The levers are nonetheless concrete. Across **40+ experiments in 5 months**: concise prompting cut latency **−32.7%**; limiting Math Agent guidance cut answer-giving **−85.5%** but *also* cut cognitive engagement **−18.09%** (the seesaw); anchoring to a mastery level (AFPM) cut answer-giving **−55.16%**; swapping GPT-4o → GPT-4.1 cut answer-giving **−96.66%**. **Key negative finding: students adapt to the guardrail and the answer-giving metric "seesaws" — you cannot fix leakage once and forget it.**

**[promising] What keeps the model from spoiling answers:**
- **Verification-style feedback beats direct answers** — [arXiv:2607.09919](https://arxiv.org/abs/2607.09919) finds verification feedback produced **82.4% productive continuations vs 62.7%** for direct-answer feedback.
- **Socratic designs are resisted under time pressure** — [arXiv:2607.01692](https://arxiv.org/abs/2607.01692) documents students repurposing answer-first shortcuts; pure Socratic dialogue is not a free win.
- **Adversarial students can defeat naive guardrails [promising]** — [arXiv:2604.18660](https://arxiv.org/abs/2604.18660) (ACL 2026) shows in-context adversarial students mostly *fail* to extract answers, but a **fine-tuned adversarial student agent** succeeds; simple defenses reduce leakage. This is a red-team recipe: assume a motivated learner will try to jailbreak the tutor.
- **Adaptive scaffolding is where LLMs currently fail** — GuideEval ([arXiv:2508.06583](https://arxiv.org/abs/2508.06583)) finds models weak at perception / orchestration / elicitation: they don't reliably notice what the student needs next. And **real-world tutor-assist works [strong]** — the Tutor CoPilot RCT ([arXiv:2410.03017](https://arxiv.org/abs/2410.03017)): 900 tutors, ~1,800 students, **+4pp topic mastery (+9pp for lower-rated tutors)** at **~$20/tutor/year**, with tutors more likely to use guiding questions. Note the design: the LLM assists the *human* tutor, sidestepping the leakage trap.

**The warning about optimizing the wrong metric [promising]:** "The learner who does not learn" ([arXiv:2610.12125](https://arxiv.org/abs/2610.12125)) is the cautionary tale — optimizing a pedagogical metric moved it from +0.05 to +0.42 while **expert human ratings dropped from 4.46 to 3.03**, because the metric was maximized by repeating a single best decision. Any internal quality metric you build for Study OS will be gameable; hold out human or learner-rated validation.

---

## 5. Explanation generation at controlled difficulty (simplification)

**[promising] Controllable text simplification works — with a real oversimplification tax.** The line of work:

- **ReadCtrl** ([arXiv:2406.09205](https://arxiv.org/abs/2406.09205)) controls output readability better than GPT-4/Claude-3 via a learned control token; **MedReadCtrl** ([arXiv:2507.07419](https://arxiv.org/abs/2507.07419)) extends it to medical text. **Re-RIGHT** ([arXiv:2604.05302](https://arxiv.org/abs/2604.05302), ACL 2026) and **Taming CATS** ([arXiv:2604.01779](https://arxiv.org/abs/2604.01779)) find **readability control is learnable but compression/length control underperforms** — you can hit a reading level, but not reliably a length at the same time.
- **Zero-shot prompting for target levels is weak [thin].** [arXiv:2409.20246](https://arxiv.org/abs/2409.20246) found zero-shot readability-controlled summarization struggled, *especially at the lowest (most simplified) levels* — exactly the levels a struggling learner needs.
- **The oversimplification tax is documented [promising].** [arXiv:2309.12551](https://arxiv.org/abs/2309.12551) found the magnitude of the level shift correlates with the source text and that **larger shifts cause more meaning loss.** "Simplifications are Absolutists" ([arXiv:2507.11981](https://arxiv.org/abs/2507.11981), RANLP 2025) shows simplifiers **neglect polysemy** and **degrade definition completeness.** This is the empirical version of the representation-friction thesis, and it is a *warning*: renaming `seen` → `box` may clarify or may destroy the conceptual mapping, and the model cannot tell you which.

**What this means concretely:** "explain like I'm 5" is controllable in *surface* difficulty and unreliable in *conceptual fidelity*. If Study OS controls difficulty by simplification, it must separately verify that the concept survived — a meaning-preservation check (NLI/entailment or human spot-check), not just a readability score.

---

## 6. Cost and latency realities

**[strong] The economics are favorable if you batch and cache.** Concrete numbers (Claude API, 2026 pricing):
- **Model tiers:** Haiku-class **$1/$5** per MTok (in/out), Sonnet-class **$2/$10**, Opus-class **$10/$50**. Generation of items/explanations belongs on Sonnet-class; *judging/scoring* on Haiku-class; only hard reasoning needs Opus-class.
- **Batch API = 50% off** the synchronous rate (24h window, up to 50,000 requests / 200 MB per batch, separate rate-limit pool). **Any offline item-bank generation should be batched**; results are keyed by `custom_id` and arrive in any order.
- **Prompt caching** cuts repeated-prefix input cost sharply. Keep the stable system prompt / rubric first, volatile per-item content last, or the cache silently invalidates.
- **LLM-as-judge is where cost hides.** A multi-judge ensemble multiplies cost by the number of judges; the cheaper and often better route is **one fine-tuned small model** (DeBERTa-class) for rubric-gradable dimensions plus a prompted judge only for the rest. **Majority-vote ensembles buy consistency, not validity.**
- **Latency:** prompt length is a first-order latency lever (Khan Academy: concise prompting → −32.7%); stream generation for anything a learner waits on.

---

## So what for Study OS

1. **Build the three loops separately.** Generation (cheap, batched, Sonnet-class) → prediction (a small fine-tuned model on embeddings, plus a prompted LLM as a second opinion) → verification (rule-based IWF filter + simulated-student pretest). Do not collapse them into one prompt; the evidence says each has different failure modes and different achievable accuracy.
2. **Treat LLM difficulty as curricular, not learner-relative.** The Easy Trap is the sharpest finding for this project: the model will call "100 : 1/2" easy because the *topic* is easy, while 66% of students fail it on a misconception. Study OS's whole thesis is representation friction — and this is a documented case where the LLM's difficulty sense is blind to exactly that kind of friction. **Calibrate any difficulty estimate against pretested items, never against the model's own opinion.**
3. **Verification is the entry ticket; the closed loop is the moat.** Generation is commoditized, and verification (IWF screening, answerability/unambiguity checks, fairness/DIF, simulated-student pretest) is now table stakes — necessary, but something anyone can copy. What is *not* copyable is a **closed loop**: your own items with real learner response data feeding back into a calibrated difficulty model. The simulated-student results (test-level ρ 0.75–0.82, *not* per-item) say you can approximate this before you have learners, but only approximately. **Data to get:** NAEP item-level statistics (public) as pretest ground truth; the IWF rubric as a free screening layer; your own learner response logs as the only *valid* difficulty labels. Everything else is a proxy.
4. **Expect the leakage seesaw.** If Study OS ever tutors directly, assume the tutoring trap and the Khan Academy result: guardrails cut answer-giving but also cut engagement, and learners adapt. Budget for continuous re-tuning, not a fixed prompt. The Tutor CoPilot design (LLM assists, human delivers) is the evidence-backed way to dodge the trap if humans are in the loop.
5. **Simplification needs a meaning check.** If difficulty is controlled by rewriting, a readability score is not enough — add an entailment/fidelity check, because the simplification literature shows systematic polysemy and definition loss.
6. **Falsifiers to hold yourself to:** (a) if a prompted LLM judge matches a fine-tuned small model + IWF filter on your items, drop the fine-tuned model; (b) if text-only difficulty prediction ever predicts *per-learner* correctness, the "difficulty is a triangle" premise breaks; (c) if your internal quality metric improves while human ratings fall (the [arXiv:2610.12125](https://arxiv.org/abs/2610.12125) trap), the metric is wrong.
