# Idea

## Pitch

Fast decision models return a choice and probabilities. A fair six-sided die is a compact way to check whether those numbers act like a calculator or like a learned decision: each face should be one sixth, whatever order the choices appear in.

The hypothesis comes from our older [Benford's Law article](https://anth.us/blog/the-dominance-of-ones/), which describes why natural datasets often have more leading 1s. This new test asks whether three decision models prefer the choice `1` in an unrelated die-roll task, and whether changing the order of the choices moves their answers. Benford's Law is a motivation, not an assumed cause: the model outputs alone cannot identify the training-data mechanism.

Run every permutation of six labels as digits and words, twice, on Jev, Kev, and Laya. Keep raw replies, model identities, request order, and local-runtime details in a public MIT repository. Publish the complete result and exact example requests and responses, including results that contradict the hypothesis.

## Audience

Engineers and product developers evaluating fast decision models for classification, routing, or ranking.

## Working title

Decision Models Are Not Calculators

Slug: `decision-models-are-not-calculators`.

## Sources and related work

- [The Dominance of Ones: A Handy Quirk of Numbers](https://anth.us/blog/the-dominance-of-ones/)
- [Biased-Decisions](https://github.com/AnthusAI/Biased-Decisions), including engine adapters and option-order methodology
- Primary model documentation for Jev, Kev, and Laya, captured with dated links in the experiment repository

## Scope boundaries

- No named-person anecdotes or criticism of engineers.
- No claim that Benford's Law caused any output pattern.
- No claim that a die prompt measures general model intelligence or deployment fairness.
- Public article remains in copywriting pending Ryan's final editorial approval.
