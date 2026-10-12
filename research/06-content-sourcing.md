# 06 — Content sourcing: decomposition logic, item composition, and the reusable skeleton

**Scope note.** This digest is about *structure*, not text. The question is not "which textbook can I copy" but "what does the pedagogical decomposition of real courses look like, and how much of it is a transferable pattern I can re-implement over my own generated content." License facts appear only where they constrain what is technically obtainable and in what format. Method: primary sources fetched directly (PDFs read and text-extracted, license pages quoted), not search snippets. Claims that are background knowledge rather than fetched-and-verified are marked **[unverified]**.

---

## 0. TL;DR

1. **The reusable asset is the skeleton, not the prose.** Standards documents (AP CEDs, CCSS, certification blueprints) publish an explicit, machine-parseable decomposition — units → topics → testable statements → skills — with exam weightings and prerequisite edges. That is exactly the artifact a generator needs, and it is *free of content*. Generate the content, reuse the structure.
2. **Step size is measurable and consistent.** In the AP CS A CED (read directly from the PDF): 4 units → 53 topics → **223 Essential Knowledge statements** (1–10 per topic, mode 3), each topic tagged to 1–2 of 12 skills. That is roughly one testable claim per ~30 minutes of instruction. This is the granularity that the "interaction-based, subway-sized" learner hypothesis actually needs.
3. **Item composition is formulaic.** Exams publish item counts, point allocations, task verbs, and weights per unit and per skill; prep products publish section blueprints. Item banks that tag *misconceptions* (Eedi) rather than just correctness are the closest public model for a distractor-first design.
4. **Outcome correlation data is thin and mostly marketing.** The one solid, replicated-ish number is MOOC certification rate (~3–6%). Pass-rate advertising from certification prep is not methodologically transparent. Treat all "our pass rate is X%" claims as unverified.
5. **What breaks in auto-generation is well documented but under-quantified.** Published LLM item-generation results are encouraging (91.56% rated relevant/high-quality in one BEA 2024 study) but human distractors are still preferred, and the failure modes are the ones you'd predict: prerequisite hallucination, granularity drift, coverage gaps, near-duplicates.

---

## 1. Decomposition logic — how established courses break a subject down

### 1.1 AP Course and Exam Descriptions (CEDs) — the most explicit public decomposition

I downloaded and parsed the **AP Computer Science A CED** (2025, 194 pages) directly. Its framework is:

- **4 units**, each with an exam weighting: Using Objects and Methods 15–25%; Selection and Iteration 25–35%; Class Creation 10–18%; Data Collections 30–40%.
- **53 topics** (`1.1` … `4.17`), each with a suggested pacing of **1–4 class periods** (45 min).
- **223 Essential Knowledge (EK) statements** with stable IDs (`1.2.B.1`), i.e. *one testable claim per node*. Distribution of EKs per topic: mode 3, mean ≈ 4.2, range 1–10.
- **5 Computational Thinking Practices → 12 skills** (`1.A`, `2.A`, `3.D`…). Each topic lists **1–2 "suggested skills"** — the topic × skill cross-product is the real item space.
- Explicit **exclusion statements** ("the other five primitive data types … are outside the scope of this course") — scope control encoded as data.
- **Progress Checks** per unit in 2–3 parts, each with ~12–21 multiple-choice items plus 0–2 free-response items — i.e. a *formative checkpoint every 5–7 topics*.

Crucially, the CED says its own sequence was "identified through a thorough analysis of the syllabi of highly effective AP teachers and the organization of typical college textbooks." **A CED is already an aggregation of many courses' decompositions** — you get the consensus ordering for free.

Why this matters: the CED is a ready-made skill graph with (a) node granularity, (b) skill tags, (c) exam weights as a proxy for emphasis, and (d) explicit prerequisites in the prose. It is *structure without content*.

### 1.2 CCSS math — a machine-readable prerequisite graph

Common Core math is a strict hierarchy: **conceptual category → domain → cluster → standard**, with canonical IDs (`CCSS.MATH.CONTENT.8.EE.A.1` = grade 8, Expressions & Equations, cluster A, standard 1). High school uses conceptual categories (Number & Quantity, Algebra, Functions, Modeling, Geometry, Statistics & Probability) instead of grades. The standards are published as structured text/JSON by several mirrors.

The **Coherence Map** (Student Achievement Partners) layers prerequisite edges on top — you click a standard and see what it builds on and what it leads to. It is a *browsable* graph, not an obviously downloadable one; I could not confirm a public bulk export, so treat it as "scrapeable HTML, not an API." **[unverified]** on exact node/edge counts.

### 1.3 OpenStax — the textbook decomposition pattern

A section of an OpenStax book (verified on *College Algebra 2e*) has a fixed recurring shape:

- **Learning Objectives** (bulleted, at the top),
- instructional prose with repeating **Example → Solution → Try It** blocks and boxed **How To** procedures,
- and a closing **Section Exercises** block grouped by *type*: **Verbal, Numeric, Algebraic, Real-World Applications, Technology, Extensions**.

This is the classic teach→practice→extend ladder, and the grouping of exercises by type is itself a difficulty ladder (Verbal = recall; Algebraic = procedure; Extensions = transfer). Chapter-level: chapter opener → sections → chapter review → key terms → review exercises → practice test.

### 1.4 Khan Academy — mastery-gated skill decomposition

Khan's math courses run **course → unit → lesson → exercise**, with mastery tracked per skill across levels (**Attempted → Familiar → Proficient → Mastered**), plus quizzes, unit tests, and a course challenge. The composition rule is *mastery-gated progression*: you unlock later material only by demonstrating mastery of prerequisites. **[unverified on exact correct-answer/streak counts per level** — the support pages 403'd.] This is the pattern to copy if you want a mastery model without a live tutor.

### 1.5 Duolingo — path + spaced repetition

The current home screen is a **linear path of levels** grouped into **units** with descriptive headers, with levels from *different* skills interleaved so that review is interleaved rather than massed. Spaced repetition is the stated ordering principle ("space out practice … rather than cram"), with intervals widening over time. The engine is **half-life regression** (Settles & Meeder 2016), which predicts recall probability per word/concept and drives what to review next.

### 1.6 Brilliant — problem-first composition

From Brilliant's own description: "**Each lesson focuses on a single concept**" and blends direct instruction with blocked problem solving; they deliberately **pretest** ("letting the learner try to find a solution before learning the procedure"), then give **practice sets with scaffolding removed** ("no more visual aids or hints"), and **review sets that combine problems from preceding lessons**. Their stated personalization goal is to "predict the optimal next problem X to ask" so the learner is ready for a later problem Y. This is the most explicit public statement of an *item-ladder-by-design* philosophy.

### 1.7 Coursera specializations & Codecademy — the commercial coarse structure

A Coursera Professional Certificate (verified on Google IT Automation) is **7 courses, ~134 hours, "6 months at 10 hours a week."** Per-course internals (modules, item counts) are *not* public — the page exposes outcomes and hour counts only. Codecademy exposes lesson/project/quiz/cheatsheet as its atomic units and labels projects "**More guidance, 48 min**" vs "**Independent**" — a two-rung scaffolding ladder. Takeaway: commercial platforms keep the *outer* structure public (course count, hours, credential) and hide the *inner* structure (module/item composition). The valuable decomposition is in standards docs and open textbooks, not in commercial catalogs.

---

## 2. Question/item composition

**Item types and counts (AP CS A, verified):** Section I = **42 multiple-choice questions, 55% of score, 90 minutes**; Section II = **4 free-response questions, 45%**, with per-question point values (7/7/5/6). The MC section mixes **discrete and set-based** items. Every FRQ is aligned to a named practice and its sub-skills (all four assess Practice 2 "Develop Code"; 2.A/2.B/2.C are distributed across them). Scoring guidelines and released FRQs are public — a ready-made, human-scored item bank with rubrics.

**How many items per concept:** with 42 MC items spanning 223 EKs, the exam gives roughly **0.2 items per EK**, but weighting is per unit (15–25/25–35/10–18/30–40%) and per skill (Practice 3 "Analyze Code" is 37–53% of MC). So emphasis is set at the *unit and skill* level, not the EK level. For a product, the practical read is: **~1–3 items per EK node, weighted by how much of the exam that node's unit/skill carries.**

**Distractor design.** The Eedi "Mining Misconceptions in Mathematics" competition (Kaggle, 2024) is the cleanest public model: multiple-choice distractors are **explicitly tied to named misconceptions**, so a wrong answer is diagnostic rather than just wrong. BEA 2024 research (Scarlatos et al.) shows LLM-generated distractors can be *ranked* to align with human-authored ones, but **"human-authored ones are still preferred over generated ones."** Practical rule: generate the stem and the correct answer with the LLM; source or template the distractors from a misconception list.

**Difficulty ladders.** LeetCode uses three tiers (**Easy/Medium/Hard**) crossed with a topic taxonomy (arrays, two pointers, sliding window, trees, graphs, dynamic programming…). NeetCode's 150 is a *curated ladder* — a hand-ordered subset across those topics. For DSA lanes specifically, this taxonomy + tiering is the de-facto public blueprint and is directly reusable as a node/skill graph.

---

## 3. What correlates with pass rates / completion

- **MOOCs — the one solid number.** Completion/certification sits around **~3–6%** of registrants; Reich & Ruipérez-Valiente's *Science* "The MOOC pivot" (2019) and Ho et al.'s HarvardX/MITx first-year report (2014, ~4%) are the canonical sources. **[numbers are background knowledge, not fetched — Science/SSRN blocked automated fetch.]** The structural correlates that recur: shorter courses, clear intent, and assessment design matter more than production values. Caveat: "completion" definitions vary wildly across studies, so treat cross-study comparisons cautiously.
- **Duolingo HLR — the strongest published outcome result in this space.** Settles & Meeder (2016, verified from the PDF): HLR "reduc[ed] error by **45%+** compared to several baselines at predicting student recall rates," and "improved Duolingo daily student engagement by **12%** in an operational user study." This is one of the few cases where a *model of item difficulty/forgetting* was validated against both prediction error and a live engagement metric.
- **Tutoring effect sizes.** VanLehn (2011) is the standard reference for "step-based ITS ≈ human tutoring, both well above answer-based systems." **[unverified — paywalled.]** Bloom's "2 sigma" is widely cited and widely contested; don't build on it.
- **Prep-product pass rates (HESI A2, NCLEX, LEED).** Advertised, not audited. There is no public methodology tying a given question bank's structure to a pass rate. **Treat as marketing.**

**Verdict on this section:** the outcome-correlation evidence is *weak except for two things* — the MOOC completion base rate (low) and the Duolingo HLR validation (strong for its narrow claim). Everything else is either paywalled, contested, or marketing.

---

## 4. The reusable skeleton (pattern vs. content)

| Transferable pattern | Subject-specific content (generate it) |
|---|---|
| Node granularity: **one testable claim per node** (EK-level) | The actual claim text |
| Topic = **3–5 EK nodes**; unit = **~10–15 topics** | Topic names |
| **1–2 skills** attached per topic; skill × topic = item space | The skill taxonomy itself (reuse AP's 12, or define your own) |
| **Weighting**: assign item emphasis by unit/skill share | The weights |
| **Prerequisite edges** as a DAG; mastery-gate progression | Which node precedes which |
| Item ladder: **pretest → worked example → guided practice → independent** | The items |
| **Scaffolding removal** between guided and independent rungs | What "scaffold" means per subject |
| **Misconception-tagged distractors** | The misconception list |
| **Interleaved review** with widening intervals | Scheduling parameters |
| **Coverage bookkeeping**: LO/EK/SK codes as a checklist to detect gaps | — |

Everything in the left column is reusable across subjects; everything in the right column is content you generate. This is the answer to "do you need the source text at all": **for the skeleton, no — the standards docs give you the graph, and you generate the leaves.**

---

## 5. Sources & formats (technical constraints only)

| Source | Format | Machine-readable | Auth / limits |
|---|---|---|---|
| OpenStax | Web HTML, PDF, **CNXML source on GitHub** (`openstax/osbooks-*`), public read-only CNX archive API | Yes (GitHub + API) | Instructor test banks gated; note books are **NC-licensed** (help center), though some GitHub `LICENSE` files still say CC-BY — a stale-metadata trap |
| MIT OCW | HTML + PDF | Scrapeable | NC; AI-training explicitly restricted |
| Khan Academy | Web HTML, YouTube | Videos NC; **exercises not openly licensed** | No public bulk export |
| Saylor Academy | Web HTML | Scrapeable | CC BY 3.0 (commercial OK) |
| Wikibooks | HTML (MediaWiki API) | Yes — MediaWiki API | CC BY-SA |
| Open Textbook Library | PDF/EPUB links | Index yes, files per-publisher | 1,887 titles, mostly CC BY |
| CCSS / Coherence Map | HTML; IDs canonical | Text parseable; **no confirmed bulk graph export** | — |
| AP CEDs | **PDF** (text-extractable — I parsed it) | Yes, with effort | No API |
| Datasets | ASSISTments (skill tags), KDD Cup 2010 (DataShop), EdNet (Riiid), Eedi (Kaggle 2024) | Yes | IRB/terms-of-use on ASSISTments |

The one format fact that changes strategy: **AP CEDs are text-extractable PDFs** (I pulled 53 topics and 223 EK IDs programmatically), so a standards-driven skill graph can be built today with a PDF parser plus light regex — no scraping infrastructure required.

---

## 6. Known failure modes of auto-generation

- **Quality is good but not parity.** Scaria et al. (BEA 2024): five models generated 510 social-science items graded on a 9-item rubric; **91.56% were rated relevant and high quality** — but ~8% were not, and there was no head-to-head win against teacher-written items.
- **Distractors lag.** Scarlatos et al. (BEA 2024): ranking improves generated distractors' alignment, but human-authored ones remain preferred.
- **Prerequisite hallucination** — the LLM invents plausible-but-wrong orderings; this is precisely why the *graph* must come from a standards doc, not from generation.
- **Granularity drift** — generated nodes are unevenly sized (some trivial, some multi-concept) unless clamped to an EK-style unit.
- **Coverage gaps / near-duplicates** — without an explicit node checklist, generation silently skips or repeats concepts. The LO/EK/SK codes exist to prevent exactly this.

---

## 7. So what for Study OS

Ranked sourcing strategies for a solo builder, on effort / quality / technical constraints:

1. **Syllabus-first + full generation (recommended default).** Take the skill graph from a standards doc — AP CS A CED for DSA/CS, CCSS for math, a certification blueprint for HESI-style lanes — and generate all prose and items with an LLM. *Effort:* low (a PDF parser + a schema). *Quality:* controllable, because node granularity, skill tags, and weights are fixed by the source. *Constraint:* none — you ship your own content. This directly implements the brief's "structure, not text" insight.
2. **OpenStax as spine + generated interactions.** Use a book's chapter/section/Learning-Objective structure as the graph and its Example/Solution/Try-It ladder as the template; generate your own items. *Effort:* medium. *Quality:* high pedagogical fit. *Constraint:* books are NC-licensed, so use them as a *structural reference* rather than shipping their text.
3. **Standards + a public misconception/item bank.** For lanes where a tagged bank exists (Eedi misconceptions, released AP FRQs with scoring guidelines, LeetCode/NeetCode taxonomy for DSA), pair your graph with real items and real distractors. *Effort:* medium. *Quality:* highest per item, because distractors are diagnostic. *Constraint:* per-source terms; released AP FRQs are the cleanest.
4. **Full scrape of a commercial catalog (Coursera/Codecademy).** *Not viable as a spine* — the inner decomposition is hidden, and the outer structure (course count, hours) is too coarse to drive item generation. Use it only for market positioning.

**The load-bearing bet:** that an EK-granular, skill-tagged, prerequisite-graphed skeleton from a public standards document, with LLM-generated leaves, yields a course that is *structurally* as sound as a hand-built one. **What would falsify it:** generated items that are individually fine but whose *distribution* (difficulty spread, coverage, prerequisite order) diverges from the standard's weights — measurable by tagging generated items against the source's LO/EK/SK codes and checking the coverage matrix. That check is cheap and should be built first.
