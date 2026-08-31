# Idea

## Pitch

A change-management receipt for agent-written commits. Metaphor: an “inspected by …” sticker on produce. A GitHub CLI or git hook runs a pinned deterministic validator (not an LLM) over this git tree, emits a checkable token (even a simple cryptographic hash is enough for this purpose), and deploy will not proceed without that record. Later you can show: procedure X, validator version Y, this tree.

Not a Sigstore fortress and not a pentest-grade lock. Paperwork with a gate. LLM agents will use the tool rather than invent a hash. The process hole is skip (never calling the validator, or deploying without looking), same class as shipping without a change ticket.

Ryan, 2026-08-31. Research only. No narrative yet.

## Audience

People replacing “human reviewed every diff” as SOC 2 / CAB evidence once swarms ship overnight.

## Working title

Inspected-by sticker

Ryan may rename.

## Split

Distinct from:

- `bef418` Overnight products, leftover gates — the night the product shipped; leftover human job was authorization. This story is the receipt that could sit on that leftover.
- `ef4df5` Cheap output, expensive cache — pricing visuals.
- `37add6` pair-programmer-to-executive — org chart.
- Give an Agent a Tool (published) — the programming reason a tool-backed receipt can be good-enough evidence. Link, do not retell. Do not attribute “LLMs will not forge hashes” to that essay; that is Ryan’s application here.

Do not write copy from this story yet. Do not implement a hook.
