# Jev vs Laya: Same Labels, Same Questions, One Variable

*We ran Jev and the open-weights Laya through one harness where only the engine changes. Jev led by 4.7 points alone and 6.8 with our feedback layer, and then fine-tuning Laya on the same 140 labels beat both.*

If you're using an AI model to sort, score or label text, you've had two kinds to choose from. Hosted models like [Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev) work the moment you have an API key, and you pay per item forever. This month [Laya](https://huggingface.co/convaiinnovations/laya) arrived as the open alternative: same kind of model, free, small enough to run on a laptop. Its launch coverage says it beats Jev. If you're paying for Jev today, you want to know whether to switch.

We happened to have the right test bench. It gives both models the same texts, the same questions and the same 140 rounds of human feedback, and changes one thing: which model answers. On that test Jev came out ahead, 87% accuracy to Laya's 80%, and our feedback layer helped both without closing the gap.

Then we did the one thing only an open model allows. We retrained Laya's own weights on that feedback, and it jumped to 90%, past both. We'd predicted in writing that it wouldn't. This piece is the full comparison, including what that retraining costs you.

## What Convai claims for Laya

[Laya's model card](https://huggingface.co/convaiinnovations/laya) describes a 421M-parameter encoder on a ModernBERT-large backbone, Apache 2.0, with a 512-token window. It answers the same three typed questions Jev does: `noul` (yes or no), `choice` and `score`. Its comparison table puts a routed set of Laya checkpoints against TypeSafe's Jev 1.13.0. These are Convai's numbers:

- **Accuracy:** 0.766 against 0.727 on their typed-decisions benchmark, 0.950 against 0.910 on AG News, 0.595 against 0.480 on DAIR Emotion.
- **Calibration:** expected calibration error (ECE) of 0.081 against 0.246.
- **Latency:** 32.8 ms for one question on a Tesla T4, against 236 to 276 ms for Jev.
- **Cost:** $0 self-hosted, against $0.042 per million tokens.

The card is more careful than most of the coverage it got. It says the 0.766 "belongs to the checkpoint fine-tuned on that benchmark's own training split", and that base checkpoints score 0.362 there zero-shot. It says the 0.081 ECE comes from refitting a temperature per question type and option count on its own data, which moved the English checkpoint's mean ECE from 0.466. It says the Jev figures are "third-party published, never measured here". And it reports a loss: 0.425 on Banking77 against Jev's 0.870.

So the launch claims mostly describe a Laya you've tuned. We tested the base English checkpoint untuned, then tuned it ourselves.

## The harness: one variable

The harness is the [Jev-Flywheel repo](https://github.com/AnthusAI/Jev-Flywheel). The harness asks an engine a short list of typed questions about each text. A small logistic model on top, the decision head, turns the answers into a verdict and a calibrated confidence. When a person disagrees with a verdict, the engine isn't retrained. Either the head is refit, which changes how much each answer counts, or an AI analyst reads the disagreements and proposes a new question, which we call a steering round. A person approves the change. It's a small data flywheel with a human in the loop, and [Fine-Tuning Jev](/blog/fine-tuning-jev/) walks through it step by step.

The test corpus is the 8,801-item sentiment set from [our fine-tuning article a year ago](/blog/fine-tuned-classification-with-confidence/), which we built with a planted bias: sports text skews positive and workplace text skews negative. In the recorded run, the analyst read 43 disagreements, named that bias in plain English, and proposed one question, `topic_domain`. Nothing above the engine adapter knows which engine answered, which is what makes a paired test possible.

## Jev vs Laya on the same 140 labels

We replayed one recording against both engines: the same 140 labels, the same 21 refit points, the same analyst proposal, the same 600 held-out items.

| | Jev | Laya |
|---|---|---|
| Engine alone | 0.768 | 0.722 |
| Last refit the gate promoted | 0.765 (87 labels) | 0.730 (52 labels) |
| After steering, 140 labels | **0.870** | **0.802** |
| Calibration error (ECE), alone to after | 0.151 to 0.030 | 0.107 to 0.015 |
| Brier score, alone to after | 0.188 to 0.093 | 0.189 to 0.130 |

Laya is free to run, so we also scored it on all 3,521 held-out items: 0.716 alone, 0.806 with the layer.

Laya gains 8 points from the layer. But the engines were 4.7 points apart alone and they're 6.8 apart with it. The layer lifts both and leaves the weaker one behind.

Calibration needs care in both directions. We measured Jev's raw over-confidence in [Can You Trust Jev's Confidence?](/blog/can-you-trust-jev-confidence/), and it shows up again here as an ECE of 0.151. Laya's raw ECE on the same items was lower, 0.107. We haven't measured how much ECE moves by chance on 600 items, so we don't read that as a win for Laya. After the layer, both are well calibrated. Brier is the clearer split and it favours Jev, because Brier rewards being confident and right, and Jev is right more often.

![Two bar panels. Calibration error falls from 0.151 to 0.030 for Jev and from 0.107 to 0.015 for Laya. Brier score falls from 0.188 to 0.093 for Jev and from 0.189 to 0.130 for Laya.](images/jev-vs-laya-calibration-brier.png)

*Calibration error (left) and Brier score (right) for the same four systems; lower is better. Both engines end well calibrated. Brier separates them, because it also pays for being right.*

## Two predictions we got wrong

We wrote predictions into [`studies/PREREGISTERED.md`](https://github.com/AnthusAI/Jev-Flywheel/blob/main/studies/PREREGISTERED.md) before Laya had answered a single item of this corpus. Two were wrong.

**We predicted the weaker engine would gain more.** Our reasoning: an encoder classifies topics well, so a topic factor would stand in for the sentiment judgement Laya does poorly. The steering step was worth +7.2 points to Laya and +10.5 to Jev. One caution: we measure each step from that lineage's last promoted refit, and the gate promoted those at different label counts (52 and 87), so the two steps don't start from the same place.

**We predicted Laya would be the over-confident one**, with raw ECE above 0.15. We took that from the card's raw 0.466. It came in at 0.107. The port ships Convai's calibration temperatures with the checkpoint, so that's the shipped model.

Two calls held: Laya with the topic question at 0.80 (it got 0.802), and a final gap of 4 to 8 points (6.8).

## What changes when Laya is the engine?

**Extra questions cost a forward pass each.** Jev reads the item once and every question rides in the same request, so another question costs a few input tokens. Laya encodes the item once per question. Our benchmark script, on an M1 Max, measured 18 ms for a single question, 75 to 82 ms for eight and 102 to 106 ms for twelve, across two runs. That's about 8 ms per added question. Both runs happened on a busy machine (1-minute load of 4.4 and 6.4, over our script's 2.0 threshold), so read them as upper bounds.

**The per-question answer cache is safe.** The cache assumes an answer doesn't depend on which other questions rode along. On Laya, 1,137 of 1,200 comparisons between a question asked alone and asked with seven others were identical, the largest difference was 0.005 in probability, and reordering changed nothing. Identical requests gave identical answers, within a process and across processes.

**The 512-token window truncates silently.** Our longest item is 48 tokens (`studies/laya_bench.json`), so this corpus never gets close. But Laya cuts overlong input without an error and returns an answer computed on part of it. Our adapter counts tokens first and refuses. Jev's [model page](https://docs.typesafe.ai/models) lists 64k tokens per request.

**Many-option questions are a known weakness.** Convai's card attributes the Banking77 result to a fixed token budget that all options share (192 tokens on the English model), which leaves 77 labels three or four tokens each. That's their explanation and their number. Our adapter refuses choice questions with more than 20 options.

**We ran a port.** [`laya-mlx`](https://pypi.org/project/laya-mlx/) is an independent Apple-silicon port, version 0.1.0, released September 19. It isn't an official Convai release.

## Does Laya's own loop find the hidden factor?

The paired replay transferred a question: the analyst wrote `topic_domain` after reading Jev's disagreements, and we asked Laya the result. So we also let the loop run on Laya alone, with no Jev answers anywhere. Labels grow from 140 to 300, 500 and 800, with a steering round after each, Kimi K3 as the analyst, three seeds. This wasn't pre-registered, and it uses more labels and rounds than the Jev runs, so the two aren't like for like.

| Held-out accuracy, 600 items | 0 labels | 140 | 300 | 500 | 800 |
|---|---|---|---|---|---|
| Seed 1 | 0.722 | 0.718 | 0.767 | 0.805 | **0.812** |
| Seed 2 | 0.722 | 0.718 | 0.737 | 0.833 | **0.818** |
| Seed 3 | 0.722 | 0.698 | 0.725 | 0.782 | **0.800** |

All three seeds proposed a question about what the text is about, somewhere between 140 and 500 labels. The plain one-round loop on Jev named that axis in 3 of 12 runs. Our guess is that the engine isn't the reason: by 300 to 500 labels there are more disagreements to read, and each round is the analyst's second or third look.

More rounds and 800 labels bought about what one transferred question bought at 140: 0.800 to 0.818, against 0.802. None of it gets near Jev's 0.870. And accuracy didn't rise monotonically. Seed 2 fell from 0.833 to 0.818 when the analyst reworded a question it had already added.

Running it exposed two faults in our own harness, both fixed. The analyst once wrote a 282-token instruction where Laya keeps 186, so the engine refused every request after the analyst call had been paid for; the host now checks proposals against the engine's limits. And we'd scored some earlier versions with a later round's wording, so now each version gets its own questions.

## Can you fine-tune Laya on the same 140 labels?

Yes, and on this corpus it beat the layer on accuracy. We didn't expect that.

The repo's [README](https://github.com/AnthusAI/Jev-Flywheel#readme) admitted we'd never compared the layer against gradient fine-tuning on the same label budget. With open weights there's no excuse, so we pre-registered the study and ran it. Four arms:

- **A: full fine-tune** of Laya on the recording's own labels.
- **B: head-only fine-tune** on that set: Laya's decision head, type embeddings and scorer train, and the ModernBERT encoder stays frozen. It's the closest gradient analogue to fitting a layer on top.
- **C: a learning curve**, the same recipe as A on random pool draws of 140, 300, 500, 800, 2,000 and 5,140 labels.
- **D: DistilBERT** on that set too, as the ordinary small-model baseline.

We fixed the recipe in [the pre-registration](https://github.com/AnthusAI/Jev-Flywheel/blob/main/studies/PREREGISTERED.md) first: AdamW, weight decay 0.01, batch 16, 6% warmup then linear decay, fp32, 10 epochs up to 500 labels and 3 above. Cross-validation inside the training labels picks the learning rate and fits one calibration temperature; held-out items never pick anything. Three seeds per arm. Each full fine-tune logged about 100 seconds of training on the M1 Max.

![Bar chart of held-out accuracy with the same 140 labels. Laya alone 0.722, Laya head-only fine-tune 0.659, Laya with the layer 0.802, DistilBERT fine-tune 0.835, Jev with the layer 0.870, Laya full fine-tune 0.896. Black lines show the range over three seeds.](images/jev-vs-laya-finetune.png)

*Six systems, the same 140 labels, the same 600 held-out items. Hatched bars are gradient fine-tunes (mean of three seeds, with the seed range as a black line). Magenta is Laya, blue is Jev, gray is DistilBERT.*

| System, same 140 labels | Accuracy (seed range) | ECE, calibrated | Brier |
|---|---|---|---|
| Laya alone | 0.722 | 0.107 (raw) | 0.189 |
| B: Laya, head-only fine-tune | 0.659 (0.620 to 0.712) | 0.061 | 0.182 |
| Laya with the layer | 0.802 | 0.015 | 0.130 |
| D: DistilBERT fine-tune | 0.835 (0.818 to 0.848) | 0.080 | 0.125 |
| Jev with the layer | 0.870 | 0.030 | 0.093 |
| A: Laya, full fine-tune | **0.896** (0.887 to 0.903) | 0.087 | 0.089 |

On all 3,521 held-out items arm A scores 0.902.

We predicted arm A would land between 0.74 and 0.80 with a seed spread of at least 3 points. It got 0.896 with a spread of 1.7. We predicted head-only would beat full fine-tuning at this budget, on the theory that 140 labels would overfit a 421M-parameter encoder. It did the opposite, and landed below untuned Laya. We'd written down one risk to those predictions: the planted cue is lexical, and a model with enough capacity might pick up a lexical shortcut from very few examples. That's the outcome we got, though we haven't tested that it's the mechanism.

Arm B needs a caveat, and it's our fault. We pre-registered a head-only learning-rate grid of 1e-4 and 1e-3, and both were probably too aggressive. The cross-validation scores said so before we touched a held-out item: 0.621 and 0.628, both below what Laya gets with no training. Read B as a badly tuned head, and don't read it as evidence that freezing the encoder fails. We reran it at gentler rates as an exploratory arm, outside the pre-registered tally, and they didn't rescue it. At 1e-5 the head barely moved and scored 0.722, exactly where untuned Laya sits. At 2e-5 it scored 0.679 (0.672 to 0.687), worse than no training. On this task the learning happens in the encoder, the part a head-only fine-tune leaves frozen.

### Which items does each system get right?

![Grouped bars of accuracy by difficulty tier. All four systems score 1.00 on strong. On medium: Jev with the layer 0.95, Laya with the layer 0.84, fine-tuned Laya 1.00, DistilBERT 0.71. On weak: 0.88, 0.79, 0.95, 0.87. On neutral: 0.72, 0.70, 0.67, 0.78.](images/jev-vs-laya-tiers.png)

*Accuracy by tier on the 600 held-out items (72 strong, 106 medium, 277 weak, 145 neutral). Fine-tuned Laya wins the two middle tiers. On neutral items it's last and DistilBERT is first.*

The corpus has four tiers. Every system solves the strong tier. Fine-tuned Laya carries the medium and weak tiers: 0.997 and 0.949, against 0.953 and 0.881 for Jev with the layer.

The neutral tier is where the planted bias lives. Its labels follow subject matter and its wording carries no sentiment, so it's the tier the analyst invented the topic question for. Fine-tuned Laya is the worst of the four systems there at 0.669, behind Laya with the layer (0.703) and Jev with the layer (0.724). DistilBERT is the best of any system on it at 0.782, and pays for that on the medium tier, where it drops to 0.714. These cells are small (145 neutral items, 106 medium), so gaps of a few points are noise. But the order holds on all 3,521 held-out items, where 800 are neutral: 0.644 for fine-tuned Laya, 0.679 for Laya with the layer, 0.720 for DistilBERT. We can't explain the DistilBERT pattern yet.

So the fine-tune's headline accuracy comes from the medium and weak tiers, 383 of the 600 items. Our guess, untested, is that gradient access lets the encoder learn this corpus's templates fast. On the items where the label depends on an unwritten convention, 140 labels of gradient descent did no better than asking the right question, and nominally worse.

### What more labels buy

![Line chart of fine-tuned Laya's held-out accuracy against number of random training labels: mean 0.884 at 140, 0.902 at 300, 0.926 at 500, with three seed dots per size. Dashed reference lines mark Laya with the layer at 0.802 and Jev with the layer at 0.870. Pending markers sit at 800, 2,000 and 5,140.](images/jev-vs-laya-learning-curve.png)

*Arm C so far: full fine-tunes on random pool draws, three seeds per size. The dashed lines are the two flywheel results at 140 labels. The three largest sizes were still training when we drew this.*

Arm C draws labels at random from the pool where arm A uses the 140 the active selector picked. At 140 random labels it scores 0.884 (0.862 to 0.895), at 300 it scores 0.902, and at 500 it scores 0.926. The first step is inside the seed spread; the second isn't. The neutral tier moves the same way: 0.623, 0.644, then 0.736 at 500, level with Jev with the layer.

We'd predicted the curve would first pass 0.802 at 300 to 500 labels and first pass 0.870 at 800 to 2,000. It passed the first at 140 in every seed, and the second at 140 on average and at 300 in every seed. We predicted about 0.93 at 5,140 labels, and it's nearly there at 500. At 800 labels it scored 0.928 (0.925 to 0.932 across seeds) and at 2,000 labels 0.938 (0.933 to 0.943), and at all 5,140 pool labels 0.942 (0.935 to 0.947), or 0.939 on the full 3,521 held-out items. That last prediction held: the curve flattens right around the 0.938 a plain text classifier reaches with every pool label. We also went the other way, as an exploratory run, to find where fine-tuning stops beating the layer. On Laya it doesn't: with 20 random labels the full fine-tune scored 0.819 (0.807 to 0.845), already above the 0.802 the layer reached on Laya with 140. It scored 0.833 at 40 labels and 0.852 at 80. It needs somewhere between 80 and 140 to pass the 0.870 of Jev with the layer. Those small runs reused a learning rate and skipped cross-validation, so they have no honest calibration, and twenty labels of a templated corpus with a lexical cue is about the friendliest case fine-tuning will ever get.

### What the layer keeps

**Calibration.** The layer's ECE is 0.015 on Laya and 0.030 on Jev. The fine-tune's is 0.087 after calibration, several times worse, and seeds range from 0.057 to 0.116. We don't trust that calibration step yet. The procedure fits one temperature per arm, and for arm A it came out at 5.07, which flattens confidences hard and made seed 1 worse than its raw output (0.116 against 0.108). [result pending] On Brier, which pays for accuracy, the fine-tune edges Jev with the layer, 0.089 to 0.093.

**A written account.** The layer's adaptation arrives as a question in English and a few coefficients. The fine-tune's arrives as 421M changed numbers.

**An engine you know didn't move.** A scorecard asks one checkpoint many questions. Fine-tune that checkpoint on the verdict and you've changed the weights under every other question too. A frozen engine can't drift, because nothing in it changed. So we asked fine-tuned Laya its other questions on the same 600 items and counted how often the top answer moved. Our pre-registered prediction was that more than 10% would. It was right, by a lot. Averaged over three seeds, the top answer changed on 9% of items for the irony question at the low end and on 75% for the intensity score at the high end. `topic_domain`, the question the flywheel added, changed on 35%. Across the eight questions we didn't train on, 42% of top answers moved. A changed answer isn't necessarily a worse one: we have no ground truth for those questions, so this measures movement and says nothing about damage. But after a fine-tune, every other score on that scorecard needs validating again. With a frozen engine, none of them do.

**The only lever Jev has.** TypeSafe's [model page](https://docs.typesafe.ai/models) says Jev "is not fine-tuned or LoRA-adapted with customer data".

There's one more comparison we care about for production: if you auto-accept every verdict above a confidence threshold, how much of the traffic clears a 95% accuracy bar? On the 600 test items, Jev alone could auto-accept 46% of its verdicts, and Jev with the layer 72%. Laya alone managed 34%, and Laya with the layer 55%. Fine-tuned Laya averaged 71%, but swung from 59% to 82% across its three training seeds, and DistilBERT averaged 63%. So the most accurate system isn't the one that knows best when it's right. How well confidence ranks right answers above wrong ones (AUROC) tells the same story: 0.853 for Jev with the layer, 0.839 for DistilBERT, 0.800 for fine-tuned Laya (0.760 to 0.851 by seed), 0.799 for Laya with the layer. This comparison is exploratory; we didn't pre-register it. Jev rounds its probabilities to two decimals, so the Jev-alone figure sits on a block of 277 tied items. And the fine-tuned models here were retrained for the probe, so their accuracies differ from the table above by up to a point.

### Where we deviated

Cross-validation used 3 folds where we'd pre-registered 5, to keep the run affordable on one GPU. Arm C chose its learning rate at 140 and 800 labels and reused it for the sizes in between. We killed the first attempt before it wrote a row, when MLX's buffer cache grew to about 24 GB on a 32 GB machine. It's all in the pre-registration file, beneath predictions we left unedited.

## What can you change with each engine?

| | Jev | Laya |
|---|---|---|
| Change what gets asked (add, reword, retire questions; sharpen criteria) | Yes | Yes |
| Fit your own decision head over the answers, and calibrate it | Yes | Yes |
| [Distill the aligned system](/blog/distilling-jev-into-a-classifier/) into a small classifier you own | Yes | Yes |
| Change the engine's weights | No: TypeSafe serves the same weights to everyone | Yes: Apache 2.0, open weights |

## Which should you use?

| Your situation | Use | Why |
|---|---|---|
| No labels yet | Jev | 0.768 against 0.722 zero-shot here; Convai's card says its base checkpoints need fine-tuning on its own benchmark |
| No hardware you want to run a model on | Jev | It's hosted; Laya is yours to serve |
| Inputs past 512 tokens | Jev | Laya truncates silently; Jev lists 64k per request. We tested no long documents on either |
| Questions with dozens of options | Jev | Convai's Banking77 result, 0.425 against 0.870 |
| You want the feedback layer | Either | It calibrated both and lifted both |
| Volume, data locality, or spare hardware | Laya | Free and local, starting several points behind |
| A few hundred labels, and accuracy matters more than explanation | Fine-tune Laya | 0.896 here at 140 labels; redo calibration afterwards and check the other questions for drift |

One option we haven't tried sits between the last two rows: fine-tune Laya only on the observation questions, like `topic_domain`, and keep the verdict in the readable head. That might buy sharper evidence without giving up the written account. It's future work.

## What we haven't tested

- **One constructed corpus.** It's templated, and the bias we planted sits in the vocabulary, which flatters any text classifier. The fine-tuning result is the one most exposed to this. A real feedback set's conventions won't be that easy to read off the words.
- **One paired run.** 600 held-out items is about ±1.4 points at one standard error. The 6.8-point gap clears that. The ECE difference between raw engines may not.
- **An unofficial port of one checkpoint.** We ran `aac6fef/laya-mlx`, an FP16 conversion of the base English `convaiinnovations/laya`. We didn't test the multilingual checkpoint, the one Convai fine-tuned on its own benchmark, or Convai's own reference implementation.
- **No long documents.** Every item here is a sentence.
- **A scripted labeler.** It answers with the corpus's reference label. We haven't tested whether a person's comments steer the loop better.
- **Latency on a quiet machine.** Ours was busy both times.

To replay the paired comparison on Apple silicon:

```bash
git clone https://github.com/AnthusAI/Jev-Flywheel && cd Jev-Flywheel
make install
make laya    # downloads the 843 MB model; about 2 minutes after that
```

The last `paper-600` row for each engine should read 0.870 for Jev and 0.802 for Laya.

The repo is deliberately small: one score, a logistic head, no database. [Plexus](https://github.com/AnthusAI/Plexus) is the industrial version of the same loop, with reviewer workflows, vetted labels, audit trails and the MLOps around continuous learning from human feedback, and the [Anthus AI Solutions](https://anth.us) team builds and runs it. If you're weighing Jev against Laya for a real scorecard, get in touch.
