# Idea

## Working title

Codex as open harness (approval + sandbox)

## Pitch

OpenAI positions Codex as model + open harness; leftover human approval/sandbox as product, not chat UI.

Codex is framed as model + **open harness** + surfaces. The harness (open-source) owns conversation state, tool use, sandbox/approval policies, streaming, and multi-turn carry. Integration paths: `codex exec` for bounded jobs, SDK for programmatic workflows, **app-server** for products that must handle approval requests and persistent threads. Framing: the reusable asset is the execution loop and leftover human approval gate, not the chat UI.

Anthus angle: agent orgs need an inspectable policy boundary (approve / sandbox / interrupt) as first-class software, not a model personality trait. Angle is **harness + approval protocol**, not “agents went remote again.”

## Audience

Builders productizing agent loops who need approval/sandbox as inspectable software, not a chat skin.

## Primary cite

- https://developers.openai.com/blog/codex-as-a-platform — August 19, 2026 — OpenAI Developers. Establishes Codex as model + open harness where approval/sandbox policies are product surface, not chat UI.

## Split / siblings

Distinct from:

- coding-agents-went-remote-2026 (`da430a`) — remote agents story. Keep this angle on **open harness + approval protocol**, not “agents went remote again.”
- give-an-agent-a-tool / related tool-surface pieces — link if needed; do not retell.

## Post shape

Short post (harness > chat) or listicle of gate types teams should expose as code.

## Boundaries

- Stay at idea until assignment
- Do not invent article.md
