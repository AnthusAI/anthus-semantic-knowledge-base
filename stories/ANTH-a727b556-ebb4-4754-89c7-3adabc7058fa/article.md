# Can You Fix It? Gating, Averaging and Fine-Tuning Against a Gendered Verdict

*The verdict moves on "she". We put a gate on the learning loop that rejects any question whose answer moves with the pronoun. It kept out the obvious ones and still made a hiring shortlist worse, because a question can ignore the pronoun and read gender from the words. Here's what worked instead.*

Change "he" to "she" in a lawyer's online bio and [Laya](https://huggingface.co/convaiinnovations/laya), the open model, reads the attorney as a paralegal on 17.8% of two thousand real bios; [Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev), the hosted model, on 3.9%. Rank the same bios by either model's confidence, keep the top 500, and women attorneys make the cut at 0.48 of the men's rate on Laya and 0.85 on Jev. [The One-Word Test](/blog/one-word-test/) is the measurement; the question here is what a deployer can do about it above the model. The learning loop that raises accuracy leaves the bias alone. A gate that rejects any question whose answer moves with the pronoun keeps out the obvious ones, and where the harm is largest it made the shortlist worse. Averaging each bio with its pronoun-swapped twin is the cheapest thing that helped. Fine-tuning, which we predicted would make things worse, didn't. The loop's machinery is in [Fine-Tuning Jev](/blog/fine-tuning-jev/).

The result that changed how we think about the gate fits in three bars. Jev alone shortlists women attorneys at 0.85 of the men's rate, above the four-fifths line. The gated loop added one question about support roles whose answer barely moved under the pronoun swap, and the ratio fell to 0.66. Remove it and it's 0.85 again.

![Three bars of the four-fifths ratio on a top-500 shortlist from 2,000 paralegal and attorney bios, on Jev. The engine alone, 0.85; the gated learning loop with one question added, 0.66; the same question removed, 0.85. A dashed line marks the four-fifths threshold at 0.8. The title reads: the gate passed the question, the shortlist got worse.](images/can-you-fix-it-cover.png)

*One question, added by the learning loop after it passed the invariance gate. The ratio is women attorneys' shortlist rate over men's; the dashed line is the four-fifths rule of thumb.*

## The invariance gate

The loop proposes questions: an analyst reads the labelled mistakes and proposes a factual question about the text, which joins the scorecard if the fit improves out of fold. The gate adds one rule: the question is promoted only if it also flips on no more than 2% of the labelled items when their pronouns are swapped. Questions that mention gender, pronouns or sex are rejected by construction, so the fix can't be "ask about gender and correct for it".

We wrote down four outcomes first. If the ungated loop halved the flip rate on its own, the gate would be redundant. If the gated loop couldn't find one question that passed, the engine reads gender in everything it says and the fix has to happen in the model. If fine-tuning's flip rate fell, gender-correlated labels didn't teach the correlation. And if Jev flipped on under 1% of bios, "Jev reads gender" wouldn't be supported at all. That last one didn't fire; Jev flipped on 1.05%.

## The loop on the surgeon pair, with and without the gate

The arms ran on surgeon or physician: 3,000 bios of each, 2,000 held out, first names redacted, one question. Both engines, three seeds each, with and without the gate.

The loop did what it's built for. On Laya, accuracy went from 0.673 to 0.733–0.757, a 6 to 8 point gain, with calibration intact; on Jev, from 0.785 to about 0.805. It did not reduce the gender flip rate as a side effect, and on Laya it made it worse: 8.4%, 10.85% and 19.05% across the seeds, the last one after promoting two plain evidence questions. The loop optimises agreement with the labels; in this corpus gender correlates with the label (14% of surgeons are women, 49% of physicians), so a gender-sensitive answer is a *useful* feature to the fit. Optimising for the labels reproduces the base-rate correlation.

The gate did what it's built for too, and it wasn't enough. On Laya it rejected every proposal in every seed: the two genuinely gendered ones, courtesy-title questions that flipped on 24% and 14% of the labelled twins, and also the plain evidence questions, because on Laya even "does the text state that this person performs surgery?" changes its answer on 4 to 7% of bios when the pronouns change. Every gated seed fell back to a refit of the holistic answer and landed at 10 to 11% flips. That's the second of the outcomes we pre-registered: the engine reads gender in everything it says about a bio, the layer can't refuse what every answer carries, and mitigation has to happen upstream, in the model. On Jev the same evidence questions flip on 0 to 1.6% of twins and pass, and the gated loop ended better on both axes, accuracy up two points and flips roughly halved on two of three seeds. On those two seeds nothing was promoted, though; the improvement came from the refit moving the threshold into a sparser part of Jev's score distribution.

| Arm, surgeon or physician, 2,000 held-out bios | Engine | Accuracy | ECE | Flip rate | Recall gap, women − men |
|---|---|---:|---:|---:|---:|
| J0, the engine alone | Jev | 0.785 | 0.165 | 1.05% | −1.4 pts |
| J1, the loop, mean of 3 seeds | Jev | 0.800 | 0.049 | 1.73% | −0.7 pts |
| J2, the loop with the gate, mean of 3 | Jev | 0.806 | 0.066 | 0.80% | +1.8 pts |
| L0, the engine alone | Laya | 0.673 | 0.191 | 7.95% | −17.2 pts |
| L1, the loop, mean of 3 | Laya | 0.741 | 0.036 | 12.8% | −18.2 pts |
| L2, the loop with the gate, mean of 3 | Laya | 0.734 | 0.062 | 10.7% | −12.2 pts |
| LF, fine-tuned, 2 of 3 seeds | Laya | 0.802 | 0.056 | 1.58% | −4.1 pts |
| Twin averaging on L1's head, mean of 3 | Laya | 0.731 | 0.036 | 0.00% | −3.6 pts |

ECE is calibration error; the last column is recall for "surgeon" on women's bios minus men's.

The third Jev seed is the one the mean hides. Its gated loop promoted a question asking whether the person is "referred to with a courtesy title such as Ms., Miss, Mrs., Mr., or Mx. rather than Dr." It passed the gate at exactly 0.0% flips, because the swap changes pronouns, reflexives and a few role nouns and never touches "Mr." or "Ms." The answer is invariant to the swap by construction, and the feature it reads is as gendered as a pronoun. That seed flipped on 1.30%, above the raw engine's 1.05%. The ungated loop was worse: all three J1 seeds promoted a courtesy-title question, against a prediction of "at most 1 of 3 seeds". On Laya the same question failed the gate, at 24% and 14%, because Laya's own answer to it moves with the surrounding pronouns. Whether a title question gets through is an accident of how each engine answers it: sensitivity to the swap and correlation with gender come apart completely.

## The attorney pair: a question can pass the gate and still hurt

The shortlist: 2,000 applications, 1,000 real attorney bios and 1,000 real paralegal bios, ranked by each engine's probability of "attorney", top 500 kept. Among the 1,000 real attorneys, 419 women and 581 men, Laya shortlists 28.9% of the women and 60.1% of the men, a four-fifths ratio of 0.48; Jev shortlists 44.4% and 52.1%, a ratio of 0.85, above the 0.80 line the [EEOC's Uniform Guidelines](https://www.law.cornell.edu/cfr/text/29/1607.4) use for adverse impact. Swap the pronouns and 82 of the 419 women make Laya's top 500 only if read as men; on Jev, 15. One mechanical detail: Jev reports two-decimal probabilities, so 733 of the 2,000 applicants tie at P = 1.00, the cut sits inside that block, and any added question becomes the tie-breaker for all of them.

![Bar chart of the share of real attorneys who make a top-500 shortlist out of 2,000 paralegal and attorney bios, women against men, per engine. Jev: 44 percent of women attorneys and 52 percent of men, a four-fifths ratio of 0.85 with a 95 percent interval of 0.73 to 0.96. Laya: 29 percent of women and 60 percent of men, a ratio of 0.48 with an interval of 0.40 to 0.55. A dashed line marks four-fifths of the men's rate, the EEOC rule of thumb.](images/encoding-prejudice-shortlist.png)

*The constructed shortlist, engines alone: 1,000 real attorney and 1,000 real paralegal bios ranked by each engine's P(attorney), top 500 kept. Solid bars are women attorneys, pale bars men. The dashed line is four-fifths of the men's rate; a women's bar below it is adverse impact by the EEOC rule of thumb.*

We also ran our learning loop on this pair, because this is where the harm is, and it taught us something we hadn't planned to learn. On Jev, the loop proposed a "paralegal, legal assistant or current law student" question, checked that its answers barely move under the pronoun swap (0.8% of labelled twins), promoted it, and cut the verdict flip rate from 3.9% to under 2.2%. And the shortlist ratio fell from 0.85 to 0.66, below the line, with 37 women attorneys shortlisted only under male pronouns against 15 before. Remove that one question and the ratio is back at 0.85. Jev answers it higher for real women attorneys than for men, and swapping the pronouns doesn't change that: the question is invariant to the pronoun and correlated with gender through what the bios say. A question can pass a cue-invariance test and still worsen the outcome for the group, while the verdict-level bias number improves. Measure in the decision's own currency, and gate on the outcome, not the cue. On Laya none of the loop's eleven proposed questions passed the invariance test at all, so it changed nothing.

![Bar chart of the four-fifths ratio on the top-500 paralegal-or-attorney shortlist, Jev, three bars: engine alone 0.85, gated loop 0.66, question removed 0.85, with a dashed four-fifths line at 0.8 that only the middle bar falls under.](images/encoding-prejudice-gate-shortlist.png)

*The gated loop's question passed the invariance test and pulled the shortlist below the line; remove it and the ratio returns.*

Zero the new question's weight and the ratio returns to 0.85; reset the holistic weight instead, keeping the question, and it stays at 0.66. The content correlation does the damage: among real attorneys the question averages −3.29 log-odds on women's bios against −3.63 on men's, and on the swapped twins −3.38 against −3.53. The second gated seed promoted three questions in the same direction and landed at 0.65 with 1,224 distinct scores and three ties, so the tie doesn't explain it either.

One caveat: paralegal is the smallest class in the corpus, so the loop's pool had 146 paralegals against 2,000 attorneys, a 93% skew against a 50/50 held-out set. Every fitted arm's raw accuracy fell for that reason alone; the shortlist ratio depends only on the ranking and is the number we report.

## Twin averaging: the fix that needs no fitting

For a cue you can swap, there's a fallback that needs no fitting: score the bio and its twin and average the two probabilities. Zero pronoun flips by construction, twice the inference. In the shortlist's currency, averaging takes Laya from 0.48 to 0.79 on paralegal/attorney and Jev from 0.85 to 0.91, at a cost of 5.6 and 1.3 points of accuracy. So on that pair pronouns carry most of the adverse impact, and the residue is in what the bios say. On nurse/physician it overshoots to 1.25 in women's favour: with the pronoun neutralised, women physicians' bios read *more* physician-like to Laya than men's. And it does nothing for a cue you can't swap.

We tried four fitted-head versions on Laya: refit on the 140 labels and their twins, penalise the gap between a bio's probability and its twin's, gate every feature including the holistic answer, and steer the analyst with the flips. Three of the four land within about half a point of twin averaging's accuracy while flipping an order of magnitude more often, 11.3 to 11.4% against zero. Gating everything removes the only feature this scorecard has, since the holistic answer itself flips on 7.95%, and collapses to 0.50 accuracy. No fitted arm beats twin averaging on both axes.

## Fine-tuning: the prediction we got wrong

Fine-tuning is the other reflex: the model is biased, so retrain it on my labels. We predicted in writing that it would make things worse, because 140 labels with a 14%-versus-49% gender mix should teach the correlation. It didn't. Laya fine-tuned on the same 140 surgeon labels scores 0.80 and flips on 0.6% and 2.6% of twins across two seeds, against 7.95% untuned, and the recall gap shrinks from 17 points to 2 and 6. On this pair, gradient fine-tuning reduced the pronoun sensitivity more than anything the layer did, and we're reporting that against our prediction. What it doesn't give you is any account of what it learned or what else it changed: when [we fine-tuned Laya on 140 sentiment labels](/blog/jev-vs-laya/), its answers to the eight questions it wasn't trained on moved by 42% on average. The flip rate is the only window into a fine-tuned model, which is one more reason to keep it as a standing test.

Two seeds, and we say so: the third's training was stopped to free the GPU, and no row is invented for it. The recipe is [Jev vs Laya](/blog/jev-vs-laya/)'s, unchanged.

## Why the people adopting these models don't measure this

The decision to adopt one of these models is itself a System 1 judgment, and it fails in two ways Kahneman named in *Thinking, Fast and Slow*. **Substitution**: when a question is hard, we quietly answer an easier one. "What does this model classify on?" is hard; "does it classify well?" has a benchmark, so the benchmark gets run and the first question goes unasked. **WYSIATI**, what you see is all there is: the hundred-odd women surgeons the open model reads as physicians aren't in the demo, the vendor's table, or the fifty items you spot-checked, and the mind doesn't register what's missing.

The usual answer is a line in a checklist: "consider the risks." Considering a bias doesn't remove it; you can know about a visual illusion and still see it. The control has to be structural.

## Decision hygiene, implemented

In *Noise*, Kahneman, Sibony and Sunstein call the structural controls decision hygiene: procedures that improve judgment without knowing which bias you're fighting, as hand-washing works without knowing which germ. Lay the list beside the [Jev-Flywheel](https://github.com/AnthusAI/Jev-Flywheel) and they match.

![Diagram in two columns. Left, Kahneman's decision hygiene: delay the intuitive conclusion; decompose into factual sub-judgments; keep the judgments independent; prefer a rule to unaided judgment; calibrate; make failure detectable. Right, the flywheel component for each: demote the v1 scorecard to one input; the analyst's proposed questions about surgical training, the operating room, and Mr. or Ms. rather than Dr.; typed questions answered separately; the fitted decision head, an improper linear model; calibration on a few hundred held-out labels; the invariance gate, the pronoun swap as a standing test.](images/encoding-prejudice-hygiene.png)

*Kahneman's decision-hygiene controls on the left, and the part of the flywheel that implements each one on the right.*

Five of the six were in the flywheel before this study. **Delay the intuitive conclusion**: the v1 scorecard, the engine's own answer as the verdict, *is* the intuition, and the first act of hygiene is to demote it to one input among several. **Decompose into factual sub-judgments**: the analyst proposes questions like "does the text state that this person performs surgery, works in the operating room, or is board certified in a surgical field?", all recorded in [the repo](https://github.com/AnthusAI/Jev-Flywheel/blob/main/studies/bios_gender_proposals.jsonl). **Keep the judgments independent**: each question gets its own probability, though Jev's claim to [evaluate them independently](https://docs.typesafe.ai/introduction) we've checked only for [sentiment](/blog/can-you-trust-jev-confidence/). **Prefer a rule to unaided judgment**: the verdict is a fitted logistic head, a few coefficients you can print, [Dawes's improper linear model](https://www.cmu.edu/dietrich/sds/docs/dawes/the-robust-beauty-of-improper-linear-models-in-decision-making.pdf). **Calibrate**: [a few hundred held-out labels get Jev there](/blog/can-you-trust-jev-confidence/).

**Make failure detectable.** This is the piece the bias problem adds, in two parts. The gate demonstrably kept out every question whose answer moved with the pronouns. It can't see a question whose answer doesn't move but still tracks gender, so the second part is the decision's own outcome, the shortlist ratio with the counterfactual counts beside it, recomputed on every promoted scorecard. Whether it helps depends on the engine, and you find out by measuring.

## Three things to do before you ship

1. **Run the counterfactual as a smoke test.** Take a few hundred items from your own data, swap the pronouns, swap the names, insert an age, and count the flips against a floor. `make bios`, `make race` and `make age` replay ours with no keys and no spend, and the scripts take a different corpus.

2. **Never let the holistic answer be the decision.** Put factual questions around it, fit a head you can read, calibrate it, and gate every new question on invariance, so the model's verdict is one input to a rule you own. Decompose, gate, and measure, in the decision's own currency: the gate keeps the obviously gendered questions out, and only the shortlist test catches the ones that aren't obvious.

3. **Demand the flip rate from your vendor.** They know it, or they can know it in a day. A model card that reports accuracy and latency and leaves this out is telling you which question its authors substituted. [BBQ](https://aclanthology.org/2022.findings-acl.165/), the standard bias benchmark for question answering, lets a model say "unknown"; a gatekeeper doesn't get that option, so it has to be measured on every answer.

## What we haven't shown

- **Two pairs, one wording each.** Surgeon or physician for the arms, paralegal or attorney for the shortlist; other wordings and decisions will move differently.
- **The labeler is the corpus.** We haven't tested whether human reviewers steer the loop the same way.
- **Fine-tuning is two seeds**, and the fitted-head arms ran on Laya only.
- **The gate is one threshold, fixed at 2% in advance.** Loosening it to 5% or 10% on Laya lets the holistic answer back in wherever its own flip rate clears the bar.
- **The attorney-pair loop couldn't be graded on accuracy**; the shortlist ranking is what we report.
- **The swap rule had two artefacts** in the surgeon-pair arms, "women's health" swapped in 0.3% of bios and Miss, Sir and Madam missed in under 0.2%; amended before the attorney-pair loop, it changed 2 of 2,000 twins.
- **We don't know what TypeSafe did**, and we're not guessing.
- **The nurse-pair loop was stopped partway** by the author; its rows are a record and nothing here rests on them.

## Companion pieces

- [We Told the AI She Was a Woman. It Demoted Her.](/blog/encoding-prejudice/) — the story, for anyone who doesn't need the method.
- [The One-Word Test: How Jev and Laya Read Gender, Race and Age](/blog/one-word-test/) — how we ran it: two models, four jobs, the shortlist, and what swapping a name or an age does.
- [Jev-Flywheel](https://github.com/AnthusAI/Jev-Flywheel) — every number replays offline; if a fast model is about to judge people on your behalf, the Anthus team builds the industrial version.
- [Fine-Tuning Jev: You Can't. Here's What Gets You the Same Effect](/blog/fine-tuning-jev/) — the analyst, the refit and the fitted head.
- [Jev vs Laya: Same Labels, Same Questions, One Variable](/blog/jev-vs-laya/) — the fine-tuning recipe and the 42% drift.
- [Can You Trust Jev's Confidence?](/blog/can-you-trust-jev-confidence/) — calibration on a few hundred held-out labels.

Everything replays from the repo:

```bash
git clone https://github.com/AnthusAI/Jev-Flywheel && cd Jev-Flywheel
make install
make bios      # the surgeon pair, both engines alone, from committed answers
make attorney  # the paralegal/attorney pair, both engines alone, amended twins
make flipopt   # twin averaging and the fitted-head arms at the 2% gate, no GPU
```

The repo is deliberately small: one score, a logistic head, no database. [Plexus](https://github.com/AnthusAI/Plexus) is the industrial version of the same loop, with reviewer workflows, vetted labels, audit trails and the MLOps around continuous learning from human feedback, and the [Anthus AI Solutions](https://anth.us) team builds and runs it. If a fast model is about to start judging people on your behalf, get in touch before it does.
