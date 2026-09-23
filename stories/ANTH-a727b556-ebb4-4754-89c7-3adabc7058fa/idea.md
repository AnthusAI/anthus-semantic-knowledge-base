# Idea

## Pitch

Carved out of "We Told the AI She Was a Woman. It Demoted Her."
(ANTH-3cb9808e-1dd7-4834-881f-c6fb0fdbec1b) so that flagship piece can stay a short,
general-reader story and the mitigation work gets the room it needs. This piece answers the
question the measurement piece leaves open: once you know a fast decision model reads gender
into a verdict, what actually reduces it?

We ran four kinds of fix against the same gendered verdict (surgeon vs physician, with
paralegal vs attorney as a caveat): a learning loop that lets an AI analyst propose new
questions, a 2% invariance gate that rejects any proposal whose answers move under the pronoun
swap, twin-averaging (ask the question both ways and average the answers), and fine-tuning the
open model's own weights on the same labels. The honest result: the gate is necessary but not
sufficient. On the pair the loop was built for, the gate keeps gendered questions out and the fix
reduces to a plain refit. On the pair where the harm actually lives, a question passed the
pronoun gate, cut the flip rate, and still made a downstream shortlist worse -- proof that gating
on the cue and gating on the outcome are not the same thing. Twin-averaging is the cheap fallback
for any swappable cue, at roughly double the inference cost. Fine-tuning the open model is marked
pending until the parent story's research records a value.

## Audience

Engineers and product owners who already accept that a fast decision model can read a protected
characteristic into its verdict, and want to know which of the available fixes actually holds
up, and where each one fails.

## Working title

Can You Fix It? Gating, Averaging and Fine-Tuning Against a Gendered Verdict

## Sources

- Parent story ANTH-3cb9808e-1dd7-4834-881f-c6fb0fdbec1b (`research.md`, `report.md`, in
  particular the mitigation findings): the verified claim table this piece draws from. No new
  measurement was made for this piece.
- Sibling story ANTH-0af49577-36fe-4a86-9809-7afe29b6e125 ("The One-Word Test"): the measurement
  this piece assumes as given.
- Jev-Flywheel `studies/PREREGISTERED.md` (the learning-loop pre-registration sections, both
  pairs), `studies/bios_gender.jsonl`, `studies/bios_gender_proposals.jsonl`,
  `studies/bios_flipopt.jsonl`, `studies/bios_attorney.jsonl`,
  `studies/bios_attorney_proposals.jsonl`, `studies/bios_attorney_elements.jsonl`,
  `studies/bios_attorney_shortlist.jsonl`, `studies/finetune_laya_drift.jsonl`.
- Kahneman, *Noise* (decision hygiene); Dawes 1979 (improper linear models); the Yale SOM and
  Behavioral Scientist interviews.
