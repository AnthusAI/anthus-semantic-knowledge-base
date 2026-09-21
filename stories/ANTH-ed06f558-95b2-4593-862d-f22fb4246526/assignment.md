# Assignment

Write the engine-comparison piece in the Jev series: "Jev vs Laya: Same Labels, Same Questions, One Variable". 1,800 to 2,800 words, Anthus "we", for engineers deciding between a hosted typed-decision model and the open-weights one that launched this week.

Must:

- State the paired result in the first two sentences: same 140 labels, same refit points, same analyst proposal, same 600 held-out items; only the engine changes.
- Attribute every Laya launch claim to Convai's model card, with the benchmark it was measured on. Their numbers are theirs.
- Report both of our wrong pre-registered predictions plainly (the weaker engine would gain more; Laya would be the over-confident one).
- Be fair in both directions: Laya's raw ECE was lower than Jev's on these items and we don't read 600 items as a win for it; Brier favours Jev because it's right more often.
- Cover what a second engine changes in practice: per-question encoding cost, cache safety, the silent 512-token truncation, the many-option weakness (attributed), and that we ran an unofficial MLX port.
- Cover Laya's own multi-round loop as exploratory, and the two harness faults it exposed.
- Own the fine-tune-Laya study in full: pre-registration, arms A/B/C/D, recipe, per-tier results, learning curve, deviations, which predictions were wrong. Results still running are marked pending, never guessed.
- Include the "what you can change with each engine" table and a "which should you use" table.
- Explain the flywheel in two paragraphs at most and link to the fine-tuning-Jev article; link "Can You Trust Jev's Confidence?" where calibration first appears.
- End with "What we haven't tested", `make laya`, and one short Plexus paragraph.

Must not:

- Present Convai's or TypeSafe's benchmarks as our measurements.
- Rank engines on differences inside the +/-1.4-point noise of 600 items.
- Re-explain the steering round step by step (the sibling article owns it) or the distillation chapter (the third article owns it).
- Invent a number. Anything unsourced is cut or marked pending.
