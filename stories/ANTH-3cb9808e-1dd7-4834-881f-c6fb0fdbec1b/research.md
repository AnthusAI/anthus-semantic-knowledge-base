# Research

Verify table. "PREREG" is `Jev-Flywheel/studies/PREREGISTERED.md` (the four bios sections at the end); other files are under `Jev-Flywheel/studies/`. Every URL was opened on 2026-09-22.

| # | Claim | Value | Source | Verified |
|---|---|---|---|---|
| 1 | Corpus: Bias in Bios, about 400,000 bios, occupation and gender labels, MIT licence | as stated | huggingface.co/datasets/LabHC/bias_in_bios; PREREG "The corpus" | yes |
| 2 | Original paper: classifiers use gender cues; removing explicit indicators reduces but does not remove the gap | as stated | arxiv.org/abs/1901.09451 (De-Arteaga et al. 2019) | yes |
| 3 | Pair: surgeon 14.8% women, physician 49.4%; 3,000 each; 2,000 held out; one choice question | as stated | PREREG "The corpus" | yes |
| 4 | Redaction of first names to `[name]`; swap touches about three tokens | 3.04 tokens mean | PREREG deviation 2 and gender Outcome | yes |
| 5 | Jev (J0, redacted): accuracy, flip rate, flips, direction, TPR gap | 0.785; 1.05%; 21; 15 of 16 male-origin toward physician; -1.4 pts (recall 0.985 women / 0.999 men) | `bios_gender.jsonl` J0 row; PREREG gender Outcome | yes |
| 6 | Laya (L0, redacted): same | 0.673; 7.95%; 159; 0.9928 of 139 male-origin flips = 138 of 139; -17.2 pts (recall 0.221 women, 136 bios / 0.397 men, 864 bios) | `bios_gender.jsonl` L0 redacted row; PREREG gender Outcome | yes (the brief's "137/137" and "0.391" are the pre-redaction/rounded figures; the file and PREREG are used) |
| 7 | Race v1: 1,571 bios; Jev floor 0.57% / race 0.89%; Laya 2.36% / 3.18%, direction 30%; intervals overlap; inconclusive | as stated | `bios_race.jsonl`; PREREG race Outcome | yes |
| 8 | Race v2 (500-bio subsample, both engines): Laya floor +0.33 [-0.07,+0.75]; Black +0.70 [+0.38,+1.02]; Hispanic +1.54 [+1.16,+1.90]; Asian -0.18 [-0.49,+0.13]. Jev floor +0.06; Black -0.35 [-0.50,-0.19]; Hispanic -0.04; Asian -0.13 (both include zero). Flip rates at or below floor for every group | as stated | `bios_race2.jsonl` sample "500" rows; PREREG race2 Outcome | yes |
| 9 | Laya's largest shift is 4.4x Jev's largest | 1.54 / 0.35 | computed from 8; PREREG | yes |
| 10 | Age: 1,231 eligible; Jev shift +0.07 [-0.08,+0.23], flips 1.30% vs floors 0.89/0.57; Laya +0.69 [+0.53,+0.87], flips 0.97% vs floors 0.49/0.65, 10 of 12 older->surgeon; Laya 61v62 floor -0.52 [-0.64,-0.39] | as stated | `bios_age.jsonl`; PREREG age Outcome | yes |
| 11 | Analyst proposals (L1 seed 1): three questions, wording quoted | as stated | `bios_gender_proposals.jsonl` | yes |
| 12 | Invariance gate: 2% threshold on labelled items under the swap; wired and specified against a scripted analyst | as stated | PREREG "Arms" and deviation 3 | yes |
| 13 | Fine-tuning Laya on 140 labels moved 42% of answers to untrained questions | 42% mean | README "What fine-tuning costs"; `finetune_laya_drift.jsonl` | yes |
| 14 | Laya: 421M, ModernBERT-large, Apache 2.0 | as stated | huggingface.co/convaiinnovations/laya | yes |
| 15 | Laya one-question latency 18 ms on a laptop | 18.0 ms | `laya_bench.json` (via the Jev vs Laya research table) | yes |
| 16 | TypeSafe quote "Unstructured state in, typed probabilistic decisions out." | verbatim | typesafe.ai/blog/introducing-system-one-models-and-jev | yes |
| 17 | Jev evaluates a request's questions independently | as stated | docs.typesafe.ai/introduction | yes |
| 18 | Turpin et al. 2023 quote | verbatim | arxiv.org/abs/2305.04388 | yes |
| 19 | Bertrand and Mullainathan 2004, AER, quote "White names receive 50 percent more callbacks for interviews" | verbatim | aeaweb.org/articles?id=10.1257/0002828042002561 | yes |
| 20 | ProPublica COMPAS 2016: 44.9% vs 23.5% | as stated | propublica.org "Machine Bias" | yes |
| 21 | Amazon résumé screener, 2018, penalised "women's" | as stated | Reuters (Dastin, 2018-10-10) | no: reuters.com could not be opened from this environment; link omitted, citation pending |
| 22 | Bolukbasi et al. 2016 analogy | as stated | arxiv.org/abs/1607.06520 | yes |
| 23 | Caliskan, Bryson, Narayanan 2017 (Science) | as stated | arxiv.org/abs/1608.07187 | yes |
| 24 | Hofmann et al. 2024, Nature 633: dialect prejudice, jobs and sentencing | as stated | nature.com/articles/s41586-024-07856-5 | yes |
| 25 | Kleinberg and Raghavan 2021, PNAS: monoculture can lower collective decision quality | as stated | pnas.org/doi/10.1073/pnas.2018340118 (abstract via arxiv.org/abs/2101.05853) | yes |
| 26 | Bommasani et al. 2022, outcome homogenization quote | verbatim | arxiv.org/abs/2211.13972 | yes |
| 27 | Griggs v. Duke Power 1971: discriminatory effect without intent | as stated | law.cornell.edu/supremecourt/text/401/424 | yes |
| 28 | Dawes 1979, improper linear models | as stated | cmu.edu (PDF of the paper) | yes |
| 29 | BBQ (Parrish et al. 2022) provides UNKNOWN answers | as stated | aclanthology.org/2022.findings-acl.165; arxiv PDF text | yes |
| 30 | Kahneman quote "Do not eliminate intuition but delay it." | verbatim | behavioralscientist.org interview, 2021 | yes |
| 31 | Kahneman at Yale: decision hygiene works on noise and bias without targeting either | as stated | som.yale.edu blog, 2021 | yes |
| 32 | Substitution, WYSIATI, the Israeli army interview | book references | *Thinking, Fast and Slow* | book, not opened online; no page numbers given |
| 33 | Decision hygiene | book reference | *Noise* (Kahneman, Sibony, Sunstein) | book, not opened online |
| 34 | L1 (loop, no gate), seeds 1-3: accuracy, flip rate, TPR gap | 0.7565 / 0.7345 / 0.7325; 8.4% / 10.85% / 19.05%; -9.9 / -12.1 / -32.5 pts | `bios_gender.jsonl` L1 rows | yes |
| 35 | L2 (loop + 2% gate), seeds 1-3: accuracy, flip rate | 0.736 / 0.7345 / 0.7315; 11.15% / 10.85% / 10.1% | `bios_gender.jsonl` L2 rows | yes |
| 36 | L2 proposals all rejected; flip rate on labelled twins: surgical evidence 3.6% / 5.7% / 7.1%; non-surgical 4.3%; dental 4.3%; courtesy title 24.3% / 13.6% | as stated | `bios_gender_proposals.jsonl` L2 rows (`flip_rate_on_labeled`, `passed_gate`) | yes |
| 37 | L1 seed 3 promoted two evidence questions | as stated | `bios_gender_proposals.jsonl` L1 seed 3 rows | yes |
| 38 | Twin-averaging baseline: accuracy, flips, TPR gap; 2x inference by construction | 0.7545; 0%; -2.9 pts | `bios_flipopt.jsonl` arm "baseline"; PREREG "optimising the head against the flip" | yes |
| 40 | Four pairs (gap in women's share): teacher/professor 15, surgeon/physician 35, nurse/physician 41, paralegal/attorney 47; 1,000 bios per label, engines only | as stated | PREREG "does the gender result hold on other decisions?" | yes |
| 41 | Laya flip rates 7.65% [6.55, 8.85] / 7.95% / 13.5% [12.05, 15.05] / 17.85% [16.15, 19.55]; direction 100% on the three new pairs (99.28% surgeon) | as stated | `bios_pairs.jsonl` | yes |
| 42 | Jev flip rates 1.25% [0.80, 1.75] / 1.05% / 3.3% [2.60, 4.10] / 3.9% [3.10, 4.85]; direction 75 / 93.75 / 100 / 93.75% | as stated | `bios_pairs.jsonl` | yes |
| 43 | Ordering prediction confirmed exactly for Laya; "Jev exceeds 3% on any pair" trigger fired on nurse and paralegal; Jev predicted at most 1.5% | as stated | PREREG pairs Outcome table | yes |
| 44 | 12,000 Jev requests, priced first | as stated | PREREG pairs Outcome; `bios_pairs_spend.md` | yes |
| 45 | Shortlist: 2,000 bios (1,000 attorney, 1,000 paralegal), ranked by P(attorney), top 500; 419 women / 581 men attorneys | as stated | PREREG "the shortlist"; `bios_shortlist.jsonl` | yes |
| 46 | Laya top 500: women 28.9%, men 60.1%, ratio 0.48 [0.40, 0.55]; top 250 ratio 0.41; top 1,000 0.76 | as stated | `bios_shortlist.jsonl` laya rows | yes |
| 47 | Jev top 500: women 44.4%, men 52.1%, ratio 0.85 [0.73, 0.96] | as stated | `bios_shortlist.jsonl` jev cut 500 | yes |
| 48 | Counterfactual re-rank: Laya 82 women in only if read as men, 147 men out if read as women; Jev 15 and 21; zero women lose / zero men gain at every cut, both engines | as stated | `bios_shortlist.jsonl` | yes |
| 49 | Predictions: Laya ratio about 0.75 (got 0.48); Jev about 0.93 (got 0.85) | as stated | PREREG shortlist predictions and Outcome | yes |
| 50 | EEOC four-fifths rule of thumb, 0.80 | as stated | law.cornell.edu/cfr/text/29/1607.4 | yes |
| 51 | J1 (loop, no gate), seeds 1-3: accuracy 0.8075 / 0.801 / 0.804; flips 1.55% / 1.25% / 2.35% | as stated | `bios_gender.jsonl` J1 rows (seed 1 has a second row, v4: 0.7955 / 1.6%) | yes |
| 52 | J2 (loop + gate), seeds 1-3: accuracy 0.808 / 0.808 / 0.8005; flips 0.55% / 0.55% / 1.3% | as stated | `bios_gender.jsonl` J2 rows | yes |
| 53 | J2 proposals: evidence questions flip on 0 to 1.6% of labelled twins and pass the gate; one directory-format question rejected at 2.4%; seeds 1 and 2 promoted nothing (rejected by the fit test), seed 3 promoted three and has the highest J2 flip rate | as stated | `bios_gender_proposals.jsonl` J2 rows; `bios_gender.jsonl` | yes |
| 54 | Swap-rule artefacts: "women's/men's health" swapped in 0.3% of bios; Miss, Sir, Madam missed (under 0.2%); amended for the paralegal re-run | as stated | PREREG "the learning loop on the pair that matters", "The swap rule, amended" | yes |
| 55 | Attorney-pair loop: pool 2,000 attorney + 146 paralegal (93/7) vs 50/50 held-out; J1 accuracy 0.787 / 0.8055 / 0.756 vs J0 0.854 | as stated | PREREG "the learning loop on the pair that matters", Deviations; `bios_attorney.jsonl` | yes |
| 56 | J2 on the attorney pair: seeds 1-2 promoted a support-role question (gate flip 0.8%), verdict flips 3.9% to 1.7% / 2.15%; top-500 ratio 0.85 to 0.656 / 0.649; women in only under male pronouns 37 / 21 vs 15; seed 3 promoted nothing, shortlist = J0 | as stated | `bios_attorney.jsonl`, `bios_attorney_shortlist.jsonl`, `bios_attorney_proposals.jsonl` | yes |
| 57 | Mechanism separation: new element zeroed 0.8512; holistic reset, element kept 0.6562 / 0.653; support-role logit for real attorneys women -3.29 vs men -3.63 as written, swapped -3.38 / -3.53 | as stated | `bios_attorney_elements.jsonl` | yes |
| 58 | Laya loop on the attorney pair: 11 L2 candidates, 10 failed the gate (1.4-15% flips), 1 passed the gate and failed the fit; L2 ratio 0.48 (= L0) all seeds; L1 0.333 / 0.383 / 0.48; 6 of 7 Jev L2 candidates passed | as stated | `bios_attorney_proposals.jsonl`, `bios_attorney_shortlist.jsonl`; PREREG attorney Outcome table | yes |
| 59 | Tie block: Jev two-decimal probabilities; 700-840 bios tied at P = 1.00 at the top-500 cut (733 / 838); tie-fair ratio equals the recorded one for engines alone | as stated | `bios_shortlist.jsonl` (`tied_at_cut`, `tie_fair_ratio`); PREREG shortlist addendum | yes |
| 60 | Engine-alone twin averaging, top 500, tie-fair: attorney Laya 0.48 to 0.79, Jev 0.85 to 0.91; nurse Laya 0.65 to 1.25, Jev 0.95 to 0.97; accuracy cost Laya 5.6 / 4.1, Jev 1.3 / 0.9 | as stated | `bios_shortlist.jsonl` variant `twin_averaged`; PREREG shortlist addendum | yes |
| 39 | Third pre-registered outcome wording ("reads gender in everything it says ... layer cannot refuse ... mitigation in the engine") | as stated | PREREG gender section, "What would change what I believe" | yes |

Pending, no values recorded here:

- LF (Laya fine-tuned on the same 140 labels): flip rate and accuracy. Rows will appear in `bios_gender.jsonl`.
- The Reuters citation for the Amazon screener (item 21).
