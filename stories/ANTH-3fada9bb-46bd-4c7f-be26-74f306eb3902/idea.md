# Idea

## Pitch

Two ways to put Git on your data.

Beads liked a board agents could share. It indexed that board with SQLite. The index is a second copy of the truth. Git cannot merge it. You have to keep it honest.

We measured. A scan of the files is enough. Rust makes it faster. Kanbus dropped the sidecar. Virtuus made that the product: one JSON file per record. Indexes live in memory and die. The files are the store.

DoltHub wanted the other shape. Apps already speak SQLite. They wanted branch, merge, and diff on the rows. They kept the SQL engine. They swapped the B-tree. Git lives inside the database file.

One leftover is unread C next to production data. The other leftover is a sync tax for a problem the filesystem had already solved.

Keep it short.

## Audience

People choosing a store for agent work. People who think they need SQLite because Beads had one.

## Working title

Git in the file, or the file in Git

Ryan may rename.

## Split

Distinct from:

- `bef418` Overnight products, leftover gates — that is the night the product ships. This is which store you keep.
- `ecc2ae` Commodity — the market.
- Yegge Thunderdome — cite Beads/Gas Town as lineage, do not retell the city.

Cites: DoltLite Beta (Sehn, 31 Aug 2026); Beads; Kanbus vs Beads; Virtuus README. Do not invent benchmark numbers beyond what those pages already print.

Do not implement. Do not reprint the Kanbus millisecond table as the lede.
