# Idea

## Pitch

System 1 decision models are being put in front of other processing as the gatekeeper: Jev,
Laya and whatever follows them answer a typed question in tens of milliseconds with a
probability and no chain of thought. The pitch is speed and cost. Nobody in the launch
coverage asks who the model judges differently.

We now have a measurement. In the Jev-Flywheel repo we asked both engines one question about
2,000 real professional biographies from the Bias in Bios corpus ("Is this person a surgeon or
a physician?"), then asked again with the pronouns swapped, with a name inserted, or with an
age inserted, and counted how often the verdict changed. The pre-registration, predictions and
outcomes are in `studies/PREREGISTERED.md`. The gender result is the spine: the open model's
verdict moves on about one bio in twelve when only the pronouns change, and every one of those
moves goes the stereotyped way; the hosted model moves on about one in a hundred, and nearly
all of those go the same way. Race (a full-name swap, second attempt after an inconclusive
first) and age (a stated age) are smaller and mixed in direction. The mitigation arms (the
flywheel's invariance gate, and fine-tuning on the same labels) are still running.

The argument is Kahneman's. Bias in an automated judge is an old problem (COMPAS, the Amazon
screener, word embeddings), but a System 1 model changes three things: the bias arrives
pre-installed with no training set to audit, there is nothing to read but a number, and every
institution that rents the same model fails the same people. The developer adopting it is
making a System 1 judgment too: substituting "does it classify well?" for "what does it
classify on?". The fix is decision hygiene, and the flywheel's parts map onto it one for one:
delay the holistic verdict, decompose into factual questions, keep the judgments independent,
prefer a fitted rule to unaided intuition, calibrate, and make failure detectable with a
standing counterfactual test.

## Audience

Engineers and product owners putting a fast decision model in front of anything that touches
people: hiring, support triage, claims, moderation. Also the people who approve that purchase.

## Working title

Encoding Prejudice: System 1 Models and the Biases Nobody Measures

## Sources

- Jev-Flywheel `studies/PREREGISTERED.md` (the four bios sections), `studies/bios_gender.jsonl`,
  `studies/bios_race.jsonl`, `studies/bios_race2.jsonl`, `studies/bios_age.jsonl`,
  `studies/bios_gender_proposals.jsonl`
- De-Arteaga et al. 2019 (Bias in Bios); Bertrand and Mullainathan 2004; Turpin et al. 2023;
  Kleinberg and Raghavan 2021; Bommasani et al. 2022; Hofmann et al. 2024; Parrish et al. 2022
- Kahneman: *Thinking, Fast and Slow*; *Noise*; the Yale SOM and Behavioral Scientist interviews
