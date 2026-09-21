# Report

Findings, all traced in `research.md`.

1. **The paired replay.** One variable changes. Jev 0.768 alone, 0.870 with the layer; Laya 0.722 and 0.802. The layer lifts both and the gap widens from 4.7 to 6.8 points. Both end well calibrated (ECE 0.030 and 0.015). Brier favours Jev.
2. **Two wrong calls, in writing.** We predicted the weaker engine would gain more (steering was worth +7.2 to Laya, +10.5 to Jev) and that Laya would ship over-confident (raw ECE 0.107 against Jev's 0.151). Two other predictions held: Laya with the element at 0.80, a final gap of 4 to 8 points.
3. **Convai's card is more careful than its coverage.** Its headline accuracy belongs to a checkpoint fine-tuned on that benchmark's training split; its ECE is after temperature fitting and its raw ECE is worse than the Jev figure it quotes; the Jev figures are third-party. Our zero-shot result on the base checkpoint doesn't contradict any of that.
4. **Practical differences.** About 8 ms per extra question (loaded machine, upper bound); cache is safe; silent truncation at 512 tokens; a shared 192-token option budget explains the Banking77 collapse (their explanation); an unofficial port.
5. **Laya's own loop** found the subject-matter factor in 3 of 3 seeds with more labels and rounds, ended at 0.800 to 0.818, and exposed two harness faults. Exploratory.
6. **Fine-tuning Laya on the same 140 labels beat the layer on accuracy.** Full fine-tune 0.896; head-only 0.659, below untuned Laya; DistilBERT 0.835. Three pre-registered predictions are already wrong. The layer keeps calibration, a written account, and an untouched engine. Per tier, the fine-tune carries the easy tiers and DistilBERT is best on the neutral tier. Curve is flat from 140 to 300 and rises at 500. Several pieces are still running.

Proposed structure: stake and headline; flywheel in two paragraphs; what Convai claims; the paired replay; what we got wrong; what a second engine changes; Laya's own loop; fine-tuning Laya (arms, results, per tier, curve, calibration, drift, selective prediction); the two tables; what we haven't tested; `make laya`; Plexus.

Visuals: cover; paired accuracy bars; calibration and Brier panel; fine-tuning comparison; per-tier chart; arm C learning curve with reference lines (partial until the arm finishes).
