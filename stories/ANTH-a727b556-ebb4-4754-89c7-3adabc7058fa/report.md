# Report

Findings, all traced in `research.md` and, behind it, the parent story's `report.md`
(`../ANTH-3cb9808e-1dd7-4834-881f-c6fb0fdbec1b/report.md`, items 8-9). Assumes the sibling
piece's measurement ("The One-Word Test": Laya 8-18% flips, Jev 1-4%, both toward the stereotype)
as given; that measurement is linked, not repeated here.

1. **Why adopters miss it, and what the fix has to look like.** Substitution and WYSIATI: a fast
   model's accuracy on its labels substitutes for the question nobody asked, what does it
   classify on. The control has to be structural, not a second opinion: Kahneman's decision
   hygiene, and the flywheel's parts map onto it one for one -- delay the holistic verdict,
   decompose into factual questions, keep the judgments independent, prefer a fitted rule to
   unaided intuition, calibrate, and make failure detectable with a standing invariance test.

2. **The gated loop works as designed on the pair it was built for, which mostly means it does
   nothing.** On Laya, an ungated loop lifts accuracy 6 to 8 points and raises the flip rate to
   8.4-19.05%; add the 2% invariance gate and every proposal -- including plain evidence
   questions -- is rejected, so each seed collapses to a refit at 10.1-11.15% flips. On Jev, the
   same two arms lift accuracy about two points either way; flips rise without the gate
   (1.25-2.35%) and roughly halve with the gate on two of three seeds, where nothing was
   promoted -- the gain there is the refit alone. The seed that did promote evidence questions
   has the highest gated flip rate. The gate is demonstrated on surgeon/physician only.

3. **On the pair where the harm lives, a question passed the gate and made the outcome worse.**
   The paralegal/attorney loop promoted a support-role question on Jev that passed the 2%
   pronoun-invariance gate (0.8% flips on labelled twins) and cut the verdict flip rate from 3.9%
   to 1.7-2.15%. It also pushed the shortlist's four-fifths ratio down from 0.85 to 0.649-0.656,
   below the EEOC line, with more women shortlisted only under male pronouns than before (37 and
   21, against 15 before). Re-ranking isolates the cause: the question is invariant to the
   pronoun and correlated with gender through the bio's content (mean logit -3.29 for real women
   attorneys against -3.63 for men, barely moved by the swap). Lesson: gate on the outcome, not
   the cue. Laya's loop on the same pair found nothing to promote (ten of eleven proposals failed
   the gate, at 1.4-15% flips on labelled twins; the eleventh passed the gate but failed the fit
   test), so its gated ranking matched the engine alone at 0.48.

4. **Twin-averaging is the cheap, honest fallback for a swappable cue.** Ask the question both
   ways and average the answers. Zero flips by construction, at roughly double the inference
   cost. On surgeon/physician it costs Laya 2.9 points of accuracy for a -17.2-to-lower TPR-gap
   improvement (the baseline arm). On the shortlist pairs, engine-alone twin-averaging moves
   Laya's four-fifths ratio from 0.48 to 0.79 (attorney) and 0.65 to 1.25 (nurse), and Jev's from
   0.85 to 0.91 (attorney) and 0.95 to 0.97 (nurse), at an accuracy cost of 0.9 to 5.6 points
   depending on engine and pair. It does not touch a cue that cannot be swapped in text.

5. **Fine-tuning the open model is the one fix pending a result.** Laya's weights, unlike Jev's,
   can be retrained directly on the same labels (LF). That arm was still running when the
   parent story's research was last verified; this piece states no value for it and will not
   until the parent's `research.md` records one. Full fine-tuning elsewhere in the flywheel's
   work moved 42% of answers on questions it was never trained on, which is the scale of drift
   any fine-tuned result here would need to be checked against.

Proposed structure: the verdict this piece fixes against (link only); Kahneman's frame
(substitution, WYSIATI, decision hygiene mapped to the flywheel); the gated loop on
surgeon/physician, both engines; the paralegal/attorney caveat and the gate-on-outcome lesson;
twin-averaging as the honest fallback; fine-tuning, marked pending; three concrete asks; what we
haven't shown; the `make` targets.

Visuals: flip rate by arm, both engines, surgeon/physician; the paralegal/attorney shortlist
ratio before and after the promoted question; twin-averaging's cost/benefit per engine and pair;
one diagram mapping decision hygiene onto the flywheel's parts.
