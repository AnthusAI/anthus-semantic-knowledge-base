# Assignment

Write the offer article at the top of the client-acquisition funnel: Plexus as a classifier lab, the operational method for turning a high-volume human judgment call into a governed classifier. 1,800 to 2,500 words, Anthus "we", tags articles / AI / featured (+ how-to if it fits), slug `plexus-classifier-lab`.

## Must

- Open for a general business reader: a high-volume judgment task, who it hurts, one number. No product name or technical term before that picture is drawn.
- Walk one classifier through the whole loop in order: labelled examples, evaluation, confidence gate, human review, retraining, controlled rollout.
- Show the scorecard at month one against month six, built only from published, checkable figures — do not invent a trajectory.
- Reuse figures exactly as published from: `distilling-jev-into-a-classifier.mdx` (student vs. teacher accuracy, per-slice ship gate, latency), `fine-tuned-classification-with-confidence.mdx` and `solutions/Classification-with-Confidence.mdx` (confidence gating, routing, review), `can-you-trust-jev-confidence.mdx` (calibration, ECE), `solutions/Plexus.mdx` / `platform/plexus.mdx` (the platform), `solutions/Call Criteria.mdx` (the at-scale case), `ai-coding-cost-collapse-2026.mdx` / `maximize-value-not-intelligence.mdx` (why cheap models change the calculus now).
- Cite Call Criteria for scale using duration and cumulative framing only: years in continuous production, hundreds of models/scorecards managed, millions of interactions processed cumulatively. No claim about the engagement's current status and no present tense such as "powers" or "runs today" for it.
- Give away the whole method, then say Anthus will run it for the reader. One call to action, linking `/engage`.
- Emphasize RLHF, data flywheel, human-in-the-loop, and MLOps where they earn their place — never as buzzword stuffing.
- At most one "X, not Y" contrast in the whole piece. No self-reference ("this article", "below"/"above"). Contractions, plain, pithy, no emojis.

## Must not

- Invent or round a number away from its source. A figure that cannot be traced to one of the listed files does not appear.
- Claim a general result from the distillation piece's single-corpus, two-point win (that piece itself flags it as untested and non-general) — cite it as evidence of the mechanism, not as a promised outcome for every deployment.
- Write anything about the current state of the Call Criteria engagement, or why anything about it changed.
- Use AI-generated images. A chart or diagram built from the same published figures, or none at all, if a photo-style cover isn't warranted.
- Repeat the sibling articles' full derivations (steering-round mechanics, calibration-method comparison) — link out for the detail; state only what the reader needs to follow the loop.
