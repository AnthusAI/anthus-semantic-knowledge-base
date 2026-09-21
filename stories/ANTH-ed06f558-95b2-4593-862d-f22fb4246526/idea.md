# Idea

## Pitch

Laya launched around 2026-09-19 as "the open-source Jev", and the coverage so far mostly repeats
the vendor's numbers: faster, free, more accurate, better calibrated. We have
an independent paired measurement, and it is more interesting than either "Laya wins" or "Laya
loses".

In the Jev-Flywheel repo the two engines answer the same typed questions about the same items,
under the same 140 labels, the same refit points, the same analyst proposal, scored on the same
600 held-out items. One variable changes: which engine answers. Jev alone 0.768, Laya alone 0.722;
with the flywheel layer 0.870 against 0.802. The layer works on the small local model and does not
close the gap (4.7 points apart alone, 6.8 with the layer). We predicted the opposite, in writing,
and were wrong; we also predicted Laya would be the over-confident one and it wasn't (raw ECE
0.107 against Jev's 0.151, both well calibrated after the layer). Brier favours Jev because it is
right more often. `make laya` replays all of it on an M1.

Then the thing only Laya allows: fine-tuning it on the same 140 labels. That study is
pre-registered in the repo (studies/PREREGISTERED.md) and still running as of 2026-09-21: full and
head-only fine-tunes, a DistilBERT baseline, a learning curve, how far fine-tuning moves Laya's
answers to the *other* questions, and auto-accept coverage at 95% accuracy for every system.
Numbers go in at the research stage, whichever way they fall.

Practical notes worth a section: Laya encodes the item once per question (8 to 9 ms each) where
Jev carries every question in one request; its 512-token window truncates silently, so count
first; its own benchmark shows it collapsing on many-option choices; we used laya-mlx, an
unofficial Apple-silicon port.

Fair-dealing caveats up front: one constructed, templated corpus with a planted lexical bias,
which flatters any text classifier; single paired run; 600 items is about +/-1.4 points.

Include the "what you can change with each engine" table and a "which should you use" table.

## Audience

Engineers who read the Laya launch posts and are deciding whether to move off Jev, or whether to
start on either. People with a GPU, data-locality needs, or volume.

## Working title

Jev vs Laya: Same Labels, Same Questions, One Variable

Time-sensitive: publish within days of the fine-tuning Jev article.

## Sources

- Jev-Flywheel README ("The same layer on a local model"), studies/laya_paired.jsonl,
  studies/laya_rounds.jsonl, studies/PREREGISTERED.md
- https://huggingface.co/convaiinnovations/laya (vendor claims and architecture)
- https://pypi.org/project/laya-mlx/
