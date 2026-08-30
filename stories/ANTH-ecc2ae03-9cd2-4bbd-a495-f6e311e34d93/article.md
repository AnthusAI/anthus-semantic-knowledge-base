

_Coding agents didn't just get better. Useful coding capability got dramatically cheaper._

That's the change I think we've underestimated. For years, the AI-coding story was about capability: bigger context windows, better tool use, and higher benchmark scores. That story is still true at the frontier. But the more practical story in 2026 is that last year's useful coding capability moved way down the cost curve.

Since Andrej Karpathy coined "vibe coding" in February 2025, the cost of reaching roughly the same benchmark performance has fallen by close to an order of magnitude for software-engineering tasks. At the same time, agents have gotten much better at staying on a task for hours instead of answering one prompt and stopping.

Those aren't two separate trends. They multiply. <mark>Cheaper models make long agent loops affordable, and longer agent loops change what you can delegate.</mark>

## The narrative flipped

You can see it in everyday work: boilerplate, lint fixes, tests, code explanations, and well-scoped repository issues. A model doesn't need to be the smartest model available to be the right model for those jobs. It needs to clear the quality bar at a sensible cost.

So the useful question isn't, "What's the smartest model?" It's: <mark>what does it cost to complete this kind of engineering task at the quality I need?</mark>

## Measure the work, not the tokens

A model can look cheap per token while burning a huge number of tokens to finish a task. Agent loops add retries, tool calls, test runs, and context re-ingestion. So the number that matters isn't the list price. It's what you spend to get useful work done.

A March 2026 paper from MIT FutureTech and CSAIL gives us a better way to measure this.<Citation
data={{
    type: "article-journal",
    title: "The Price of Progress: Price Performance and the Future of AI",
    author: ["Hans Gundlach", "Jayson Lynch", "Matthias Mertens", "Neil Thompson"],
    "container-title": "arXiv",
    DOI: "10.48550/arXiv.2511.23455",
    URL: "https://arxiv.org/abs/2511.23455",
    accessed: { 'date-parts': [[2026, 8, 16]] },
    issued: { 'date-parts': [[2025, 11]] }
  }}
/> The authors combine historical inference-price data with benchmark token use from Epoch AI's Benchmark Hub and evaluate **price-adjusted performance** across knowledge, math, and software engineering—not just raw API rates.

Their headline finding: the cost of a given benchmark-performance level on the Pareto frontier has been falling roughly **5× to 10× per year** across GPQA-Diamond, AIME, and SWE-bench Verified. For software engineering specifically, SWE-bench Verified's sample is still small, so confidence intervals are wide—but the estimated annual price reduction is in the same ballpark, about **4.7× per year** on the cost-performance frontier.<Citation
data={{
    type: "article-journal",
    title: "The Price of Progress: Price Performance and the Future of AI",
    author: ["Hans Gundlach", "Jayson Lynch", "Matthias Mertens", "Neil Thompson"],
    "container-title": "arXiv",
    DOI: "10.48550/arXiv.2511.23455",
    URL: "https://arxiv.org/html/2511.23455v2",
    accessed: { 'date-parts': [[2026, 8, 16]] },
    issued: { 'date-parts': [[2025, 11]] }
  }}
/>

<BlogImage
  images={props.pageContext.frontmatter.images}
  name="price-of-fixed-capability.png"
  className="full"
  alt="Log-scale chart of the cost to reach a fixed benchmark score from 2023 to 2026, falling roughly five to ten times per year across GPQA-Diamond, AIME, and SWE-bench Verified."
/>

That method matters because the researchers price a full **benchmark run**—including input, output, reasoning, and cached tokens. They're measuring the cost of reaching a fixed result, not claiming that models somehow got ten times smarter.

For SWE-bench Verified, the estimate is about **4.7× cheaper per year**, although the sample is small and the confidence interval is wide. Compounded over the roughly 18 months since "vibe coding" was coined, that's close to the order-of-magnitude change in the opening. The exact multiplier is less important than the direction: the same useful capability keeps getting cheaper, fast.

<BlogImage
  images={props.pageContext.frontmatter.images}
  name="ai-coding-cost-collapse-2026.png"
  className="centered"
  alt="Three falling log-scale cost curves for GPQA-Diamond, AIME, and SWE-bench Verified, annotated 5x to 10x cheaper per year, titled Same coding capability, falling price."
/>

## What one resolved task costs

For engineering leaders, the MIT paper is the academic anchor. For day-to-day tool selection, agent benchmarks are more legible.

_Dialogue-SWEBench_, a June 2026 benchmark from UC Santa Cruz, reports **dollars per resolved interactive repository task** on a dialogue-driven software engineering benchmark—not list price.<Citation
data={{
    type: "article-journal",
    title: "A Benchmark for Dialogue-Driven Coding Agents",
    "container-title": "arXiv",
    URL: "https://arxiv.org/html/2606.13995v1",
    accessed: { 'date-parts': [[2026, 8, 16]] },
    issued: { 'date-parts': [[2026, 6]] }
  }}
/> That's much closer to what you actually pay when a coding agent does the work.

| Model / agent configuration          | Resolved tasks | Cost per task |
| ------------------------------------ | -------------: | ------------: |
| GPT-5 mini + schema-guided agent     |          58.8% |         $0.24 |
| GPT-5 + schema-guided agent          |          58.0% |         $0.86 |
| Qwen 3 Coder + schema-guided agent   |          32.3% |         $0.13 |
| Devstral 2 Small + interactive agent |          42.2% |         $0.30 |

Here's the interesting part: **GPT-5 mini rivals GPT-5 at a fraction of the cost** in this setting. The larger model still does better on the hardest tasks, but it's worse on simpler work—partly because it asks unnecessary questions and runs longer interactions.

Here's the point: for a lot of everyday engineering work, the best-value model is moving **down** the capability ladder faster than most developers are changing their defaults.

## The second curve: agents work longer

The price collapse didn't arrive alone. Agents also got better at working autonomously for longer instead of answering one prompt and stopping.

The capability curve is real and dated. OpenAI's December 2025 Codex release was, in their own words, "the moment that people began to believe that using autonomous coding agents could be reliable"—driven by "a huge jump in how long the model could reliably follow instructions." A single 25-hour uninterrupted run produced 30,000 lines of code on 13 million tokens, with the model running its own verification steps at every milestone.<Citation
data={{
    type: "webpage",
    "container-title": "OpenAI Developers",
    title: "Run long horizon tasks with Codex",
    URL: "https://developers.openai.com/blog/run-long-horizon-tasks-with-codex",
    accessed: { 'date-parts': [[2026, 8, 16]] }
  }}
/> METR's time-horizon metric—the length of task a frontier model can complete with 50% reliability—has been **doubling roughly every seven months since 2019**.<Citation
data={{
    type: "webpage",
    "container-title": "Addy Osmani",
    title: "Long-running Agents",
    URL: "https://addyosmani.com/blog/long-running-agents/",
    accessed: { 'date-parts': [[2026, 8, 16]] }
  }}
/>

While the price of a fixed level of coding was falling, the length of task an agent could stay on was doubling roughly every seven months. Two exponential curves were moving at the same time.

<BlogImage
  images={props.pageContext.frontmatter.images}
  name="two-multiplying-curves.png"
  className="full"
  alt="Two stacked log-scale charts: the price of fixed coding capability falls while the length of task an agent can complete at fifty percent reliability rises. Together, the trends make more work practical to automate."
/>

These curves don't just make existing automation cheaper. Together, they move the boundary of what can be automated. As agents stay reliable for longer and the cost of those long loops falls, tasks that were impractical a few months ago cross into ordinary use.

## Why the two effects multiply

Long autonomous loops are exactly the kind of token-heavy work that only becomes practical when model usage gets cheap.

An agent loop isn't a chat. A chatbot returns one response. An agent plans, calls tools, verifies, retries, and re-reads its accumulated context on every step. Stanford's Digital Economy Lab gave this a name: the **"pricey context snowball."** Input tokens, not output tokens, drive the cost of agentic work, because the agent re-sends a growing context window on every step of a multi-step task.<Citation
data={{
    type: "webpage",
    "container-title": "Stanford Digital Economy Lab",
    title: "How are AI agents spending your tokens?",
    URL: "https://digitaleconomy.stanford.edu/news/how-are-ai-agents-spending-your-tokens/",
    accessed: { 'date-parts': [[2026, 8, 16]] }
  }}
/> A 50-turn session can run a 25:1 ratio of input to output tokens. The cost grows with the loop length, not the answer length.

<div className="blockDiagram">
  <div className="title">The Context Snowball</div>
  <div className="container">
    {[
      ["Turn 1", 2, "~4K tokens"],
      ["Turn 10", 35, "~70K tokens"],
      ["Turn 25", 75, "~150K tokens"],
      ["Turn 50", 100, "~200K (full window)"],
    ].map(([label, pct, note]) => (
      <div
        key={label}
        style={{
          display: "flex",
          alignItems: "center",
          gap: "0.7em",
          margin: "0.4em 0",
          fontSize: "0.9em",
        }}
      >
        <span style={{ width: "5em", flexShrink: 0 }}>{label}</span>
        <span
          style={{
            flex: 1,
            height: "12px",
            borderRadius: "6px",
            backgroundColor: "#E6E6E6",
            display: "inline-block",
            overflow: "hidden",
          }}
        >
          <span
            style={{
              display: "block",
              height: "100%",
              width: `${pct}%`,
              backgroundColor: "#9B1B30",
              borderRadius: "6px",
            }}
          />
        </span>
        <span
          style={{
            width: "11em",
            flexShrink: 0,
            textAlign: "right",
            color: "#6B6B6B",
            whiteSpace: "nowrap",
          }}
        >
          {note}
        </span>
      </div>
    ))}
    <p
      style={{
        textAlign: "center",
        fontStyle: "italic",
        color: "#6B6B6B",
        marginTop: "0.9em",
        marginBottom: 0,
      }}
    >
      Every turn re-sends the whole conversation. On a 50-turn session, that's
      roughly
      <b> 25 input tokens for every 1 output token</b> &mdash; illustrative, not
      a measured trace.
    </p>
  </div>
</div>

Agent hierarchies can make that curve even steeper. A single top-level task can expand into a tree of agents, each carrying its own context window.<Citation
data={{
    type: "webpage",
    "container-title": "Web Developer",
    title: "Claude Code Adds Nested Subagents and Experimental Agent Teams",
    URL: "https://webdeveloper.com/news/claude-code-nested-subagents-agent-teams/",
    accessed: { 'date-parts': [[2026, 8, 16]] }
  }}
/> Delegating a milestone to a hierarchy of agents is token-expensive by design.

That's why this isn't just a savings story. <mark>Cheaper model usage made longer loops and agent hierarchies affordable.</mark>

A July 2026 paper, _The Harness Effect_, measures the second lever directly: holding the model constant, swapping in a better orchestration layer cut cost per task by 41%—more than switching between the cheapest and most expensive model did. The harness moved cost per task more than model choice did.<Citation
data={{
    type: "article-journal",
    title: "The Harness Effect: How Orchestration Design Sets the Token Economics of Enterprise Agentic AI",
    "container-title": "arXiv",
    URL: "https://arxiv.org/html/2607.06906",
    accessed: { 'date-parts': [[2026, 8, 16]] },
    issued: { 'date-parts': [[2026, 7]] }
  }}
/> Better orchestration and cheaper models arrived together. That's what made this kind of delegation practical.

## What this changes

This is where the cost collapse becomes more interesting than a cheaper invoice. The unit of work starts to change. Instead of asking an assistant for a few lines, a developer can hand a patch-shaped problem to agents that read the repo, edit it, test it, and return work for review.<Citation
data={{
    type: "webpage",
    "container-title": "DevTools Academy",
    title: "State of AI Coding Agents 2026",
    URL: "https://www.devtoolsacademy.com/blog/state-of-ai-coding-agents-2026",
    accessed: { 'date-parts': [[2026, 8, 16]] }
  }}
/> The pieces are already familiar: isolated subagents, git worktrees that keep parallel changes apart, shared task lists, and batch commands that fan out mechanical work.<Citation
data={{
    type: "webpage",
    "container-title": "Anthropic",
    title: "Claude Code subagents and Agent Teams",
    URL: "https://docs.claude.com/en/docs/claude-code/sub-agents",
    accessed: { 'date-parts': [[2026, 8, 16]] }
  }}
/> The harnesses matured. What was missing was the economics.

As the cost per agent falls, the same budget can support several agents working in parallel. Three to five concurrent agents is a common practical range, while genuinely parallel jobs can fan out much further.<Citation
data={{
    type: "webpage",
    "container-title": "Totalum",
    title: "Claude Code subagents: the 2026 production playbook",
    URL: "https://www.totalum.app/blog/claude-code-subagents-totalum",
    accessed: { 'date-parts': [[2026, 8, 16]] },
    issued: { 'date-parts': [[2026, 8]] }
  }}
/>

<BlogImage
  images={props.pageContext.frontmatter.images}
  name="robot-workstation-army.png"
  className="full"
  alt="Endless rows of identical pink-eyed robots tethered by cable to developer workstations, monitors glowing with code, in a dim teal-lit industrial office."
/>
<p
  style={{
    textAlign: "center",
    fontStyle: "italic",
    color: "#6B6B6B",
    marginTop: "-1em",
  }}
>
  The same budget, a different unit of work.
</p>

The real change isn't simply that coding got cheaper. It's that the same budget can support a different way of working, including whole milestones handed to a supervised group of agents. Augment describes the shift as moving the unit of engineering work from the pull request to the project.<Citation
data={{
    type: "webpage",
    "container-title": "Augment Code",
    title: "Accelerating large engineering projects with Cosmos",
    URL: "https://www.augmentcode.com/blog/accelerating-large-engineering-projects-with-cosmos",
    accessed: { 'date-parts': [[2026, 8, 16]] }
  }}
/>

## Chore tasks that finally get done

Every engineering team has a list of work that is obviously valuable and never gets done. Documentation drifts. Dependency upgrades sit in the backlog until something breaks. Tests get written for new code and never backfilled for old code. Security advisories get triaged when they're loud enough. Changelogs get written when someone remembers.

These are the tasks that parallel agents are best at, because they are well-scoped, repeatable, and low-stakes enough to run under human review rather than by hand. And the tooling for them is now shipping.

Documentation maintenance is the clearest example. Tools like Scribe read the actual structure of a codebase from the code graph, regenerate an accurate `AGENTS.md`, and open a merge request when the docs have drifted from the code—then keep watching and open another MR the next time they drift.<Citation
data={{
    type: "software",
    "container-title": "GitHub",
    title: "Scribe: self-updating AGENTS.md from the code graph",
    URL: "https://github.com/brn-mwai/scribe",
    accessed: { 'date-parts': [[2026, 8, 16]] }
  }}
/> The pattern generalizes: a CI drift check compares the merge diff against the prose, an agent drafts a correction, and a human reviews and merges. The same loop applies to dependency upgrades, test backfills, changelog generation, and triaging security advisories.<Citation
data={{
    type: "webpage",
    "container-title": "Augment Code",
    title: "Self-Updating Documentation: Docs Agents Keep in Sync",
    URL: "https://www.augmentcode.com/guides/self-updating-documentation-docs-agents-sync",
    accessed: { 'date-parts': [[2026, 8, 16]] }
  }}
/>

The catch is that none of this should auto-merge. The agents do the work that used to never get done; a human still decides whether it ships. <mark>The productivity dividend lands in the gap between "nobody had time" and "somebody reviewed it."</mark>

## The bottleneck moves

When implementation gets cheaper, specification matters more. You have to define what "done" means, notice when working code is still the wrong code, and make good decisions quickly enough to keep several streams of work pointed in the same direction.

That's the human payoff of the cost collapse. It isn't just that you can spend less. A person or a small team can attempt work that used to require a much larger group. But the part left to the human is still the hard part: judgment.

## The Jevons catch

But there's another side to this, and it's the same paradox we saw when content got cheap.

When a resource gets cheaper, you don't use less of it. You use more of it, because the cheapness enables new uses that weren't viable before. William Stanley Jevons noticed it with coal in 1865: Watt's efficient steam engine didn't reduce coal consumption, it increased it, because steam engines became economical for factories, trains, and pumps that would never have used them before.<Citation
data={{
    type: "book",
    title: "The Coal Question: An Inquiry Concerning the Progress of the Nation, and the Probable Exhaustion of Our Coal-Mines",
    author: "William Stanley Jevons",
    publisher: "Macmillan and Co.",
    issued: { "date-parts": [[1865]] }
  }}
/>

Coding follows the same curve. Demand expands in two directions: we run familiar automations more often, and we automate longer, more complicated work that agents couldn't reliably finish before. The unit price falls while both the number and the size of viable tasks grow, so total spending can still rise.

<mark>
  Cheaper coding doesn't necessarily lower your bill. It changes what you can
  afford to attempt.
</mark> That's not waste. It's the predictable result of turning an expensive capability
into a commodity.

## The flip side of this curve

The same curve that lowers the cost of _writing_ software also lowers the cost of _breaking_ it. Cheap coding agents also create a new attack surface.

That's a separate story, and we take it up in the companion piece: [Bugonomics: The Flip Side of Cheap Coding](/blog/bugonomics-cheap-exploits-2026/). The short version: the productivity dividend and the vulnerability dividend are the same curve, and you cannot have one without the other.

## What became scarce

What changed in 2026 wasn't just that coding models got smarter. <mark>Useful coding capability got cheaper at the same time agents learned to work for longer.</mark> Cheap models made long loops affordable. Long loops made larger units of work delegable. The same budget began to buy a team instead of a single assistant.

That doesn't make the human less important. It moves the scarce resource from implementation toward specification, taste, and judgment. The code became a commodity. Knowing what should be built—and whether the result is actually good—didn't.

## Companion pieces

- [Maximize Value, Not Intelligence](/blog/maximize-value-not-intelligence/) — the practical half: which providers to buy, how to route between tiers, and which jobs just became worth automating.
- [Bugonomics: The Flip Side of Cheap Coding](/blog/bugonomics-cheap-exploits-2026/) — the same curve applied to breaking software.

## References

<CitationsList citationFormat="apa" />
