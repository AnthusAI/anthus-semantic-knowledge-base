# Research notes

From Researcher, 2026-08-29. They read the pages. Do not invent commands. Official product docs do NOT call `/goal` or `/loop` “Ralph” — only Claude’s Ralph Loop plugin uses the name.

Keep this separate from `f10464`.

## Distinction

- Ralph = same prompt, fresh context, files/git as memory, loop until done.
- Product `/goal` = keep this session going until a condition.
- Product `/loop` = re-run on an interval.
- Cousins, not the same command.

## Namesake / explainer

Ryan said Ralph Wiggins. Verified: Ralph **Wiggum** (The Simpsons), not Wiggins. Named after Ralph Kramden. Huntley never writes “I named it after Wiggum because he fails and tries again.” That gloss is later writers.

- https://ghuntley.com/ralph/ — Huntley original. Title is “Ralph Wiggum as a ‘software engineer’.” “Ralph is a technique. In its purest form, Ralph is a Bash loop.” Canonical: `while :; do cat PROMPT.md | claude-code ; done`. This post is canonical. Later writeups say 14 Jul 2025.
- https://ghuntley.com/cursed/ — “running Claude in a while true loop (aka ‘Ralph Wiggum’)”.
- https://ghuntley.com/loop/ — Jan 2026: “everything is a ralph loop.”
- https://en.wikipedia.org/wiki/Ralph_Wiggum — “Me fail English? That’s unpossible!”
- https://venturebeat.com/technology/how-ralph-wiggum-went-from-the-simpsons-to-the-biggest-name-in-ai-right-now — 2026-01-06. Best press explainer. Tale of two Ralphs (Huntley bash vs Anthropic plugin).
- https://futureagi.com/blog/loop-engineering/ralph-loop/ — clean explainer close to Huntley.
- https://www.thoughtworks.com/radar/techniques/ralph-loop — 2026-04-15, Assess. Also called Wiggum loop.
- https://www.codecentric.de/en/knowledge-hub/blog/the-ralph-wiggum-loop-autonomous-code-generation-with-a-fresh-context — 2026-04-06.

## Official commands (rechecked 2026-08-29)

### Claude Code

- https://code.claude.com/docs/en/goal — `/goal` built-in. `/goal clear`.
- https://code.claude.com/docs/en/scheduled-tasks — `/loop` bundled skill. Interval/cron, NOT Huntley-Ralph. Alias `/proactive`.
- https://code.claude.com/docs/en/commands — has `/goal` and `/loop`. Does NOT list `/ralph-loop`.
- https://claude.com/plugins — official “Ralph Loop” plugin.
- https://github.com/anthropics/claude-plugins-official/blob/main/plugins/ralph-loop/README.md — `/ralph-loop` and `/cancel-ralph`.

### Cursor

- https://cursor.com/docs/agent/overview — `/goal`. “Rolling out.”
- https://cursor.com/changelog/08-19-26 — 2026-08-19. `/goal` and pairing with `/loop`.
- https://cursor.com/docs/skills — `/loop` built-in skill. No `/ralph-loop`.
- https://cursor.com/docs/hooks — stop-hook `followup_message`. Not a slash command.

### Codex

- https://developers.openai.com/cookbook/examples/codex/using_goals_in_codex — `/goal` from 0.128.0. Event-driven rather than a simple loop. No `/loop` on this page.
- https://developers.openai.com/codex/use-cases/follow-goals — `/goal`.
- Official slash-command tables list `/goal`. `/loop` NOT found. Do not invent it.

### Google Antigravity

- https://antigravity.google/docs/slash-commands/ — `/goal`. No `/loop`. Interval is `/schedule`. Funny official command: `/grill-me`.
- https://www.antigravity.google/docs/getting-started — same `/goal` wording.

## Funny / related

- https://github.com/repomirrorhq/repomirror/blob/main/repomirror.md — while-true Claude, ~1100 commits, one agent pkill’d itself.
- https://github.com/snarktank/ralph — PRD loop, progress.txt.
- https://dev.to/sean8/i-accidentally-made-claude-ask-itself-the-same-question-1966-times-1c5h — 1,966 iterations.
- https://github.com/anthropics/claude-plugins-official/issues/65 — `/cancel-ralph` ignored.
- https://github.com/anthropics/claude-plugins-official/issues/66 — ralph-loop.local.md parse mess.
- Huntley: “you’ll wake up to a broken codebase that doesn’t compile from time to time.”
