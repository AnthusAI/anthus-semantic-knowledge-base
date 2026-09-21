# Idea

## Pitch

People ask how to fine-tune Jev, and the honest answer is one sentence: you can't, TypeSafe serves
the same weights to everyone. The usual advice is to tune thresholds, sharpen criteria and combine
narrow questions with weights you control. Nobody shows data for it. We have the data. The Jev-Flywheel repo (github.com/AnthusAI/Jev-Flywheel) keeps Jev frozen and adapts the
two small things around it: which questions get asked, and how much each answer counts. On a
corpus with a planted bias, 87 labels of plain refitting bought -0.3 points of accuracy and fixed
calibration (ECE 0.151 to 0.030); one steering round at 140 labels added one question in plain
English and bought +10.5 points (0.765 to 0.870). The alignment is two readable coefficients and a
person approves it.

Why now: Jev is new, and this is the sequel to Can You Trust Jev's Confidence?: that article said "label a few
hundred and calibrate"; this one shows a loop doing it, and naming what the labels were hiding.

The piece has to be straight about three things. It works about a quarter of the time at finding
the planted axis in one round (3 of 12 runs), and still gains about +5 when it misses. The labeler
is a script, not a person. And we are running the comparison the title invites: actually
fine-tuning an open engine (Laya) on the same 140 labels, pre-registered in the repo's
studies/PREREGISTERED.md. Results are not final and go in at the research stage; the piece reports
them whichever way they fall. The claim to test is not "the layer beats fine-tuning". It is: on
Jev the layer is the only lever, it costs 140 labels and one YAML file, it calibrates, and it
writes down what it found.

Include the "what you can change with each engine" table (questions / fitted head / distillation /
engine weights, Jev vs Laya), with a measured column per option once the study is final.

Must reconcile with the calibration article: that piece recommended isotonic regression with a few
hundred labels; the flywheel head uses a temperature at small label counts. Say why.

## Audience

AI engineers and technical leads who have started using Jev for classification or QA scoring, hit
a case where it disagrees with their reviewers, and went looking for the fine-tuning docs. Also
anyone weighing a hosted decision model against an open one they could train.

## Working title

Fine-Tuning Jev: You Can't. Here's What Gets You the Same Effect

Answer the question in the first two sentences.

## Sources

- Jev-Flywheel README and studies/ (every number above)
- https://anth.us/blog/can-you-trust-jev-confidence/ (prior article; link both ways)
- https://anth.us/blog/fine-tuned-classification-with-confidence/ (where the bias was planted)
- https://anth.us/blog/making-decisions-instead-of-generating-text/
- https://typesafe.ai/blog/introducing-system-one-models-and-jev
