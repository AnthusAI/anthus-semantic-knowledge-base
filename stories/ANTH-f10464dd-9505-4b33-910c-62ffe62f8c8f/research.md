# Research notes

From Researcher, 2026-08-29. More notes will arrive.

## Spine (Ryan editorial shape)

Cite three, then generalize. Do not open with a cousin catalog.

1. Plexus (ours, the boast).
2. AlphaEvolve (DeepMind). Prestige.
3. Karpathy Autoresearch. Prestige. Plain English: modify the code, run a timed check, keep or discard, repeat.

Start with those trusted names as the company, then Plexus as the same discipline pointed at classifiers. Then paint the broader technique.

## Spine cites

- AlphaEvolve (DeepMind, 2025-05-14, follow-up 2026-05-07): https://deepmind.google/discover/blog/alphaevolve-a-gemini-powered-coding-agent-for-designing-advanced-algorithms/ and https://deepmind.google/blog/alphaevolve-impact/ — they recast scheduling/kernels/TPU circuits as programs a metric can score, then keep the better. They climb algorithms. We climb classifiers.
- Karpathy Autoresearch (2026-03): https://github.com/karpathy/autoResearch — “It modifies the code, trains for 5 minutes, checks if the result improved, keeps or discards, and repeats.”

## Optional color (not the spine)

Dropbox, FAPO, PromptLayer, Benoit, Willison, GEPA, and Bonetto stay here as optional color.

- Simon Willison, Designing agentic loops (2025-09-30): https://simonwillison.net/2025/Sep/30/designing-agentic-loops/ — reduce to a goal + tools that iterate.
- GEPA (Agrawal et al., ICLR 2026, arXiv:2507.19457) — named algorithm: reflect on misses, rewrite the program.
- Dropbox Dash (2026-03-17 and 2026-06-25): https://dropbox.tech/machine-learning/optimizing-dropbox-dash-relevance-judge-with-dspy and https://dropbox.tech/machine-learning/how-we-turned-ai-evaluations-into-better-responses-in-dash-chat — closest meta-shape. They hill-climbed a 1–5 relevance judge (NMSE 8.83→4.86), named miss families, then used that judge as fitness for the chat agent (−26% incomplete answers in the first day).
- Cisco FAPO (2026): https://cisco-foundation-ai.github.io/blogs/fully-automated-prompt-optimization/ — closest loop. Coding agent edits a versioned program, composite score, clustered step-attribution (retrieval/cascade/format/reasoning), keep-the-better. QA example val EM 39.3%→70.3%. After climb 2 they admitted the rest was retrieval, which a prompt cannot fix.
- PromptLayer assertion judge (Jonathan Pedoeem, 2026-07-10): https://www.promptlayer.com/blog/the-ultimate-guide-to-llm-as-a-judge/ — closest artifact. Boolean classifier, 18 versions, 1,000-row fitness. They refused an “improved” prompt at 84.4% vs 88.6% incumbent. Every DO NOT line is a miss they had already seen.
- Harold Benoit / Microsoft AI, ode to GEPA (2026): https://haroldbenoit.com/blog/ode_to_gepa/ — same job class as Plexus: web-page quality classifiers as an evolvable program.
- Felipe Sens Bonetto, Auto-Architecture (Show HN 2026-04-28): https://github.com/FeSens/auto-arch-tournament/blob/main/docs/auto-arch-tournament-blog-post.md and https://news.ycombinator.com/item?id=47937380 — Karpathy’s generate-test-keep loop pointed at a textbook SystemVerilog RISC-V core. Formal + Verilator + P&R + CoreMark CRC as the verifier. 73 hypotheses / 10 wins / 9h 51m. 301→578 CoreMark iter/s (+92%), LUT count down ~40% after DIV/REM left the hot ALU. 63 of 73 ideas were wrong. The verifier is the product.

## Do not mix

DeckBot / Marp / Terraform / Playwright / “give an agent a tool” belong on `0bf71a` (EaC intro). Not here.
