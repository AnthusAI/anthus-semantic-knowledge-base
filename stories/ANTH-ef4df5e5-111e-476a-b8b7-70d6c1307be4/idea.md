# Idea

## Pitch

A short visual companion to Maximize Value, Not Intelligence. That piece is the buying philosophy. This one is four pictures of a bill.

Grok 4.6 sells near-frontier intelligence at $2 in / $6 out (3×). Claude and OpenAI flagships still sit at 5–6×, with output at $10 to $50. A handful of August 2026 cost blogs named that inverted ratio. The interesting claim is not “Grok is cheaper.” It is that the rate card lies until you pick a workload.

Four example mixes, live cards from 31 Aug 2026, not measured traces:

1. Short chat (2.5k in, 400 out). Rounding error. Grok $0.007, Opus $0.023.
2. One-shot patch (18k in, 10k out). Cheap output wins. Grok $0.10, Sonnet $0.14, Sol promo $0.27, Opus $0.34.
3. 30-turn coding agent (each turn 60k cache, 8k new, 2.5k out). Cheap cache wins. Grok $1.83, Sonnet $1.59, Terra $1.74. Grok’s cache is $0.50 vs Sonnet’s $0.20. Output is only 25% of Grok’s invoice.
4. Six-agent swarm, 12 turns (each 40k cache, 6k new, 5k out of code/plans). Grok back in front ($4.46 vs Sonnet $5.04 vs Opus $12.60), but not by the 4× the output rates imply.

Cursor does not discount Grok output. Cursor list is the same $2/$6 (Fast $4/$12). What Cursor changes is the included Cursor Models pool, and Composer 2.5 in that pool at $0.50/$2.50.

The charts from the 2026-08-31 scout sit in this story folder: `grok-pricing-scenarios.png`, `grok-pricing-output-share.png`. Recheck official xAI / Anthropic / OpenAI / Cursor cards before publish. Do not invent a fifth mix. Do not treat the mixes as production traces.

## Audience

People already buying models by value, who are about to pick Grok for a swarm because the output column looks cheap.

## Working title

Cheap output, expensive cache

Ryan may rename. Alternate: The rate card lies.

## Split

Sibling (link, do not retell):

- Maximize Value, Not Intelligence — https://anth.us/blog/maximize-value-not-intelligence/ — routing and value per dollar. This piece is the workload picture that article did not draw.

Distinct from:

- Never Use Fast (`8bde91`) — plan buckets and Fast as a surcharge. Fast on Cursor Grok is 2×, not cheaper output.
- Bugonomics — cheap offense. Not this.
- The Year Coding Became a Commodity — the frontier moving. Link, do not retell.
- `bef418` Overnight products, leftover gates — QA/privacy/security at swarm speed. Not pricing.

Do not claim Cursor sells Grok output cheaper than the API. Do not use tracker pages that still print Sol at $5/$30; live short-context promo on 31 Aug 2026 was $4/$20 through at least 21 Nov 2026. Do not smear retired Grok Fast ($0.20/$0.50) onto Grok 4.6.
