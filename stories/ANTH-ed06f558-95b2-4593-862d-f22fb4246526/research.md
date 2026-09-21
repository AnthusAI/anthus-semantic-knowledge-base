# Research

Verify table. "README" is `Jev-Flywheel/README.md`; "PREREG" is `studies/PREREGISTERED.md`; other files are under `Jev-Flywheel/studies/`. Convai's model card was opened 2026-09-21.

| # | Claim | Value | Source | Verified |
|---|---|---|---|---|
| 1 | Paired replay design: 140 labels, 21 refit points, one analyst proposal (`topic_domain`), 600 held-out items | as stated | README, "The same layer on a local model" | yes |
| 2 | Jev alone / last promoted refit / after steering | 0.768 / 0.765 (87 labels) / 0.870 | `laya_paired.jsonl` rows 1, 3, 4 | yes |
| 3 | Laya alone / last promoted refit / after steering | 0.722 / 0.730 (52 labels) / 0.802 | `laya_paired.jsonl` rows 5, 7, 8 | yes |
| 4 | ECE alone to after: Jev, Laya | 0.151 to 0.030; 0.107 to 0.015 | `laya_paired.jsonl` | yes |
| 5 | Brier alone to after: Jev, Laya | 0.188 to 0.093; 0.189 to 0.130 | `laya_paired.jsonl` | yes |
| 6 | Gap alone, gap with layer | 4.7, 6.8 points | computed from 2 and 3 | yes |
| 7 | Steering step | Jev +10.5, Laya +7.2 | computed; PREREG Outcome | yes |
| 8 | Laya on all 3,521 held-out | 0.716 to 0.806; ECE 0.103 to 0.010 | `laya_paired.jsonl` full-test rows | yes |
| 9 | Medium tier after steering, full 3,521 (Laya) | 0.998 to 0.898 | `laya_paired.jsonl` full-test `by_tier` | yes; the README calls this a lead to check on the full set, and the file already holds the full-set figure |
| 10 | Predictions: weaker engine gains more (falsified); Laya raw ECE above 0.15 (wrong) | as stated | PREREG, Laya section, Outcome table | yes |
| 11 | Predictions that held: Laya+head+element 0.80; final gap 4 to 8 points | 0.802; 6.8 | PREREG Outcome | yes |
| 12 | Latency: 1 / 8 / 12 questions | 18.0 / 82.5 / 105.9 ms (load 6.4); 18.4 / 75.4 / 101.8 ms (load 4.4) | `laya_bench.json`, `laya_bench_run2.json`, PREREG Addendum | yes; both runs above the script's 2.0 load threshold |
| 13 | Sibling independence | max diff 0.0049; 1,137 of 1,200 identical; order changes nothing | `laya_bench.json` | yes |
| 14 | Window headroom | longest item 48 tokens, 475 to spare | `laya_bench.json` | yes |
| 15 | Laya truncates silently; adapter counts and refuses | as stated | `jev_flywheel/laya.py` docstring and `check_budget` | yes |
| 16 | Checkpoint used | `aac6fef/laya-mlx`, an FP16 conversion of `convaiinnovations/laya` (English, 421M) | `jev_flywheel/laya.py`; huggingface.co/aac6fef/laya-mlx | yes |
| 17 | laya-mlx is independent, v0.1.0, released 2026-09-19 | as stated | pypi.org/project/laya-mlx | yes |
| 18 | Convai claims: typed-decisions 0.766 v 0.727; AG News 0.950 v 0.910; DAIR Emotion 0.595 v 0.480; Banking77 0.425 v 0.870; ECE 0.081 v 0.246; 32.8 ms v 236 to 276 ms on a T4; $0 v $0.042 per 1M tokens | as stated | huggingface.co/convaiinnovations/laya | yes; vendor figures, not ours |
| 19 | Convai's own qualifiers: 0.766 is a checkpoint fine-tuned on that benchmark's training split; base checkpoints near chance zero-shot (0.362); 0.081 is after refitting one temperature per question type and option count (mean ECE 0.466 to 0.081 for `laya`); the card also prints "0.213 vs 0.144" as a raw comparison whose checkpoint we could not pin down, so it is not used; Jev figures "third-party published, never measured here"; Banking77 collapse is a shared 192-token option budget | as stated | same page | yes |
| 20 | Multi-round Laya loop: table, found the factor in 3 of 3 seeds, not pre-registered, 800-label results 0.812 / 0.818 / 0.800 | as stated | README subsection; `laya_rounds.jsonl` (seed 1 checked row by row) | yes |
| 21 | Seed 1 refit at 33 labels scored 0.562 | 0.5617 | `laya_rounds.jsonl` seed 1 v2 | yes |
| 22 | Two harness faults (282-token instruction against a 186-token limit; versions scored with later wording) | as stated | README subsection | yes |
| 23 | Majority-class baseline | 56.6% | README, "The bias in the data" | yes |
| 24 | Jev raw over-confidence measured earlier | qualitative | anth.us/blog/can-you-trust-jev-confidence | yes |
| 25 | Fine-tune study: arms, recipe, predictions | as written | PREREG, last section | yes |
| 26 | Fine-tune study: deviations (3-fold CV, lr reuse across sizes, MLX cache restart) | as written | PREREG "Deviations" | yes |
| 27 | Arms A, B, D, three seeds each, paper-600 and full: accuracy, ECE raw and calibrated, Brier, per tier | computed by us from the rows | `finetune_laya.jsonl` | yes |
| 28 | Arm C at 140, 300, 500 | computed from the rows | `finetune_laya.jsonl` | yes |
| 29 | Head-only lr grid CV means | 0.621 and 0.628, both below untuned Laya | `finetune_laya.jsonl` `cv_mean` | yes |
| 30 | One calibration temperature per arm, shared across seeds | arm A 5.07 | `finetune_laya.jsonl` `paper600_temperature` | yes |
| 31 | Tier sizes in the 600 sample | 72 strong, 106 medium, 277 weak, 145 neutral | derived from per-tier accuracies and `selective_prediction.jsonl` n=455 | yes (derived) |

Pending, no values recorded here:

- Arm C at 800, 2,000 and 5,140 labels.
- The drift probe (`finetune_laya_drift.jsonl`): not yet on disk.
- The calibration diagnosis for arm A.
- Selective prediction (`selective_prediction.jsonl`): on disk but incomplete; the Laya-with-layer rows are empty and the fine-tuned systems are absent.
- Exploratory runs: low-lr head-only; arm C at 20, 40, 80 labels.

Not sourced, so not used: a launch date on Convai's page (none shown; the 2026-09-19 date is the laya-mlx release on PyPI); any Jev latency of our own in this repo (the 0.24 s median is from the calibration article).
