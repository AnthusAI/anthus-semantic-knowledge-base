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

The public MIT project, [AnthusAI/benford-decisions](https://github.com/AnthusAI/benford-decisions),
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

## Collection status before editorial report

- Laya: 2,880 valid responses collected across both representations and both
  passes. Strict “face 1 always has the unique highest probability” claim:
  false. Descriptively, face 6 is dominant in returned probability mass and
  selected choices for this prompt; digit and word forms differ substantially.
- Kev: 2,880 valid responses collected across both representations and both
  passes. Strict “face 1 always has the unique highest probability” claim:
  false. Face 1 wins more often than any other face among the top-probability
  choices in the first digit pass but is not top for every order; word-label
  and option-position effects must be reported separately.
- Jev: the local TypeSafe credential is present, but the first request returned
  HTTP 402 because the organization has no available API credits. No model
  response was returned; fail-fast stopped the run after that single attempt.
  The conservative estimate for the remaining full matrix is $0.093, under the
  approved $1 cap. Do not write the three-model conclusion until all 2,880 Jev
  responses are available. No auto-reload or credit purchase was initiated.

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
