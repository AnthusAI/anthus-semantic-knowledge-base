# Chatticus: The AI Organization You Control

The best product I have tried in this category made the machinery disappear.
That was also the problem.

[Grok Bot](/blog/grok-bot-gave-my-coding-agents-a-boss/) gave me a roster of
named bots and a persistent computer without asking me to become the system
administrator. I gave bots jobs. They remembered those jobs, used applications,
talked to one another, assigned work to coding agents, and returned when they
needed me. The experience felt less like opening another AI chat and more like
directing a small organization.

But it was an organization inside somebody else's boundary. The service chose
the computer. Its public documentation did not expose a per-bot model choice. I
could shape a bot's role, tools, and instructions, but I could not decide that
one job belonged on a cheap model, another justified a premium model, and a
third had to remain inside my own cloud.

[OpenClaw](https://openclaw.ai/blog/introducing-openclaw) approaches the problem
from the other direction. It runs on a machine you choose—a laptop, homelab, or
VPS—and supports multiple models and providers. Your infrastructure, keys, and
data are the point. But somebody still has to provide, secure, patch, monitor,
and keep that machine available. For many people, “self-hosted” quietly becomes
a dedicated Mac Mini in a closet. For others it becomes a powerful agent on the
same computer they use for email, banking, development credentials, and
everything else.

The emerging choice looked unnecessarily stark: a polished agent organization
inside a vendor service, or control of the system with the operating burden
attached.

We are building Chatticus to occupy the missing space between them.

## Your agents. Your models. Your cloud.

Chatticus is a roster of persistent, named AI teammates designed to run on
computers in your AWS account or on hardware you control.

Each teammate has a role, memory, procedures, and a place in the organization.
They can share work without making the human carry every message. One teammate
can research a problem. Another can coordinate the response. Specialist agents
can perform the work, return evidence, revise weak results, and send the
integrated outcome back for human review.

That organizational shape comes from what I saw after coding agents moved
beyond pair programming. In [From pair programmer to executive](/blog/from-pair-programmer-to-executive/),
I described a Researcher agent reporting a bug to a Software Director. The
director assigned parallel coding work, reviewed what came back, rejected work
that did not clear its requirements, and returned the combined change to the
Researcher for checking.

The important part was not that the models had become employees. They had not.
It was that reporting lines and review procedures had become software design.
The human defined who could assign work, who could reject it, what evidence had
to travel with a result, and which decisions still required a person.

Chatticus turns that observation into a product architecture. The goal is not
autonomy at any cost. It is a controlled organization in which software can run
the day-to-day loop while the human keeps direction, boundaries, and final
responsibility.

## Model choice is an operating policy

Model selection is often presented as a dropdown in a chat window. In an agent
organization, it is closer to hiring and budget policy.

A research watch that scans a known set of sources does not necessarily need
the same model as an architect reviewing a security-sensitive migration. A
mechanical documentation update should not inherit the cost profile of a
complex incident investigation. A bot that works all day should not be locked
to the most expensive model because another bot occasionally needs it.

[The Year Coding Became a Commodity](/blog/ai-coding-cost-collapse-2026/)
described the economic shift underneath this. Useful coding capability became
cheaper at the same time agents became better at staying on a task. Those two
curves multiply: cheaper models make long loops affordable, and longer loops
change what is worth delegating.

The policy that follows is simple to state and harder to practice: use the
least expensive model that clears the quality bar, then escalate when the task
or the evidence says it needs more.

That does not mean choosing the lowest list price. [Maximize Value, Not Intelligence](/blog/maximize-value-not-intelligence/)
argued that the number worth measuring is accepted outcomes divided by model
cost, tool cost, and human repair time. A model that costs one sixth as much but
leaves fifteen minutes of cleanup behind is not cheaper.

Chatticus is designed so model and settings can be chosen by bot, role, or task
class. Routine work can start in a value tier. Difficult or consequential work
can begin in a premium tier, or escalate there after tests, evidence, or review
show that the cheaper attempt is not improving. Budgets can belong to the
organization instead of disappearing into one opaque default.

The first Chatticus implementation is OpenAI-first. The agent loop is separated
from the provider interface so the model vendor does not become the product
architecture; Amazon Bedrock is the next provider we expect to consider. The
long-term point is not a long logo wall. It is that the customer, not the bot
vendor, sets the model policy.

## Your cloud is part of the security design

Who owns the computer determines more than who pays the AWS bill. It determines
which network the agent inhabits, which credentials are present, what private
systems are reachable, and where the evidence of its work remains.

Public cloud connectors are useful when they fit. Structured APIs and MCP tools
are usually more reliable than operating a website through pixels, and
Chatticus prefers them when they exist. But a public SaaS connector should be an
option, not the only way into a private system.

A company may want an agent to reach an internal deployment service, a private
knowledge base, a database available only inside a VPC, or hardware attached to
a local machine. Chatticus workers pull work from the control plane. The
control plane does not open an inbound connection into the worker. That means a
worker inside a private network can use tools available there without turning
the network itself into a public endpoint.

The computer is also summoned, not assumed. A text-only turn can run without a
browser or desktop. When an agent first needs the browser, filesystem, or other
computer capability, the same turn can ask for it. The shared computer starts
when needed and returns to zero when nobody is working, rather than becoming an
always-on machine that quietly acquires production importance.

Local hardware still has a place. Some work genuinely depends on a development
environment, private network, USB device, or repository that lives on a nearby
machine. But that machine can be an intentional worker with an explicit role.
It does not have to be the laptop containing the rest of your life.

## Control survives the org chart

Every bot belonging to one user shares that user's computer. They can hand work
off because they share files, browser sessions, and command-line credentials.
That convenience is also a security fact: separate bot names and separate
screens are not security boundaries.

Chatticus puts the boundary around the user and the systems that user is
allowed to reach. Inside that boundary, bots have separate memory and roles.
Outside it, access is explicit. Sending, purchasing, publishing, deleting, and
production changes can stop at approval gates based on the actual action and
its arguments, not a reassuring summary written by the same model requesting
permission.

Approvals alone do not make an autonomous system safe. A manager agent can
accept bad work. A summary can distort evidence. A capable agent can work
around a blocker in a way nobody intended. The agency that keeps an
organization moving is the same agency that can amplify a bad decision.

The answer is not to pretend the risk disappears inside a friendly chat
interface. It is to make the operating structure visible and configurable:
durable records, scoped tools, review procedures, model policies, budgets, and
human gates. The organization should keep working without requiring the human
to route every handoff. It should not stop being accountable to the human.

## Three answers to the same problem

The category is young enough that these systems should not be flattened into a
feature checklist, but the control boundary is already clear.

**OpenClaw** gives the user broad control over models and infrastructure. The
user also supplies and operates the host. That is a good fit for people who
want to assemble and secure the system themselves.

**Grok Bot** provides the smoothest version I have used of named bots on a
persistent shared computer. [Its documentation](https://docs.x.ai/grok-bot/overview)
describes a vendor-operated cloud computer, bot collaboration, connectors,
browser use, and durable roles. Its [billing FAQ](https://docs.x.ai/grok-bot/faq)
describes plan allowances and on-demand model and token usage, but the public
configuration documentation does not describe choosing a different model for
each bot.

**Chatticus** is designed for the customer who wants the organization without
surrendering the boundary: customer AWS or intentional local hardware,
role-specific model policy, private integrations, durable work, and explicit
approval gates.

That does not make Chatticus “OpenClaw with a nicer interface” or “Grok Bot in
another cloud.” The placement changes the system. Customer-owned
infrastructure, pull-based private workers, per-role model economics, and
approval enforcement are not deployment details added after the product. They
are the product.

## What we are building

Chatticus is under active development. Its thin cloud turn path already proves
the durable transcript, named bots, per-turn streaming, worker claims, and the
ability for a text-only agent to begin before a computer is ready. The complete
web product, computer handoff, approval experience, customer deployment, and
multi-provider routing are still being built.

That distinction matters. This article describes the product direction and the
architecture supporting it, not a claim that every part is available today.
We will publish it when Chatticus launches, after the software can carry the
promise.

The promise itself is compact:

**Your agents. Your models. Your cloud.**

Not because ownership is a virtue by itself. Because an organization of agents
is powerful enough that you should be able to decide who works, what they cost,
where they run, what they can touch, and when they must stop and ask you.
