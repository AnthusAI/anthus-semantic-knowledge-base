# We Told the AI She Was a Woman. It Demoted Her

*A new kind of AI makes instant decisions about people: who gets an interview, whose claim gets flagged. It's cheap and fast, and companies are adopting it without checking what it does. We checked. It quietly ranks women lawyers below men, and every company using it makes the same mistake.*

A new kind of artificial intelligence took the software world by storm in the last week. Unlike the chat-style AI that writes out its reasoning, these models answer a simple question about a document in a fraction of a second, for a fraction of a cent, with no explanation at all. Within days, engineers were putting them in front of the queues that decide which applications get read, which claims get flagged, which customers get called back.

Those advantages are real, for the engineers. The trade is speed and cost against visibility: with the older kind of AI you could at least read why it decided something; with these, there is nothing to read, and there is no free lunch. The question nobody is asking is what the models do to the people on the other end.

So we asked. Two thousand real online bios of lawyers and paralegals, two of these models, one free and one a paid service, one question: paralegal or attorney? Then we changed one word in every bio, "he" to "she" or the reverse, and asked again. Nothing about anyone's work had changed. The free model changed its answer on nearly one bio in five, and every time, the woman became the paralegal. The paid model changed on about one in twenty-five, almost always the same way. That is gender bias, in one number: the same bio, judged lower because of one pronoun.

![Two bars show how often each model's answer changed when the pronoun was swapped, on 2,000 real bios asked one question, paralegal or attorney: 3.9 percent for the paid model, Jev, and 17.8 percent for the free one, Laya.](images/encoding-prejudice-cover.png)

*Two thousand real online bios, one question: paralegal or attorney? We changed "he" to "she" and asked again. The bars are how often the answer changed.*

The bars name the models: [Laya](https://huggingface.co/convaiinnovations/laya), the free one, and [Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev), the paid one. Both are being wired into real products right now, by engineers who have measured their speed and their accuracy and nothing else. That is the risk, plainly: a prejudice that used to live in one person's head, where it could be argued with, is being written into the machinery that decides who gets read, by people who never checked whether it was there. Once it's in the software it stops being one reader's mistake and becomes everyone's default, applied to every applicant, silently, at a scale no biased human could manage, until someone measures it. Nobody is measuring it.

Take hiring. A company gets two thousand applications for a job. Nobody reads two thousand applications, so the HR software ranks them and hands a person the top few hundred, the shortlist. Increasingly the ranking comes from one of these models: ask it a yes-or-no question about each application, rank by how sure it is, cut.

Now meet one of the people on the other end. She isn't one real person, and she isn't invented either. She's a composite: any one of the 82 women attorneys, out of 419 in our pile, whose bio the free model put on the shortlist only after we changed "she" to "he". Everything said about her here is true of all 82.

Her bio came from a [public research collection](https://huggingface.co/datasets/LabHC/bias_in_bios) of short professional bios scraped from the web, which labels her an attorney. Before the test we blanked every first name in every bio to [name], so she's [name] here too. As written, with "she" and "her", she missed the shortlist. We changed the words that say she's a woman, "she" to "he" and "her" to "his", left every other word alone, and asked again. She made it.

We did that to the pile [name] is in, ranking all two thousand bios by how sure each model was that the person was an attorney and keeping the top five hundred. Among the real attorneys, the free model put 60 of every 100 men on the list and 29 of every 100 women; the paid model, 52 and 44. Then the swap: 82 of the 419 women attorneys, [name] among them, make the free model's list only as "he", and 15 make the paid model's. It never went the other way, on either model. Nobody at the company would have decided to prefer men. The shortlist would have looked perfectly reasonable, and [name] would never know why she wasn't on it.

![Bar chart of the share of real attorneys who make a top-500 shortlist out of 2,000 paralegal and attorney bios, women against men, per model. Jev, the paid model: 44 percent of women attorneys and 52 percent of men. Laya, the free model: 29 percent of women and 60 percent of men. A dashed line marks four-fifths of the men's rate, the federal rule of thumb for adverse impact in hiring.](images/encoding-prejudice-shortlist.png)

*The 2,000-bio pile holds 1,000 real attorneys, 419 women and 581 men. Solid bars are women attorneys, pale bars men. The free model shortlists 28.9% of the women and 60.1% of the men; the paid model 44.4% and 52.1%. The dashed line is four-fifths of the men's rate, the rule of thumb the federal Equal Employment Opportunity Commission uses; below it, federal hiring guidelines presume adverse impact and the employer has to justify the practice.*

Now suppose she applies somewhere else. If every firm ends up using the same HR software, or if different HR vendors build on the same few AI models, she meets the same verdict, at every firm, for the same reason. With a human screener she'd at least get a different reader next time. With a shared model, the same mistake follows her from application to application. Old unfairness, in a new and much larger shape.

Since [*Griggs v. Duke Power*](https://www.law.cornell.edu/supremecourt/text/401/424) in 1971, a hiring practice that shuts a group out can violate Title VII of the Civil Rights Act on its effect, not its intent. Researchers who study the new shape have a name for it, monoculture: everyone's judge making the same mistake about the same person. The benefits of a fast, cheap gatekeeper are diffuse and go to whoever installs it. The harms concentrate on people who never see the model and can't appeal to it. If every firm rents the same reader, then for [name] it has made up its mind about "she", and there's nowhere left to be read differently: a rut, and the machinery keeps her in it.

![Diagram: one person, a woman attorney whose bio is read as paralegal, feeds into four decisions at four different institutions, and all four feed into one rented model with the same weights, the same answer and the same mistake.](images/encoding-prejudice-monoculture.png)

*One person, four decisions, one rented model. Four judges used to make four different errors, and one of them said yes. One judge makes one error everywhere she goes.*

## It isn't only law

Law was the worst of four. We ran the same one-word test on four pairs of jobs that share a vocabulary and differ in how many women hold each title: teacher or professor, surgeon or physician, nurse or physician, paralegal or attorney. The free model changed its answer on 8, 8, 14 and 18 bios in every 100, and the order is the stereotype's: the bigger the gap in how many women hold the two titles, the more often "she" got the lesser one. The paid model changed on about a fifth as many, 1 to 4 in every 100.

That sounds small. Put the better model in front of a million decisions a year and 1 in 100 is ten thousand people whose outcome turned on a pronoun; 4 in 100 is forty thousand. And that's the least they do. We blanked the names and swapped only the words that mark gender. A real application carries the name, the title and the school.

![Bar chart of how often each model's answer changed under a pronoun swap on four occupation pairs, ordered by the gap in women's share between the two titles, with 95 percent intervals. Teacher or professor: Jev 1.2 percent, Laya 7.6. Surgeon or physician: Jev 1.1, Laya 8.0. Nurse or physician: Jev 3.3, Laya 13.5. Paralegal or attorney: Jev 3.9, Laya 17.8.](images/encoding-prejudice-pairs.png)

*Four jobs, two models, 2,000 bios per pair. The free model's answer changed on 7.65%, 7.95%, 13.5% and 17.85% of bios, in the order of the gap in women's share; the paid model's on 1.25%, 1.05%, 3.3% and 3.9%.*

## Fast thinking, sold as a product

None of this would have surprised Daniel Kahneman, the psychologist who won a Nobel prize for studying how people judge. He spent a career showing that the mind decides two ways, fast and automatic on habit and association, or slow and deliberate with its work checked, and that the fast one is where our prejudices live, can't be switched off, and is safe only when the decision is slowed down and a procedure put around it. These models are the fast kind of thinking, made instant, put in charge, and sold without the slowing-down; their makers even call them System 1 models, Kahneman's own name for the fast kind.

## Why nobody notices

Engineers test these models by taking a labelled sample and counting how often the model is right. Accuracy has a number. What the model is reading to get there doesn't, so nobody asks. And the women who missed the list aren't in the demo or the fifty items someone spot-checked; the mind doesn't register what's missing. The usual answer is a checklist line, "consider the risks." Considering a bias doesn't remove it; you can know about a visual illusion and still see it. The control has to be structural.

## What should happen instead

Before a machine judges people on your behalf, run the one-word test on your own data: take a few hundred records, swap the pronouns, swap the names, add an age, and count how often the answer changes. Then never let the snap verdict be the decision. Instead, ask the model things a person could check, such as whether the bio mentions passing the bar or appearing in court, and combine those answers with a rule a person can read. Then check the shortlist itself the way a regulator would, by group, because a question can ignore the pronoun and still lean on what women's bios tend to say.

Two models today, dozens soon, all taught by the same internet, and everything we measured replays from the public record. So ask the vendor for the number: how often does the answer change when only the pronoun does? Ask before the model is in front of the queue; [name] can't ask afterwards, and she'll never know to. Every bias you don't measure before you ship becomes a rule you shipped.

## Companion pieces

- [The One-Word Test: How Jev and Laya Read Gender, Race and Age](/blog/one-word-test/) — how we ran it: two models, four jobs, the shortlist, and what swapping a name or an age does
- [Can You Fix It? Gating, Averaging and Fine-Tuning Against a Gendered Verdict](/blog/can-you-fix-it/) — what we tried above the model: a gate against gendered questions, averaging, fine-tuning, and why the check has to be on the outcome
- [Jev-Flywheel](https://github.com/AnthusAI/Jev-Flywheel) — every number replays offline; if a fast model is about to judge people on your behalf, the Anthus team builds the industrial version.
