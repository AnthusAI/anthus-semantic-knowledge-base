# Idea

## Working title

Spotify Portal: shunt Claude Code I/O (~90%)

## Pitch

Companion receipt to thrifty-tokenmaxxing — Spotify PreToolUse hooks block oversized reads and force Gemini Flash workers; ~90% bulk-read token savings. Not another commodity-receipts list row.

Most coding-agent spend is I/O, not reasoning: bulk file reads and boilerplate generation fed to a frontier model. Spotify built Portal “AiKA modes” (`bulk-reader`, `code-writer`) on Gemini 2.5 Flash, then a Claude Code plugin (`shunt`) with PreToolUse hooks that **block** oversized Reads / bash cats and force delegation. Benchmarks on a Java monorepo report ~90% mean savings on bulk-read scenarios. Core claim: routing is architecture (hooks), not advisory CLAUDE.md. Failure modes matter: can’t safely delegate editing or deep reasoning; latency and a line threshold matter.

Anthus angle: the expensive model is a scarce executive; grunt I/O should be shunted to Flash-class workers, and enforcement must be mechanical.

## Audience

Engineers already tokenmaxxing agent loops who need a checkable hook-enforced receipt, not another “use a cheaper model” tip.

## Primary cite

- https://engineering.atspotify.com/2026/9/portal-by-spotify-cut-my-claude-code-token-usage-by-90 — early September 2026 (Spotify Engineering; HN + DEV coverage ~Sep 5). Establishes PreToolUse hook shunt to Gemini Flash workers with ~90% bulk-read token savings.

## Split / siblings

Link (do not retell):

- thrifty-tokenmaxxing (`c28002`) — click-path catalog / vendor traps; this is the Spotify hook receipt companion.
- never-use-fast (`8bde91`) — Fast surcharge; not this.
- maximize-value-not-intelligence (`0b6e8b`) — buying philosophy; link only.

Distinct from:

- commodity-receipts (`c9ed5b`) — model-swap listicle rows. This is **not** another commodity-receipts list row; it is a companion receipt to thrifty-tokenmaxxing about hook-enforced I/O shunt.

## Post shape

Short post or essay companion to thrifty-tokenmaxxing — lead with the Spotify receipt, then generalize the three-layer pattern (hooks → scripts → skills).

## Boundaries

- Stay at idea until assignment
- Do not invent article.md
