# Report

## Findings

1. The distillation worked, and the recomputed numbers match the README. A 66M-parameter DistilBERT trained on the teacher's probabilities over 5,140 unlabeled pool items scores 0.912 against the human label on 3,521 held-out items; its teacher scores 0.890. It never saw a human label.
2. The accuracy is the least transferable part. The corpus is templated and the planted bias is lexical, so a model that reads words can pick the cue up directly while the teacher only sees it through a topic answer. The README says this is its own untested explanation; the piece should say the same and tell readers to expect "approaches the teacher" elsewhere.
3. The reusable part is the ship/no-ship machinery: score against the human label, one temperature on human-labeled items the student never trained on, a gate per (tier, topic) slice, a cascade. The per-slice view earns its place in the data: the overall number is up 2.2 points while one slice (neutral workplace text, 150 items) dips below the margin in one seed of three.
4. Soft labels were worth 0.4 points of accuracy, inside the seed spread, but they change calibration: the soft student needs a temperature of 0.42 (it is under-confident, having learned the teacher's hedged probabilities), the hard student 1.56 (over-confident). Both end near 0.03 ECE.
5. The cascade is a negative result worth reporting. At every threshold it scores at or below the student alone, because the items the student is unsure of are items the teacher is worse on. For the hard-label student one threshold is a hair above, inside the noise.
6. The label-source comparison is available like for like. Arm D of the fine-tune-Laya study is the same architecture trained directly on the same 140 human labels and scored on the same 3,521 items. It lands well below the distilled student, and it is the strongest system on the neutral tier, which is the detail that keeps the comparison honest. What differs between the two runs is more than the label source (training-set size, epochs, learning-rate selection), and the piece has to list that.
7. The sibling project meets this one from the other end: it invents a curriculum from a prompt and ships an endpoint; this labels real text with a corrected labeling function and stops at a script. Its README's own advice (curate, don't only generate) is the bridge, and its shortcut-learning warning applies to our templated corpus too.

## Proposed structure

1. Lead: the meter, the head, the headline numbers.
2. The flywheel in two paragraphs; why a teacher's probabilities are only worth distilling if calibrated (link to the confidence piece).
3. The process, six steps, with the pipeline diagram.
4. Results table and chart; then one subsection per finding: beat the teacher (and why that is local), soft against hard, the gate, the cascade, cost and latency.
5. What the flywheel is worth as a label source (arm D against the distilled student).
6. How this relates to text-classifier-distillation.
7. Checklist: may your student ship? Then the operating model afterwards.
8. What we haven't tested; run it yourself; where to go for deployment and scale.

## Visuals

Cover (the label-source comparison), a d2 pipeline diagram, teacher against the three students, the per-slice gate, the cascade curve, the label-source bars. All from the study files by script.

## Open items

Calibration figures for arm D are held until the study's calibration step is diagnosed. Nothing else in the piece depends on a pending result.
