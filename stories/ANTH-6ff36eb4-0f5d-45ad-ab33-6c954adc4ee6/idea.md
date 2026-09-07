# Idea

## Working title

Warp: skills that improve via human-merged PRs

## Pitch

Outer improver skill reads PR/issue feedback and opens PRs that edit inner skills; humans merge. Leftover authenticity gate is merge review.

Warp’s agents were noisy until feedback stopped being session-local. Pattern: **inner skill** (domain how-to as a file) → humans leave feedback where work already happens (PR/issue comments) → scheduled **outer improver skill** reads that corpus and opens a PR that edits the inner skill. Humans merge; next run inherits the change. Demo: triage agent missed a “ready to spec” label; maintainer explained why; improver proposed a minimal skill patch. Tips from the piece: principles not exhaustive rules; explain why; keep skills small; put effort into the reusable improver; never auto-merge wrong feedback.

Anthus angle: leftover gate is the skill PR review, not babysitting every agent turn — hill-climbing the factory definition itself.

## Audience

Teams running agent orgs who want skills-as-code with a human authenticity gate at merge time, not another swarm tour.

## Primary cite

- https://claude.com/blog/how-warp-builds-self-improving-agents-on-claude — August 26, 2026 — Michael Segner (Anthropic / Claude blog); Warp (Zach Lloyd). Establishes outer improver skill → PR that edits inner skills → human merge as the authenticity gate.

## Split / siblings

Distinct from:

- ralph-loops (`78500a`) — the loop / loop engineering. This is outer-loop **skill evolution** with merge gates, not a single agent bash loop.
- agent-zoo farm tours — skip the swarm spectacle; keep the merge-gate pattern.
- Grok Bot Gave My Coding Agents a Boss (`629b47`) — hierarchical boss; different gate.

## Post shape

Short post (pattern diagram + one triage vignette) or listicle of Warp’s skill-writing rules mapped to Anthus practice.

## Boundaries

- Stay at idea until assignment
- Do not invent article.md
