# article.md (mirror of site-content published article)

Source of truth: `AnthusAI/anthus-site-content` `main` @ `fa6f524`, file `commodity-receipts.mdx` (`state: published`).
Production: https://anth.us/blog/commodity-receipts/

---
title: "Commodity receipts: who actually swapped"
slug: "commodity-receipts"
date: "2026-09-05"
authors:
  - author: <a href="/ryan">Ryan Porter</a>
excerpt: "Price charts showed useful capability getting cheaper. These are the procurement receipts — named orgs, what they swapped or routed, and one checkable number each."
preview_image: "./images/commodity-receipts.png"
images:
  - "./images/commodity-receipts.png"
state: published
tags:
  - articles
  - ai
  - economics
---

import BlogImage from "../components/blog-image"
import { Citation, CitationsList } from "gatsby-citation-manager"

<BlogImage
  images={props.pageContext.frontmatter.images}
  name="commodity-receipts.png"
  className="centered"
  alt="German cash-register receipt (Kassenbon) scanned and centered on a light sky background — Wikimedia Commons, public domain."
/>

_Public domain: Wikimedia Commons [File:Kassenbon.jpg](https://commons.wikimedia.org/wiki/File:Kassenbon.jpg), author Lkawer._

AT&T's AI Gateway sees about **45 billion tokens a day**.<Citation
data={{
    type: "webpage",
    title: "The Tokenomics Equation: Balancing Cost and Performance",
    "container-title": "AT&T",
    URL: "https://about.att.com/blogs/2026/the-tokenomics-equation.html",
    accessed: { "date-parts": [[2026, 9, 5]] }
  }}
/> Most of those turns don't need the newest frontier model. Their cache-aware router picks a cheaper one — and can change mid-session — and they say AI costs are down **as much as 90%**, already saving millions.

That's a receipt. Not a benchmark curve, not a pricing table. A named organization, a concrete swap (or route), and a number you can check against a primary write-up.

[The Year Coding Became a Commodity](/blog/ai-coding-cost-collapse-2026/) made the case that useful coding capability got dramatically cheaper while agents learned to stay on a task longer. This piece doesn't retell those curves. It walks the field: who moved production traffic, what they moved it to, and what the bill did.

Buying philosophy lives elsewhere — [Maximize Value, Not Intelligence](/blog/maximize-value-not-intelligence/), [Never Use Fast](/blog/never-use-fast/), [Thrifty tokenmaxxing](/blog/thrifty-tokenmaxxing/). Here we're just reading the invoices people published.

## AT&T: route every turn

Scale is the first thing that lands. Forty-five billion tokens a day is enough that "default to the latest model" stops being a preference and starts being a budget category.

AT&T's answer is an AI Gateway with cache-aware routing: match each task to the most cost-effective model that still clears the quality bar, and allow the model to change during a multi-turn session. They're also training telco-specific open models so more work can land on right-sized hardware. The efficiency claim they put in public is blunt — up to a 90% cost cut, millions already saved.

The wonder isn't that a telco built a router. It's that at that volume, model choice becomes operations: every turn is a procurement decision, and the gateway automates it.

## Lindy: migrate the managed fleet

Lindy's pricing only works if inference keeps getting cheaper. Staff engineer Bruno Škvorc published the migration that made that bet real: most managed-agent traffic — Claude/Sonnet paths and the remaining Gemini routes — moved to **DeepSeek v4 Flash** on Atlas Cloud.<Citation
data={{
    type: "webpage",
    title: "Migrating from Claude to DeepSeek",
    author: ["Bruno Škvorc"],
    "container-title": "Lindy",
    URL: "https://www.lindy.ai/blog/migrating-from-claude-to-deepseek",
    accessed: { "date-parts": [[2026, 9, 5]] },
    issued: { "date-parts": [[2026, 6, 24]] }
  }}
/> On the migrated traffic, inference costs fell about **90%**. Sonnet stays when a user picks it or a higher-intelligence path needs it.

The part that stuck with me: they tried Kimi K2.5 in offline evals, then rolled a small slice to real users, and someone said it felt like their assistant had had brain surgery overnight. Offline scores weren't enough. Provider choice mattered too — the same nominal model scored differently depending on who served it. Prompt optimization (their GEPA loop), internal rollout, online evals, and retention all had to hold before they ramped to 100% of the target traffic.

Changing the model string was the easy headline. Earning the right to move the traffic was the job.

## Shopify Flow: fine-tune the skill

Sidekick's natural-language → Flow skill used to sit on a frontier model Shopify didn't control. The engineering team fine-tuned **Qwen3-32B** into a tool-calling agent that writes Flow automations — **2.2× faster**, **68% cheaper**, and now carrying the **majority of production traffic** for that skill.<Citation
data={{
    type: "webpage",
    title: "Flow generation through natural language: An agentic modeling approach",
    "container-title": "Shopify Engineering",
    URL: "https://shopify.engineering/fine-tuning-agent-shopify-flow",
    accessed: { "date-parts": [[2026, 9, 5]] },
    issued: { "date-parts": [[2026, 4, 22]] }
  }}
/>

They didn't get there with a dropdown. Synthetic trajectories reverse-engineered from real merchant workflows, a Python DSL closer to pretraining than Flow's JSON, weekly retraining on Tangle, and a production flywheel that scored conversations with a calibrated LLM judge. Benchmarks said ready; a 1% traffic slice said activation was 35% lower than the prompt-based agent. Two weeks of flywheel work closed the gap.

Six months from rented frontier to owned weights, on data only Shopify has, at 68% lower cost. The version serving merchants is already worse than the one retraining behind it.

## Shopify Sidekick GraphQL: compress failures into weights

A second Shopify receipt sits next to Flow, and it's even louder on dollars. Sidekick's GraphQL agent — merchant questions answered by writing and running Admin GraphQL — serves up to about **2,000 requests per minute**.<Citation
data={{
    type: "webpage",
    title: "Sidekick's continual learning loop",
    "container-title": "Shopify Engineering",
    URL: "https://shopify.engineering/sidekicks-continual-learning-loop",
    accessed: { "date-parts": [[2026, 9, 5]] },
    issued: { "date-parts": [[2026, 8, 5]] }
  }}
/> Serving that traffic on a frontier model was estimated at roughly **$27M/yr**. The fine-tuned continual-learning loop brings it closer to **~$1M/yr** — about **96%**.

The loop is the story: rubric → calibrated judge → autoresearch on the harness → hard negatives from production → heal with a panel of frontier critics → SFT + GRPO into smaller weights → gist-compress the long system prompt. Frontier models launch the product; the flywheel turns production failures into parameters. Quality rises past the frontier baseline while latency and GPU count fall.

That's what "commodity" looks like when you own the skill: the expensive general model becomes training signal and a fallback, not the serving path.

## Firetiger: Claude → DeepSeek with the eval bar held

Firetiger runs incident-investigation and monitoring agents whose COGS is mostly inference. Claudes on Bedrock had been the default. For the first three agent types migrated to **DeepSeek v4 Pro**, real-dollar spend went from roughly **$606K/yr to $231K/yr** — a **62%** cut.<Citation
data={{
    type: "webpage",
    title: "Migrating from Claude to DeepSeek without breaking everything",
    "container-title": "Firetiger Blog",
    URL: "https://blog.firetiger.com/migrating-from-claude-to-deepseek-without-breaking-everything/",
    accessed: { "date-parts": [[2026, 9, 5]] }
  }}
/>

Drop-in failed. DeepSeek with reasoning started at 80% task completion against Sonnet 4.6's 94%. A self-improvement loop rewrote prompts — second-order effects from a PR diff, how to treat prior notes as directional context — until DeepSeek hit 92% without hurting Sonnet. Then cost-per-task still lagged list-price spreads because the cheap model wrote more and wandered into web rabbit holes. More prompt work, tool-policy hardening, and session replays before production.

The one-sentence lesson they published: the eval score is the start of the work, not the end. Getting the right answer tells you migration is possible; getting there efficiently, by a path you understand, is the finish line.

## FrontierHarness: hold the model, vary the harness

One more receipt flips the axis. RUNTA's FrontierHarness Eval keeps the model fixed — **Kimi K3** on Fireworks — and varies the agent harness across nine harnesses and twelve configs, 30 tasks, 360 cold starts.<Citation
data={{
    type: "webpage",
    title: "Introducing FrontierHarness Eval",
    "container-title": "RUNTA",
    URL: "https://runta.com/blog/introducing-frontierharness-eval/",
    accessed: { "date-parts": [[2026, 9, 5]] }
  }}
/> Pass rates clustered **50.0%–66.7%**. Median cost per completed pass ran from **$1.05** (Exo) to **$18.34** (Claude Code) — about **17×**.

Same weights. Same tasks. Same cold-start restore. Very different bills.

The authors flag an important caveat: Claude Code's high end may reflect harness–model–gateway interaction (Anthropic-oriented caching against K3's implicit cache), not the harness alone. Even with that asterisk, the spread is the point. Commodity pricing isn't only "which model." It's which loop burns the tokens.

## What the labs do next

When application companies publish moves like these, labs answer on the rate card. The Financial Times covered OpenAI and Anthropic in a price war as Chinese rivals gain ground — the same pressure showing up on the rate card.<Citation
data={{
    type: "webpage",
    title: "OpenAI and Anthropic in price war as Chinese AI rivals gain ground",
    "container-title": "Financial Times",
    URL: "https://www.ft.com/content/32a70a3c-7d28-40b4-808e-36edb58c7d01",
    accessed: { "date-parts": [[2026, 9, 5]] }
  }}
/>

Useful work is moving down the cost curve in public, with numbers attached. Gateways route. Fleets migrate. Skills get fine-tuned. Harnesses get measured. The scarce thing keeps sliding toward judgment: which traffic can move, what quality bar to hold, and whether the cheaper path still feels like the product.

## References

<CitationsList citationFormat="apa" />
