# Assignment

Write a how-to for engineers who are on Jev, hit a case where it disagrees with their reviewers, and went looking for the fine-tuning docs. Answer the title's question in the first two sentences: Jev can't be fine-tuned (TypeSafe's docs say the same weights serve every account); what you can adapt is the question set and a small fitted head around it, and the Jev-Flywheel repo measured what that buys.

Must:

- Explain the mechanism from the Jev-Flywheel README: frozen engine; elements become factors; a fitted, readable decision head; one request carries every question; refit versus steering and why they have different jobs.
- Report the natural experiment (87 labels of refitting against one steering round at 140), the analyst's own words, the two fitted coefficients, the approval gate, and that the agent never writes a weight or sees the held-out split.
- Be the sequel to "Can You Trust Jev's Confidence?" and reconcile the two: that piece found isotonic regression best with hundreds of labels; the flywheel head uses one temperature at small label counts. Establish the reason from the code, or leave it open.
- Tell "how often does it work" straight: 3 of 12, the gains when it hits and misses, 3 of 12 that promoted nothing, the checklist nudge, the pre-registered second agent that made it worse.
- One section on actually fine-tuning an open engine (Laya) on the same 140 labels, with our wrong pre-registered predictions, the "what you can change with each engine" table, and a link to the Jev vs Laya piece for detail.
- A numbered "what we'd do with this" list and a "What we haven't tested" section.
- Close with the repo (`make demo`, offline, no keys) and one short pointer to Plexus.

Must not:

- Claim the layer beats fine-tuning. It didn't, on raw accuracy, on this corpus.
- Present the scripted labeler as a person, or the constructed corpus as a real feedback set.
- Rank the four analyst models; 600 held-out items doesn't support it.
- State any number that isn't in the repo's README or `studies/` records. Results still running are marked pending.
- Repeat the engine comparison or the distillation chapter at length; those are the sibling pieces.
