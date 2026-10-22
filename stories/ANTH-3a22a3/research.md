# Research — Decision Models Are Not Calculators

## Editorial finding

The die prompt is a compact way to show why a typed decision endpoint should
not be mistaken for a calculator. A fair die's unknown outcome has equal
probability on all six faces. A model can still return an uneven distribution,
and its selected answer can change as the options move. Neither result explains
why the model learned that pattern.

The prior Anth.us article, [The Dominance of Ones: A Handy Quirk of
Numbers](https://anth.us/blog/the-dominance-of-ones/), explains Benford's Law
for leading digits in many real-world datasets. The new experiment deliberately
does not put Benford into any request. It can test whether `1` is preferred in
this prompt; it cannot attribute any preference to training data or Benford's
Law. A six-face choice task is not itself a Benford-distributed dataset.

## Frozen procedure

The public MIT project, [Decision Models Are Not Calculators](https://github.com/AnthusAI/Decision-Models-Are-Not-Calculators),
contains the pre-registration committed before inference, the request matrix,
adapters, tests, and result-validation tools. It exhausts all 720 semantic-face
permutations with digit labels and all 720 with word labels, repeated once per
model: 2,880 responses each, 8,640 total. Each face occurs 120 times at every
position within each representation/pass. It records returned per-option
probabilities as well as selected choice and separately measures list-position
effects. The repeats are a stability check; inference is descriptive, not a
significance test.

Exact prompt:

- State: `A fair six-sided die was rolled once. The result is unknown.`
- Question: `Which face showed on the roll?`
- Criteria: `the die face marked {1|2|3|4|5|6}`; word condition uses
  `{one|two|three|four|five|six}`. Criterion insertion order is permuted.

## Engine identity and caveats

- **Jev** is TypeSafe's hosted System One service; the request targets the
  mutable `jev-latest` alias. The provider reports $0.042/M input tokens and no
  output charge. Official introduction: [TypeSafe — System One Models & Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev).
- **Kev** is Jared Palmer's open-weight 0.8B decision checkpoint served through
  a TypeSafe-compatible endpoint, not the hosted Jev service. This run pins
  `jaredpalmer/kev-0.8b@54f4f8777356cd5bbbb6c6919c657f26e6f2f6d8` and logs its
  server/base revisions. [Kev repository and API](https://github.com/jaredpalmer/kev)
  and [Kev-0.8B model card](https://github.com/jaredpalmer/kev/blob/main/docs/model-cards/kev-0.8b.md).
- **Laya** is Convai Innovations' open-weight ModernBERT decision model, run
  from pinned snapshot `55cf4c4ebb4ebe31b2550e8bdf3bd21b99753851` on Apple
  MPS. [Laya model card](https://huggingface.co/convaiinnovations/laya).
  Runtime warning: this snapshot emits invalid temperatures for some option
  cardinalities; the package clamps them. Its confidence field should not be
  described as calibrated for affected entries. The experiment retains the
  returned choice distribution and does not make calibration claims.

## Completed collection

All 8,640 planned request IDs have a valid response (2,880 per engine),
checksummed in the public release. Jev initially returned HTTP 402; Ryan
purchased credits and the run completed under the approved $1 input cap.
Jev's two-decimal returned probabilities sometimes sum to 0.99 after rounding.
The validator was corrected to accept up to six independent half-centesimal
rounding errors, without altering the returned values or the frozen requests.
The [measurement note](https://github.com/AnthusAI/Decision-Models-Are-Not-Calculators/blob/main/docs/measurement-notes.md)
records that correction and excluded failed attempts.

- **Jev:** face 1 selected and uniquely top-probability in all 2,880 replies,
  including both digit and word forms and both passes. The service returned
  `jev-1.13.0` for every recorded reply. Digit pass 1 mean probability of 1:
  89.7%; word pass 1: 87.6%.
- **Kev:** face 1 selected 1,410/2,880 (49.0%), not always. Digit pass 1
  selected the first-listed choice in 426/720 (59.2%) balanced orders.
  Digit-to-word wording changed the selected face in 324/720 paired orders.
- **Laya:** face 6 selected 2,610/2,880 (90.6%), not face 1. Digit pass 1
  never selected the last-listed option (0/720); digit-to-word wording changed
  the selected face in 129/720 paired orders. Its returned confidence field
  should not be called calibrated.
- **Repeats:** all three selected the same semantic face on every exact repeat;
  Jev's numeric probabilities moved slightly, while Kev's and Laya's matched
  exactly. The repeats are stability checks, not independent samples.

## Claims to keep bounded

1. A probability distribution is a model output, not a guarantee of a fair
   random process and not a calculation of the die's physical result.
2. This test can show whether a returned distribution or choice changed under
   the frozen label/order manipulations; it cannot identify the training-data
   mechanism that caused it.
3. The models are particular versions and builds, and Jev's alias is hosted.
   Avoid generalizing the result to every decision model or future version.
4. The lead digit of a semantic label, the word used for that label, its option
   position, and its learned association are different factors. The matrix
   separates the first two treatments and balances the third; it does not
   isolate an internal mechanism.

## Draftable reader takeaway

For a consequential workflow, test the model on the decision distribution you
actually need: change order, preserve exact probabilities, compare chosen
labels with returned confidence, and pin the model version. If randomness is
required, sample a real random source; asking a classifier to choose a face is
not a substitute for a random-number generator.
