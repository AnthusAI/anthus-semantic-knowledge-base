# Assignment

Record research for a possible Anth.us piece on an inspected-by change record for agent commits. No narrative, no draft.

## Gist

A pinned deterministic validator inspects this git tree, emits a receipt (hash/token) via a CLI/git hook, and deploy refuses without it. Change-management safeguard with documentation, not a security lock. Give-an-agent-a-tool: using the checker is easier than faking a hash. Skip is the process hole.

## Must

- Keep the produce-sticker test: can you put it on the truck without the inspector’s mark?
- Bind: inspector = pinned validator version; object = this tree; required at deploy.
- SOC 2 CC8.1: the criterion is a process (authorize, document, test, approve, implement), not “a human read every diff.” Auditor habit is a second identity on approval. AICPA has not said whether an agent counts.
- Cousins bind one axis each. The combination is unoccupied.
- Link leftover-gates (`bef418`). Do not merge the stories.

## Must not

- Do not write reader-facing copy.
- Do not evaluate as a Sigstore / forge pentest.
- Do not claim Give an Agent a Tool itself said models will not forge hashes.
- Do not invent AICPA quotes beyond the CC8.1 sentence vendors all share: the entity authorizes, designs, develops or acquires, configures, documents, tests, approves, and implements changes.

## Research notes

See `research.md` on this story (from Researcher, 2026-08-31).
