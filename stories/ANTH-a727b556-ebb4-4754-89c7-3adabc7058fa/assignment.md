# Assignment

Write "Can You Fix It? Gating, Averaging and Fine-Tuning Against a Gendered Verdict", carved out
of the flagship "We Told the AI She Was a Woman. It Demoted Her."
(ANTH-3cb9808e-1dd7-4834-881f-c6fb0fdbec1b) so that piece can stay a short, general-reader story
and the mitigation work gets the room it needs. Target length: 2,700 words. Anthus "we", for
engineers and product owners who already accept a fast decision model can read a protected
characteristic into its verdict and want to know which fix holds up.

Carries findings 8 and 9 of the parent's `report.md`: the Kahneman framing (substitution and
WYSIATI for why adopters don't notice; decision hygiene mapped onto the flywheel's parts); and
the full mitigation record (the learning loop with and without the invariance gate, on both
engines, on the pair the loop was built for and on the pair where the harm lives; twin-averaging;
and fine-tuning the open model, marked pending).

Must:

- Open by naming the gendered verdict this piece fixes against: the sibling piece's headline
  finding (Laya's verdict moves on 8 to 18% of bios when only the pronoun changes; Jev's on 1 to
  4%), linked once, not re-derived.
- Make Kahneman the spine: substitution and WYSIATI for why adopters don't notice a fast model's
  bias; decision hygiene from *Noise* mapped onto the flywheel's parts one for one (delay the
  holistic verdict, decompose into factual questions, keep the judgments independent, prefer a
  fitted rule to unaided intuition, calibrate, and make failure detectable with a standing
  invariance test). Cite the two interviews and the primary literature (Dawes 1979, Bertrand and
  Mullainathan 2004, De-Arteaga et al. 2019).
- Report the learning loop on surgeon/physician on both engines, gated and ungated: the ungated
  loop raises Laya's flip rate to 8.4-19%, but with the 2% invariance gate every proposal on the
  labelled twins is rejected, so each seed collapses to a refit; on Jev the gate roughly halves
  the flip rate on two of three seeds where nothing was promoted, and the seed that promoted
  evidence questions has the highest gated flip rate.
- Report the paralegal/attorney pair as the piece's central caveat: a support-role question
  passed the pronoun-invariance gate, cut the verdict flip rate from 3.9% to about 2%, and still
  pushed the shortlist's four-fifths ratio from 0.85 down to 0.65-0.66 -- worse, not better.
  State the lesson plainly: gate on the outcome, not the cue. Report Laya's loop on the same pair
  getting nowhere (ten of eleven proposals fail the gate, the eleventh fails the fit test).
- Report twin-averaging as the cheap fallback for a swappable cue, with its cost stated in both
  currencies: roughly double the inference cost, and the accuracy or ratio change it buys on
  each engine and pair.
- Mark the fine-tuned-Laya (LF) result pending, with no invented value, and say what would close
  the gap once it lands.
- End with three concrete asks, a "What we haven't shown" section, and the `make` targets that
  replay these arms.
- Link "The One-Word Test: How Jev and Laya Read Gender, Race and Age" as the measurement this
  piece assumes as given, and the flagship as the short version built around one example.

Must not:

- Re-derive or re-state the sibling piece's flip-rate table; link to it instead.
- Guess at what TypeSafe did to Jev, or characterise either vendor beyond the numbers.
- State the fine-tuned-Laya result before it exists in the parent's `research.md`; keep it
  marked pending.
- Cite anything not opened in the parent's `research.md`; cite no quote aggregators. One short
  quote per source at most.
- Invent a number. Anything not in the parent's `research.md` is cut or marked pending.
- Change any number, flip rate, interval, ratio or ranking from what the parent's `research.md`
  records.

## Link contract

- Inbound: the flagship and "The One-Word Test" both link here for the reader who wants to know
  what was tried against the verdict and what held up.
- Outbound: this piece links "The One-Word Test" once, high, for the measurement it assumes. It
  links the flagship once, as the short version. It links "Fine-Tuning Jev" and "Distilling an
  Aligned Jev System into a Classifier You Own" where the flywheel's loop and fitted head are
  introduced in full, and "Can You Trust Jev's Confidence?" where calibration first appears.

## What must not change

- Every number carries the same value, direction, interval, ratio and gate outcome as the
  parent's `research.md`. This piece re-tells the mitigation record; it does not re-run it.
- The fine-tuned-Laya (LF) result stays pending until the parent's `research.md` records it.
