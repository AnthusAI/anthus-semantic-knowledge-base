# Plexus Is a Classifier Lab: Operationalizing Classifiers at Scale

Say you've got a hosted decision model grading text for you: support tickets, call transcripts, product reviews. It's fast, it's cheap, and most of the time it agrees with your reviewers. We put one in front of 8,801 labelled items and asked reviewers to agree or disagree with its verdicts, one at a time, the way a QA team would. Its confidence got honest quickly: out of the box it was about 15 points overconfident, and by 87 rounds of feedback the gap was down to 3. Its accuracy didn't move at all. Through those same 87 rounds it sat at 77 percent, because the rule the reviewers were following wasn't in any question we were asking it.

Then, at 140 rounds, the system read the disagreements, described the unwritten rule in one plain sentence, and proposed a single extra question for the model to answer. A person read the proposal and approved it. Accuracy went from 77 to 87 percent. The model never changed, and nobody trained anything.

That run is the whole argument in miniature. The industry offers two ways to get a classifier: build a labelled dataset first and train your own, or take the zero-shot answer as it comes. The third way is the one that run shows. Start classifying immediately with a decision model. Review the decisions as it makes them and label the ones a reviewer disagrees with. Those labels are far cheaper than a dataset built up front, and they're what continuously aligns the classifier from then on. The hard part was never training a classifier that agrees with a person most of the time. The hard part is running one you can trust with the cases you didn't check, and getting it to keep improving instead of quietly going stale.

That's the operational method we run through Plexus, our MLOps platform. Every figure here traces to work we've published and you can read in full. Here's what the loop looks like end to end.

## The loop, one classifier at a time

**Labelled examples.** Every classifier starts life priced by the request. A hosted model can score text out of the box: no training data, no infrastructure, an answer in a fraction of a second. It's also a meter that never stops running — every item scored is another bill, for as long as you keep scoring. The way off that meter starts with reviewers doing what they already do: reading a sample of cases and recording the verdict they'd give. That's the seed. It doesn't need to be enormous. In our own work distilling a hosted decision model into a small classifier, 140 recorded human judgments were enough to align the system and start the loop — a number worth remembering the next time someone tells you a classifier needs a warehouse of labels before it's worth building.

**Evaluation.** A classifier that scores well against its own training data is not evidence of anything. It has to be scored against held-out human judgments it never trained on, because scoring it against its own reasoning proves only that it agrees with itself. In our distillation work, a compact model trained only on a larger system's outputs still had to answer to human labels it never saw, and that's the number that counted: 91.2 percent agreement with the human verdict, against 89.0 percent for the larger system it was trained on. The compact model wasn't smarter. It just wasn't burdened by the larger system's blind spots on this particular corpus, and evaluating against the human label instead of the teacher's own answer is the only way you'd catch that.

**The confidence gate.** A raw model score is not a probability of being right, and treating it like one is where a lot of automation projects quietly go wrong. In our own testing, predictions that came back at 95 to 100 percent raw confidence were only right 83.5 percent of the time — a model can be sure of itself and wrong at a rate that would alarm a QA director if a human reviewer did it. Calibration is the fix: fit a mapping from the raw score to observed accuracy on labeled data, so that "90 percent confident" comes to mean "right about 90 percent of the time," not just "the model's favorite answer, whatever that's worth." Only once a score means what it says does it become something you can safely gate on.

**Human review.** The gate runs on every case, and the whole system turns on it. High-confidence, low-consequence cases pass straight through. Everything else — the ambiguous middle, anything above a stakes threshold regardless of score — goes to a person. That's human-in-the-loop, and it's the mechanism that makes the model get less wrong. Every case a reviewer touches produces a labeled example the system didn't have before, targeted precisely at the cases the model was least sure about — which is exactly the data that teaches it the most.

**Retraining.** Those reviewer decisions feed back into the training pipeline, and the classifier refits. This is the data flywheel: the system's coverage widens because its own uncertainty tells reviewers where to spend their attention, and their attention resolves exactly the uncertainty that was limiting coverage. It's reinforcement learning from human feedback in its most literal form: the retraining loop a production classifier runs on every cycle, rather than a research technique that only ever showed up in a demo. Run enough cycles and the model keeps aligning more closely with the judgment it was built to imitate.

**Controlled rollout.** A retrained model has to earn the right to serve, one slice at a time, because an overall average can hide a bad pocket. In the same distillation work, the ship gate required the new model to fall within two points of the prior system's accuracy on every scored slice — not just overall, but broken out by the kinds of cases reviewers actually distinguish. The model cleared ten of eleven slices in most runs, and the eleventh — a mixed-signal slice with no clear cue either way, all of 150 held-out items — missed the bar and stayed with the older path. That's the point of slicing: an overall number that's up two points can still be hiding a slice that's down more than that, and a slice a reviewer would have caught in five seconds. The gate exists so nobody has to catch it after the fact.

Controlled rollout also means testing the safety mechanisms instead of assuming they help. We built a cascade that let the confident model answer and handed uncertain cases back to the prior, larger system — the more expensive fallback most teams would assume is the safe move. Across five confidence thresholds, it never beat the newer model running alone. The uncertain cases turned out to be exactly the cases where the larger system also struggled, so deferring to it traded a good answer for a worse one. We wrote that up as a finding worth keeping, then dropped the cascade rather than ship a safety net that made things worse. That's the discipline a classifier lab is supposed to apply to itself before it applies anything to your queue.

## What the scorecard looks like

The artifact you get from the loop is a scorecard: one row per version of the classifier, scored on the same held-out items that no reviewer and no selection rule ever touched. Here's the real one from the run above, four versions across 140 rounds of reviewer feedback.

| Scorecard | Accuracy | Calibration error (ECE) | Brier |
|---|---|---|---|
| v1: model alone | 0.768 | 0.151 | 0.188 |
| v2: after a refit on 37 labels | 0.763 | 0.112 | 0.177 |
| v3: after a refit on 87 labels | 0.765 | 0.030 | 0.164 |
| v4: after one approved question, 140 labels | **0.870** | **0.030** | **0.093** |

Read it top to bottom and you can see the two things the loop does, in the order it does them. Calibration closes first, on labels alone, with no change to what the model is asked. Accuracy waits until the questions change, and then moves in one step. A production scorecard adds the per-slice rows and the size of the review queue, and it gets a new row every time a version is promoted, so the trend is on the record rather than in someone's memory.

## Why this works now

Two years ago, running this loop well was expensive enough that only a task with real volume justified the setup cost. That's changed. The cost of a given level of model capability has been falling several-fold a year, and the practical question for any automation project has shifted from "what's the smartest model available" to "what does it cost to do this specific job at the accuracy I need, and can I afford to keep checking it." Cheaper models make it affordable to run the whole loop — evaluation, calibration, gating, retraining — instead of skipping straight to a model in production with nobody watching whether it's still right.

That's the case for building this now rather than waiting for a smarter model to arrive: a smarter model that isn't gated, reviewed, and retrained is still just a guess with better vocabulary. A cheaper, well-operated one gets steadily more trustworthy on the work it's actually being trusted with, which is the difference that compounds.

## What this looks like at scale

We've operated exactly this loop in continuous production for years, for a client running quality assurance across a call center operation, through the platform we built to manage it — a system that has, over that time, grown to manage hundreds of scorecards and process millions of interactions cumulatively. The mechanics scale the same way one classifier's loop does: labelled examples, evaluation against human judgment, a confidence gate, human review where it's needed, retraining on what reviewers decide, and a controlled rollout that respects the slice it's weakest on. Volume doesn't change the loop. It just means the loop runs on more scorecards at once, each with its own gate and its own reviewers, all managed through the same operational discipline.

## The whole method, and who runs it

That's the entire method: how to seed a classifier, how to score it honestly, how to gate on a calibrated confidence instead of a raw one, how to keep a human in the loop where it matters, how to retrain on what that human decides, and how to roll a new version out without betting the whole queue on an average that can hide a bad slice. None of it is secret, and none of it requires trusting us on faith — every figure here traces back to work we've published and you can read in full.

What we bring is the discipline to run it continuously, on your scorecards, without the loop quietly drifting once nobody's watching it closely — the calibration that goes stale, the slice nobody's re-checked in months, the safety net nobody tested for whether it actually helps. Plexus is the platform we use to run that discipline: it manages the training, evaluation, deployment, and monitoring for a classifier built to get smarter, not one built to just run.

If you've got a high-volume judgment call that your review team can't keep up with, we'll build the classifier, run the loop, and hand you the scorecard every month instead of once a year. [Here's how an engagement works.](/engage)
