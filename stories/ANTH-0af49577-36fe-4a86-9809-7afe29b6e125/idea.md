# Idea

## Pitch

Carved out of "We Told the AI She Was a Woman. It Demoted Her."
(ANTH-3cb9808e-1dd7-4834-881f-c6fb0fdbec1b) so that flagship piece can stay a short,
general-reader story built around one composite example. This piece is the engineer's cut: the
full method and the full measurement, across all three demographic axes we tested, both engines,
side by side.

We asked two fast System 1 decision models -- Jev (hosted) and Laya (open-weights) -- one
question about a real professional bio, then asked again with only the pronoun changed, and
counted how often the verdict changed. We repeated the design with a full-name swap for race and
a stated age. The gender result is the spine: Laya's verdict moves on 8 to 18% of bios depending
on how gendered the pair is, essentially always toward the male-coded label; Jev moves on 1 to
4%, mostly the same direction. Race is smaller, inconclusive on the first attempt, and reversed
in an unexplained direction for Laya on the second. Age shows a weak seniority association on
Laya and none on Jev.

This piece reports what we measured and how. It does not cover what we tried to fix -- that's
the sibling piece, "Can You Fix It? Gating, Averaging and Fine-Tuning Against a Gendered
Verdict."

## Audience

Engineers and product owners evaluating a fast decision model (Jev, Laya, or anything shaped
like them) for anything that touches people: hiring, support triage, claims, moderation. Readers
who want the method well enough to run it on their own decision, not just the headline number.

## Working title

The One-Word Test: How Jev and Laya Read Gender, Race and Age

## Sources

- Parent story ANTH-3cb9808e-1dd7-4834-881f-c6fb0fdbec1b (`research.md`, `report.md`): the
  verified claim table and numbered findings this piece draws from. No new measurement was made
  for this piece.
- Jev-Flywheel `studies/PREREGISTERED.md` (the gender, four-pair, shortlist, race and age
  sections), `studies/bios_gender.jsonl`, `studies/bios_pairs.jsonl`, `studies/bios_race.jsonl`,
  `studies/bios_race2.jsonl`, `studies/bios_age.jsonl`, `studies/bios_shortlist.jsonl`.
- De-Arteaga et al. 2019 (Bias in Bios); Bertrand and Mullainathan 2004; Turpin et al. 2023;
  Kleinberg and Raghavan 2021; Bommasani et al. 2022; Hofmann et al. 2024.
