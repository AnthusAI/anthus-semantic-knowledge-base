# Idea

## Working title

Agent-loop TDD may be theater (Birgitta)

## Pitch

Small eval — in-loop TDD didn’t clearly win; many human TDD benefits don’t transfer when the agent writes and “proves” red. Push authenticity to mutation tests / Approved Scenarios.

Birgitta Böckeler’s exploratory eval: Sonnet 4.6 implements greenfield tasks with vs without full in-loop TDD; Opus judges quality blind. No clear win for TDD; non-TDD often ranked higher on design/tests. Hypothesis: TDD instructions block upfront design, so architecture emerges from locally minimal red/green steps. Many human TDD benefits (fear management, sitting with friction, watching *why* a test went red) don’t transfer when the agent both writes and “proves” the red. Cost: ~3×+ token volume (directional; cache caveats). Recommendation: stop prompting agents to TDD; invest in mutation testing, structural reviews, and human-gated approaches like Ivett Ördög’s **Approved Scenarios**.

Anthus angle: don’t cargo-cult human rituals into the agent loop — move authenticity to outcome oracles and human approval of frozen scenarios.

## Audience

Engineers tempted to prompt “always TDD” into agent loops; readers of cybernetic-development who need the productive tension.

## Primary cite

- https://martinfowler.com/articles/exploring-gen-ai/tdd-in-the-agent-loop.html — August 10, 2026 — Birgitta Böckeler (Thoughtworks / *Exploring Gen AI*). Establishes that in-loop TDD did not clearly win a small blind eval and that many human TDD benefits fail to transfer to agents.

## Split / siblings

Tension with (link, do not flatten):

- cybernetic-development — praises BDD/TDD as externalized System 2. This piece is a productive **tension**, not a dupe. Frame as “discipline for humans / oracles for agents.”

Not a rewrite of craft-process cheerleading; push leftover authenticity to mutation tests and Approved Scenarios.

## Post shape

Essay companion (disagree-and-extend) or short post: “TDD for agents is theater; mutation + approved scenarios aren’t.”

## Boundaries

- Stay at idea until assignment
- Do not invent article.md
