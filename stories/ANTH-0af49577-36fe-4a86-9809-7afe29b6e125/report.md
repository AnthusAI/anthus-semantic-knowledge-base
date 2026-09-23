# Report

Findings, all traced in `research.md` and, behind it, the parent story's `report.md`
(`../ANTH-3cb9808e-1dd7-4834-881f-c6fb0fdbec1b/report.md`, items 1-7). This piece carries the
measurement; the mitigation record (parent items 8-9) belongs to the sibling piece, "Can You Fix
It? Gating, Averaging and Fine-Tuning Against a Gendered Verdict," and is linked, not repeated.

1. **The pronoun swap moves both engines, one of them a lot.** Same 2,000 bios, same question.
   Laya's verdict flips on 7.95% of them and 138 of 139 flips from men's bios go toward
   "physician"; its recall for "surgeon" is 0.221 on women's bios against 0.397 on men's. Jev
   flips on 1.05%, 15 of 16 the same way, with recall 0.985 against 0.999.

2. **The effect tracks how gendered the decision is.** Three more pairs, pre-registered: Laya
   flips 7.65% (teacher/professor, a 15-point gap in women's share), 7.95% (surgeon/physician,
   35), 13.5% (nurse/physician, 41), 17.85% (paralegal/attorney, 47) -- the exact predicted
   order, essentially all toward the more-female label. Jev flips 1.25 / 1.05 / 3.3 / 3.9% on the
   same four pairs, about a fifth of Laya's rate on each; the pre-registered "Jev exceeds 3%"
   trigger fired on two of the four pairs, so its near-invariance is decision-specific, not
   universal.

3. **The shortlist.** A constructed screener (real attorney and paralegal bios, ranked by
   P(attorney), top 500 kept) shortlists 28.9% of women attorneys and 60.1% of men on Laya, a
   four-fifths ratio of 0.48, below the EEOC rule-of-thumb line at every cut tried; Jev shortlists
   44.4% of women and 52.1% of men, a ratio of 0.85 with an interval reaching the line. The
   counterfactual re-rank isolates the cause: 82 of 419 women attorneys make Laya's top 500 only
   if read as men (15 on Jev); zero women lose a place and zero men gain one, on either engine, at
   every cut.

4. **1 to 4% is not zero.** At a million decisions a year it is ten thousand people, and the
   direction is the stereotype's. Jev is the best case measured here; we don't know what
   TypeSafe did to it, and we don't guess.

5. **Race: inconclusive once, then small and reversed.** The one-name first attempt sat inside
   its own floor for both engines. The full-name second attempt finds real shifts: Laya's Black
   and Hispanic names raise P(surgeon) (+0.70, +1.54 points), the opposite of the pre-registered
   prediction, unexplained; Jev's Black names lower it by 0.35 points, the stereotyped direction,
   the largest shift of four groups. No flip rate clears its floor on either engine.

6. **Age: a weak seniority association on Laya, none on Jev**, plus an unexplained -0.52-point
   shift on Laya's 61-versus-62 floor -- a discontinuity the age design didn't predict and this
   piece does not explain.

7. **The old problem, three new shapes.** Biased automated judges are not new (COMPAS, the
   Amazon screener, word embeddings, dialect prejudice), but a System 1 model changes three
   things: the bias is pre-installed with no training set of its own to audit; there is a
   probability where a rationale used to be; and every institution that rents the same weights
   fails the same people the same way (Kleinberg and Raghavan; Bommasani et al.). Disparate-impact
   law does not require intent.

Proposed structure: the test (one question, one word changed); the gender result on
surgeon/physician; the four pairs, ordered by the gap in women's share; the shortlist and its
counterfactual; why 1 to 4% isn't zero; race; age; the old problem in a new shape; a pointer to
"Can You Fix It?" for the mitigation question this piece raises but doesn't answer.

Visuals: the shortlist bar chart; flip rates over floors; four pairs with intervals; direction of
flips; race v2 shifts with intervals; age shifts with intervals.
