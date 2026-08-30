

_Use the cheapest model that can do the job. Then measure whether it actually did._

In December 2023, we wrote about a text classifier that didn't work as a business.<Citation
data={{
    type: "webpage",
    "container-title": "Anth.us",
    title: "Maximize Profit, Not Intelligence",
    author: "Ryan Porter",
    URL: "https://anth.us/blog/maximize-profit-not-intelligence/",
    issued: { 'date-parts': [[2023, 12, 22]] }
  }}
/> The requirement was 95% accuracy at under $200 per million classifications. GPT-4 hit the accuracy easily and cost about $1,700 per million. A great demo and a dead venture.

The fix was not a better model. It was a worse one. We went down the ladder—GPT-3.5, then Ada 2 embeddings with logistic regression, then BERT running free in a Colab notebook—and only stopped at Word2Vec, where accuracy fell far enough that the remaining savings stopped being worth it. The thesis was <mark>use the dumbest model that the problem will bear</mark>, and the only way to find that point is to go one step too far and come back.

Here's that 2023 argument in one chart—the same four models, ranked by the only number that mattered:

<BlogImage
  images={props.pageContext.frontmatter.images}
  name="profit_per_million_GPT-4_GPT-3.5_Ada_2_BERT.png"
  className="centered"
  alt="Bar chart from the 2023 article showing profit per million classifications across GPT-4, GPT-3.5, Ada 2 and BERT, with the cheapest adequate model earning the most."
/>

GPT-4 is the most capable model on that chart and the only one that loses money—about $1,500 per million classifications. The winner was the least sophisticated model that still cleared the accuracy bar, and it happened to be free.

That discipline still holds in 2026. But two things changed underneath it, and together they changed what the question even is.

## The other half of the ratio

Our companion piece looked at one side of the equation: [the falling price of a fixed level of AI capability](/blog/ai-coding-cost-collapse-2026/). A 2026 MIT FutureTech and CSAIL paper estimated annual price declines of roughly 5× to 10× on its knowledge and math benchmarks. Its software-engineering estimate pointed in the same direction, but with less data and much wider confidence bands.<Citation
data={{
    type: "article-journal",
    title: "The Price of Progress: Price Performance and the Future of AI",
    author: ["Hans Gundlach", "Jayson Lynch", "Matthias Mertens", "Neil Thompson"],
    "container-title": "arXiv",
    DOI: "10.48550/arXiv.2511.23455",
    URL: "https://arxiv.org/abs/2511.23455",
    accessed: { 'date-parts': [[2026, 8, 17]] },
    issued: { 'date-parts': [[2025, 11]] }
  }}
/> That's the denominator.

Value is capability divided by cost, and the numerator moved too. The cheap tier didn't just get cheaper—it got _good_. That's the difference between a discount and a windfall. A discount lets you do the same work for less. <mark>A windfall changes which work is worth doing at all.</mark>

The practical question is how to collect that windfall: how to route work without creating a cleanup bill, and which neglected jobs just became worth automating.

## Now we have a scoreboard

In 2023, you had to build the cost-versus-accuracy chart yourself. Now public leaderboards such as BenchLM rank coding models by capability per dollar.<Citation
data={{
    type: "webpage",
    "container-title": "BenchLM",
    title: "Best Value LLM for Coding — Cost-Adjusted Rankings",
    URL: "https://benchlm.ai/best/best-value-coding",
    accessed: { 'date-parts': [[2026, 8, 30]] }
  }}
/>

That's useful, but it comes with two warnings.

First, dividing by a tiny price can make a weak model look like an incredible bargain. BenchLM says this directly: a model can lead on value and still miss the quality bar for your work.

Second, the rankings can age almost immediately. During this review, BenchLM still showed Luna's old $6 output price even though OpenAI cut it to $1.20 on July 30.<Citation
data={{
    type: "webpage",
    "container-title": "OpenAI",
    title: "Advancing the price-performance frontier with GPT-5.6",
    URL: "https://openai.com/index/advancing-the-price-performance-frontier-with-gpt-5-6/",
    accessed: { 'date-parts': [[2026, 8, 30]] },
    issued: { 'date-parts': [[2026, 7, 30]] }
  }}
/> A leaderboard that combines current benchmark scores with yesterday's price can reorder itself before you make a decision.

So use public rankings to find models worth testing. Don't treat them as a shopping list.

## Don't optimize the wrong ratio

But reading a ranking too literally will cost you money.

Value rankings naturally favor cheap models, even when their absolute performance isn't very good. Divide by a small enough price and almost anything looks like a bargain, including models you shouldn't point at a real repository. Even after filtering those out, a bare Score/$ ranking is a starting point, not a shopping list.

This is the part of the 2023 lesson that's easy to remember wrong. <mark>We didn't choose BERT because it was the cheapest. We chose it because it was the cheapest thing that still cleared the bar.</mark> Word2Vec was even cheaper, and we rejected it. The quality bar came first. We optimized the price underneath it.

Score-per-token is useful for finding models worth trying. It isn't the number you should run your shop on, because it ignores everything that happens after the tokens are billed: retries, tool calls, test runs, and your own attention. The number that matters is **accepted changes divided by model cost, tool cost, and human repair time.**

Which means four numbers, tracked per model and per task class:

- **Merge rate** — did the agent produce a change you accepted without major repair?
- **All-in cost per accepted change** — not token cost. Include retries and sandbox time.
- **Human minutes per accepted change** — usually the largest term, and the one nobody logs.
- **Escalation rate** — what fraction of tasks fell through the cheap tier to an expensive one?

A model that's six times cheaper and leaves you with fifteen minutes of cleanup isn't cheaper. In 2023, we ran an evaluation set and read the cost per million from a chart. Now you have to measure the real workflow, because agent cost is shaped by the task, not just the tokens.

## Two tiers, and when to leave one

For most coding work, you need two practical tiers.

The **value tier** is your default. It should be capable enough to navigate the repository, run tests, and revise its own work, but cheap enough to use freely on routine changes.

The **premium tier** buys a higher chance of success on difficult work. Use it when failure would be expensive: ambiguous requirements, cross-cutting architecture, fragile migrations, and security-sensitive changes.

Start ordinary work in the value tier. If it fails, don't just send the same prompt again. <mark>Retries aren't independent.</mark> A model that took a wrong path will happily take it again unless you change what it knows. Feed it the failing test, its previous diff, or the compiler error, then try again—but only two or three times.

Escalate when you have a concrete signal: CI is still red, tests still fail, the result isn't improving, or the change keeps spreading into more files. And skip the value tier when a task is obviously high-risk from the beginning.

The model names will change. The policy doesn't have to.

## The jobs that just crossed the line

This is where better value becomes more interesting than a smaller invoice.

Price isn't the only thing moving the break-even point. Agents are also getting better at staying on a task, recovering from failures, and completing longer sequences of work. Some jobs crossed the line because they became cheaper. Others crossed because an agent could finally finish them reliably. Most crossed because both things happened together.

In 2023, our classifier had a break-even point: $200 per million classifications. Below that, it was a business. Above it, it was a demo. Every neglected job on your backlog has the same kind of line. It's worth automating when the agent cost plus your review time is lower than the value of finally getting it done.

For a growing list of jobs, that answer quietly changed from “not worth it” to “obviously worth trying.”

<BlogImage
  images={props.pageContext.frontmatter.images}
  name="break-even-crossing.png"
  className="full"
  alt="A timeline from 2023 to 2026 showing more engineering and business tasks becoming practical to automate as model costs fall and reliable task horizons grow."
/>

_Nobody announces the crossing. You just notice the backlog stopped growing._

The best candidates aren't glamorous. They're well-scoped jobs that are valuable, repeatable, and easy for a person to review:

| Job                          | Run it      | Human gate                 |
| :--------------------------- | :---------- | :------------------------- |
| Documentation drift          | on merge    | review the pull request    |
| Dependency upgrades          | weekly      | review the diff and CI     |
| Missing-test backfills       | nightly     | review the pull request    |
| Changelogs and release notes | per release | edit before publishing     |
| Security-advisory triage     | on advisory | decide the severity        |
| Flaky-test quarantine        | nightly     | review the quarantine list |
| Mechanical migrations        | as needed   | review the generated diffs |
| Bug report to reproduction   | on report   | confirm the reproduction   |

Documentation is a good example because the tooling already exists. Scribe reads a codebase's structure, regenerates its AGENTS.md, and opens a merge request when the documentation drifts from the code.<Citation
data={{
    type: "software",
    "container-title": "GitHub",
    title: "Scribe: self-updating AGENTS.md from the code graph",
    URL: "https://github.com/brn-mwai/scribe",
    accessed: { 'date-parts': [[2026, 8, 17]] }
  }}
/>

The pattern is simple: a trigger, a bounded task sent to the cheapest model that clears the quality bar, and a proposed change for a person to review.

<mark>None of this auto-merges.</mark> The value lives in the gap between “nobody
had time” and “somebody reviewed it.” Removing the review doesn't create more value.
It just moves failures into production.

<BlogImage
  images={props.pageContext.frontmatter.images}
  name="the-backlog-worked.png"
  className="full"
  alt="A small pink-eyed robot filing paper under a warm desk lamp at night, surrounded by towering stacks of untouched paperwork in a dark office."
/>
<p
  style={{
    textAlign: "center",
    fontStyle: "italic",
    color: "#6B6B6B",
    marginTop: "-1em",
  }}
>
  The work nobody had time for, finally getting done.
</p>

## The security tax

Every job in that table creates a non-human identity with access to your repository, cloud, or inbox. That's not a reason to avoid the work. It's part of the cost.

The cheap tier is exactly where the temptation to skip scoping is strongest, because each agent feels too small to matter. Recent work on LLM-in-the-loop vulnerabilities found they are consistently harder to repair with automated agents than conventional flaws, with an average **10.8% lower Pass@1 rate**. Across twenty agent–model configurations, sixteen produced a <mark>0% Pass@1 result</mark> in at least one of three difficult categories: Generated Query Execution, Agent or Tool Action, and Model Output Rendering.<Citation
data={{
    type: "article-journal",
    title: "Towards Demystifying and Repairing LLM-in-the-Loop Vulnerabilities",
    "container-title": "arXiv",
    URL: "https://arxiv.org/abs/2605.28893",
    accessed: { 'date-parts': [[2026, 8, 17]] },
    issued: { 'date-parts': [[2026, 5]] }
  }}
/>

Scope each agent to the minimum access its job needs, treat everything it ingests—error logs, issues, docs, web pages—as untrusted input to a privileged executor, and keep an inventory. We take this up properly in the companion piece: [Bugonomics: The Flip Side of Cheap Coding](/blog/bugonomics-cheap-exploits-2026/).

## The Jevons resolution

Our first companion ended on an honest catch: cheaper coding does not lower your bill, because the cheapness is what makes the larger workflow affordable enough to depend on. Total spend rises even as unit price falls.

That's true, but it isn't necessarily bad. <mark>If value per dollar rose twenty times and your spending rose three times, you came out ahead.</mark> Spending more can be a perfectly sensible response to a price collapse. It's what “the same budget now buys a team” means in practice.

The problem isn't spending more. It's spending more _without measuring the return_. That's the 2023 mistake: shipping a beautiful GPT-4 classifier and waiting for the invoice to discover it wasn't a business. The four metrics above let you finish the sentence “we spend more on agents than we did last year” with a result instead of a shrug.

## What changed

In 2023, the answer was to find the dumbest model the problem would bear and stop paying for the rest. That still works, with one change: <mark>it's a routing policy now, not a one-time choice.</mark>

Start with the value tier. Escalate when the task or the evidence tells you to. Measure accepted work, including the time you spend fixing it. Then use the savings on boring, valuable jobs that never used to make economic sense—with a person still reading every diff.

Maximize value, not intelligence.

## Companion pieces

- [The Year Coding Became a Commodity](/blog/ai-coding-cost-collapse-2026/) — why the price of coding capability collapsed, and why the cost curve and the autonomy curve multiply.
- [Bugonomics: The Flip Side of Cheap Coding](/blog/bugonomics-cheap-exploits-2026/) — the same curve applied to breaking software, and the coding agent as an attack surface.
- [Maximize Profit, Not Intelligence](/blog/maximize-profit-not-intelligence/) — the 2023 original, where the ladder only went down.

## References

<CitationsList citationFormat="apa" />
