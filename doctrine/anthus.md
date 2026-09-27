# Anth.us blog doctrine (Slice 1)

This pod runs the Anth.us blog through a file-native newsroom workflow before any
hosted Papyrus infrastructure exists.

## Publication boundary

- The pod is upstream. `article.md` in the story workspace is the source of truth.
- Generated MDX under `Anth.us/src/blog/` is build output, not an editing surface.
- Do not edit pod-generated CMS files directly; regenerate from the pod.

## Process versus authority

- Kanbus enforces stage order and artifact gates.
- The pod does not prove who performed editor selection; git history is the audit trail.
- Verified identity and access control belong to hosted Papyrus.

## Voice and scope

- Posts explain how Anthus builds and operates AI systems for real editorial work.
- Voice: wonder and excitement, participant register, no hedging — same bar as
  Chatticus blog Voice (see Anth.us `AGENTS.md` / site-content README).
- Research and report artifacts are internal notes; only `article.md` is reader-facing.
- Keep doctrine short enough to read in full every run.

## Story job

- Every story names its job before research starts: who we are, why we're here,
  vision, teaching, values in action, or "I know what you're thinking". Pick the
  smallest job that removes the reader's main barrier. One job leads; do not force
  all six into one piece.
- Every story names the sourced moment it is built on: a real situation, tension,
  choice, and result from published work or the boards. Never manufacture a person,
  event, quote, statistic, or outcome. Thin material becomes a concise factual
  example, not a dramatic scene.
- Editor selection checks four things before copywriting: a clear job, a real
  source, a visible turning point, and a takeaway proportionate to the evidence.
- Client-acquisition stories: the opening is a teaching story or a values-in-action
  scene; the closing section is a vision of the reader's month six; the engage box
  is why we're here. The lesson is named after the event earns it.
- The full guidance is the `writing-effective-stories` skill in the Limatus repo.

## Board names

When Ryan says the newsroom board or the Papyrus board, he means this publication board (ANTH), not the Papyrus product board (PPY).
