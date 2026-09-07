# Idea

## Working title

Refactoring as agent opex (83% input-token cut)

## Pitch

Giles Edwards-Alexander — same change after Fowler-style splits: 159k → 27k input tokens. Structure is an opex lever for agent work.

Author vibe-built a ~150kLoC agent-written app; one Firestore data-access file grew to 17,155 lines. Experiment: after each strict Fowler-style refactoring step, a **fresh** sub-agent runs the same representative change; measure input tokens. Same task fell from 159,564 → 27,360 input tokens (**83%**). Total LoC barely changed — savings came from boundaries that let the agent **locate** a smaller working set. Agents were bad at inventing the refactor plan without human guidance; the expensive cliff happened when the monolith finally split into per-trait store files.

Anthus angle: clean structure is now an **opex** lever for agent work, not just human maintainability — refactor spend tokens once to shrink every future change.

## Audience

Teams paying agent I/O bills who treat refactoring as “nice for humans” rather than a recurring token-cost lever.

## Primary cite

- https://martinfowler.com/articles/exploring-gen-ai/refactoring-economic-benefit.html — July 30, 2026 — Giles Edwards-Alexander (*Exploring Gen AI*). Establishes 159k → 27k input-token cut (~83%) for the same change after Fowler-style splits; structure as agent opex.

## Split / siblings

Pairs with:

- Spotify Portal shunt (`3da04d`) / thrifty-tokenmaxxing (`c28002`) — *route* dumb I/O cheap *and* *structure* so the executive reads less.
- code-based-hill-climbing (`f10464`) — complements; not covered as a published Anth.us title on refactoring-as-opex.

Distinct from a generic “refactor more” tip; the news is the measured token locality cliff.

## Post shape

Short post with the before/after table, or essay companion pairing with Spotify / thrifty-tokenmaxxing.

## Boundaries

- Stay at idea until assignment
- Do not invent article.md
