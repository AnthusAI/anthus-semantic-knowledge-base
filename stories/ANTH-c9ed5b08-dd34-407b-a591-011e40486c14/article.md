# article.md (mirror of site-content published article)

Source of truth: `AnthusAI/anthus-site-content` `main` @ `9cd2da5`, file `commodity-receipts.mdx` (`state: published`).
Production: https://anth.us/blog/commodity-receipts/

---
title: "Who actually swapped off premium models"
slug: "commodity-receipts"
date: "2026-09-05"
authors:
  - author: <a href="/ryan">Ryan Porter</a>
excerpt: "Useful capability got cheaper. Here are named orgs that moved production traffic, what they swapped or routed to, and one checkable number each."
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
  alt="Simple diagram: two Fantastic AI product cards on white — left $100, right same $100 with a hand-drawn red slash and handwritten red $1."
/>

Six months ago, "frontier" and "cheap" didn't belong in the same sentence. Then a wave of Chinese models — DeepSeek, Kimi, Qwen — started clearing real production quality bars at a fraction of the price, and the big U.S. labs started answering on their own price sheets instead of pretending not to notice. The Financial Times now has OpenAI and Anthropic in an open price war as Chinese rivals close the gap.<Citation
data={{
    type: "webpage",
    title: "OpenAI and Anthropic in price war as Chinese AI rivals gain ground",
    "container-title": "Financial Times",
    URL: "https://www.ft.com/content/32a70a3c-7d28-40b4-808e-36edb58c7d01",
    accessed: { "date-parts": [[2026, 9, 5]] }
  }}
/>

That doesn't mean AI got cheap across the board. It means one layer of AI work got cheap — the tasks routine enough that a much cheaper model handles them exactly as well as the flagship one. Nobody has a fixed list of which tasks qualify, and nobody will; it's a moving target, redrawn every time a router gets smarter or a fine-tune gets sharper. [Maximize Value, Not Intelligence](/blog/maximize-value-not-intelligence/) made the argument for routing every job to the cheapest model that clears the bar. [The Year Coding Became a Commodity](/blog/ai-coding-cost-collapse-2026/) showed that happening to coding specifically. Go back far enough and [How Does the Drama at OpenAI Affect You?](/blog/a-world-with-no-moats/) was written the week OpenAI's board drama cracked the idea that any one lab was a safe, permanent one-stop shop — the moats were already looking thin in 2023.

The trend is real. The hard part is noticing it's landed on your own workload before someone else finds out the expensive way. So skip the price charts — here are named companies that moved, what they swapped or routed, and one number each you can check for yourself.

## AT&T: route every turn

AT&T's AI Gateway handles about **45 billion tokens a day**.<Citation
data={{
    type: "webpage",
    title: "The Tokenomics Equation: Balancing Cost and Performance",
    "container-title": "AT&T",
    URL: "https://about.att.com/blogs/2026/the-tokenomics-equation.html",
    accessed: { "date-parts": [[2026, 9, 5]] }
  }}
/> At that volume, "just use the latest model" stops being a preference and becomes a line item. So AT&T built a system that picks the cheapest model that still clears the quality bar for each task — and can swap models mid-conversation. They say it's cut AI costs by **as much as 90%**, saving millions already, and they're training their own smaller models to run on cheaper hardware too.

The takeaway isn't that a phone company built a router. It's that at this scale, picking which AI to use for a job stops being an engineering choice and starts being a budgeting one.

## Lindy: migrate the managed fleet

Lindy runs AI assistants for other businesses, and its pricing only works if the AI underneath keeps getting cheaper. So it moved most of that traffic off Claude and Gemini and onto **DeepSeek**, a much cheaper Chinese model, cutting inference costs on that traffic by about **90%**.<Citation
data={{
    type: "webpage",
    title: "Migrating from Claude to DeepSeek",
    author: ["Bruno Škvorc"],
    "container-title": "Lindy",
    URL: "https://www.lindy.ai/blog/migrating-from-claude-to-deepseek",
    accessed: { "date-parts": [[2026, 9, 5]] },
    issued: { "date-parts": [[2026, 6, 24]] }
  }}
/> Customers who specifically ask for Claude still get it.

It wasn't instant. An earlier swap made the assistant feel, in one tester's words, like it "had brain surgery overnight" — so this time Lindy tested offline first, rolled it out to a small slice of real users, and kept tuning before flipping the switch for everyone. Changing the model was the easy part; earning the right to actually ship the change was the job.

## Shopify Flow: fine-tune the skill

Shopify's Sidekick assistant used to lean on a big, general-purpose AI model to turn plain-English requests into "Flow" automations for merchants. Shopify trained its own smaller, specialized model for that one job instead — and it now runs **2.2× faster**, costs **68% less**, and handles the **majority of production traffic** for that feature.<Citation
data={{
    type: "webpage",
    title: "Flow generation through natural language: An agentic modeling approach",
    "container-title": "Shopify Engineering",
    URL: "https://shopify.engineering/fine-tuning-agent-shopify-flow",
    accessed: { "date-parts": [[2026, 9, 5]] },
    issued: { "date-parts": [[2026, 4, 22]] }
  }}
/>

Getting there took real work: months of generating practice examples from real merchant workflows, then continually retraining as new mistakes turned up in production. It took six months to go from renting a frontier model to owning a smaller one that's cheaper and better at this one job — because that's the only job it has to do.

## Shopify Sidekick GraphQL: compress failures into weights

A second, louder Shopify number: its GraphQL assistant answers merchant questions by writing and running database queries, at up to **2,000 requests per minute**.<Citation
data={{
    type: "webpage",
    title: "Sidekick's continual learning loop",
    "container-title": "Shopify Engineering",
    URL: "https://shopify.engineering/sidekicks-continual-learning-loop",
    accessed: { "date-parts": [[2026, 9, 5]] },
    issued: { "date-parts": [[2026, 8, 5]] }
  }}
/> Running that on a frontier model was projected to cost roughly **$27M a year**. By continually retraining a smaller model on its own production mistakes, Shopify brought that down to about **$1M a year** — a **96%** cut — without losing quality.

The pattern here: the expensive general model stops being the thing that serves customers. It becomes the teacher. The cheap model that learned from its mistakes does the actual work.

## Firetiger: Claude → DeepSeek with the eval bar held

Firetiger builds AI agents that monitor other companies' software and investigate outages; it ran on Claude by default. After moving its first three agent types to **DeepSeek**, its yearly AI bill fell from roughly **$606K to $231K** — a **62%** cut.<Citation
data={{
    type: "webpage",
    title: "Migrating from Claude to DeepSeek without breaking everything",
    "container-title": "Firetiger Blog",
    URL: "https://blog.firetiger.com/migrating-from-claude-to-deepseek-without-breaking-everything/",
    accessed: { "date-parts": [[2026, 9, 5]] }
  }}
/>

It wasn't a simple swap. The cheaper model was noticeably worse out of the box, and it took real tuning to close the accuracy gap — and even then, the cheap model tended to overthink and ramble, which quietly ate into the savings until they reined it in. Their own lesson: passing the accuracy test is the starting line, not the finish line.

## FrontierHarness: hold the model, vary the harness

One more field note flips the question around. Instead of comparing different AI models, RUNTA plugged the same one — **Kimi K3**, a Chinese model — into nine different agent tools and had each run the same 30 tasks.<Citation
data={{
    type: "webpage",
    title: "Introducing FrontierHarness Eval",
    "container-title": "RUNTA",
    URL: "https://runta.com/blog/introducing-frontierharness-eval/",
    accessed: { "date-parts": [[2026, 9, 5]] }
  }}
/> Same brain, different bodies. Every setup passed at roughly the same rate, but the cost per completed task ranged from about **$1** to about **$18** — a **17×** spread — depending purely on how the surrounding tool was built.

Which model you pick isn't the only cost lever. How the software around it uses that model can matter just as much.

## What the labs do next

When companies publish savings like these in public, the big labs feel it in their own pricing — hence that open price war among OpenAI, Anthropic, and their Chinese rivals mentioned up top.

Useful AI work keeps sliding down the cost curve, in public, with numbers attached. Companies are routing traffic to cheaper models, retraining their own smaller ones, and measuring exactly what it costs. What's left to judgment: figuring out which of your own tasks belong in that cheaper tier, and whether the savings are still worth it once you count the engineering it took to get there.

## References

<CitationsList citationFormat="apa" />
