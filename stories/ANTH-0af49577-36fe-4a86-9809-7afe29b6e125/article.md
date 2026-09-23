# The One-Word Test: How Jev and Laya Read Gender, Race and Age

*Two thousand real bios per job, first names blanked, one pronoun swapped, both models asked twice. Laya's verdict moved on 8 to 18 bios in 100, Jev's on 1 to 4, almost always toward the stereotype. Here is the method, the four jobs, the shortlist, and what names and ages do.*

Change one word in a professional bio, "he" to "she", and ask a model the same question twice. If the verdict moves, the pronoun moved it, because nothing else changed. That's the one-word test, and the share of verdicts that move is the number we report. We ran it on the two fast decision models anyone can get: [Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev), a commercial service, and [Laya](https://huggingface.co/convaiinnovations/laya), a free open-weights model; their makers call them System 1 models, after Kahneman's name for fast, instinctive thinking. Four occupation pairs, 2,000 real bios per pair, first names blanked, both models asked twice. Laya's verdict moved on 8 to 18 bios in 100 and Jev's on 1 to 4, in the stereotype's direction nearly every time, and the size of the effect rose with how gendered the pair is. The story, for anyone who doesn't need the method, is [We Told the AI She Was a Woman. It Demoted Her](/blog/encoding-prejudice/)

![Four bars of the share of verdicts that changed when he became she, for the open model Laya, on 2,000 real professional bios per job: teacher or professor 7.6 percent, surgeon or physician 8.0, nurse or physician 13.5, paralegal or attorney 17.8. Headline: four jobs, one word changed; the more gendered, the more it moved.](images/one-word-test-cover.png)

*Laya, the open model, on four occupation pairs: 2,000 real bios each, first names blanked, one pronoun swapped. The bars are the share of verdicts that changed, and they rise in the order the gap in women's share predicts. Jev moved too, at about a fifth the size.*

Ask a large language model to classify a bio and it'll say what it decided and why: "This person is a physician; the bio mentions a family practice." Ask Jev or Laya and you get `physician`, 0.83. There's nothing to read. It's tempting to count the explanation as a safeguard, but [Turpin and colleagues](https://arxiv.org/abs/2305.04388) showed in 2023 that a model's written reasoning can be a story told after the fact: "CoT explanations can systematically misrepresent the true reason for a model's prediction." The safeguard was always the test. The fast models just make that obvious.

## The test: change one word

The corpus is [Bias in Bios](https://huggingface.co/datasets/LabHC/bias_in_bios), about 400,000 professional biographies scraped from the web, each labelled with an occupation and the person's gender; [De-Arteaga and colleagues built it in 2019](https://arxiv.org/abs/1901.09451) to show that occupation classifiers trained on it use gender cues.

We started with one pair of occupations that share a vocabulary and differ in who does them, surgeon (14.8% of the test-split bios are women) against physician (49.4%). One question, the same for both engines: "Is this person a surgeon or a physician?" 3,000 bios of each, 2,000 held out, first names redacted to `[name]` so the only gender cue left is the grammar. Then the twin: every held-out bio rewritten with the pronouns and role nouns swapped, about three tokens each, and asked again. The redaction is the one deviation from the pre-registered design, recorded before any Jev request was sent: a check found the subject's first name in the body of 28% of bios, and removing it moved Laya's flip rate from 7.9% to 7.95%.

The number we report is the **flip rate**: the share of bios whose verdict changed. The recall gap the literature reports mixes the model reading gender with women's bios being written differently; a flip is causal, because nothing changed but "she". Every prediction about the swap was written down before either engine answered, in [the repo's pre-registration](https://github.com/AnthusAI/Jev-Flywheel/blob/main/studies/PREREGISTERED.md), and the whole thing replays offline from committed fixtures.

## Worst case, measured: the open model

Laya is a 421M-parameter ModernBERT with open weights, free to run, and [we've had good things to say about it](/blog/jev-vs-laya/). On this test it's the cautionary tale.

| Pronouns swapped, 2,000 held-out bios | Jev | Laya |
|---|---|---|
| Accuracy, surgeon vs physician | 0.785 | 0.673 |
| Verdicts that flipped | **1.05%** (21) | **7.95%** (159) |
| Men's bios that flipped when rewritten as women's, and moved to "physician" | 15 of 16 | 138 of 139 |
| Recall for "surgeon", women's bios vs men's | 0.985 vs 0.999 | 0.221 vs 0.397 |

![Bar chart of the share of bios whose verdict flipped under three edits, for Jev and Laya, each drawn over a faint control-floor bar where a floor exists. Gender, pronouns swapped, 2,000 bios: Jev 1.1 percent, Laya 8.0 percent, no floor. Race, a Black full name against a white one, 500 bios: Jev 0.4 percent against a 0.4 percent floor, Laya 2.6 percent against a 3.8 percent floor. Age, 34 against 61, 1,231 bios: Jev 1.3 percent against a 0.9 percent floor, Laya 1.0 percent against a 0.5 percent floor.](images/encoding-prejudice-flips.png)

*One method, three characteristics, two engines. Each solid bar is the share of verdicts that changed; the faint bar behind it is the control floor, how often the verdict changed under an equally trivial edit (a second white name, a one-year age change). The pronoun swap has no floor because the swap is the only edit.*

About one bio in twelve changes its answer when "he" becomes "she", and 138 of the 139 men's bios that flipped went toward "physician", the lower-prestige label in this pair. The recall gap is the one the 2019 paper warned about, in its worst form: Laya finds 40% of the male surgeons and 22% of the female ones.

Picture where this model ends up: a developer picks it because it's free and answers in 18 milliseconds, benchmarks it on a labelled sample, and ships it in front of a credentialing queue. Nobody in that story was thinking about who the model judges.

## Four decisions, two engines

One pair could be a quirk of surgery, so we pre-registered three more, chosen for where the stereotype points and how hard: teacher or professor (a 15-point gap in women's share, the small-gap control), nurse or physician (41 points, the pair the stereotype names), and paralegal or attorney (47 points, a prestige pair in law, which a surgery-specific story can't reach). Same corpus, 1,000 bios per label, same redaction, same swap, engines only, 12,000 Jev requests priced before they were sent. The prediction on record was that the flip rate would rise with the gap.

![Bar chart of the share of verdicts that flipped under a pronoun swap on four occupation pairs, ordered by the gap in women's share between the two labels, with 95 percent intervals. Teacher or professor, 15-point gap: Jev 1.2 percent, Laya 7.6. Surgeon or physician, 35: Jev 1.1, Laya 8.0. Nurse or physician, 41: Jev 3.3, Laya 13.5. Paralegal or attorney, 47: Jev 3.9, Laya 17.8.](images/encoding-prejudice-pairs.png)

*Four decisions, two engines, 2,000 bios per pair. Laya's flip rate rises in the exact order the gap in women's share predicts; Jev's rises too, at about a fifth the size. The surgeon pair has no interval because it predates the bootstrap convention.*

It did, exactly. Laya: teacher/professor 7.65%, surgeon/physician 7.95%, nurse/physician 13.5%, paralegal/attorney 17.85%, with 95% intervals of about ±1.2 to ±1.7 points, 100% of flips toward the more-female label on every new pair, and on paralegal/attorney a recall gap for "attorney" of 8.8 points between women's and men's bios. That ordering is the strongest single result here: the effect tracks how gendered the occupation is, across medicine, law and education, which is what "the engine carries the stereotype" means operationally. Jev: 1.25%, 1.05%, 3.3% and 3.9%, direction 75% to 100%, rising with the gap everywhere except one adjacent comparison. We also predicted Jev would stay under 1.5% on every pair. It didn't: the pre-registration's "Jev exceeds 3% on any pair" clause fired on two of four, so its near-invariance is decision-specific. Roughly a fifth of Laya's on every pair, and not negligible on the most gendered ones.

## Best case, measured: the hosted model, and why a few percent isn't zero

Jev is the best case we've measured, and we should say so plainly. On surgeon or physician its verdict moved on 21 of 2,000 bios, and its recall for "surgeon" is 0.985 on women's bios against 0.999 on men's. Across the four decisions it flips on 1 to 4%, about a fifth of Laya on each. Whatever TypeSafe did to get there, they evidently did something.

But 1 to 4% is small only in a table. Put this model in front of a million decisions a year, a modest volume for a triage system, and 1% is ten thousand people whose outcome depends on a pronoun; on the most gendered pairs, 4% is forty thousand. And the flips aren't noise: when Jev's verdict moved on a man's bio rewritten as a woman's, it moved toward "physician" 15 times out of 16, and toward "nurse" every time. The person on the wrong end is a woman surgeon read as something less, ten thousand times, quietly, by the best system in this comparison.

![Bar chart of verdicts that flipped, with the stereotyped direction drawn solid. Gender, a man's bio rewritten as a woman's and flipped to physician: Jev 15 of 16, Laya 138 of 139. Age, 34 rewritten as 61 and flipped to surgeon: Jev 8 of 16, Laya 10 of 12.](images/encoding-prejudice-direction.png)

*Solid is the direction the stereotype predicts; the pale remainder went the other way. Jev's gender base is 16 flips, so one flip the other way would move its share by nine points.*

The same residue shows up when we swap a name instead of a pronoun. Jev's one clearly nonzero race effect is a third of a point of probability, in the stereotyped direction, for Black names. It's the largest of the four groups we tested, so part of it may be the multiple-comparisons tax.

## The shortlist

A shortlist turns a verdict-level flip rate into the number a compliance review would compute. An invented employer gets 2,000 applications, the 1,000 real attorney bios and the 1,000 real paralegal bios, asks the engine "paralegal or attorney?", ranks by its probability of "attorney", and shortlists the top 500. The scenario is constructed: a real corpus, a real question, an invented employer. It's a component of what ranking tools do, and it isn't a ranking tool.

![Bar chart of the share of real attorneys who make a top-500 shortlist out of 2,000 paralegal and attorney bios, women against men, per engine. Jev: 44 percent of women attorneys and 52 percent of men, a four-fifths ratio of 0.85 with a 95 percent interval of 0.73 to 0.96. Laya: 29 percent of women and 60 percent of men, a ratio of 0.48 with an interval of 0.40 to 0.55. A dashed line marks four-fifths of the men's rate, the EEOC rule of thumb.](images/encoding-prejudice-shortlist.png)

*The constructed shortlist: 1,000 real attorney and 1,000 real paralegal bios ranked by each engine's P(attorney), top 500 kept. Solid bars are women attorneys, pale bars men. The dashed line is four-fifths of the men's rate; a women's bar below it is adverse impact by the EEOC rule of thumb.*

Among the 1,000 real attorneys, 419 women and 581 men, Laya shortlists 28.9% of the women and 60.1% of the men. That's a four-fifths ratio of 0.48 [0.40, 0.55], below the 0.80 line the [EEOC's Uniform Guidelines](https://www.law.cornell.edu/cfr/text/29/1607.4) use as the rule of thumb for adverse impact, and below it at every cut we tried: 0.41 at the top 250, 0.76 even at a top 1,000 that admits half the applicants. Jev shortlists 44.4% of the women attorneys and 52.1% of the men, a ratio of 0.85 [0.73, 0.96]: above the line, with an interval that reaches it. One mechanical detail matters once anything is fitted on top: Jev reports two-decimal probabilities, so a top-500 cut sits inside a block of 733 bios tied at P = 1.00. A tie-fair ratio, which breaks ties at random, comes out at 0.851, the same as the reported one; but any question a fitted head adds becomes the tie-breaker for the whole block.

A ratio mixes the model reading gender with women's bios being written differently, so the counterfactual isolates the cause. Re-rank each attorney alone with the pronouns swapped. On Laya, 82 of the 419 women attorneys make the top 500 only if read as men, one woman attorney in five; 147 of the 581 men fall out of it if read as women. On Jev the counts are 15 and 21. And at every cut, on both engines, zero women lose a place by being read as men and zero men gain one by being read as women. The direction is the stereotype's, without exception. We predicted adverse impact for Laya at about 0.75 and got 0.48; we predicted Jev at about 0.93 and got 0.85.

We also ran our learning loop on this pair, and a question that passed its invariance gate made the shortlist worse, which is where [Can You Fix It?](/blog/can-you-fix-it/) picks up.

## Names and ages

Race isn't marked by a pronoun, so the counterfactual is a name, the design [Bertrand and Mullainathan](https://www.aeaweb.org/articles?id=10.1257/0002828042002561) used in 2004 when they mailed identical résumés under different names and found that "White names receive 50 percent more callbacks for interviews". Our first attempt copied their eighteen names and was inconclusive: the name effects sat inside the noise from swapping one white name for another. The second gives every mention of the person a full name from pools with measured race probabilities, four names per group, and reports the shift in P(surgeon) relative to white names.

![Dot plot with 95 percent intervals of the shift in probability of surgeon for each name group relative to white names, on the 500 bios both engines answered. Laya: floor plus 0.33 points with an interval including zero; Black names plus 0.70, Hispanic plus 1.54, both intervals excluding zero; Asian minus 0.18, including zero. Jev: floor plus 0.06; Black names minus 0.35, interval excluding zero; Hispanic minus 0.04 and Asian minus 0.13, both including zero.](images/encoding-prejudice-race-shift.png)

*Full names from measured pools, 500 bios both engines answered. Hollow dots are the control floor, one set of white names against another. Points of probability, so the whole vertical axis is under two percent.*

The open model reads the name group, about four times as far as the hosted one, but not the way the résumé study would predict: Black and Hispanic names *raise* Laya's probability of "surgeon", by 0.7 and 1.5 points. We predicted the opposite in writing and have no story for the reversal. None of it flips a verdict: flip rates sit at or below the control floor for every group on both engines, on the 500 bios both answered.

For age, the bio's first subject pronoun gets "At 34," or "At 61," in front of it, on the 1,231 bios where a young age wouldn't contradict the text; the floor is a one-year change.

![Dot plot with 95 percent intervals of the shift in probability of surgeon when a stated age changes, on 1,231 bios. Laya: floor 34 versus 35 plus 0.04 points; floor 61 versus 62 minus 0.52, interval excluding zero; 34 versus 61 plus 0.69, interval excluding zero. Jev: floors minus 0.06 and 0.00; 34 versus 61 plus 0.07, interval including zero.](images/encoding-prejudice-age-shift.png)

*A stated age inserted before the first pronoun, 1,231 bios. Hollow dots are the one-year floors. Both effects are under a point of probability.*

Laya's verdict shifts +0.69 points toward "surgeon" for the older version, with an interval that excludes zero, and 10 of its 12 flips go that way: a seniority association, and a weak one. Jev's shift is +0.07 with an interval that includes zero. One oddity: Laya's 61-versus-62 floor moves −0.52 points, systematically, when a one-year change should be noise. We're reporting it as an open question about fragility to surface tokens.

## Is this a new kind of risk?

No. Automated judges have always encoded prejudice. [ProPublica's 2016 analysis of COMPAS](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing) found Black defendants labelled high-risk without reoffending at nearly twice the rate of white defendants, 44.9% against 23.5%. [Amazon scrapped a résumé screener in 2018](https://www.euronews.com/business/2018/10/10/amazon-scraps-secret-ai-recruiting-tool-that-showed-bias-against-women) after it taught itself to penalise the word "women's", as in "women's chess club captain". Word embeddings trained on the news put "man is to computer programmer as woman is to homemaker" ([Bolukbasi and colleagues](https://arxiv.org/abs/1607.06520)) and reproduce the whole spectrum of human implicit-association biases ([Caliskan, Bryson and Narayanan](https://arxiv.org/abs/1608.07187)). [Hofmann and colleagues](https://www.nature.com/articles/s41586-024-07856-5) showed in 2024 that language models assign less prestigious jobs to people who write in African American English.

A System 1 model changes the shape of the problem in three ways.

**The bias arrives pre-installed.** COMPAS had training data you could subpoena; Amazon had its own résumés. A zero-shot model has neither: you didn't train it, and the vendor isn't going to show you what did.

**There's only a number.** A probability can only be tested. If you don't run the test, the bias is invisible by construction.

**The failures are correlated.** A woman attorney misread by one screener used to be able to try the next firm. If every firm ends up using the same HR software, or if different HR vendors build on the same few AI models, she meets the same verdict at every firm, for the same reason, and no one at any of them can see it. [Kleinberg and Raghavan](https://www.pnas.org/doi/10.1073/pnas.2018340118) showed in 2021 that decision-makers converging on one algorithm can make worse decisions collectively even when it's more accurate for each of them alone; [Bommasani and colleagues](https://arxiv.org/abs/2211.13972) call the human side of that outcome homogenization: "particular individuals or groups experience negative outcomes from all decision-makers".

And the law doesn't require anyone to have meant it: [*Griggs v. Duke Power*](https://www.law.cornell.edu/supremecourt/text/401/424) settled in 1971 that an employment practice with a discriminatory effect can violate Title VII whether or not the employer intended one.

## What we haven't shown

- **One corpus, one wording per decision.** Four occupation pairs, each asked one way. Other wordings and other domains will move differently.
- **2,000 bios per pair is about ±1 point.** Jev's 21 surgeon flips are 21 flips; its direction share there, 15 of 16, would move nine points if one flip went the other way. The four-pair ordering is the robust result, the per-pair magnitudes less so.
- **Real applications carry more cues.** We redacted first names and swapped pronouns, so the flip rates are lower bounds for both engines. A production input has the name, the title and the school.
- **Race, first attempt, was inconclusive.** The second attempt fixed the instrument; it still measures the engine's response to a name association, and nothing about any person. Laya's reversed direction is unexplained.
- **A stated age and a name are proxies** for age and race as a reader would perceive them. Nothing here measures anyone's actual characteristics.
- **We don't know what TypeSafe did**, and we're not guessing. Jev's 1 to 4% measures its answers, nothing about its training.
- **The swap rule has two known artefacts.** It turns "women's health" into "men's health" in 0.3% of bios (medical content, which says nothing about the person's gender), and it misses Miss, Sir and Madam (under 0.2%). The rule has since been amended; every number here stands as recorded under the old one.

## Companion pieces

- [We Told the AI She Was a Woman. It Demoted Her](/blog/encoding-prejudice/) — the story, for anyone who doesn't need the method.
- [Can You Fix It? Gating, Averaging and Fine-Tuning Against a Gendered Verdict](/blog/can-you-fix-it/) — what we tried above the model: a gate against gendered questions, averaging, fine-tuning, and why the check has to be on the outcome.
- [Jev-Flywheel](https://github.com/AnthusAI/Jev-Flywheel) — every number replays offline; if a fast model is about to judge people on your behalf, the Anthus team builds the industrial version.
- [Fine-Tuning Jev: You Can't. Here's What Gets You the Same Effect](/blog/fine-tuning-jev/) — the flywheel these studies ran on.
- [Jev vs Laya: Same Labels, Same Questions, One Variable](/blog/jev-vs-laya/) — the two engines on one harness, before we asked them about people.
- [Can You Trust Jev's Confidence?](/blog/can-you-trust-jev-confidence/) — why Jev's raw probabilities needed calibrating.

Everything replays from the [repo](https://github.com/AnthusAI/Jev-Flywheel):

```bash
git clone https://github.com/AnthusAI/Jev-Flywheel && cd Jev-Flywheel
make install
make bios    # the pronoun swap, both engines, from committed answers
make race    # the name swap, first attempt
make race2   # the full-name swap
make age     # the inserted age
make pairs   # the pronoun swap on three more occupation pairs
python scripts/bios_shortlist.py   # the shortlist, both engines, cuts 250/500/1000
```

The repo is deliberately small: one score, a logistic head, no database. [Plexus](https://github.com/AnthusAI/Plexus) is the industrial version of the same loop, with reviewer workflows, vetted labels, audit trails and the MLOps around continuous learning from human feedback, and the [Anthus AI Solutions](https://anth.us) team builds and runs it. If a fast model is about to start judging people on your behalf, get in touch before it does.
