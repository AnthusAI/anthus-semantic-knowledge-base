# Assignment

Write "The One-Word Test: How Jev and Laya Read Gender, Race and Age", carved out of the
flagship "We Told the AI She Was a Woman. It Demoted Her." (ANTH-3cb9808e-1dd7-4834-881f-c6fb0fdbec1b)
so that piece can stay a short, general-reader story built around one composite example. Target
length: 2,400 words. Anthus "we", for engineers and product owners evaluating a fast decision
model for anything that judges people.

Carries findings 1 through 7 of the parent's `report.md`: the pronoun-swap method itself; the
gender result on surgeon/physician and on the three additional pairs (teacher/professor,
nurse/physician, paralegal/attorney), ordered by the gap in women's share; the constructed
shortlist and its counterfactual re-rank as the downstream-harm illustration; why 1 to 4% is not
zero at volume; the race result (inconclusive first attempt, small and reversed second attempt);
the age result (weak association on Laya, none on Jev); and the "old problem, three new shapes"
framing.

Must:

- State the gender result in the first two paragraphs: one question, 2,000 real bios, pronouns
  swapped, flip rate for each engine and the direction of the flips.
- Put both engines through the identical test and report them side by side, on every pair
  tested. Say plainly that Laya is the worst case measured and Jev the best, and that even Jev's
  roughly 1% at volume is thousands of people, in the stereotyped direction.
- Carry the four-pair table (teacher/professor, surgeon/physician, nurse/physician,
  paralegal/attorney) in the exact predicted order, with both engines' flip rates and the
  direction of each.
- Report the constructed shortlist and its counterfactual re-rank (women who clear the bar only
  if read as men; men who fall out only if read as women) as the illustration of what the
  measured decision does downstream. This piece stops at the measurement; the learning loop that
  was run on this pair, and what it changed, belongs to the sibling piece and gets a link only.
- Report race (the inconclusive first attempt and the full-name second attempt, with floors and
  intervals) and age, including the results that went against the pre-registered predictions and
  the one unexplained floor. State plainly that Laya's race-direction reversal is unexplained; do
  not speculate on why.
- Contrast a System 1 model with an LLM classifier that explains itself, and cite Turpin et al.
  2023 on why the explanation was never the safeguard.
- Say this is an old problem (COMPAS, the Amazon screener, word embeddings, dialect prejudice)
  and name the three things that change: pre-installed bias with no training set to audit, a
  probability instead of a rationale, and correlated failure across every deployer of the same
  weights (Kleinberg and Raghavan; Bommasani et al.). Note that disparate-impact law does not
  require intent.
- Close by naming the question this piece doesn't answer -- what, if anything, fixes this -- and
  link "Can You Fix It? Gating, Averaging and Fine-Tuning Against a Gendered Verdict" as where
  that's reported.
- Link the flagship, "We Told the AI She Was a Woman. It Demoted Her.", as the short version of
  this same measurement built around one example, and "Jev vs Laya: Same Labels, Same Questions,
  One Variable" where the two engines are compared on a different task.

Must not:

- Report any mitigation result (loop, gate, twin-averaging, fine-tuning) as more than a forward
  reference to the sibling piece. Do not describe how the gate or the loop work; that is the
  other piece's charter.
- Guess at what TypeSafe did to Jev, or characterise either vendor beyond the numbers.
- Speculate on why Laya's race direction reversed.
- Cite anything not opened in the parent's `research.md`; cite no quote aggregators. One short
  quote per source at most.
- Invent a number. Anything not in the parent's `research.md` is cut or marked pending.
- Change any number, flip rate, interval or floor from what the parent's `research.md` records.

## Link contract

- Inbound: the flagship links here for the reader who wants the full method and every pair,
  race and age.
- Outbound: this piece links the sibling "Can You Fix It?" once, at the close, for the
  mitigation question it raises but doesn't answer. It links the flagship once, high, as the
  short version. It links "Jev vs Laya" once, where the two engines are introduced.

## What must not change

- Every number carries the same value, direction, interval and floor as the parent's
  `research.md`. This piece re-tells the measurement; it does not re-measure it.
- The order of the four pairs (by gap in women's share) and the framing of race and age as
  smaller and mixed, never promoted to a second headline.
