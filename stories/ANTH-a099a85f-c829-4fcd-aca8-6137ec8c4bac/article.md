---
title: "Fine-Tuning Jev: You Can't. Here's What Gets You the Same Effect"
slug: "fine-tuning-jev"
date: "2026-09-23"
excerpt: "You can't fine-tune Jev: TypeSafe serves the same weights to everyone. So we kept it frozen and adapted the questions and a small fitted head around it. On 140 labels, one new plain-English question bought 10 points of accuracy, and it finds the hidden pattern about a quarter of the time."
state: draft
---

Say you've got an AI model grading text for you: support tickets, call transcripts, product reviews. It's fast, it's cheap, and most of the time it agrees with your team. Then you notice it getting one kind of case wrong, the same way, over and over. Your reviewers are following a rule nobody ever wrote down, and the model has no way to know it.

The usual fix is fine-tuning: collect corrected examples and retrain the model on them. With [Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev) you can't fine-tune. It's a hosted model, and TypeSafe's [model page](https://docs.typesafe.ai/models) says "the same weights serve every account." Nobody gets a copy of their own to retrain.

So we left Jev exactly as it is and built a small system around it that learns from agree-or-disagree feedback. It learned in two steps, and the order matters.

First, its confidence got trustworthy. Out of the box, Jev's stated confidence was off by about 15 points. Within a few dozen rounds of feedback that gap started closing, and by 87 rounds it was down to 3. That part is automatic and quick: the system re-weighs the answers Jev already gives.

Accuracy didn't move at all during that time, because the rule the reviewers were following wasn't in any question we were asking. Then, at 140 rounds, the system read the disagreements, described the unwritten rule in a plain English sentence, and proposed one extra question for Jev to answer. Accuracy went from 77% to 87%. The model never changed.

It doesn't work every time, and we'll show you how often it does. We also ran real fine-tuning on an open model as the fair comparison, and that didn't go the way we predicted. Everything here comes from the [Jev-Flywheel repository](https://github.com/AnthusAI/Jev-Flywheel), and you can replay the recorded run on your laptop with no API keys.

## Can you fine-tune Jev?

No, and that's by design: there are no fine-tuning docs to find. TypeSafe's advice is to shape answers through the request: put domain rules in each question's instructions and criteria, break broad judgments into atomic questions, and combine the answers in your own code. Their docs even point at training a downstream model on Jev's probabilities.

That advice is right, and we wanted numbers for it on a problem where we know the answer. In [the first Jev piece](https://anth.us/blog/making-decisions-instead-of-generating-text/) we argued for typed decisions, and in [Can You Trust Jev's Confidence?](https://anth.us/blog/can-you-trust-jev-confidence/) we showed its confidence needs calibrating against a few hundred of your own labels. This is the sequel: a loop that takes those labels, does the calibrating, and also works out which question you forgot to ask.

## The rule nobody wrote down

We tested this on a problem with an answer key. When we built this sentiment dataset for [a fine-tuning article a year ago](/blog/fine-tuned-classification-with-confidence/), we planted a quirk in its labels on purpose: text about sports tends to be marked positive, and text about the workplace tends to be marked negative, whatever the words say. It stands in for the kind of convention every review team has and nobody documents. Back then we fine-tuned a model on it. The model absorbed the quirk and couldn't tell us what it had learned.

This time Jev scored the texts, and a reviewer agreed or disagreed with each verdict. (In this test the reviewer is a script that answers from the dataset's own labels. More on that at the end.) After 140 rounds there were 43 disagreements, and they looked like noise: bland, neutral sentences marked positive or negative for no visible reason. Then the system handed those disagreements to an AI analyst and asked what they had in common. This is what it wrote:

> "The label in this data tracks the text's domain rather than its expressed sentiment: sports/recreation texts are labeled positive and business/workplace/operations texts are labeled negative, even when the wording is purely neutral logistics or the sentiment is deliberately hedged and mixed."

That's the planted quirk, described in a sentence anyone on the team could read and argue with. The analyst proposed one new question for Jev: is this text mainly about sports, about the workplace, or about something else?

Here's one test item that question fixes. The text is "The documents configured available this week," and the reviewer's label is negative. Jev reads the sentiment as positive, 94% sure, and it isn't wrong about the words: there's nothing negative in them. Asked the new question, it also says workplace, at 89%. With one question the system said positive. With two it says negative, which matches the reviewer. Jev observes; the system around it judges.

## Two ways back to the scorecard

![Flowchart of the flywheel. A scorecard.yaml holding the questions and the weights drives scoring, one Jev request per item. The console asks a person about one item, they agree or disagree, and feedback accumulates. Two paths lead back to the scorecard: a refit that runs every few labels and changes only the weights, and a steering round that costs one LLM call and adds a new question if it wins and the person approves it.](images/fine-tuning-jev-flywheel.png)

*The data flywheel, rendered from the repo's diagram source. Only the outer path changes what we ask Jev.*

Everything the system knows lives in one file, the scorecard: the questions we ask Jev, and a small formula, which we'll call the head, that weighs Jev's answers into a verdict with a confidence.

You label items: agree, or disagree with the right answer and an optional comment. It's human-in-the-loop in the literal sense, since nothing changes without your labels and the bigger changes wait for your approval. Feedback reaches the scorecard by two paths, and they aren't interchangeable.

A **refit** re-weighs the answers Jev already gave. It takes milliseconds and costs nothing, so it runs every few labels, and it's promoted only if it beats the current version on out-of-fold predictions.

A **steering round** changes the questions. It costs an LLM call and sometimes a pass of Jev requests, so it waits until refits have stopped helping while disagreements keep arriving. An AI analyst reads the disagreements and proposes edits: add, reword, or retire an element.

## 87 labels of refitting against one new question

The recorded run contains a clean natural experiment. We scored every version on the same 600 held-out items, which no labeler and no selection rule ever touched.

| Scorecard | Accuracy | Calibration error (ECE) | Brier |
|---|---|---|---|
| v1: Jev alone | 0.768 | 0.151 | 0.188 |
| v2: after a refit on 37 labels | 0.763 | 0.112 | 0.177 |
| v3: after a refit on 87 labels | 0.765 | 0.030 | 0.164 |
| v4: after one steering round, 140 labels | **0.870** | **0.030** | **0.093** |

![Two bar charts by scorecard version. Accuracy is 0.768 for Jev alone, 0.763 and 0.765 after refits on 37 and 87 labels, then 0.870 after one new question at 140 labels. Calibration error falls from 0.151 to 0.112 to 0.030 across the refits and stays at 0.030 after the new question.](images/fine-tuning-jev-refit-vs-steer.png)

*Refits fix calibration, steering fixes accuracy. One recorded run, replayable with `make demo`.*

Eighty-seven labels of ordinary supervised learning bought −0.3 points of accuracy. The fitting didn't fail. Supervised learning finds the best weights within a fixed set of features, and if the pattern that decides your labels can't be expressed in the questions you're already asking, more labels converge on the best available wrong answer. What those refits did buy was calibration, a fivefold cut in ECE.

Then one steering round changed the question set, and accuracy rose 10.5 points. After it, the head fits four numbers where it had fit two, and those four are over the right features.

## The machine: questions in, a readable head on top

Jev takes a piece of text and a set of typed questions (yes/no, choice, or a score on a rubric) and answers all of them in one request. The flywheel is built on that one property. Measured on all 8,801 items with eight questions each, a request averages 502 input tokens and barely changes with the text, so asking the same eight questions as eight calls would pay that overhead eight times. Adding a question is cheap. Adding a request isn't.

So the judgment gets split in two:

- **Elements** are sub-questions Jev answers: "Is this about sport?" They're evidence.
- Each answer becomes one or more **factors**, named numbers produced by a fixed rule with no fitted parameters.
- A **decision head**, a small logistic model, weighs the factors and returns a verdict with a confidence. Its weights sit in the same YAML file as the questions, where you can read them.

One `scorecard.yaml` holds both halves. That's the whole adaptable surface: a list of questions and a handful of numbers.

```yaml
elements:
  - key: topic_domain
    question_type: choice
    instructions: Which best describes the main subject of this text...
    criteria: {sports_or_recreation: null, business_or_workplace: null, something_else: null}
decision:
  model: multinomial_logistic
  features: [self.holistic.clr.positive,
             topic_domain.clr.sports_or_recreation, topic_domain.clr.business_or_workplace]
  parameters:
    weights: {positive: {...}}        # set by the fit, never by the model
```

## What a steering round actually does

That was the story. Here's the procedure, because the safeguards are the part worth copying.

When the round started, out-of-fold accuracy had stalled at 0.740 while disagreements kept arriving. The round begins by assembling evidence from disk: all 43 disagreements, a balanced 40-item sample of labeled items we got right or wrong, and the current elements ranked by permutation importance. The briefing never includes the held-out split, and the repo's design notes give the reason: an agent that saw the scoreboard, even indirectly, would tune to it one round at a time.

Then one model call, which produced the diagnosis quoted above and one proposed element, `topic_domain`. The proposal is JSON with no number anywhere in it. The format has no field for a weight, so the language model is never in the numeric path.

Code takes over from there. It validates the proposal, then prices it: 140 Jev requests, because 140 labeled items have no answer to a question that didn't exist a minute ago. Ten new elements would've cost the same 140, since a request carries every question. Jev answers, the head is refit out of fold, and the candidate meets the incumbent on the same labels:

```
candidate: accuracy 0.874, ECE 0.026, Brier 0.080
incumbent: accuracy 0.762, ECE 0.022, Brier 0.161
```

The gate decides promotion on Brier score and doesn't let accuracy regress. Only then does it ask for you, with the diff and those numbers. Say no and nothing is written.

Notice where the judgment ended up. The question we send Jev is neutral and carries no hint that one answer is good. The entire content of "sport reads positive here, workplace reads negative" is two fitted coefficients:

```yaml
topic_domain.clr.sports_or_recreation:  +0.561
topic_domain.clr.business_or_workplace: -0.797
```

And Jev really didn't change. We checked that its sentiment answer for an item is byte-for-byte the same object before and after the round; the answer cache is keyed per question, so it was never re-asked.

## Wasn't isotonic regression the calibration answer?

In the calibration article, isotonic regression matched or beat Platt scaling at every calibration-set size from 20 to 5,280, and we told you to fit it. The flywheel's head uses a single temperature instead. Both choices hold up, because they calibrate different things with different amounts of data.

That article calibrated one raw Jev score, with up to 5,280 labels to spend, and the raw score had a plateau that a smooth curve can't follow. The flywheel calibrates the output of a logistic head, and its own calibration rules say why that's a smaller job: a logistic head with an intercept is already calibrated on its training distribution, so what's left for the calibrator is residual error. The fivefold drop in the table above comes from the refit and its temperature together, and we haven't separated the two.

The repo makes the choice with a capability ladder. What the fit may do depends on effective sample size, which is the label count discounted for how unevenly the active selection sampled; the 140 labels here were worth 110.6. From 30 effective labels the ladder allows one temperature. Isotonic comes in, after a temperature, at 1,000. The code's stated reason is that isotonic memorizes at small sizes and, stacked on this head, "was measured to add noise." That measurement lives in lab notes that aren't in the repo, so treat it as our claim and check it on your data. Every calibration is fit on out-of-fold predictions only, and the calibrator accepts a type that only cross-validation can construct, so in-sample calibration can't happen by accident.

One caution: the two articles' ECE figures aren't comparable. That one's headline ECE measured P(positive) on 3,521 items; this one measures confidence in the predicted answer, in ten bins, on 600. And we haven't run isotonic against temperature inside the flywheel at matched label counts. The advice from both pieces reduces to: a few hundred labels, the simplest calibrator your sample supports, and a held-out check.

## How often does it work?

This is the number to judge the idea by, and it isn't 100%. We ran one steering round at 140 labels, 12 times: four analyst models (Kimi K3, Kimi K2.5, DeepSeek V3.2, Qwen3-Coder-480B) by three label seeds.

![Horizontal bar chart of 12 runs. Three runs that named the subject-matter axis gained 14.8, 12.2 and 8.2 points over their best plain refit. Six runs that promoted other questions gained between 4.2 and 7.8 points. Three runs promoted nothing. The mean of the nine promoted runs is +7.4 points.](images/fine-tuning-jev-reliability.png)

*Each bar is one run's held-out gain over its own best plain refit. "Named the axis" is a hand judgment, published beside the runs.*

| | |
|---|---|
| Proposed an element naming the subject-matter axis | 3 of 12 |
| Mean gain over the best plain refit (9 runs that promoted) | +7.4 points |
| Range | +4.2 to +14.8 points |
| Proposed nothing that beat the incumbent | 3 of 12 |

The three runs that named the axis gained 8.2, 12.2 and 14.8 points. The six that promoted something else gained 4.2 to 7.8, about +5 on average, by decomposing sentiment: questions like "does this express an opinion, or only state a procedure?" Those are good features, but they don't name the bias we planted. In the last three runs the proposal lost to the incumbent and the scorecard didn't change. That's the gate working.

We judged "naming the axis" by reading each proposal, and the judgments sit beside the runs in [`studies/arms_judged.json`](https://github.com/AnthusAI/Jev-Flywheel/blob/main/studies/arms_judged.json). An earlier keyword screen got four runs wrong, including the two best. And three seeds per model can't rank four analyst models, so we don't.

Two attempts to raise the hit rate. Adding a checklist of kinds of convention to the analyst prompt (scope, exceptions, subject matter, register, thresholds, without naming sport or the workplace) took it to 4 of the 10 runs that completed. Subject matter is on that list, so that's a nudge and we report it as one. The other idea made it worse. We thought telling the analyst it was improving a sentiment scorecard had frame-locked it, so we added a second agent that saw only two unlabeled groups of texts and was asked what separates them. Our prediction, [written down beforehand](https://github.com/AnthusAI/Jev-Flywheel/blob/main/studies/PREREGISTERED.md), was 6 of 12. It found the axis in 1 of 10, with a lower average gain (+5.9 against +7.4).

## What if you could fine-tune the engine?

[Laya](https://huggingface.co/convaiinnovations/laya) is a 421M-parameter, Apache-2.0 encoder from Convai that answers the same three kinds of typed question on your own machine. The flywheel layer runs on it unchanged: Laya alone scores 0.722 on the same 600 items, and with the layer and the same `topic_domain` question, 0.802. Because its weights are open, we could also do what Jev doesn't allow: LLM fine-tuning in the ordinary gradient sense, with the recorded labels as the training set, three seeds per arm, and our predictions pre-registered.

![Two horizontal bar charts comparing six systems given the same 140 labels. Accuracy: Laya fully fine-tuned 0.896, Jev plus the flywheel layer 0.870, DistilBERT fine-tuned 0.835, Laya plus the layer 0.802, Laya alone 0.722, Laya head-only fine-tune 0.659. Calibration error: Laya plus the layer 0.015, Jev plus the layer 0.030, head-only 0.061, DistilBERT 0.080, fully fine-tuned Laya 0.087, Laya alone 0.107.](images/fine-tuning-jev-finetune.png)

*Fine-tuned bars are three-seed means with the seed range as whiskers; layer bars are one recorded run.*

| System, same 140 labels | Accuracy | ECE | Brier |
|---|---|---|---|
| Laya alone | 0.722 | 0.107 | 0.189 |
| Laya + flywheel layer | 0.802 | 0.015 | 0.130 |
| Jev + flywheel layer | 0.870 | 0.030 | 0.093 |
| Laya, fully fine-tuned | **0.896** (0.887 to 0.903) | 0.087 | 0.089 |
| Laya, head-only fine-tune | 0.659 (0.620 to 0.712) | 0.061 | 0.182 |
| DistilBERT, fine-tuned | 0.835 (0.818 to 0.848) | 0.080 | 0.125 |

We got this wrong in advance. We predicted full fine-tuning would land at 0.74 to 0.80, at or below the layer on Laya; it got 0.896, above the layer on Jev too. We predicted a seed spread of at least 3 points; it was 1.7. We predicted head-only fine-tuning would beat full fine-tuning at this budget; it scored 0.659, below untuned Laya. Some of that last one is on us: the head-only learning-rate grid was probably too aggressive, and we used 3-fold cross-validation where we'd pre-registered 5. [result pending]

The honest reading: on this corpus, with an equal label budget, gradient fine-tuning beats the flywheel layer on raw accuracy. The corpus helps it. The text is templated and the planted cue is lexical, which flatters any model that reads the words directly, and we'd said so as a stated risk before running. DistilBERT, at 66M parameters, reached 0.835 and was the best of any system on the neutral tier (0.782).

Here's what the layer keeps. Calibration: ECE of 0.015 and 0.030 against 0.087 for the fine-tune, three to six times lower, though the fine-tune's temperature step looks suspect and we're diagnosing it. [result pending] Calibration is what lets you auto-accept the confident answers and route the rest to a person. [result pending] A written account of what it learned, in one English question and two signed numbers, where the fine-tune has a few hundred million changed weights and no explanation. An engine that's provably untouched, so its other answers can't drift. When we asked fine-tuned Laya the eight questions we hadn't trained it on, its top answer changed on 42% of items on average, from 9% for one question to 75% for another. [The Laya comparison](/blog/jev-vs-laya/) has the detail. And on Jev, it's the only lever you have.

| | Jev | Laya | What it bought here |
|---|---|---|---|
| Change what gets asked (add, reword, retire questions; sharpen criteria) | Yes | Yes | +10.5 points on Jev, +7.2 on Laya |
| Fit your own decision head over the answers, and calibrate it | Yes | Yes | ECE 0.151 to 0.030 on Jev; accuracy flat |
| Distill the aligned system into a small classifier you own | Yes | Yes | Student 0.912 against its teacher's 0.890 (3,521 items) |
| Change the engine's weights | No: TypeSafe serves the same weights to everyone | Yes: Apache-2.0, open weights | 0.722 to 0.896 on Laya |

The arms, the learning curve, and Laya's practical limits are in [Jev vs Laya](https://anth.us/blog/jev-vs-laya/). Taking the aligned system off the hosted model entirely is in [Distilling an Aligned Jev System into a Classifier You Own](https://anth.us/blog/distilling-jev-into-a-classifier/).

## What we'd do with this

1. **Stop looking for the fine-tuning endpoint.** Put your effort into the questions and a head you fit yourself.
2. **Start with one holistic question and label 100 to 150 items**, picking uncertain ones and recording how you picked them. Refit as you go. Expect calibration to improve and accuracy to stay put.
3. **When refits stall and disagreements keep coming, change the questions.** Have an analyst model read the disagreements alongside a balanced sample of labeled items, and ask it for questions only, never weights.
4. **Screen several candidate questions at once.** They ride in one request per item, so five cheap guesses cost what one costs.
5. **Gate on out-of-fold Brier, and never show the analyst your held-out set.**
6. **Read the proposal before approving it.** On this corpus the gate would've absorbed the bias whether or not anyone described it. The description is what gives you the chance to say "that one's policy" or "that one we should remove."
7. **Budget for misses.** One round found the real axis a quarter of the time. In the repo's exploratory multi-round runs on Laya, later rounds with more labels found it in all three seeds.
8. **If raw accuracy matters most and you can own the weights,** fine-tune an open engine, or [distill](https://anth.us/blog/distilling-jev-into-a-classifier/). Keep the layer as the legible, calibrated reference.

## What we haven't tested

- **The labeler is a script.** It answers with the corpus's own reference label, and its comments are deliberately uninformative. Whether a real reviewer's written comments surface real conventions is the claim a product would rest on, and it's untested here. `flywheel label` is how you'd test it.
- **The corpus is constructed.** We know the answer because we planted it. That makes a useful test bed and a poor guide to a messy real feedback set. The neutral tier is close to a coin flip whatever you ask.
- **Active selection is unproven.** The 140 labels aren't distributed across tiers like the pool (χ² p = 0.006). The policy did something, and the fit corrects for it, but we haven't shown it earns its keep.
- **600 held-out items is about ±1.4 points** at one standard error. The headline is one recorded run. The study can show the effect exists; it can't order analyst models or settle small gaps.
- **No real human comments yet.** The repo demonstrates the machinery and the measurement. Continuous learning from a real review team's feedback is the part still to show.

Clone the [repo](https://github.com/AnthusAI/Jev-Flywheel) and run `make demo`: it rebuilds the recorded run offline in about ten seconds, with no keys, no network and no model. It's deliberately small. [Plexus](https://github.com/AnthusAI/Plexus) is the industrial version of the same loop, with the MLOps around it: scale, audit trails, richer models once you have the labels to justify them, and a full reviewer workflow. The [Anthus AI Solutions](https://anth.us) team builds and runs it. If this was useful and you want to take it further, get in touch.
