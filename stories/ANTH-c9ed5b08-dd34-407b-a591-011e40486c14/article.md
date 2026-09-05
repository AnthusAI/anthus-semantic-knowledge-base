# article.md (mirror of site-content draft)

Source of truth: `AnthusAI/anthus-site-content` branch `posts/commodity-receipts`, file `posts/commodity-receipts.mdx` (`state: draft`).

---
title: "Commodity receipts: who actually swapped"
slug: "commodity-receipts"
date: "2026-09-05"
authors:
  - author: <a href="/ryan">Ryan Porter</a>
tags:
  - posts
excerpt: |
  [The Year Coding Became a Commodity](/blog/ai-coding-cost-collapse-2026/) argued that useful capability got cheaper. Here are the procurement receipts — named orgs, what they moved, one checkable number.
preview_image: "../images/posts/commodity-receipts.png"
images:
  - "../images/posts/commodity-receipts.png"
state: draft
---

import BlogImage from "../../components/blog-image"

<p class="center-full-image">
  <BlogImage
    className="center-small-image"
    images={props.pageContext.frontmatter.images}
    name="commodity-receipts.png"
    alt="Bright stacked receipt cards in magenta, blue, and green on a light sky background"
  />
</p>

[The Year Coding Became a Commodity](/blog/ai-coding-cost-collapse-2026/) argued that the price of fixed coding capability fell — and that cheap models times long agent loops change what you can delegate. Price charts are one kind of proof. Procurement moves are another.

Here are field receipts: who swapped, what they swapped to, and one number you can check.

- **AT&T.** A cache-aware [AI Gateway](https://about.att.com/blogs/2026/the-tokenomics-equation.html) routes each task to the most cost-effective model (and can change mid-session) across an average of **45 billion tokens a day**. They say AI costs are down **as much as 90%**, already saving millions.

- **Lindy.** Most managed-agent traffic moved from Claude/Sonnet and remaining Gemini paths to **DeepSeek v4 Flash** on Atlas Cloud. On the migrated traffic, [inference costs fell about 90%](https://www.lindy.ai/blog/migrating-from-claude-to-deepseek). Sonnet stays when a user picks it or a higher-intelligence path needs it.

- **Shopify Flow.** Sidekick's natural-language → Flow agent went from a frontier model to a fine-tuned **Qwen3-32B** tool-calling agent: [**2.2× faster**, **68% cheaper**](https://shopify.engineering/fine-tuning-agent-shopify-flow), and it now carries the majority of production traffic for that skill.

- **Shopify Sidekick (GraphQL).** The continual-learning flywheel that compresses production failures into weights cut estimated serving cost from roughly **$27M/yr to ~$1M/yr — about 96%** — at up to ~2k requests per minute ([engineering write-up](https://shopify.engineering/sidekicks-continual-learning-loop)).

- **Firetiger.** First three agent types migrated from Claude on Bedrock to **DeepSeek v4 Pro**: roughly **$606K/yr → $231K/yr**, a **62%** real-dollar cut ([migration post](https://blog.firetiger.com/migrating-from-claude-to-deepseek-without-breaking-everything/)). Same eval bar took prompt work; the cheap model wasn't a dropdown.

- **FrontierHarness Eval.** Hold the model fixed (**Kimi K3**), vary the harness: nine harnesses, twelve configs, 30 tasks, 360 cold starts. Pass rates clustered **50.0%–66.7%**; median cost per pass ran **$1.05** (Exo) to **$18.34** (Claude Code) — about **17×** ([RUNTA](https://runta.com/blog/introducing-frontierharness-eval/), [leaderboard](https://frontierharness.org/)). Authors flag Claude Code's high end may be harness–model–gateway interaction, not the harness alone.

That's the wonder of this moment: useful work is moving down the cost curve in public, with numbers attached. Labs are answering with list-price cuts of their own ([FT on the price war](https://www.ft.com/content/32a70a3c-7d28-40b4-808e-36edb58c7d01)) — consequence, not a sixth company row.

Sibling notes for buying philosophy and vendor traps stay elsewhere ([Maximize Value, Not Intelligence](/blog/maximize-value-not-intelligence/), [Never Use Fast](/blog/never-use-fast/), [Thrifty tokenmaxxing](/blog/thrifty-tokenmaxxing/)). This post is just the receipts.
