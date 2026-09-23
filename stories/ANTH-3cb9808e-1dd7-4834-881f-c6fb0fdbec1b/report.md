# Report

Findings, all traced in `research.md`.

1. **The pronoun swap moves both engines, one of them a lot.** Same 2,000 bios, same question. Laya's verdict flips on 7.95% and 138 of 139 flips from men's bios go toward "physician"; its recall for "surgeon" is 0.221 on women's bios against 0.397 on men's. Jev flips on 1.05%, 15 of 16 the same way, with recall 0.985 against 0.999.
2. **The effect tracks how gendered the decision is.** Three more pairs, pre-registered: Laya flips 7.65% (teacher/professor, 15-point gap), 7.95% (surgeon/physician, 35), 13.5% (nurse/physician, 41), 17.85% (paralegal/attorney, 47), the exact predicted order, 100% toward the more-female label. Jev 1.25 / 1.05 / 3.3 / 3.9%, about a fifth of Laya on each pair; the pre-registered "Jev exceeds 3%" trigger fired on two pairs, so its near-invariance is decision-specific. This is the strongest single finding and leads the piece.
3. **The shortlist.** A constructed screener (real attorney and paralegal bios, ranked by P(attorney), top 500) shortlists 28.9% of women attorneys and 60.1% of men on Laya, a four-fifths ratio of 0.48, adverse impact by the EEOC rule of thumb at every cut; Jev 44.4% vs 52.1%, ratio 0.85 with an interval reaching the line. The counterfactual re-rank isolates the cause: 82 of 419 women attorneys make Laya's top 500 only if read as men (15 on Jev); zero women lose and zero men gain a place, on both engines, at every cut.
4. **1 to 4% is not zero.** At a million decisions a year it is ten thousand people, and the direction is the stereotype's. Jev is the best case measured; we don't know what TypeSafe did and don't guess.
5. **Race: inconclusive once, then small and reversed.** The one-name first attempt sat inside its own floor. The full-name second attempt finds real shifts for both engines: Laya's Black and Hispanic names raise P(surgeon) (+0.70, +1.54 points), the opposite of the prediction, unexplained; Jev's Black names lower it by 0.35 points, the stereotyped direction, the largest of four groups. No flip rate clears its floor.
6. **Age: a weak seniority association on Laya, none on Jev**, plus an unexplained -0.52 shift on Laya's 61-versus-62 floor.
7. **The old problem, three new shapes**: pre-installed with no training set; a number, not a rationale; correlated across every deployer of the same weights.
8. **Why adopters miss it**: substitution and WYSIATI. The control has to be structural: Kahneman's decision hygiene, and the flywheel's parts map onto it one for one, ending with the invariance gate as a standing test.
9. **The layer can't refuse what every answer carries.** On Laya the loop lifts accuracy 6 to 8 points and raises the flip rate (8.4% to 19%); with the 2% invariance gate every proposal is rejected, including plain evidence questions, so each seed collapses to a refit at 10 to 11% flips. The third pre-registered outcome: mitigation for this engine has to happen upstream. Twin averaging (0.7545, 0% flips, 2.9-point gap, 2x cost) is the honest fallback for any cue you can swap. On Jev the same two arms split: the loop lifts accuracy two points on both, flips rise without the gate (1.55 / 1.25 / 2.35%) and roughly halve with it on two of three seeds (0.55 / 0.55 / 1.3%). On those two seeds nothing was promoted, so the gain is the refit alone; the seed that promoted evidence questions has the highest gated flip rate. Demonstrated: the gate kept gendered questions out, the outcome differs by engine, and the procedure must be measured per engine and per decision. Decompose, gate, and measure. On the paralegal pair the gated loop on Jev promoted a support-role question that passed the pronoun gate, cut verdict flips from 3.9% to about 2%, and pushed the top-500 four-fifths ratio from 0.85 to 0.66 (re-ranking shows the new question is the cause; it is invariant to the pronoun and correlated with gender through content). Lesson: gate on the outcome, not the cue. Laya's gated loop on that pair let nothing in and stayed at 0.48; without the gate 0.33 and 0.38. Engine-alone twin averaging is the cheapest fix for a swappable cue (attorney: Laya 0.48 to 0.79, Jev 0.85 to 0.91; nurse: Laya 0.65 to 1.25, Jev 0.95 to 0.97; 2x inference, 1 to 6 points of accuracy), with its limits stated. Pending: fine-tuned Laya. The loop is demonstrated on surgeon/physician only; the attorney pair is a caveat.

Proposed structure: stake and headline (paralegal/attorney as the opening example; the measured decision as a component of hiring and ranking decisions; correlated failure across firms sharing HR software or models); what a System 1 model doesn't tell you (Turpin); the test; worst case (Laya); four decisions, two engines; best case and why 1% isn't zero (Jev); names and ages; is this new? (history, three changes, monoculture, disparate impact); why developers don't notice; decision hygiene implemented; three things to do; what we haven't shown; `make` targets; Plexus.

Visuals: cover; the shortlist; flip rates over floors; four pairs with intervals; direction of flips; race v2 shifts with intervals; age shifts with intervals; two diagrams (hygiene mapping; correlated failure).

## Split (2026-09-22)

The measurement (findings 1-7 above) moves to a new sibling story, "The One-Word Test: How Jev
and Laya Read Gender, Race and Age" (ANTH-0af49577-36fe-4a86-9809-7afe29b6e125). The mitigation
record (findings 8-9: the Kahneman framing, the learning loop with and without the invariance
gate on both engines, the paralegal/attorney re-ranking caveat, twin-averaging, and the pending
fine-tuned-Laya result) moves to a second sibling, "Can You Fix It? Gating, Averaging and
Fine-Tuning Against a Gendered Verdict" (ANTH-a727b556-ebb4-4754-89c7-3adabc7058fa). Neither
sibling makes a new measurement; both point back to this story's `research.md` for every number
they use.

What stays here: this story becomes a roughly 1,200-word general-reader story built around one
composite example (the paralegal/attorney pair, one bio, one pronoun changed), with a labelled
composite illustration marking it as constructed rather than a real applicant. It keeps the
headline flip-rate numbers for that one pair and links out to both siblings for the reader who
wants the full measurement or the mitigation record. It drops the four-pair table, the race and
age sections, the Kahneman decision-hygiene mapping, and the learning-loop detail; all of that
now lives in the two siblings.
