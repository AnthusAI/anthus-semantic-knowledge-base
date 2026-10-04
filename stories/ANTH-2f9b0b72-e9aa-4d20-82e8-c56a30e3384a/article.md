# An agent skill for not sounding like AI

_An editor caught a rewrite that dropped a contraction. That's the whole story, and also not even close to the whole story._

I asked an agent to tighten a sentence in an article I'd written: "The difference is that I don't have to sit inside every conversation." The rewrite came back as "The difference is that I no longer have to manage every exchange." Grammatically fine. Not how I talk. I said so, and that one correction turned into a real look at why the tool got it wrong — and then into a general-purpose skill for copy-editing AI-generated text so it reads less like AI and more like whoever's publication it's supposed to be.

That skill and the tooling underneath it live in [Papyrus](https://github.com/AnthusAI/Papyrus), the open-source content system Anth.us builds and runs its own newsroom on. This is what building it actually looked like, including the parts where it didn't work the first time.

## Two different problems wearing one name

"Make this sound less like AI" is actually two separate requests. The first is generic: cut the hedging, the empty lead-ins, the "it's important to note," the intensifiers that assert weirdness or importance without earning it. That's detectable pattern-matching, and most style guides already ban the worst offenders by name.

The second problem is specific to whoever's reading: does this sound like *this* publication, in *this* voice, following *this* publication's own rules? That's not generic at all. Anth.us writes in first person, uses contractions, and leans on a deliberate "X is not A, it's B" contrast as a signature move. [Pilobolus](https://pilobol.us), a much stranger publication we also build on Papyrus, bans that exact construction outright, writes in impersonal third person, and would consider "I" showing up in the prose a bug. Same underlying tool. Opposite rules. A copy-editing skill that only knows how to fight generic slop is solving half the problem.

## The cycle: diagnose, decide, apply, verify

Papyrus's `editorial diagnose` command is a read-only scanner. Point it at a draft and a publication's style profile, and it returns structured findings — redundancy, voice mismatch, vague claims, empty lead-ins, density — each with a stable id, a text span, an excerpt, and a rationale. It does not rewrite anything. It never has, on purpose: the moment a scanner starts silently rewriting prose, you've lost the ability to see what it thought was wrong and decide whether it was right.

That last part matters more than it sounds like it should. A scan that hands back forty findings you can't verify is just noise with better formatting. A scan that hands back a `finding-a3ea9c72f3b4804f` with an exact span and a stated rationale is something you can actually argue with — skip it, rewrite it, or leave a note explaining why the flag was wrong. The findings are signal. Deciding what to do with them is still, deliberately, a judgment call.

The shape of the cycle: scan a draft, review what came back, record a decision per finding — skip, rewrite, delete, keep — write those decisions down, optionally generate a few candidate rewrites for anything marked "rewrite," apply whichever ones actually improve the piece, and then scan the result again to check that the edit helped instead of just moving the same problems somewhere else.

That closing step is the one it's easiest to skip and the one that matters most. An edit that "feels" better isn't the same as an edit that's verifiably better. Re-running the scanner on the edited draft and comparing before and after — fewer real findings, no new ones, density holding steady — is the difference between believing an edit worked and knowing it did.

## Findings are signal, not verdicts

The scanner flags patterns. It does not know your publication's rhetorical habits from your accidental ones, and treating every flag as a verdict produces worse writing, not better. A few real examples from actually running this:

An essay opened with an anecdote held at arm's length and closed by naming what had actually happened — the same four words showing up at the start and the end. The redundancy check flagged it. It was the essay's whole payoff move. Skip.

A section heading restated as its paragraph's topic sentence — "Kanbus organized the work. It did not manage it" as a heading, then "But Kanbus organized the work; it did not manage it" opening the next paragraph. Flagged as repeated phrasing. It's a standard thesis-then-develop pattern, not a mistake. Skip.

Two sentences about the same company, sharing a couple of proper nouns and a markdown link, flagged as redundant with each other. They made completely different points. False positive — the shingle-matching the scanner uses to catch repeated phrasing doesn't know the difference between reused wording and reused vocabulary.

And the contrast construction that opens this section — "X is not A, it's B" — is exactly the device Pilobolus bans and Anth.us leans on. Same underlying pattern-match. Opposite correct answer, because the correct answer was never really about the pattern. It was about the publication.

## The bug that started this

Back to the dropped contraction. The style profile already said, in plain English, "use contractions." The rewrite pipeline still produced "I no longer have to manage every exchange" instead of keeping "I don't." Tracing it down turned up three real gaps, not one:

The prompt asking for a rewrite never anchored to the sentences immediately around the flagged span — no signal at all about the local register, the contractions already in use, the sentence-initial conjunctions the piece favored.

The style profile listed reference samples — prior published pieces in the same voice — that the code loaded into memory and then never actually sent to the model generating rewrites. A resource that existed specifically to answer "what does this publication's prose actually sound like" sat unused in the one place it would have mattered most.

And nothing checked a generated option against the style profile's own rules before handing it back. The instruction to use contractions was real. Nothing enforced it.

The fix was three changes, not one: anchor the rewrite prompt to the text immediately surrounding the flagged span, actually send a short excerpt from the reference corpus, and add a lint that flags — not blocks, flags — any option that drops a contraction the original text had. Re-running the exact same rewrite after the fix, the model *still* defaulted to "no longer." But this time every option came back carrying the warning: *flagged text used a contraction; this option has none.* The tool didn't get smarter about writing like me. It got honest about the gap instead of hiding it. I still had to write "But I don't have to sit inside every conversation" by hand — none of the three generated options were usable as-is — but I knew that going in, instead of finding out from a complaint.

That's the actual design principle, not just the bug fix: a rewrite suggestion is a candidate, never a mandate. The tool proposes. A human — or an agent doing the work with real editorial judgment, not pattern-matching — still decides, and can still write something better than any of the options when none of them land.

## Testing it on a publication built to disagree

The clean way to find out whether a design generalizes is to run it against something built to break your assumptions. Pilobolus's house voice reads like Anth.us's mirror image: third person instead of first, no publication "we" at all, sources woven into the sentence they support instead of a trailing citation list, and that "not A, it's B" construction explicitly banned as a tell for AI-generated prose rather than embraced as a signature move.

Building a style profile for it — the same schema Anth.us uses, just with entirely different rules loaded in — surfaced two real bugs the Anth.us profile had missed entirely, because Anth.us content didn't happen to contain the specific thing that broke:

Anth.us articles are plain markdown. Pilobolus articles open with a YAML frontmatter block carrying a `standfirst` field that's deliberately short by genre convention — the whole point of a standfirst is to read in about four seconds. The scanner had no reason to know frontmatter existed, so it didn't skip it, and it started flagging that short, intentional standfirst for choppy cadence. The fix — mask the frontmatter block before any check runs, for every publication, not just the one that exposed the gap — was a five-line change once the actual cause was clear.

The other bug was the same shape as one already fixed once. Anth.us content embeds citations as JSX components; Papyrus's redundancy checker already knew to mask those out, because the shared boilerplate field names — `container-title`, `accessed`, `date-parts` — were getting caught as "repeated phrasing" across every citation in a piece. Pilobolus runs on a different renderer with its own colon-fenced directive syntax for the same kind of embedded metadata — figures, pull-quotes, video embeds — carrying the exact same trap: shared attribute names like `src=` and `credit=` repeating across every figure in an article. Nobody had taught the checker that this second syntax existed either. Two articles went from a combined six false-positive redundancy findings down to zero once it did.

Running the scan at scale, later, across roughly eighty live Anth.us articles turned up its own list of real catches worth naming plainly: a corrupted, orphaned sentence fragment sitting in published copy; an internal production note that had never been meant for readers, left in a live article; a citation dated two different years in the same piece. None of those are redundancy or voice findings. They're just the kind of thing a careful read-through catches and an automated scanner, tuned only for the patterns it already knows to look for, will walk right past.

## What the point actually is

None of this makes editorial judgment optional. If anything it makes the opposite case: the scanner is worth building precisely because it doesn't try to replace the judgment part. It gives that judgment something concrete to work from — a stable id instead of "I think the third paragraph feels off," a re-scan instead of a guess about whether an edit helped, a warning instead of a silent register drift nobody would have caught until a reader did.

The interesting failures weren't the scanner missing something. They were the scanner being confidently, specifically wrong — a contraction quietly dropped, a rhetorical device it couldn't tell was deliberate, a boilerplate attribute mistaken for repeated prose — in ways that were fixable exactly because the tool was legible enough to be wrong out loud.

Papyrus is open. The diagnose pipeline, the style-profile schema, the masking fixes, the skill file that walks through the whole cycle — [it's all on GitHub](https://github.com/AnthusAI/Papyrus), including the commits where a first attempt at a fix was too broad and had to be tightened after it flagged perfectly ordinary sentences as slop. That's most of what building this actually looks like: not one clean rule, but a lot of specific, checkable corrections, each one traceable to a real sentence that was actually wrong.
