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


## Perplexity page confirmation (2026-09-05)

Page: https://www.perplexity.ai/page/major-u-s-firms-swap-premium-a-b19jyW_nS_iIX1A3OooDQw — title “Major U.S. firms swap premium AI for cheap open-weight models,” published ~17h before fetch. Synthesis, not a cite of record. Aligns with our receipt rows; adds named routers **Airbnb** and **Siemens** (need primaries before publish). Points at FT price-war piece as a better lab-cuts primary than Startup Fortune:

- Financial Times: “OpenAI and Anthropic in price war as Chinese AI rivals gain ground” — https://www.ft.com/content/32a70a3c-7d28-40b4-808e-36edb58c7d01
- Also cites Futurism, TechStartups (Goldman / AT&T 56%), YC X post.

Ollama CEO Jeffrey Morgan quoted on AT&T’s ~40% open-weight token plan; Ollama self-claims (85% Fortune 500, $65M Series B) are vendor PR — use sparingly or verify.

DeepSeek V4 Flash ~$0.28/M input vs ~$15 for Claude Opus 4.6 / GPT-5.4 — verify against current rate cards before publishing.


## More receipts (scout 2026-09-05)

Full notes: `/workspace/research-commodity-receipts-more-2026-09-05.md`

### Shopify — Flow agent → fine-tuned Qwen3-32B (high)

Primary: https://shopify.engineering/fine-tuning-agent-shopify-flow (updated 2026-04-22). Frontier → fine-tuned **Qwen3-32B** tool-calling agent for Flow NL→automation in Sidekick. **2.2× faster**, **68% cheaper**, outperforms closed models, majority of production traffic for that skill.

### Shopify — Sidekick continual-learning flywheel (high)

Primary: https://shopify.engineering/sidekicks-continual-learning-loop (2026-08-05). Frontier baseline → specialized fine-tuned smaller model; estimated serving **~$27M/yr → ~$1M/yr (~96%)**; up to ~2k rpm. Companion to Flow post (base family named there as Qwen3-32B).

### Firetiger — Claude → DeepSeek v4 Pro (high)

Primary: https://blog.firetiger.com/migrating-from-claude-to-deepseek-without-breaking-everything/ First three agent types: **~$606K/yr → ~$231K/yr (62%)** real-dollar cut (path to ~70% if cache matches Claude). Via Baseten; evals + prompt work required.

### Airbnb — Qwen-heavy CS agent (high, press + letter)

Chesky interview (Fortune/Bloomberg 2025-10-21): customer-service agent relies heavily on **Alibaba Qwen**; OpenAI latest used less because faster/cheaper alternatives exist. House letter 2026-04-29: https://homeland.house.gov/wp-content/uploads/2026/04/2026.04.29-Homeland-China-Select-Letter-to-Airbnb-re-PRC-AI.pdf · Fortune: https://fortune.com/2025/10/21/brian-chesky-openai-tools-not-ready/

### Siemens — in-house open-weight LLM API (high)

Primary: https://blog.siemens.com/2024/04/open-source-llms-for-everyone/ — **vLLM + LiteLLM + Kong**, OpenAI-compatible, data internal. 2025 slides: https://opensource.siemens.com/events/2025/slides/Roger_Meier__Latest_news_on_Open_Source_%40_Siemens_and_open_weight_LLM_usage_in-house.pdf Soft on token %; clear self-hosted open-weight default for internal assistants.

### Coinbase — GLM + Kimi gateway defaults (medium)

Secondary: https://thenewstack.io/multi-model-ai-infrastructure/ (2026-07-07). Defaults to self-hosted **GLM 5.2** and **Kimi 2.7**; ~**half** AI bill while tokens grew; ~1,200 agents. Prefer Coinbase primary before publish.

### LiteLLM anon production auto-router (medium)

https://docs.litellm.ai/blog/auto-router-production-savings — flagship → Haiku/Sonnet/Opus cascade; **51%** save; **95%** of requests never needed flagship (Apr–Aug 2026). Closed-family cascade, not open-weight, but same “only hard work to premium” lever.

### Adjacent (different pattern)

- **Cursor/Anysphere Composer 2** built on **Kimi K2.5** (~1/4 compute from base): https://techcrunch.com/2026/03/22/cursor-admits-its-new-coding-model-was-built-on-top-of-moonshot-ais-kimi/ — product weights, not API traffic swap.
- **Vercel** AI Gateway customer mix includes DeepSeek/GLM; >1T tokens/day: https://techcrunch.com/2026/07/06/vercel-ceo-guillermo-rauch-on-the-fight-to-split-off-models-from-agents/ — ecosystem signal, not Vercel’s own app swap.
