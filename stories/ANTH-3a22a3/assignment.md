# Assignment

Write the article **Decision Models Are Not Calculators** for Anth.us, slug
`decision-models-are-not-calculators`. Audience: engineers and technical product
leaders who may use a fast decision model to assign probabilities or choose among
fixed options. Target 1,200–1,800 words; tags `articles`, `AI`, `explainer`,
`mathematics`; Ryan Porter is the author.

## Research question

When Jev, Kev, and Laya are asked which face a fair six-sided die showed, do they
assign equal probabilities and select each label equally often when choice order
changes? Does the result differ between digit labels (`1`–`6`) and word labels
(`one`–`six`), and is each engine stable on an exact repeat?

## Frozen test design

- One item: `A fair six-sided die was rolled once. The result is unknown.`
- One question: `Which face showed on the roll?`
- Choice descriptions define the same face meaning in both representations;
  only the displayed label changes between digits and words.
- Submit all 720 permutations for each representation to each engine, then
  repeat the full matrix once in the same fixed order: numeric permutations in
  lexicographic order, then word permutations in lexicographic order, pass 1
  then pass 2. This is 2,880 requests per engine and 8,640 total.
- Preserve the order sent, full response, reported probabilities, selected
  choice, model identity, timing, usage, software identity, and failures.
- Do not tune the wording, sample, checkpoint, calibration, or analysis after
  observing any answers. If an input or runtime has to change, append a dated
  amendment before collecting under the changed condition.

## Analysis fixed in advance

- The strict hypothesis is that label `1` is the most probable face in every
  numeric response for both passes. Report whether it holds without weakening
  the wording after results arrive.
- For every engine, form, and pass, report each face's top-choice count and
  mean probability; also report mean probability and top-choice count by option
  position.
- Pair numeric and word rows by permutation rank; report per-face probability
  and choice differences.
- Compare pass 1 and pass 2 at matching engine, form, and permutation; report
  top-choice disagreement and absolute probability differences.
- Enumerate all orders, so report exact counts and means without sampling
  confidence intervals. State explicitly that prompt-order permutation is not
  random sampling of training data and cannot establish Benford's Law as a
  causal mechanism.
- Publish every cell and exact preregistered example request/response, plus
  spend, timing, omissions, retries, and model identity.

## Required outputs

- Public MIT GitHub repository [Decision Models Are Not Calculators](https://github.com/AnthusAI/Decision-Models-Are-Not-Calculators) with a tested,
  resumable harness, pre-registration committed before live inference, raw
  response records, generated result tables/figures, model setup, and replay
  instructions.
- A response-derived social cover at 1200×630 using the current Anth.us
  headline-chart visual treatment, and a readable article figure covering
  face preference, option position, digit/word form, and repeat stability.
- Exact response examples for the canonical order `1, 2, 3, 4, 5, 6` and its
  word equivalent.
- Article links to the prior [Benford's Law article](/blog/the-dominance-of-ones/),
  the public experiment, and primary documentation for all three engines.

## Editorial constraints

- Use the Anth.us voice profile and verify the final draft with Limatus.
- Do not mention the text to Ryan's father, single out any engineer, or imply
  that a practitioner should already know the result.
- Benford's Law motivates the test; output preferences are not evidence that
  Benford's Law trained or caused them.
- Describe the limits: one short task, exact prompt and model versions, no
  general claim about intelligence, fairness, or every possible die.
- End at newsroom `copywriting`, pending Ryan's final approval. Open site/content
  pull requests for review and do not merge or deploy.

## Acceptance

The pre-registration is pushed before inference. Each engine has all 2,880
planned successful responses, or every incomplete cell is explicitly recorded
with its reason and remaining action. Replay reproduces the published tables and
figures. The article contains the actual response examples, passes Limatus
verification without new unsupported claims, and both review pull requests are
open against their correct target branches.
