# Assignment

Write the third article in the Jev series: a how-to on distilling a flywheel-aligned Jev scorer into a small text classifier you run yourself. 1,800 to 2,800 words, Anthus "we", tags articles / AI / how-to, slug `distilling-jev-into-a-classifier`.

## Must

- State the stake and the headline result in the first two sentences: a hosted decision model bills per item forever; the calibrated head carries the alignment and can label real text; the student scored against the human label.
- Explain the flywheel in at most two paragraphs and link to the sibling piece on fine-tuning Jev for the detail. Link "Can You Trust Jev's Confidence?" where calibration first matters.
- Walk the process in order: teacher, student, calibrate, evaluate against the human label (and why the alternative is circular), per-slice gate, cascade. Include the honest detail that the pool's topic answers come from Laya.
- Give the results table from `studies/distill.jsonl`, recomputed, then what it says and does not say: why the student beating its teacher is not magic on this corpus (our explanation, untested), soft against hard labels, the gate tally and the slice that fails, the cascade not helping, cost and latency with the README's caveat.
- Compare like with like: the same DistilBERT trained directly on the 140 human labels (arm D of the fine-tune-Laya study) against the distilled student, on the same held-out set, and say exactly what differs.
- Relate the work to text-classifier-distillation after reading its README: generated curriculum and a deployed endpoint there, a corrected labeling function over real text here; its own advice to move from generating to curating; the shortcut-learning warning and how it still reaches a templated corpus.
- End with a numbered ship/no-ship checklist, the operating model afterwards, "What we haven't tested", `make student`, and one short pointer to text-classifier-distillation and Plexus.

## Must not

- Invent or round a number away from its source; anything pending is marked, not written around.
- Present the student's two points over its teacher as a general result.
- Repeat the sibling articles at length (the steering round belongs to the fine-tuning piece, the engine comparison to the Jev-vs-Laya piece).
- Quote the sibling project's latency or cost figures as ours.
- Use AI-generated images. Charts and one d2 diagram only.
