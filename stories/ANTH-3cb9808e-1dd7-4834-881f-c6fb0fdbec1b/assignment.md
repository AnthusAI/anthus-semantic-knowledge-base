# Assignment

Write the fourth piece in the Jev series: "Encoding Prejudice: System 1 Models and the Biases Nobody Measures". 1,800 to 2,800 words, Anthus "we", for engineers and product owners putting a fast decision model in front of anything that judges people.

Must:

- State the gender result in the first two paragraphs: one question, 2,000 real bios, pronouns swapped, flip rate for each engine and the direction of the flips.
- Put both engines through the identical test and report them side by side. Laya is the worst case measured, Jev the best; say both plainly, and say that a 1% flip rate at volume is still thousands of people, with the direction it goes.
- Report race (the inconclusive first attempt and the full-name second attempt, with floors and intervals) and age, including the results that went against the pre-registered predictions and the one unexplained floor.
- Contrast a System 1 model with an LLM classifier that explains itself, and cite Turpin et al. 2023 on why the explanation was never the safeguard.
- Say this is an old problem (COMPAS, the Amazon screener, word embeddings, dialect prejudice) and name the three things that change: pre-installed bias with no training set to audit, a probability instead of a rationale, and correlated failure across every deployer of the same weights (Kleinberg and Raghavan; Bommasani et al.). Note that disparate-impact law does not require intent.
- Make Kahneman the spine: substitution and WYSIATI for why adopters don't notice; decision hygiene from *Noise* mapped onto the flywheel's parts (delay the holistic verdict, decompose, independence, a fitted rule, calibration, a standing invariance test). Cite the two interviews and the primary literature (Dawes 1979, Bertrand and Mullainathan 2004, De-Arteaga et al. 2019, BBQ).
- End with three concrete asks, a "What we haven't shown" section, the `make` targets, and one short Plexus paragraph.
- Link "Can You Trust Jev's Confidence?" where calibration first appears, and the sibling articles where the flywheel and fine-tuning come up.

Must not:

- Guess at what TypeSafe did to Jev, or characterise either vendor beyond the numbers.
- State mitigation-arm results before they exist; mark them pending.
- Speculate on why Laya's race direction reversed.
- Cite anything not opened; cite no quote aggregators. One short quote per source at most.
- Invent a number. Anything unsourced is cut or marked pending.
