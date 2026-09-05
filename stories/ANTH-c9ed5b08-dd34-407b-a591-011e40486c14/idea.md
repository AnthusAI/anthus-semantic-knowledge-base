# Commodity receipts: who actually swapped off premium models

## Pitch

Companion to the published essay [The Year Coding Became a Commodity](https://anth.us/blog/ai-coding-cost-collapse-2026/). That piece owns the thesis: useful coding capability got dramatically cheaper, and cheap models × long agent loops change what you can delegate. This follow-up does not retell the curves. It is a short field-receipts list — named orgs, what they swapped, one checkable number, primary cite — so a reader of Commodity can point at procurement, not just price charts.

Shape: 5–7 one-sentence receipts. Link Commodity. Stop.

## Audience

Readers of Commodity who want proof the market story landed in real budgets; CFOs and eng leads already routing or about to.

## Working title

Commodity receipts (who actually swapped)

Ryan may rename. Alternatives: “The swap list,” “Not every token needs Opus.”

## Structure

1. One short open: Commodity argued the price of fixed capability fell; here are receipts that buyers acted.
2. Bullet list (not a narrative retell). Each row: org → move → one number → cite.
3. Optional closing beat: labs cut prices in response (consequence, not a sixth company row).
4. Link siblings; do not retell Maximize Value, Never Use Fast, or Thrifty tokenmaxxing.

## Split

Parent / link:

- The Year Coding Became a Commodity (`ecc2ae` / published `ai-coding-cost-collapse-2026`) — thesis. This is the receipts appendix.

Distinct from:

- Maximize Value, Not Intelligence (`0b6e8b`) — buying philosophy.
- Never Use Fast (`8bde91`) — Fast surcharge table.
- Thrifty tokenmaxxing (`c28002`) — click-path catalog / vendor traps.
- Cheap output, expensive cache (`ef4df5`) — rate card vs workload mix.
- FrontierHarness as a standalone essay — one row here is enough; do not become a harness review.

## Starter receipt rows (verify before copy)

### 1. AT&T — AI Gateway / Ask AT&T

Primary: AT&T blog, “The Tokenomics Equation: Balancing Cost and Performance,” https://about.att.com/blogs/2026/the-tokenomics-equation.html (Chief Data and AI Officer). States average **45 billion tokens a day**; cache-aware AI Gateway routes each task to the most cost-effective model and can change models mid-session; claims AI costs reduced **as much as 90%**, already saving millions.

Secondary (56% coding / 2% quality / LiteLLM / ~40% open-source share): The Information via roundups (e.g. Gate News 2026-08-22 summarizing Mark Austin). Prefer primary or named interview before publishing the 56%/2% pair; do not invent.

### 2. Lindy — production traffic to DeepSeek V4

Primary: Lindy, “Migrating from Claude to DeepSeek,” https://www.lindy.ai/blog/migrating-from-claude-to-deepseek (reviewed by Flo Crivello; published July 10, 2026). Moved most managed-agent traffic (Claude/Sonnet paths and remaining Gemini paths) to **DeepSeek v4 Flash** on Atlas Cloud; on migrated traffic, inference costs fell **about 90%**. Sonnet remains when a user picks it or a higher-intelligence path needs it.

Secondary: The New Stack, https://thenewstack.io/lindy-deepseek-anthropic-switch/ — Crivello’s “100% of Lindy traffic” / “saves us millions” framing; migration “100x more work than we thought.”

### 3. DoorDash — DashBench hybrid scout/reviewer

Primary: DoorDash, “How we learned to trust our AI code reviewer at DoorDash,” https://careersatdoordash.com/blog/how-we-learned-to-trust-our-ai-code-reviewer-at-doordash/ (bot-protected fetch; verify numbers from live page before publish). DashBench replays historical PRs for real findings vs plausible comments.

Reported model-mix result (verify on primary): **Kimi K2.6 scout + Claude Fable 5 reviewer** led weighted recall (**65.2%**) and F1 (**75.3%**) at **$3.81**/PR vs production all-Claude (Sonnet 4.6 scout + Opus 4.8 reviewer) at **53.6%** recall / **66.3%** F1 / **$3.91**/PR (secondary summaries: FourWeekMBA, ZenML LLMOps database). Use primary table if the careers page exposes it.

### 4. FrontierHarness Eval — same model, different harness bill

Primary: RUNTA, “Introducing FrontierHarness Eval,” https://runta.com/blog/introducing-frontierharness-eval/ (Sep 1, 2026). Nine harnesses / twelve configs, **Kimi K3** fixed, 30 tasks, 360 cold-start trials. Pass rates **50.0%–66.7%**; median cost per pass **$1.05** (Exo) to **$18.34** (Claude Code) — ~**17×**. Leaderboard: https://frontierharness.org/ · data: https://github.com/runta-dev/frontier-harness-eval. Authors warn Claude Code’s high cost may be harness–model–gateway interaction; cite the caveat.

### 5. Labs answer with cuts (consequence row)

Secondary roundup: Startup Fortune Sep 4, 2026, https://startupfortune.com/corporate-america-is-swapping-claude-and-gpt-for-cheaper-deepseek-models/ — cites Axios late July OpenAI GPT-5.6 Luna ~**80%** cut (to **$0.20**/M input, **$1.20**/M output) and VentureBeat on Anthropic Opus 5 pricing vs Fable 5. Prefer Axios / OpenAI / Anthropic primaries before publish; Perplexity page https://www.perplexity.ai/page/major-u-s-firms-swap-premium-a-b19jyW_nS_iIX1A3OooDQw is a synthesis pointer, not a cite of record.

## Editorial boundary

Idea artifact only. Do not draft the listicle here. Leave stage at `idea`. Do not invent numbers. Prefer company blogs over aggregators. Do not implement product or post live from this story.
