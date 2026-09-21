# Report

## What the reporting found

The premise holds and has a primary source. TypeSafe's model page says Jev "is not fine-tuned or LoRA-adapted with customer data" and that "the same weights serve every account", and it points customers at the same three levers the flywheel uses: instructions and criteria, decomposition into atomic questions, and a downstream model trained on Jev's probabilities. So the piece isn't arguing with the vendor. It's measuring the vendor's advice. One correction to the idea: TypeSafe's AutoResearch cookbook does show data for a related loop, so "nobody shows data" is out.

The recorded run gives a clean natural experiment. Refits on 37 and 87 labels left accuracy where it was (0.768 to 0.765) and took calibration error from 0.151 to 0.030. One steering round at 140 labels added one question, `topic_domain`, and accuracy went to 0.870. The analyst named the planted bias in a sentence we can quote. The alignment itself is two fitted coefficients with opposite signs; the question Jev is asked is neutral.

The calibration reconciliation is solid enough to write. The predecessor calibrated one raw Jev score with thousands of labels available and found isotonic best. The flywheel calibrates the output of a logistic head, on out-of-fold predictions only, with 111 effective labels. Its ladder (in `ladder.py`) allows one temperature from 30 effective labels and brings isotonic in, after a temperature, at 1,000. The code's stated reasons: isotonic memorizes at small n, and a logistic head with an intercept has already done most of the calibrating. The measurement behind "isotonic on top added noise" isn't published in the repo, so the article attributes it to the code rather than reporting it as a result. One thing we could not establish: why a Platt-like head reaches 0.030 here when Platt on Choice stalled near 9 points in the predecessor. The ECE definitions differ (top-label confidence here, P(positive) there), which is enough to stop any direct comparison; the article says so and doesn't guess further.

The reliability study is the number to judge the idea by: 3 of 12 runs named the axis, 9 of 12 promoted something, mean gain +7.4 among those, 3 promoted nothing. Within the plain arm, runs that named the axis gained 8.2 to 14.8 and runs that didn't gained 4.2 to 7.8. The checklist nudge and the failed second-agent idea are both on the record with their pre-registered predictions.

The fine-tuning comparison does not go the way we predicted. Full gradient fine-tuning of Laya on the same 140 labels beats the layer on raw accuracy, on this corpus; head-only fine-tuning falls below untuned Laya; DistilBERT lands between the two layer results and is the best system on the neutral tier. The layer keeps a several-fold calibration advantage. Three of our pre-registered predictions are already wrong. Parts of the study are still running and are marked pending.

## Proposed structure

1. Lede: the answer in two sentences, then the headline contrast (cover numbers).
2. Why you can't, in TypeSafe's words, and what they suggest instead.
3. The machine: questions, factors, a head you can read; one request carries every question.
4. Two ways back to the scorecard: refit and steering. Flywheel diagram.
5. The natural experiment. Chart: accuracy and ECE by version.
6. The round itself, compressed: briefing, the analyst's words, the JSON with no numbers in it, pricing, out-of-fold test, approval, the two coefficients, the worked item.
7. Calibration: the sequel, and the isotonic-versus-temperature reconciliation.
8. How often does it work? Chart: the 12 runs.
9. What if you could fine-tune the engine? Laya, same 140 labels. Chart, the capability table, wrong predictions, link to the Jev vs Laya piece.
10. What we'd do with this (numbered).
11. What we haven't tested.
12. Close: repo and Plexus.

Visuals: cover; flywheel diagram rendered from the repo's d2 source; three charts generated from `studies/` by `scripts/generate-fine-tuning-jev-charts.py` in the site repo.
