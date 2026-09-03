# Research notes

Starter pack, 2026-09-01. Cursor Auto cite pack added the same morning. Not a finished report. Drafting paused until Ryan says go.

Full Cursor receipts (live docs + staff forum, fetched 2026-09-01): `stories/ANTH-c28002/cursor-auto-factcheck.md`.

## Lead (do not bury)

Ryan, 2026-09-01: open on the analog. Token vendors offer the new Fast (or whatever premium default) like a drug dealer giving you a free first hit, except it isn't even free. You are your own advocate. Interests are misaligned. They don't want you to notice. Just say no.

Ryan recast the same morning: this is a larger collection of specific instructions, not a single-thesis sequel. Never Use Fast is one technique in that list (turn off the latency premium). Maximize Value stays the policy essay. Auto is another row, not a sequel.

## Job after the lead

Catalog specific instructions per coding app, including Fast as one row. Verify quota pools, reset cycles, default aliases, cheap-model names.

## Split

- `8bde91` Never Use Fast — published Fast surcharge table. This list includes that instruction and links the table. Do not reprint the 6× numbers here.
- `0b6e8b` Maximize Value — philosophy.
- `ef4df5` Cheap output, expensive cache — rate card vs workload mix. Charts stay there.
- `ecc2ae` Commodity — market.
- `629b47` Grok Bot review — not this piece.
- This card (`c28002`) — the how-to instruction list (Fast, pin vs Auto, buckets, aliases, multiplex, patience).

## Known notes to verify (leaked into Never Use Fast)

| App | Technique | Last checked | Risk |
| --- | --- | --- | --- |
| Antigravity / Google AI Pro | Two pools: Gemini vs Claude/GPT. Exhaust both. Pro 5h until weekly. AI Credit Overages = Never. | **2026-09-01** | Plan names still move |
| Cursor | Cursor Models vs Other Models. Pin Composer 2.5 Standard. Auto bills at routed-model list. Individuals have Auto, not Router. | **2026-09-01** | Pool unpublished; Pro Individual exception no longer in live docs |
| Grok Bot | Own weekly pool on paid Cursor. Overflow = shared on-demand. Do not quote “unused dies.” | **2026-09-01** | Display vs Other Models still disputed |
| Codex | Bare `gpt-5.6` / Power = Sol. Pin `gpt-5.6-luna` (or Terra). GPT-5.4 retired ChatGPT-signed Codex 2026-08-31. | **2026-09-01** | Aliases retire |
| Claude | Don't live on Fable 5. Default Sonnet/Opus 5. Fast = credits-only, link 8bde91. Prefer `--worktree`. | **2026-09-01** | Fast table stays on 8bde91 |
| Any picker | Kimi, GLM 5.2, DeepSeek, Qwen | 2026-08-27 | Which apps actually expose them |

## Cursor Auto — fact-checked 2026-09-01 (the other free-hit lever)

Situation: Auto used to be the thrift default. From June 2025 until each seat's next renewal after 2025-09-15, Cursor said Auto was unlimited for individuals. Guides still tell people to leave Auto on. Live docs now: all Auto modes bill at the list price of the model each request is routed to. Help's example is Opus 5.

Ryan's four claims, with receipts:

1. **Auto used to be the cheap path — TRUE.** Cursor blog [Updates to Teams and Auto](https://cursor.com/blog/aug-2025-pricing): Dec 2023–Jun 2025 Auto cost the same as other premium models; Jun 2025–next renewal after 2025-09-15 **unlimited for individuals**. Forum residue still says leave Auto on to save money.
2. **Auto can now cost a lot more than the cheapest pin — TRUE.** [models-and-pricing](https://cursor.com/docs/models-and-pricing): “All Auto modes bill at the list price of the model each request is routed to.” [Help](https://cursor.com/help/account-and-billing/pricing.md): “if your request is routed to Opus 5, you are billed at Opus 5 pricing.” Composer 2.5 Standard is $0.50 / $2.50. Opus 5 is $5 / $25. Included usage is a pool, not a discount; on-demand is the same list. Do **not** write “Pro Auto is still $1.25/$6.” That leftover flat card is **legacy Enterprise Auto Cost until 2026-09-07**, then it joins list-price billing. Even $1.25/$6 is already above Composer Standard.
3. **Pin Composer 2.5 because Auto uses dearer models — MIXED.** Pin **Standard**, not Fast. Fast is a row in this list (turn it off) and the 6× table lives on `8bde91`; link, do not reprint. Auto *may* land on Composer, or on Grok / Sol / Opus / Fable. Pool is unpublished ([deanrie, 2026-07-30](https://forum.cursor.com/t/which-models-are-behind-cursor-auto-mode/167010)). Pinning caps the rate at $0.50/$2.50 and keeps you on the larger Cursor Models pool. Luna is cheaper *list* ($0.20/$1.20) but drains the smaller Other Models pool.
4. **No router unless you are Enterprise — MIXED.** Right about individuals, wrong that it is Enterprise-only. [cursor-router](https://cursor.com/docs/cursor-router): “currently only available on Teams and Enterprise plans.” [Changelog 2026-07-22](https://cursor.com/changelog/router): on by default for **Teams**; Enterprise admins enable from the dashboard (**off** by default). Staff Colin 2026-07-24: “Cursor Router is currently not available on individual plans!” Do **not** write “you lack a router because you are not Enterprise.” Write: individuals have Auto, not Router; Teams has Router on; Enterprise has Router off until an admin flips it.

### Cursor instruction candidates (settings, after the principle)

1. Pin the model. Do not leave Auto. Model picker, or Settings → Models. ([agent prompting](https://cursor.com/docs/agent/prompting))
2. Pin Composer 2.5 **Standard**. Fast off is the Never Use Fast row; screenshot the hover-Edit toggle in-product (forum staff, not a docs click path).
3. Subagents: `model: composer-2.5[]` or `composer-2.5[fast=false]`. Do not `inherit` if the parent is Auto. ([subagents](https://cursor.com/docs/subagents))
4. Teams: Auto → Optimize For. Balance is the default for new users and is the one that billed people at Sol/Opus list. Prefer Cost, or leave Auto.
5. Teams/Enterprise admin: unhide Underlying model. If the chip changes mid-session, you paid a cache miss. Do not Impose Auto unless you mean it (soft and hard both default off).
6. Enterprise: leave Router off unless you have a reason. After 2026-09-07, even Cost mode loses the $1.25/$6 card.
7. Start (India): already no Auto and Fast cannot be enabled. Stay on Composer 2.5 / Grok in Cursor Models. Do not upgrade “for Auto.”

### Cursor prices fetched 2026-09-01 (writer table, not the lede)

Source: https://cursor.com/docs/models-and-pricing

| Model | Input / 1M | Cache read | Output / 1M |
| --- | --- | --- | --- |
| Composer 2.5 Standard | $0.50 | $0.20 | $2.50 |
| Grok 4.6 | $2.00 | $0.50 | $6.00 |
| GPT-5.6 Luna | $0.20 | $0.02 | $1.20 |
| GPT-5.6 Sol | $4.00 | $0.40 | $20.00 |
| Claude Opus 5 | $5.00 | $0.50 | $25.00 |
| Legacy Enterprise Auto Cost (until 2026-09-07) | $1.25 | $0.25 | $6.00 |

Composer Fast rates stay on `8bde91`. Do not reprint them here.

Cursor Token Rate $0.25/M on third-party is Teams/Enterprise only, not on Grok/Composer, not on individual Auto.

Full other-apps receipts (live docs 2026-09-01): `stories/ANTH-c28002/other-apps-recheck.md`.

## Other apps — fact-checked 2026-09-01

1. **Antigravity — confirmed.** Gemini vs Claude/GPT are separate tanks. Pro refreshes every 5h until weekly. When one family is at 0%, switch family. Setting: **AI Credit Overages = Never**. `/usage` or `/quota`. ([Plans](https://antigravity.google/docs/plans/), [blog](https://antigravity.google/blog/changes-to-antigravity-plans))
2. **Codex — confirmed, plus a catalog change.** Bare `gpt-5.6` and the Power preset are Sol. Pin `model = "gpt-5.6-luna"` in `~/.codex/config.toml` (or `/model` → Advanced → Luna; Terra for everyday). GPT-5.4 retired from ChatGPT-signed Codex **2026-08-31** — move leftover configs to Terra/Luna. Fast → Never Use Fast; wall-clock substitute is Ultra/subagents. ([Codex Models](https://developers.openai.com/codex/models), [API naming](https://developers.openai.com/api/docs/guides/latest-model))
3. **Claude — confirmed.** Don't live on Fable 5 (Max = 50% of the same weekly bar, burns faster; Pro = usage credits from token one). Official coding default is Sonnet, escalate to Opus. Fast is credits-only, Opus-only — one Never Use Fast row, no tax table. Prefer `claude --worktree`. Settings > Usage shows Opus-only and all-other-models bars. ([Fable on your plan](https://support.claude.com/en/articles/15424964-claude-fable-5-on-your-plan), [worktrees](https://code.claude.com/docs/en/worktrees))
4. **Grok Bot — confirmed weekly, not calendar-month.** Separate included weekly pool on paid Cursor; overflow is shared on-demand (cap $0 to hard-stop). Do **not** quote “unused dies” (UNVERIFIED vendor copy). 2026-09-01 users still dispute `grok-bot-*` vs Other Models enforcement — watch both bars. Several Bots in parallel is documented; coding agents on cursor.com/agents are *not* the weekly Bot pool. ([Grok Bot plans](https://cursor.com/help/grok-bot/plans))

### Instruction rows for the list (not draft)

- Exhaust both Antigravity tanks; don't buy overages.
- Never type Codex `gpt-5.6`; pin Luna/Terra.
- Don't live on Fable; `/fast` can switch you onto Opus.
- Spend the Grok Bot weekly bar; it is not the 1st-of-month Cursor cycle.
- Patience: 5h / weekly windows exist so waiting is cheaper than Fast.
- Multiplex: Codex Ultra/subagents, Claude `--worktree`, Grok Bot many-Bots.
- Compaction epochs: `/compact` before every model swap so you don't invalidate a fat cache prefix.

## Compaction epochs (Ryan, 2026-09-03) — instruction row, not draft

One technique for giving expensive models the exact context they need, and not too much: use a cheaper model (Haiku) to explore and describe what it found, then `/compact` before swapping up.

Do not pass a large Haiku session to Opus. A model change costs tokens by invalidating the cache prefix. Compact first.

Then:

1. Haiku explores the files involved in the issue and writes a short description of what it found.
2. `/compact`.
3. Opus writes a planning document.
4. `/compact` again.
5. Sonnet does the work.

Always compact history before changing models. Those cuts are **compaction epochs**. Recent references use that name in sophisticated harnesses. It matters because invalidating a long cache prefix is one of the expensive mistakes this list is for.

Related to `ef4df5` (cheap output, expensive cache) but this is a click-path: compact, then swap. Do not merge the two articles. Do not retell the rate-card charts.

Cites: still to attach. Researcher should file the recent "compaction epochs" harness writeups on this story. Do not invent a paper list.

## Still to research

- Other apps with a real bucket trick (only if verified). Do not invent a fifth app. Kimi/GLM/DeepSeek/Qwen picker row still 2026-08-27 — which apps actually expose them.
- Multiplex arithmetic without restating Fast (click paths are in; dollar math is not).
- Attach cites for "compaction epochs" in agent harnesses. Do not invent titles.

## Do not

- Reprint Fast multiples.
- Open the piece on a spreadsheet.
- Invent a fifth app to fill a table.
- Treat `ef4df5` as this article.
- Write “router is enterprise-only.”
