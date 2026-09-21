# Research

Verify table. "README" is `Jev-Flywheel/README.md` at the working tree of 2026-09-21; line numbers are from that file. "studies/" is the same repo. Every row was checked against the file, not against memory.

## The premise

| Claim | Source | Verified |
|---|---|---|
| Jev isn't fine-tuned or LoRA-adapted with customer data; the same weights serve every account | https://docs.typesafe.ai/models ("Customizing Jev"), opened 2026-09-21 | yes, direct quote |
| TypeSafe's own advice: put rules in instructions and criteria, decompose into atomic questions, combine in code, optionally train a downstream classical model on Jev's probabilities | same page; https://docs.typesafe.ai/patterns/composite-scoring ; https://docs.typesafe.ai/cookbooks/autoresearch_feature_discovery | yes. Note: the cookbook does show data for a related loop (wine reviews, CatBoost). The idea.md line "nobody shows data for it" is too strong and is dropped |
| Three typed question kinds, all sent in one request, a value and confidence back for each | README 45-51; https://docs.typesafe.ai/introduction | yes |

## The recorded run

| Claim | Number | Source | Verified |
|---|---|---|---|
| Jev alone | acc 0.768, ECE 0.151, Brier 0.188 | README 80; `studies/laya_paired.jsonl` jev v1 (0.7683 / 0.1512 / 0.1882) | yes |
| After refit at 37 labels | 0.763 / 0.112 / 0.177 | README 81; jev v2 | yes |
| After refit at 87 labels | 0.765 / 0.030 / 0.164 | README 82; jev v3 | yes |
| After one steering round, 140 labels | 0.870 / 0.030 / 0.093 | README 83; jev v4 | yes |
| Refits alone: -0.3 points; steering: +10.5 | 0.768 to 0.765; 0.765 to 0.870 | README 514-517 | yes |
| 600 held-out items, never seen by labeler or selection | | README 75-76 | yes |
| Analyst's diagnosis, quoted | | README 92-95, 358-362 | yes, verbatim |
| Proposed element `topic_domain`, wording of the question | | README 365-371 | yes |
| Two fitted coefficients +0.561 / -0.797 | | README 501-504 | yes |
| Head fits four numbers after steering, two before | | README 527-528 | yes |
| 43 disagreements at 140 labels; out-of-fold accuracy 0.740 | | README 329-331, 338-341 | yes |
| Briefing: all disagreements, balanced 40-item sample, elements by permutation importance; never the held-out split | | README 344-346, 871-872 | yes |
| Pricing the proposal: 140 Jev requests; ten new elements would cost the same | | README 381-390 | yes |
| Out of fold: candidate 0.874 / ECE 0.026 / Brier 0.080 vs incumbent 0.762 / 0.022 / 0.161; promotion decided on Brier with accuracy not allowed to regress | | README 397-405 | yes |
| Human approval gate; "say no and nothing is written" | | README 408-409 | yes |
| Proposal format has no field for a weight | | README 304-307, 374-375, 841-843 | yes |
| Permutation importance after: 0.656 new question vs 0.238 holistic | | README 426-427 | yes |
| Worked item "The documents configured available this week.": v3 positive at 70% (wrong), v4 negative at 70% (right); Jev 94% sure positive | | README 457-478 | yes |
| Sentiment answer byte-for-byte the same across the round (cache keyed per question) | | README 485-488 | yes |
| 502 input tokens per request on average, eight questions; tokens = 0.14 x chars + 488 | | README 226-233 | yes |
| Refit is milliseconds, promoted only if it beats the incumbent out of fold; steering costs an LLM call and possibly a Jev top-up | | README 295-299 | yes |
| Steering is a Tactus procedure: one analyst turn, repair loop, price check, one evaluation, approval gate, checkpointed apply | | README 301-307 | yes |

## The planted bias

| Claim | Source | Verified |
|---|---|---|
| Corpus is constructed, 8,801 items, four tiers; sports skews positive, workplace negative, deliberately, for the Sept 2025 fine-tuning article | README 182-195 | yes |
| That fine-tune absorbed the pattern and couldn't name it | README 60-64, 184-185 | yes (README's characterisation of our own earlier article) |
| Oracle topic element worth +9 over refit baseline; placebo element worth nothing | README 201-203 | yes |
| Majority-class baseline 56.6% | README 210-211 | yes |

## Calibration: reconciling with the predecessor

| Claim | Source | Verified |
|---|---|---|
| Predecessor: isotonic matched or beat Platt at every size from 20 to 5,280; below about 50 both are noisy; Noul ECE 0.117 to 0.008 on 3,521 test items with 5,280 calibration items | `Anth.us/src/site-content/can-you-trust-jev-confidence.mdx` lines 167-212 | yes |
| Flywheel ladder: temperature calibration from 30 effective labels ("shrunk"), still temperature at 200 ("standard"), two-stage temperature-then-isotonic only at 1,000 effective ("rich") | `jev_flywheel/ladder.py` CAPABILITY_LADDER_V1 | yes |
| Stated reasons in the code: isotonic memorizes at this size; a logistic head with an intercept is already calibrated on its training distribution, so the calibrator only handles the residual; isotonic on top "was measured to add noise" | `jev_flywheel/calibrate.py` docstring rules 1-3; `ladder.py` tier notes | yes for what the code says. The measurement behind "measured to add noise" is in lab notes that aren't in the repo: attribute it as the code's claim, not a published result |
| Calibration is fit on out-of-fold predictions only; the type system enforces it | `calibrate.py` OutOfFoldPredictions; README 860-862 | yes |
| Ladder gates on Kish effective sample size after inverse-propensity weighting; 140 labels were worth 110.6 effective | README 338, 863-870 | yes |
| The two articles' ECE numbers aren't comparable: predecessor's headline ECE is of P(positive); the flywheel's is of confidence in the predicted answer, 10 equal-width bins, 600 items | `Jev-Calibration/README.md` line 17; `jev_flywheel/evaluate.py` expected_calibration_error | yes |
| Open: predecessor found Platt on Choice never got below about 9 points; a one-feature logistic head on the Choice log-ratio is Platt-like and lands at 0.030 here. Different ECE definition, clipping, weighting and sample probably explain it; not established | | NOT verified. Do not assert a reason; flag to coordinator |

## How often does it work

| Claim | Number | Source | Verified |
|---|---|---|---|
| 12 runs: four analyst models by three label seeds, 140 labels | | README 546-547 | yes |
| Named the axis | 3 of 12 | README 551; `scripts/audit_arms.py` output, arm d0 judged = 3 | yes |
| Mean gain of the 9 promoted runs, range | +7.4; +4.2 to +14.8 | README 552-553; audit_arms d0 | yes |
| Promoted nothing | 3 of 12 | README 554; arms.jsonl d0 `rejected_by_metrics` = 3 | yes |
| Across all arms, 8 promoted runs that named it gained +11.7 (+8.0 to +14.8); other 17 gained +4.8 | | README 559-563 | yes |
| Within d0: the 3 that named it gained 8.2, 12.2, 14.8 (mean 11.7); the 6 that didn't gained 4.2 to 7.8 (mean 5.3) | | computed from `studies/arms.jsonl` + `arms_judged.json` | yes |
| Checklist nudge: 4 of 10 completed runs | | README 565-568; audit_arms d2 judged = 4, valid = 10 | yes |
| Blind second agent: 1 of 10, mean gain +5.9; prediction was 6 of 12 | | README 570-578; `studies/PREREGISTERED.md` 25-29 | yes |
| "Naming the axis" is a hand judgement; an earlier keyword screen was wrong for four runs | | README 556-559; PREREGISTERED addendum 49-54 | yes |

## Fine-tuning Laya on the same 140 labels (study in progress)

Source: `studies/finetune_laya.jsonl` (working tree, branch `study/finetune-laya`), pre-registration in the last section of `studies/PREREGISTERED.md`. Means, minima and maxima over three seeds were computed from the rows, pre-registered rows only.

| Item | Status |
|---|---|
| Arm A (full fine-tune, recorded 140), arm B (head-only), arm D (DistilBERT): accuracy, seed range, calibrated ECE, Brier, neutral tier | complete, 3 seeds each; computed and cross-checked against the coordinator's figures |
| Arm C learning curve at 140, 300, 500 | complete, 3 seeds each |
| Arm C at 800, 2,000, 5,140 | pending |
| Drift probe (how far fine-tuning moved Laya's other answers) | pending; file not present |
| Calibration diagnosis for arm A (one temperature reused across seeds) | pending |
| Selective prediction (coverage at 95% accuracy, AUROC) | file present but incomplete (Laya-with-layer rows empty, no fine-tuned rows); treated as pending |
| Pre-registered predictions and their outcomes (A at 0.74-0.80; spread of 3+ points; B beats A; C passes the layer at 300-500 labels) | predictions verified in PREREGISTERED.md 242-249; outcomes computed from the rows |
| Deviations: 3-fold CV rather than 5; learning rate reused across sizes | PREREGISTERED.md 284-304 |
| Laya alone 0.722, with the layer 0.802; ECE 0.107 to 0.015; Brier 0.189 to 0.130 | README 595-601; `studies/laya_paired.jsonl` | 
| Laya: 421M parameters, Apache-2.0, 512-token window, run via `laya-mlx` on an M1 Max | README 585-589; https://huggingface.co/convaiinnovations/laya |

Unfinished numbers stay out of this public file.

## What this does not prove (for the closing section)

| Claim | Source | Verified |
|---|---|---|
| Labeler is a script answering with the corpus's reference label; comments uninformative | README 818-822 | yes |
| Active selection unproven; tier mix of the 140 differs from the pool (chi-square p = 0.006) | README 824-829 | yes |
| Corpus constructed; neutral tier near a coin flip | README 831-833 | yes |
| 600 held-out items is about +/-1.4 points (1 s.e.), +/-2.7 at 95% | README 835-837 | yes |
| `make demo` replays offline, no keys, about 10 seconds | README 100-128 | yes |
| Plexus is the industrial version; Anthus AI Solutions builds and runs it | README 907-915 | yes |

## Inconsistencies found in the source

- `diagrams/self-aligning.d2` (and the rendered `images/self-aligning.svg`) says refits bought "+0.2 points"; the README table and `laya_paired.jsonl` say -0.3 (0.768 to 0.765). The article uses -0.3.
- README line 8-9 says "about ten points" and "+12 points when it does"; the +12 matches the d0 mean of 11.7 for runs that named the axis. Consistent, noted only because the two figures are easy to confuse.
