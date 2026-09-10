# Agent Instructions

## Project management with Kanbus

Use Kanbus for task management.
Why: Kanbus task management is MANDATORY here; every task must live in Kanbus.
When: Create/update the Kanbus task before coding; close it only after the change lands.
How: See CONTRIBUTING_AGENT.md for the Kanbus workflow, hierarchy, status rules, priorities, command examples, and the mistakes to avoid. Never inspect project/ or issue JSON directly (including with cat or jq); use Kanbus commands only.
Performance: Prefer kbs (Rust) when available; kanbus (Python) is equivalent but slower.
Warning: Editing project/ directly violates The Way. Do not read or write anything in project/; work only through Kanbus.
Git / PR policy: Rules for product-code commits, branch names, pull requests, and human approval live in this repository's AGENTS.md (outside this Kanbus section). CONTRIBUTING_AGENT.md covers Kanbus board mechanics such as `kbs commit`; follow AGENTS.md for product code and git workflow.

## This repository

Public Kanbus newsroom for Anth.us articles. GitHub: https://github.com/AnthusAI/anthus-semantic-knowledge-base. Publication board key `ANTH`.

When Ryan says **the newsroom board** or **the Papyrus board**, he means this repo, not the Papyrus product board (`PPY`) and not the Gatsby site-ops board.

Do not use `~/Projects/Papyrus/pods/anthus-blog` (often a stale empty template). Prefer this standalone clone: `/workspace/anthus-semantic-knowledge-base` on the box, or `~/Projects/anthus-semantic-knowledge-base` on laptops.

## Git commits and pull requests (newsroom board)

- Shared board state lives on **`main`**. Prefer `kbs commit` for Kanbus issue/event board commits when available; otherwise commit only the Kanbus-produced `project/` and `stories/` paths that belong to the change.
- Do not edit `project/issues/` or `project/events/` by hand.
- Product-code or policy changes (hooks, doctrine, skills, `.kanbus.yml`, specs) should go through a pull request into `main` unless Ryan explicitly asks for a direct commit.
- Keep unpublished article work off the Anth.us site `main` branch. Amplify deploys Anth.us from `main`. Gatsby MDX in `Anth.us/src/blog/` remains the live editing surface until a later extraction milestone.
- Stage order for stories: `idea` → `assignment` → `research` → `report` → `editor_select` → `copywriting` → `published`. Do not invent pipeline artifacts to skip ahead.
- AI agents must record provenance on every `kbs create` and `kbs comment` (see CONTRIBUTING_AGENT.md). Prefer session env defaults such as `KANBUS_AGENT_PLATFORM=cursor` and `KANBUS_AGENT_MODEL=composer-2.5`.

## Agent skill

Use `skills/advance-story/SKILL.md` when advancing work in this pod. Read `doctrine/anthus.md` in full when context is thin.
