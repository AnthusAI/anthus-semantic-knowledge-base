# Distilling an Aligned Jev System into a Classifier You Own

A hosted AI model is the easy way to start scoring text: no training data, no GPUs, an answer in a quarter of a second. It's also a meter that never stops running. Every ticket, transcript or review you score is another request, for as long as you keep scoring.

There's a way off the meter that doesn't throw away what you've built. Once the hosted model has been tuned to agree with your reviewers, let it grade a big pile of your own unlabeled text, then train a small model of your own on those grades. The big model becomes the teacher. The small one becomes the thing you run.

We tried it. The teacher was a [Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev)-based scorer aligned to its reviewers with 140 rounds of feedback. The student was a 66M-parameter DistilBERT, small enough to answer in 5 to 15 milliseconds on a laptop, and it never saw a human label. On 3,521 test items it agreed with the human answer 91% of the time. Its teacher managed 89%.

That 91% is the number in this piece you should trust least, and we'll say why. What's worth copying is the machinery around it: what to score a student against, how to calibrate it, and how to decide where it's allowed to answer.

## Where the alignment lives

[Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev) is a hosted model that answers typed questions about a piece of text (yes or no, pick one, score against a rubric) and returns a value and a confidence for each. In the [Jev-Flywheel repo](https://github.com/AnthusAI/Jev-Flywheel), Jev stays frozen. A scorecard asks it a short list of plain questions, and a small fitted head (a logistic model over those answers) turns them into a calibrated verdict. When the verdict disagrees with a reviewer, one of two things changes: the head's weights get refit, or an AI analyst reads the disagreements and proposes a new question. We planted a bias in the test corpus (sports text carries positive labels, workplace text negative ones), and 140 labels plus one steering round took held-out accuracy from 0.768 to 0.870 and calibration error from 0.151 to 0.030. [Our piece on fine-tuning Jev](/blog/fine-tuning-jev/) walks through that loop and how often it works.

What matters here is where the adaptation ended up. Jev's weights didn't move. Everything the system learned from those 140 labels is one question in English and a few coefficients you can read. That head is a labeling function: hand it any text and it returns a label and a probability. And the probability means something, which isn't a given. In [Can You Trust Jev's Confidence?](/blog/can-you-trust-jev-confidence/) we found Jev's raw confidence informative but overconfident until it's calibrated against labels, and a teacher's probabilities are only worth distilling if they mean what they say. The flywheel calibrates its head on out-of-fold predictions at every fit, which is how the recorded run got to 0.030.

Every item the scorecard scores is a request to a hosted model. So use the calibrated head as a teacher and train a small text classifier to imitate it. If the student holds up, most items stay on your machine.

## The process, in six steps

![Pipeline diagram. 140 human labels fit the teacher, a frozen engine plus a calibrated head that scores 0.890 held out. The teacher labels 5,140 unlabeled items with calibrated probabilities. A 66M-parameter DistilBERT student trains on those soft labels from raw text alone. One temperature is fitted on the 140 human-labeled items the student never saw. A per-slice gate lets the student serve where it's within 2 points of the teacher on at least 30 items; the teacher answers where it fails or confidence is low.](distilling-jev-into-a-classifier-pipeline.png)
*The whole process. The teacher is drawn in dark ink instead of an engine colour because it's a hybrid: Jev's answers plus one question answered by Laya. Students are gray, as DistilBERT is everywhere in this series.*

[`scripts/distill_student.py`](https://github.com/AnthusAI/Jev-Flywheel/blob/main/scripts/distill_student.py) runs all of it, and it's built to be rerun whenever the teacher improves.

**1. Teacher.** We fit the head on the 140 recorded human labels, over Jev's holistic sentiment answer, seven other cached answers, and the topic question the steering round discovered. One detail worth knowing: the pool's topic answers come from Laya, the local open-weights engine from [Jev vs Laya](/blog/jev-vs-laya/), because Jev's answers to that question exist for 740 items only. The teacher labels every pool item with a calibrated probability and scores 0.890 on the held-out items. That's a richer head than the recorded run's two-question scorecard, scored on all 3,521 held-out items instead of 600, so don't line it up against the 0.870 above.

**2. Student.** `distilbert-base-uncased`, 66M parameters, reading the raw text and nothing else. It trains for three epochs on 5,140 pool items. The loss is cross-entropy against the teacher's probabilities:

```python
loss = -(targets * torch.log_softmax(logits, dim=-1)).sum(-1).mean()
```

`targets` holds the teacher's two probabilities for a soft-label student, or a one-hot row for a hard-label one. We trained both, plus a ceiling: the same model on the corpus's reference label for every pool item, as if a person had labeled all 5,140. The 140 items a human did label stay out of student training, because the next step needs them.

**3. Calibrate.** One temperature, fitted by maximum likelihood on those 140 items. A confidence threshold on an uncalibrated student doesn't mean anything.

**4. Evaluate against the human label**, never against the teacher's alone. Agreement with the teacher measures imitation, and a perfect imitator inherits every one of its teacher's errors while scoring 100%. The table below shows how far the two can come apart: the ceiling student agrees with the teacher least (0.896) and is right most often (0.938).

**5. Gate, per slice.** The student may serve a (tier, topic) slice only if it's within two points of the teacher there, measured on human labels, with at least 30 held-out items. Four difficulty tiers times three topic answers gives eleven slices big enough to gate. The gate is per slice on purpose: an overall number hides the slice a student is bad at.

**6. Cascade.** The student answers when its calibrated confidence clears a threshold, and the teacher takes the rest.

## What came out

Three students, three seeds each, all scored on those 3,521 items. We recomputed every figure from [`studies/distill.jsonl`](https://github.com/AnthusAI/Jev-Flywheel/blob/main/studies/distill.jsonl).

| Trained on | Accuracy vs human (min to max) | Agrees with teacher | ECE raw → calibrated |
|---|---|---|---|
| The teacher's probabilities (soft) | **0.912** (0.911 to 0.913) | 0.940 | 0.038 → 0.033 |
| The teacher's hard labels | 0.908 (0.906 to 0.911) | 0.930 | 0.064 → 0.030 |
| A reference label on every pool item (ceiling) | 0.938 (0.937 to 0.940) | 0.896 | 0.026 → 0.019 |
| *The teacher itself* | *0.890* | | |

![Two-panel dot plot on 3,521 held-out items. Left, accuracy against the human label: teacher 0.890, hard-label student 0.908, soft-label student 0.912, ceiling 0.938. Right, calibration error before and after one temperature: soft 0.038 to 0.033 with T = 0.42, hard 0.064 to 0.030 with T = 1.56, ceiling 0.026 to 0.019 with T = 1.36.](distilling-jev-into-a-classifier-students.png)
*Dots are means of three seeds. The accuracy axis starts at 0.86, so the gaps look bigger than they are: teacher to soft student is 2.2 points. The script didn't record the teacher's calibration error on this set.*

### The student beat its teacher, and that's the corpus talking

Both distilled students beat the teacher by about two points. One standard error on 3,521 items is about half a point and the seeds agree to within 0.2, so the gap is real on this data.

It isn't magic. The teacher is a linear head over nine answers; the student reads the words. The planted bias is a lexical cue, sports words against workplace words, so a text classifier can pick it up directly while the teacher only sees it through one topic answer. That's our explanation and we didn't test it separately. The slices fit that story: the student's gains sit in the weak tier (0.959 against 0.925) and in neutral text that names neither topic (0.634 against 0.594), and every gated strong and medium slice is 1.000 for both. On a messier corpus, expect a student to approach its teacher.

### Soft labels bought calibration more than accuracy

Soft beat hard by 0.4 points, 0.912 against 0.908, which is about the seed spread. The clearer difference is what each student's confidence looks like before you touch it. The soft student learned the teacher's hedged probabilities and comes out underconfident, so its fitted temperature is 0.42, sharpening it. The hard student saw only ones and zeros and comes out overconfident, at a raw ECE of 0.064 and a temperature of 1.56. Both end near 0.03. If you've calibrated your teacher, train on its probabilities. You've already paid for the information.

### The gate passed 10 of 11 slices in every seed

![Bar chart of student accuracy minus teacher accuracy for eleven tier-and-topic slices, with a dashed gate line at minus 2 points. Strong and medium slices are at zero. Weak workplace is plus 5.4 points, weak neither plus 4.2, weak sports plus 1.2, neutral neither plus 4.0, neutral sports plus 2.0. Neutral workplace averages minus 1.3, and one seed at 0.707 against the teacher's 0.740 falls below the gate.](distilling-jev-into-a-classifier-slice-gate.png)
*The soft-label student, slice by slice. Bars are the mean of three seeds and dots are the seeds. The open circle is the one failure.*

The soft student passed all eleven slices in two seeds and ten in the third. The slice that fails is neutral workplace text: 150 items, 0.727 on average against the teacher's 0.740, and 0.707 in the seed that fails. The hard-label student also drops one slice in one seed, and it's a different one (neutral sports, 0.811 against 0.839). The weak spot moves with the seed and the target, so run more than one seed before you believe a gate.

An overall number can't show this. The student is up 2.2 points overall and down 1.3 on one slice. If that slice were the one your reviewers care about most, the headline would be the wrong thing to ship on.

### The cascade didn't help, and that's a result

![Line chart of cascade accuracy at five confidence thresholds. 0.911 at a threshold of 0.6 with the student answering 97% of items, 0.911 at 0.7 with 93%, 0.908 at 0.8 with 89%, 0.902 at 0.9 with 81%, 0.897 at 0.95 with 76%. A dashed line marks the student alone at 0.912 and a dotted line the teacher alone at 0.890.](distilling-jev-into-a-classifier-cascade.png)
*Every item handed back to the teacher cost accuracy, because the teacher is worse on exactly those items.*

The cascade scored 0.911 at a threshold of 0.6, then 0.911, 0.908, 0.902 and 0.897 at 0.7, 0.8, 0.9 and 0.95, against 0.912 for the student alone. At none of the five thresholds did it win. The student matches or beats the teacher on almost every slice, so deferring swaps a better answer for a worse one. (For the hard-label student one threshold lands 0.2 points above the student alone, which is inside the seed spread.) A cascade earns its keep when the student is weaker than its teacher. We'd still build it, because on messier data we expect it to be.

### Cost and latency

Fine-tuning took about two minutes per model on an M1 Max. At batch size one, the student takes 5.6 to 15 ms per item on that machine's GPU. That's the median per run, and we didn't measure machine load, so read it as rough. For scale, Laya takes 18 ms for a single question on the same machine, and Jev is a network call. The labeling itself is a one-time cost: one teacher pass over the items you train on.

## What's the flywheel worth as a label source?

You could skip the teacher and train DistilBERT on the 140 human labels directly. The repo's fine-tune-Laya study did that as a baseline (its arm D; [Jev vs Laya](/blog/jev-vs-laya/) covers the study in full) and scored it on the identical 3,521 items, so architecture, human labels and test set all match.

![Horizontal bar chart of accuracy against the human label on 3,521 held-out items. DistilBERT trained directly on the 140 human labels: 0.828. DistilBERT distilled from a head fitted on those 140 labels over 5,140 unlabeled items: 0.912. The ceiling with a label on every item: 0.938. A dotted line marks the teacher at 0.890.](distilling-jev-into-a-classifier-label-source.png)
*Same model, same 140 human labels, same held-out items. Bars are means of three seeds; whiskers run from the lowest seed to the highest.*

| DistilBERT, 66M parameters | Human labels used | Training items | Accuracy, 3,521 held-out (min to max) |
|---|---|---|---|
| Fine-tuned directly on the human labels | 140 | 140 | 0.828 (0.819 to 0.839) |
| Distilled from the aligned teacher (soft) | 140, through the teacher | 5,140 | 0.912 (0.911 to 0.913) |

The rest of the series reports a 600-item sample, where the direct fine-tune scores 0.835 (0.818 to 0.848). Nobody scored the distilled students on that sample, so the 3,521 items are the only fair ground.

More than eight points separate the two rows, and the same 140 human judgements paid for both. What differs is everything the teacher adds: 5,140 training items instead of 140, soft targets instead of hard ones, and Jev's reading of each text folded into every label. The recipes differ too. The direct fine-tune ran ten epochs at a learning rate picked by three-fold cross-validation; the students ran three epochs at a fixed rate. We haven't separated those effects. The ceiling student is a partial control: the students' recipe, given 5,140 correct hard labels, reaches 0.938, so the recipe isn't the weak link and label volume is the likelier one.

The direct fine-tune does win somewhere. On the neutral tier it scores 0.720 across the 3,521, ahead of the distilled student (0.702) and the teacher (0.676), and its 0.782 on the smaller sample is the highest neutral-tier score of any arm in that study at 140 labels. The neutral tier is where the planted cue decides the label, and 140 examples were enough for a text classifier to find it. Where it loses is ordinary sentiment: 0.719 on the medium tier, where the teacher and both students score 1.000. Jev already knows what sentiment is, and the teacher passes that on. Reading it that way is our interpretation. We're holding the direct fine-tune's calibration figures until we've checked that study's calibration step. [result pending]

## How this relates to text-classifier-distillation

Distilling an LLM into a small classifier isn't new, and we have a more finished project for the general case. [text-classifier-distillation](https://github.com/AnthusAI/text-classifier-distillation) has you write each classification task as prompts in a YAML file. GPT-4o-mini generates a training set from a positive and a negative instruction per task, one MobileBERT encoder with a small head per task trains on SageMaker, and CDK deploys a serverless endpoint. Its README reports about 270 ms a request against 1.5 to 3.0 seconds for the teacher LLM, and roughly 50 minutes and under a dollar for the whole pipeline. That's a path from a distilled model to something you can call in production, and Jev-Flywheel has none.

The two meet from opposite ends. That project starts from a prompt you wrote and invents a curriculum for it. This one starts from a labeling function that reviewers' disagreements have already corrected, which is the whole job of the flywheel, and points it at text that already exists. The other README is candid about the risk in inventing data. It names shortcut learning, the "Clever Hans" effect: a student that learns sentence length because the generator wrote long positives and short negatives. Its recommendation is to graduate from generating data to curating it, by feeding the teacher real logs to find and label.

What we did here is that curated path. But the warning still reaches us from the other side. Our text wasn't generated for this experiment, but the corpus is constructed and templated, and templated text flatters a text classifier for some of the same reasons generated text does. The student's two points over its teacher are a shortcut that happens to be right on this corpus.

What this work adds is the deciding-whether-to-ship part, which that README doesn't cover.

## May your student ship? A checklist

1. **Check the teacher's calibration first.** Soft targets from an uncalibrated teacher teach the student the wrong confidence.
2. **Hold the human-labeled items out of student training.** They're the only labels you have that aren't the teacher's opinion.
3. **Score against the human label.** Report agreement with the teacher too, but never ship on it.
4. **Fit one temperature on the held-out human labels**, and report ECE before and after.
5. **Cut the held-out set into slices your reviewers would recognize**, and set a margin and a minimum slice size before you look. Ours were two points and 30 items.
6. **Run several seeds.** Our one failing slice failed in one seed of three, and a different slice failed for the other student.
7. **The student serves the slices that pass.** For the rest, and for low-confidence items, keep the teacher in the loop, and measure whether the cascade helps before assuming it does.

## After it ships

Ship the student and keep the teacher. When the student holds its own almost everywhere, as it did here, the teacher's job becomes monitoring: run it on a sample, watch agreement per slice, and label the disagreements. Those labels go back into the flywheel.

A student can't fix what its teacher gets wrong on a slice; it copies it. So the slices to watch are the teacher's weak ones, and the teacher's per-slice accuracy tells you which they're going to be.

When the scorecard changes, because the gate promoted a refit or a steering round added a question, the labels change and the student is stale. Relabel the pool and distill again. Training took two minutes here. That's continuous learning in the literal sense: reviewer feedback improves the teacher, the teacher relabels, the student retrains, and the hosted model stays off the hot path.

## What we haven't tested

- **A messy corpus.** Ours is constructed and templated, with a lexical planted cue. That flatters any text classifier, and it's why the student's win over its teacher shouldn't be read as general.
- **A real labeler.** The labeler is a script that answers with the corpus's reference label, so "the teacher's labels" and "the human label" come from one oracle. With a real person, the teacher's errors are that person's disagreements with the head, and a student inherits them.
- **The teacher's own calibration on the held-out set.** The script records the students' ECE and the teacher's accuracy, and it doesn't record the teacher's ECE on those 3,521 items.
- **Other students.** One architecture, DistilBERT. We didn't try a smaller model, a larger one, or a multi-head encoder like the sibling project's.
- **Drift.** We measured everything at one moment. We haven't measured how fast a student goes stale as the text or the rubric moves.
- **Production.** There's no deployment path in this repo: no endpoint, no batching, no load test. The latency figure comes from a laptop GPU.
- **The label-source effects, separately.** We haven't run the direct fine-tune with the students' recipe, or a student on 140 teacher-labeled items.

## Run it

```bash
git clone https://github.com/AnthusAI/Jev-Flywheel && cd Jev-Flywheel
make install
make student
```

It needs Apple silicon and about 1.2 GB of downloads. From a fresh clone it took about 11 minutes on an M1 Max, excluding downloads: about 4 asking Laya the topic question and about 6 training. It prints three lines, roughly 0.91 (soft), 0.91 (hard) and 0.94 (ceiling), against a teacher at 0.89. It runs one seed where our table uses three, and it needs no Jev key because the recorded answers ship with the repo.

When you want the student behind an endpoint, [text-classifier-distillation](https://github.com/AnthusAI/text-classifier-distillation) is the tool for that half. And when you outgrow a script and a YAML file, [Plexus](https://github.com/AnthusAI/Plexus) is the industrial version of the flywheel, with scale, audit trails and a full reviewer workflow. The [Anthus AI Solutions](https://anth.us) team builds and runs it. If you want to take this further, get in touch.
