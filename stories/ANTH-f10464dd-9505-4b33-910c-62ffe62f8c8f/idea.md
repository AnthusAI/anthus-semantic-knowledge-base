# Idea

## Pitch

If you recast a use case as a coding task, LLMs are good at it. This piece is not the EaC intro. It is code-based hill-climbing: a coding agent edits a program, runs a metric, keeps the better version, repeats. Feature Plexus as the main example and boast. Marketing wants us talking about Plexus more.

## Audience

Engineers who already believe agents can write code, and product people who need a reason to talk about Plexus.

## Working title

Code-based hill-climbing

## Split

Two pieces, not one. DeckBot / everything-as-code-integration (`0bf71a`) introduces EaC: make the world into source a runtime can reject. Slides as Markdown. Leave Marp, Terraform, Playwright, and "give an agent a tool" there. Do not mix those into this hill-climb story.

## Boast

Text-classifier alignment is an iterative code problem. Classifiers live in a custom DSL. A coding agent edits the DSL, runs evals for accuracy, does root-cause on the misses, keeps the better version, repeats. A self-optimizer, not one-shot codegen. Cousins climb prompts or kernels. Plexus is the one with a classifier DSL plus an agent that diagnoses the misses.

## Client brag (Ryan, 2026-08-29)

Part of this article is a public brag: we used the same everything-as-code hill-climbing technique to improve a privacy scanning tool for a new privacy-and-security client. We instrumented their tool with evaluations that compare its output against ground-truth labels, then we run agent loops to automatically improve it. Quantifiable.
