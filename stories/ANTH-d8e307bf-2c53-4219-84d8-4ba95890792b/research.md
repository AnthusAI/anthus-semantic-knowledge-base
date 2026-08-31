# Research

## Firsthand Anth.us foundation

- **Grok Bot Gave My Coding Agents a Boss** establishes the category through
  direct use: named bots, one shared persistent computer, bot-to-bot handoffs,
  Cursor dispatch, and the tension between useful agency and disobedience.
  `/blog/grok-bot-gave-my-coding-agents-a-boss/`
- **From pair programmer to executive** establishes the organizational thesis:
  the human defines roles, boundaries, and acceptance requirements while a
  manager agent runs part of the day-to-day loop.
  `/blog/from-pair-programmer-to-executive/`
- **The Year Coding Became a Commodity** establishes that falling cost and
  longer reliable task horizons multiply, making persistent agent teams
  economically practical. `/blog/ai-coding-cost-collapse-2026/`
- **Maximize Value, Not Intelligence** establishes the routing policy: use the
  least expensive model that clears the quality bar, measure accepted work and
  repair time, and escalate when evidence demands it.
  `/blog/maximize-value-not-intelligence/`

## OpenClaw facts

Primary: https://openclaw.ai/blog/introducing-openclaw

- Current name is OpenClaw; the official introduction describes the path from
  Clawd to Moltbot to OpenClaw.
- It is an open agent platform that runs on a machine the user chooses:
  laptop, homelab, or VPS.
- Its stated position is the user's infrastructure, keys, and data.
- It supports multiple models and providers.
- The user is responsible for installing, operating, and hardening the host.
- Its own introduction says prompt injection remains unsolved and points users
  to security best practices.

Use this as evidence of ownership plus operating burden, not as evidence that
OpenClaw is categorically unsafe.

## Grok Bot facts

Primaries:

- https://docs.x.ai/grok-bot/overview
- https://docs.x.ai/grok-bot/faq
- https://docs.x.ai/grok-bot/bots

- Grok Bot provides named, persistent bots and a vendor-operated persistent
  cloud computer.
- Bots share files, browser sessions, and logins on one user-scoped computer;
  separate bot screens are not security boundaries.
- Bots can collaborate, use connectors, and act through a browser.
- The FAQ describes plan availability, included weekly usage, and optional
  on-demand usage billed from model and token cost.
- Public documentation reviewed on 2026-08-31 does not document per-bot model
  selection. State that precisely; do not overclaim that no internal variation
  exists.

Use this as evidence of a polished vendor-operated experience, not as an
argument that SaaS is inherently wrong.

## Chatticus repository facts

Sources: Chatticus `README.md`, `docs/PRODUCT.md`, `docs/ARCHITECTURE.md`,
`docs/STACK.md`, `docs/MESSAGING.md`, and `docs/THREAT_MODEL.md` as of
2026-08-31.

- A bot is a persistent named teammate with its own memory.
- One user-scoped Linux computer is shared by that user's bots. It may run on
  Fargate, EC2, or local Docker.
- Workers pull work; the control plane does not reach into private workers.
- The computer is summoned when a tool needs it rather than kept running by
  default.
- Consequential actions are approval-gated.
- Structured APIs, MCP servers, and connectors are preferred where they exist;
  the browser is a fallback. Private custom tools can be deployed inside the
  customer's boundary.
- The current implementation uses OpenAI first. Amazon Bedrock is a later
  provider option. Per-bot model and cost routing is positioning and intended
  design, not a shipped capability today.
- The front door and the thin-turn path are implemented; the complete web app,
  private customer deployment experience, full computer handoff, approvals,
  and multi-provider routing remain under development.

## Safe comparison language

The useful frame is a tradeoff triangle:

- OpenClaw: customer control, customer operation.
- Grok Bot: vendor operation, vendor boundary.
- Chatticus: productized operation inside a customer-controlled boundary.

The Chatticus claim is a design target until launch. Use future-facing language
and include an explicit under-development statement.

## Image

Generated original illustration saved as
`images/chatticus-ai-organization-you-control.png`: one human director, one
manager agent, three worker agents, private infrastructure, and a visible gate.
No text, logos, or competitor marks.
