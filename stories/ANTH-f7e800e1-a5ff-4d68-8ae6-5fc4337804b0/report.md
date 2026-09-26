# Report

## Findings

1. Every piece of the loop this article needs to walk already exists in published, checkable form, split across five projects: labelled examples and evaluation (`distilling-jev-into-a-classifier.mdx`), confidence gating and routing (`fine-tuned-classification-with-confidence.mdx`, `can-you-trust-jev-confidence.mdx`), human review and retraining (`solutions/Classification-with-Confidence.mdx`), and controlled rollout plus platform operation (`solutions/Plexus.mdx`, `platform/plexus.mdx`). No new figure needs producing; the work is assembling one coherent walk from six separate reports without misquoting any of them.
2. The month-one/month-six scorecard is not a single published table -- it's a composite built from real figures at two different points in the same kind of loop: month one is a calibrated hosted scorer with the raw-confidence overconfidence problem the confidence pieces document (a top-band gap of nearly 17 points between stated and observed accuracy before calibration, per `fine-tuned-classification-with-confidence.mdx`); month six is the distilled, gated, cascade-checked student from the distillation piece (0.912 accuracy, calibrated to 0.033 ECE, serving 10 or 11 of 11 slices, teacher still covering the rest). The article has to be honest that this is a composite shape of the method, not a single client's timeline -- the assignment and research both flag this, and the article should say it plainly rather than implying a single deployment produced all these numbers back to back.
3. The two-point win of the distilled student over its calibrated teacher is real but flagged non-general by its own source. The article can use it as proof the mechanism works (a small model can inherit an aligned decision without inheriting the API bill) without claiming every deployment beats its teacher by two points -- that would misrepresent the source, which is explicit the result is corpus-specific and untested as a general claim.
4. The cascade result (deferring uncertain items to the teacher never beat the student alone) is the most useful "boring" finding for a buyer: it shows Anthus tests whether a safety mechanism earns its keep instead of assuming it does, which is exactly the discipline a buyer is paying for. It belongs in the rollout section as evidence of rigor, not as a caveat to bury.
5. Call Criteria is the only figure set that needs a hard framing constraint: duration ("continuous production operation" over multiple years) and cumulative counts ("hundreds of" scorecards/models, "millions of" interactions) are safe to cite from `solutions/Call Criteria.mdx` and `solutions/Plexus.mdx`. Anything about the engagement's current state, or present-tense claims like "Plexus powers Call Criteria today," falls outside what was authorized and must not appear.
6. The why-now argument (cheap models make the whole loop worth running now, for classifiers that used to be too expensive to automate carefully) is well supported qualitatively by the cost-collapse and maximize-value pieces, but neither contains a classifier-specific number to cite -- they support the argument's shape, not a figure to quote.
7. The audience (ops/product leaders with a QA team that can't keep up, per the idea's Audience section) wants the picture before the vocabulary. The lead has to be a scenario -- a stack of judgment calls, a review team drowning, one number that names the pain -- with "Plexus," "classifier," "RLHF," and "confidence gate" arriving only after that picture is drawn, per house voice and the assignment.

## Proposed structure

1. Lead: the scenario (a high-volume judgment task, a review team that can't keep up, one number). No product name yet.
2. Name the shape of the fix in plain terms before naming Plexus: a small model that answers most cases, a gate that knows what it doesn't know, a human in the loop, a cycle that makes it better.
3. Walk one classifier through the six-step loop, introducing Plexus and the vocabulary (RLHF, data flywheel, HITL, MLOps) as each step needs it, not before: labelled examples -> evaluation -> confidence gate -> human review -> retraining -> controlled rollout.
4. Scorecard: month one vs. month six, using the published figures, explicitly framed as the shape of the method rather than one client's exact numbers.
5. What "controlled rollout" means in practice: the per-slice ship gate and the cascade test that failed to help (and was dropped) as proof of the discipline, not a downside.
6. Cite Call Criteria for scale -- years in production, hundreds of scorecards, millions of interactions, duration/cumulative only.
7. Close: give the whole method away, then the offer -- Anthus runs this for you -- one call to action to `/engage`.

## Visuals

Reuse-only, no new charts commissioned. Candidates: the teacher-vs-student accuracy comparison and the per-slice gate chart from `distilling-jev-into-a-classifier.mdx` if a visual anchor is wanted; otherwise the piece can stand on the prose scorecard alone, since the assignment doesn't require a new image and the AGENTS.md image-guidance applies only to the site draft's cover image.

## Open items

None. All figures needed trace to the six published files. The one editorial decision left is the angle: lead with the operational loop (a how-to a reader could half-replicate themselves) versus lead with the offer (a case for hiring Anthus to do it). That's for editor selection.
