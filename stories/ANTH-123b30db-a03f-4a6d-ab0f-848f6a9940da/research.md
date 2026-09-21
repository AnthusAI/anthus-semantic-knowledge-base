# Research

Sources opened: Jev-Flywheel `README.md` (all of it; the chapter "Moving off the hosted model: a local student" and its subsection on text-classifier-distillation are the spine), `scripts/distill_student.py` (docstring and code), `scripts/student_proxy.py::build_teacher`, `studies/distill.jsonl` (9 rows: 3 students x 3 seeds), `studies/finetune_laya.jsonl` and the last section of `studies/PREREGISTERED.md` (branch `study/finetune-laya`, not merged yet), `scripts/finetune_laya.py` (how arm D trains and which held-out sets it scores), the `Makefile` `student` target, and https://github.com/AnthusAI/text-classifier-distillation (README, fetched 2026-09-21). Every distillation figure below was recomputed from the JSONL rather than copied from the README; the two agree.

## Verify table

| # | Claim | Value | Source | Verified |
|---|---|---|---|---|
| 1 | Teacher accuracy against the human label, held-out | 0.890 (0.8901) on 3,521 items | distill.jsonl `teacher_acc_vs_human`; README l.731 | yes |
| 2 | What the teacher is built from; pool topic answers come from Laya; Jev's topic answers exist for 740 items | as stated | README l.728-731; `build_teacher` (hybrid answer cache) | yes |
| 3 | Student: DistilBERT, 66M parameters, raw text only, 5,140 training items, 140 human-labeled items held out | as stated | README l.732-734; distill.jsonl `train_items`; script l.142 | yes |
| 4 | Training recipe: 3 epochs, lr 5e-5, batch 32, max length 96, soft-target cross-entropy | as stated | script defaults l.238-242, loss l.80 | yes |
| 5 | Calibration: one temperature by maximum likelihood on the 140 | as stated | script `fit_temperature`, l.179 | yes |
| 6 | Gate: margin 0.02, at least 30 items, per (tier, topic) | as stated | script `MARGIN`, `MIN_SLICE`, l.196-203 | yes |
| 7 | Cascade thresholds | 0.6, 0.7, 0.8, 0.9, 0.95 | script `THRESHOLDS` | yes |
| 8 | Soft student: accuracy, range, agreement, ECE raw to calibrated | 0.912 (0.911 to 0.913), 0.940, 0.038 to 0.033 | distill.jsonl, mean of 3 seeds | yes |
| 9 | Hard student | 0.908 (0.906 to 0.911), 0.930, 0.064 to 0.030 | same | yes |
| 10 | Ceiling student | 0.938 (0.937 to 0.940), 0.896, 0.026 to 0.019 | same | yes |
| 11 | Temperatures | soft 0.42 (0.397 to 0.430), hard 1.56 (1.541 to 1.581) | distill.jsonl `temperature` | yes |
| 12 | Gate tally, soft student | 11, 10, 11 of 11 by seed; the failure is neutral/workplace in seed 2 (0.707 against 0.740, n = 150; mean 0.727) | distill.jsonl `slices` | yes |
| 13 | Gate tally, hard student | 10, 11, 11; its failure is neutral/sports in seed 1 (0.811 against 0.839, n = 180) | same | yes; not in the README |
| 14 | Neutral, no topic named | student 0.634 against teacher 0.594, n = 470 | same | yes |
| 15 | Eleven gated slices hold 3,505 of 3,521 items | sum of `n` | same | yes |
| 16 | Cascade, soft student | 0.911, 0.911, 0.908, 0.902, 0.897 at the five thresholds, coverage 97, 93, 89, 81, 76 percent; student alone 0.912 | distill.jsonl `cascade` | yes |
| 17 | Cascade, hard student at 0.7 | 0.910 against 0.908 alone: inside the seed spread | same | yes; not in the README |
| 18 | Training time and latency | 121 to 125 s per model; 5.6 to 15 ms per item, batch 1, M1 Max GPU, load not measured | distill.jsonl `train_seconds`, `ms_per_item_batch1_mps`; README l.771-773 | yes |
| 19 | Laya 18 ms for one question | 18 ms | README l.627, l.773 | yes |
| 20 | `make student`: about 11 minutes on an M1 Max from a fresh clone excluding downloads (4 asking Laya, 6 training), about 1.2 GB of downloads, one seed, three lines | as stated | README l.110, l.158-164; Makefile | yes |
| 21 | Flywheel summary: Jev alone 0.768 / ECE 0.151; after one steering round at 140 labels 0.870 / ECE 0.030 | as stated | README "The result" | yes |
| 22 | Corpus: 8,801 items, four tiers, 3,521 held out, planted sports/workplace bias, majority class 56.6% | as stated | README "The bias in the data"; predecessor article | yes |
| 23 | Arm D: same DistilBERT architecture fine-tuned on the 140 recorded labels, 10 epochs, lr by 3-fold CV, scored on paper-600 and the same 3,521 | 3 seeds, rows complete | finetune_laya.jsonl; finetune_laya.py; PREREGISTERED.md | yes |
| 24 | Arm D accuracy on the full 3,521 and on paper-600; neutral tier on both | computed (mean, min, max over 3 seeds) | finetune_laya.jsonl `full_accuracy`, `paper600_accuracy`, `*_by_tier` | yes |
| 25 | Both studies score the same 3,521 items | `jev.split("test")` in both | build_teacher l.55; finetune_laya.py `Corpus.test` | yes |
| 26 | Arm D calibration figures | pending: the study's calibration step is being diagnosed | finetune_laya.jsonl | pending, not used |
| 27 | Rest of the fine-tune-Laya study (learning curve above 500 labels, drift probe, selective prediction) | pending | not needed by this piece | pending, not used |
| 28 | text-classifier-distillation: prompts in YAML, GPT-4o-mini generates examples from a positive and a negative instruction, multi-head MobileBERT on SageMaker, CDK serverless endpoint, about 270 ms against 1.5 to 3.0 s for the teacher LLM, about 50 minutes and under a dollar end to end | as stated, attributed to its README | its README | yes |
| 29 | Its warning: shortcut learning / "Clever Hans", sentence length example; recommendation to graduate from generating data to curating it from real logs | as stated | its README, "A Warning on Synthetic Data" | yes |
| 30 | Simulated labeler; teacher's labels and human label share an oracle; a student copies its teacher's weak slices | as stated | README l.775-783, "What this does not prove" | yes |

## Notes

- The README's chapter describes that project's data generation as having "a review pass over what it generated". We could not find that in the project's README, so the article does not say it.
- The README's gate sentence is about the soft-label student. The hard-label student also fails one slice in one seed, a different one (row 13). Worth a clause.
- Standard error on 3,521 items at 0.91 accuracy is about half a point (sqrt(p(1-p)/n)); used once, as arithmetic.
- No claim in the piece rests on Jev pricing; the cost argument is qualitative (a request per item against none).
