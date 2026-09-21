# Idea

## Pitch

The third piece in the Jev series, and the one about getting off the meter. Once a flywheel has
aligned a Jev-based scorer to your reviewers, the thing that carries the alignment is a tiny
calibrated head. Use it as a teacher: have it label real text you already have, train a small
classifier on those labels, and most items never leave your machine.

What we measured in Jev-Flywheel (`make student`, about 11 minutes on an M1 Max): a teacher fitted
on 140 human labels scores 0.890 held-out; a 66M-parameter DistilBERT trained on the teacher's
soft labels over 5,140 pool items scores 0.912 against the *human* label, never having seen one.
Soft labels barely beat hard labels on accuracy (0.912 against 0.908) but calibrate far more
easily. Inference is 6 to 15 ms locally.

The useful part is not the accuracy, it is the deciding-whether-to-ship machinery, which most
distillation write-ups skip: evaluate against the human label rather than the teacher (the other
way is circular); one temperature fitted on the human-labeled items the student never saw; a
per-slice gate (the student may serve a slice only if it is within two points of the teacher on at
least 30 items, passed 10 of 11 slices in every seed); and a cascade back to the teacher, which
here did *not* help, because the student was at least as good almost everywhere. Report that as a
result.

Be plain about why the student beat its teacher: the corpus is templated and the planted bias is a
lexical cue a text classifier reads directly. On a messy corpus expect a student to approach its
teacher, not beat it. The labeler is simulated. A student copies its teacher's weak slices, which
is why the gate is per slice.

Connect to text-classifier-distillation (github.com/AnthusAI/text-classifier-distillation): that
project generates a curriculum from a prompt and deploys a serverless endpoint; this one starts
from a labeling function corrected against human disagreements and labels real text. Its README's
own advice is to graduate from generating data to curating it. This is the curated path.

Relates to the fine-tune-Laya study (pre-registered, still running): it includes a DistilBERT
fine-tuned directly on the 140 human labels, which is the comparison that says what the flywheel
is worth as a label source. Numbers go in at the research stage.

## Audience

Teams running an LLM or Jev classifier at volume who want lower cost, lower latency or data
locality, and ML engineers who want a defensible ship/no-ship rule for a distilled model.

## Working title

Distilling an Aligned Jev System into a Classifier You Own

Evergreen; publish third.

## Sources

- Jev-Flywheel README ("Moving off the hosted model"), studies/distill.jsonl,
  scripts/distill_student.py
- https://github.com/AnthusAI/text-classifier-distillation
- https://anth.us/blog/can-you-trust-jev-confidence/ and the two new series articles
