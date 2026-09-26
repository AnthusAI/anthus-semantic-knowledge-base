# Report — Decision Models Are Not Calculators

## Finding

The original conjecture splits by model. With the exact same fair-die question,
Jev chose one in every ordering, Kev chose one in 49.0% of responses, and Laya
chose six in 90.6%. This makes a stronger, more honest story than “models always
say one”: a decision model's structured probability output can look precise
while depending on model identity, labels, and option order.

## Evidence ready for copy

The preregistered matrix contains 8,640 valid responses: three engines × two
representations × 720 semantic-face permutations × two passes. Each semantic
face appears 120 times at every list position within each representation/pass.
All raw successful replies are released as checksummed JSONL archives at
[Decision Models Are Not Calculators](https://github.com/AnthusAI/Decision-Models-Are-Not-Calculators).
The [summary](https://github.com/AnthusAI/Decision-Models-Are-Not-Calculators/blob/main/results/summary.json),
[exact request/reply examples](https://github.com/AnthusAI/Decision-Models-Are-Not-Calculators/blob/main/results/examples.json),
and [social/chart images](https://github.com/AnthusAI/Decision-Models-Are-Not-Calculators/tree/main/images)
are generated from those records.

| Model | Face 1 selected | Face 6 selected | Digit pass 1: mean P(1) | Digit pass 1: mean P(6) |
| --- | ---: | ---: | ---: | ---: |
| Jev | 2,880 | 0 | 0.8971 | 0.0445 |
| Kev | 1,410 | 294 | 0.1860 | 0.1844 |
| Laya | 40 | 2,610 | 0.0653 | 0.6619 |

The strict confirmatory claim (“1 is uniquely most probable in every digit
response”) held for Jev and failed for Kev and Laya. Jev selected one at each
list position 120/720 times in the first digit pass, exactly as balancing
predicts for a face-fixed choice. Kev selected position one in 426/720 digit
requests; Laya selected position six in zero. In matched digit/word pairs,
the selected face changed in 0/720 Jev orders, 324/720 Kev orders, and 129/720
Laya orders. Selected faces remained stable across the second pass for every
engine; Jev probabilities fluctuated by a mean absolute 0.0084 on face one
for digit pairs, while Kev and Laya probabilities were identical.

The canonical request uses state “A fair six-sided die was rolled once. The
result is unknown.” and asks “Which face showed on the roll?” with criteria
`1` through `6`. In that single order, Jev replied `1` at P(1)=0.88, Kev
replied `6` at P(6)=0.1975, and Laya replied `1` at P(1)=0.3988. The Laya
single-row answer is worth contrasting with its aggregate face-six dominance;
don't present one example as a summary statistic.

## Limits and handling

Benford's Law applies to leading digits in certain observed datasets, not to
the six faces of an unseen fair die. The experiment never inspects training
data and does not identify the mechanism behind any output bias. A true fair
die distribution is 1/6 per face; model distributions here are response
behavior, not measurement of the hidden outcome. These are one prompt and
three specific builds; Jev's `jev-latest` is a mutable alias, though all
recorded replies identified `jev-1.13.0`.

Initial Laya setup failures and Jev's HTTP 402/precision failures are excluded
from valid response archives and counted in the release manifest. The Jev
probability-precision adjustment is documented, preserves raw numbers, and
does not alter the request matrix. No significance tests or training-data
causal claims are appropriate for this exhaustive, descriptive design.

Editorial recommendation: open with an unseen die and the three startling
answers, then show the exact request and response examples, chart the model
and representation differences, and close with a practical robustness check
for engineers. Link back to [The Dominance of Ones](https://anth.us/blog/the-dominance-of-ones/)
as the motivating question, not as the explanation.
