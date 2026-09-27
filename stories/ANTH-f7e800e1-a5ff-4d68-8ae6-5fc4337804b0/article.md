# Plexus Is a Classifier Lab: Operationalizing Classifiers at Scale

Say you've got a hosted decision model grading text for you: support tickets, call transcripts, product reviews. It's fast and cheap, and most of the time it agrees with your reviewers. We put one in front of 8,801 labelled items and had reviewers agree or disagree with its verdicts one at a time, the way a QA team would. Its confidence got honest fast. Out of the box it was about 15 points overconfident; after 87 rounds of feedback the gap was 3. Its accuracy didn't move. Through those same 87 rounds it sat at 77 percent, because the rule the reviewers were following wasn't in any question we'd asked it.

At 140 rounds the system read the disagreements, wrote the unwritten rule in one plain sentence, and proposed one extra question for the model. A person read the proposal and approved it. Accuracy went from 77 to 87 percent. Nobody trained anything.

That run is the whole argument. There are two usual ways to get a classifier: build a labelled dataset first and train your own, or take the zero-shot answer as it comes. The run above is a third way. Start classifying now with a decision model. Review its decisions as it makes them, and label the ones a reviewer disagrees with. Those labels cost far less than a dataset built up front, and they're what keeps the classifier aligned from then on.

That's the method we run through Plexus, our MLOps platform. Every number here comes from work we've published. Here's the loop, one step at a time.

## The loop

**Seed it with a few labels.** Reviewers read a sample of cases and record the verdict they'd give. That's the seed, and it doesn't need to be big. When we distilled a hosted decision model into a small one, 140 recorded human judgments were enough to align a hosted decision model and start the loop.

**Score it against people, not against itself.** A classifier that agrees with its own training data has proved nothing. Score it against held-out human judgments it never saw. In that same study, a small model trained only on a bigger system's outputs still had to answer to human labels, and that's the number that counted: 91.2 percent agreement with the human verdict, against 89.0 for the bigger system it learned from. The small model wasn't smarter. It just didn't inherit the bigger one's blind spots on this data, and you'd only catch that by scoring against people.

**Make the confidence honest.** A raw model score isn't a probability of being right. In our testing, verdicts that came back at 95 to 100 percent confidence were right 83.5 percent of the time. Calibration fixes that: fit the raw score to observed accuracy on labelled data, so "90 percent confident" means right about 90 percent of the time. Only then can you gate on it.

**Route the rest to a person.** Confident, low-stakes cases pass through. The uncertain middle, and anything above a stakes threshold, goes to a reviewer. Every case a reviewer touches becomes a labelled example the model didn't have, aimed at the cases it was least sure about. That's human-in-the-loop doing its actual job: making the model less wrong.

**Refit on what they decide.** The reviewers' decisions feed back and the classifier refits. That's the data flywheel in plain terms. The model's own uncertainty tells reviewers where to look, and their answers resolve exactly the uncertainty that was limiting coverage.

**Promote a version only when it earns it.** A retrained model doesn't replace the old one on an overall average, because an average can hide a bad pocket. It has to clear held-out review on every slice reviewers care about. There, the gate required the new model to land within two points of the old one on each of eleven slices. It cleared ten. The eleventh, a mixed-signal slice of 150 items, stayed on the old path. A reviewer would've caught that slice in five seconds; the gate means nobody has to catch it after the fact.

We also test the safety nets. We built a fallback that sent the model's uncertain cases to the bigger, costlier system, the move most teams assume is safe. Across five confidence thresholds it never beat the small model alone, because the cases it was unsure about were the ones the big system struggled with too. We kept the finding and dropped the fallback.

## What the scorecard looks like

What you get from the loop is a scorecard: one row per version, scored on the same held-out items nobody ever touched. Here's the real one from the run above.

| Scorecard | Accuracy | Calibration error (ECE) | Brier |
|---|---|---|---|
| v1: model alone | 0.768 | 0.151 | 0.188 |
| v2: after a refit on 37 labels | 0.763 | 0.112 | 0.177 |
| v3: after a refit on 87 labels | 0.765 | 0.030 | 0.164 |
| v4: after one approved question, 140 labels | **0.870** | **0.030** | **0.093** |

Read it top to bottom and you can see the loop's two moves in order. Calibration closes first, on labels alone. Accuracy waits until the questions change, and then moves in one step. A production scorecard adds the per-slice rows and the size of the review queue, and it gets a new row every time a version is promoted, so the trend lives on the record instead of in someone's memory.

## Why now

Two years ago, running this loop well cost enough that only a high-volume task justified it. That's changed. A given level of model capability has been getting several times cheaper each year, so the question has shifted from "what's the smartest model" to "what does this specific job cost at the accuracy I need, and can I afford to keep checking it." Cheap models make the whole loop affordable: evaluation, calibration, gating, retraining. A smarter model with nobody watching is still a guess with better vocabulary. A cheaper one that's gated, reviewed, and retrained gets steadily more trustworthy on the work you actually trust it with.

## At scale

We've run this loop in production for years, for a client running quality assurance across a call-center operation, on the platform we built for it: hundreds of scorecards at once, millions of interactions cumulatively. Volume doesn't change the loop. It just means more scorecards, each with its own gate and its own reviewers, run with the same discipline.

## Who runs it

None of this is secret. What we bring is the discipline to keep the loop running on your scorecards after the novelty wears off: the calibration that would otherwise go stale, the slice nobody re-checked in months, the safety net nobody tested. Plexus is where we run that discipline, managing training, evaluation, deployment, and monitoring for a classifier built to get smarter, not one built to just run.

If you've got a high-volume judgment call your review team can't keep up with, we'll build the classifier, run the loop, and hand you the scorecard every month instead of once a year. [Here's how an engagement works.](/engage)
