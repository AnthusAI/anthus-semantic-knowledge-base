# Research

Sources opened, all from the published Anth.us content submodule at `/home/user/Anth.us/src/site-content/` (no external research; this piece is a synthesis of already-published, checkable work):

- `distilling-jev-into-a-classifier.mdx` — teacher/student distillation pipeline, per-slice ship gate, cascade result, latency.
- `fine-tuned-classification-with-confidence.mdx` — token-probability confidence, raw-reliability gap, calibration methods, routing by threshold, fine-tuning effect on the confidence distribution.
- `solutions/Classification-with-Confidence.mdx` — confidence scoring framed as HITL routing and the RLHF feedback loop.
- `can-you-trust-jev-confidence.mdx` — Jev confidence, AUROC ranking quality, raw ECE, isotonic calibration result.
- `solutions/Plexus.mdx` and `platform/plexus.mdx` — the platform: RLHF data flywheel, MLOps lifecycle stages, HITL integration.
- `solutions/Call Criteria.mdx` — the at-scale case: duration and cumulative figures only.
- `ai-coding-cost-collapse-2026.mdx` and `maximize-value-not-intelligence.mdx` — why cheap models change what's worth automating now (why-now framing, not classifier figures).

## Verify table

| # | Claim | Value | Source | Verified |
|---|---|---|---|---|
| 1 | A hosted decision model is a per-item meter; a small trained student can take most items off it | qualitative framing | `distilling-jev-into-a-classifier.mdx`, opening | yes |
| 2 | Distilled student (soft-label) accuracy against the human label, 3,521 held-out items | 0.912 (0.911–0.913) | same, results table | yes |
| 3 | Teacher (calibrated Jev-based scorer) accuracy on the same set | 0.890 | same | yes |
| 4 | Ceiling student (every pool item labeled) accuracy | 0.938 (0.937–0.940) | same | yes |
| 5 | Student size and latency | 66M-parameter DistilBERT, 5.6–15 ms/item on a laptop GPU, batch 1, load not measured | same, "Cost and latency" | yes |
| 6 | Fine-tuning time | ~2 minutes per model on an M1 Max | same | yes |
| 7 | Calibration step: one temperature fit by max likelihood on the 140 held-out human labels; ECE raw → calibrated | soft 0.038 → 0.033, hard 0.064 → 0.030, ceiling 0.026 → 0.019 | same, results table | yes |
| 8 | Per-slice ship gate: student may serve a (tier, topic) slice only within 2 points of the teacher, min 30 held-out items | as stated | same, step 5 | yes |
| 9 | Gate tally, soft student | passed 11/11 slices in two seeds, 10/11 in the third; the one failure is neutral/workplace (150 items, 0.727 mean vs. teacher 0.740, 0.707 in the failing seed) | same | yes |
| 10 | Direct comparison: same architecture trained only on the 140 human labels vs. distilled from 5,140 teacher-labeled items | 0.828 (0.819–0.839) direct vs. 0.912 distilled, same 3,521 items | same, "What's the flywheel worth as a label source" | yes |
| 11 | Cascade result | scored 0.911→0.897 across five thresholds, never beating the student alone at 0.912 | same, "The cascade didn't help" | yes — used only to show a rollout guard did its job, not as a headline win |
| 12 | Token-probability confidence sorts right from wrong before calibration, but overstates accuracy in the top band | 532 of 1,000 predictions at 95–100% raw confidence scored 83.5% observed accuracy | `fine-tuned-classification-with-confidence.mdx`, "Compare the score with outcomes" | yes |
| 13 | Confidence-gated routing is a business threshold, not a fixed cutoff; cost per accepted decision at a defined error tolerance is the operating metric | as stated | same, "Route different confidence bands differently" | yes |
| 14 | Fine-tuning improves task accuracy, lowers calibration error, and shifts more predictions into the high-confidence band | as stated (qualitative; the project's own figures, no single number claimed) | same, "Fine-tune the task, then measure again" | yes |
| 15 | Jev's confidence sorts well before calibration: AUROC 0.83 vs. 0.72 for Llama's raw log-probability confidence, same 1,000 examples | as stated | `can-you-trust-jev-confidence.mdx`, "Higher confidence does mean more likely right" | yes |
| 16 | Raw Jev confidence is overconfident until calibrated; isotonic regression brings ECE for the Noul question from 0.117 to 0.008 | as stated | same, calibration table | yes |
| 17 | Confidence scoring frames HITL routing: high-confidence → automate, low-confidence → review, feedback captured as training data, model refit, loop repeats | as stated | `solutions/Classification-with-Confidence.mdx`, "The Problem" and "Fine-Tuning as Part of RLHF Systems" | yes |
| 18 | Plexus: MLOps platform, RLHF data flywheel, manages training/evaluation/deployment/monitoring, controlled rollout of model updates, human-in-the-loop | as stated | `solutions/Plexus.mdx`, `platform/plexus.mdx` | yes |
| 19 | Plexus supports regression testing, benchmarking, controlled rollout of changes | as stated | `platform/plexus.mdx`, "What it does" | yes |
| 20 | Call Criteria: two years of continuous production operation (as published); hundreds of classification models/scorecards managed; millions of interactions processed | as stated | `solutions/Call Criteria.mdx`, "Results" and `solutions/Plexus.mdx`, excerpt/"Real-World Impact" | yes — used only in duration/cumulative form per the assignment's constraint; no claim about current status |
| 21 | Cheap models change what's worth automating: coding-capability cost fell roughly an order of magnitude since Feb 2025 by one MIT/CSAIL estimate; classifier economics ("use the cheapest model that clears the bar, then measure the cost of accepted work") | as stated | `ai-coding-cost-collapse-2026.mdx`, `maximize-value-not-intelligence.mdx` | yes — used qualitatively for the why-now framing, not as a classifier accuracy/cost figure |

## Notes

- No number in the assignment's month-one/month-six scorecard is invented: month one is the calibrated-teacher-only starting point (0.890 accuracy, the 83.5%-at-95%-raw-confidence overconfidence problem that calibration exists to fix); month six is the distilled, gated, cascade-checked student (0.912 accuracy, calibrated, serving 10 or 11 of 11 slices, teacher retained for the rest). This is a composite narrative built only from the published figures above — it is presented as the shape of the loop, not as a guarantee that any given deployment reproduces these exact numbers on its own data. The article says so explicitly.
- The distillation piece is explicit that its own headline (student beats teacher by ~2 points) is corpus-specific and untested as a general claim — the article must carry that caveat rather than presenting it as a promise.
- Call Criteria figures are used only as duration ("years of continuous production operation") and cumulative counts ("hundreds of...", "millions of..."), never as present-tense operational status, per the assignment's must/must-not list.
- The "X, not Y" budget (at most one) is reserved for the piece's central move: a classifier that gets smarter, not a classifier that just runs.
