---
title: "Jev: Intelligence Your Software Can Use"
slug: "jev-useful-decisions"
date: "2026-09-17"
authors:
  - author: <a href="/ryan">Ryan Porter</a>
tags:
  - AI
  - explainer
  - economics
excerpt: |
  Jev puts fast, typed decisions inside ordinary software. That fits the Anthus approach to AI: define the quality bar, measure the whole workflow, and spend less to get more useful work done.
preview_image: "./images/jev-useful-decisions.png"
images:
  - "./images/jev-useful-decisions.png"
state: draft
---

# Jev: Intelligence Your Software Can Use

A call-quality scorecard can contain dozens of questions. Did the representative identify the company? Did they resolve the customer's request? Did a particular policy apply? Each answer is small. Across an entire call center, those small answers become a substantial engineering and economics problem.

That's the kind of work that makes Jev interesting to us.

[TypeSafe introduced Jev on September 15](https://typesafe.ai/blog/introducing-system-one-models-and-jev), opening early access to its first System One model. It gives up free-form text generation to produce typed decisions, using a parallel sampler and a training method called Reinforcement Learning for Calibrated Decisions, or RLCD.

There's a familiar ambition here. In [Maximize Value, Not Intelligence](https://anth.us/blog/maximize-value-not-intelligence/), we argued that the quality requirement comes first. Then you find the cheapest way to meet it, counting the work people actually accept and the time they spend fixing everything else.

Jev takes that question into the design of the model itself. If the job is to answer a bounded question inside a program, how much of a conversational model do you need to pay for?

## A small decision can do a lot of work

TypeSafe's [manifesto](https://typesafe.ai/manifesto) argues that useful intelligence already exists; making it dependable and easy to compose is the next engineering challenge. Its proposed division of labor is appealing: let models interpret meaning, and let code handle exact computation and control flow.

That matches how we think about building AI applications. A business process contains different kinds of work. Some steps require interpretation. Others require checking a date, applying a rule, or adding numbers. Treating every step as a reasoning problem makes the system more expensive and harder to inspect.

Consider a support workflow. Recognizing that a customer is asking for an exchange takes language understanding. Checking whether the purchase falls inside the return window takes ordinary code. Authorizing an exception belongs to the business's policy. Those responsibilities can stay separate even when they contribute to the same outcome.

Jev's [interface](https://docs.typesafe.ai/introduction) makes that separation explicit. You send state and a collection of questions. Its primitives cover three useful shapes:

- **Choice:** select from defined options, with a probability distribution over them.
- **Score:** evaluate against a rubric.
- **Noul:** return the probability that a yes-or-no question has a yes answer.

Questions are evaluated independently against the same state. If one decision depends on another, the application has to handle that dependency. The programmer still designs the workflow.

That's a good fit for the argument in [How AI Agents Do Things](https://anth.us/blog/how-ai-agents-do-things/): the application owns execution. A model contributes judgments; the surrounding software determines what those judgments can cause.

## A scorecard in the playground

Our playground example puts a bank-call transcript alongside several scoring questions. The visible responses include a 99% yes probability for whether the representative thanked the customer, and 91% for whether they identified whom they represented. A separate question about successfully handling the call gives YES 95%, NO 5%, and NA 0%.

That's already a useful shape for a quality-assurance system. Each answer can become a scorecard field, a candidate for review, or an input to a rule. Nobody has to read a paragraph to discover which box the model checked.

There's also a deliberately strange question: was the proper protocol for space aliens invoked? The supplied criteria allow NA when aliens aren't mentioned. Jev assigns NA 80% and NO 20% in the displayed run.

The odd question makes a practical issue visible. A scorecard needs a way to represent a requirement that doesn't apply. Otherwise, absence can turn into failure. It also needs clear boundaries: the example's NO description includes a protocol that wasn't required, which overlaps with the NA case. Before blaming a model for uncertainty, inspect what you asked it to distinguish.

One more detail matters for anyone copying the example: the keys `is_human_escalation` and `is_repeat_contact` are attached to questions about thanking the customer and identifying the company. They aren't evidence that the model detected escalation or repeat contact. TypeSafe's [Choice documentation](https://docs.typesafe.ai/primitives/choice) says question IDs aren't shown to the model. They're labels for the application. Rename them before wiring those fields into a real scorecard.

This is one visible run, with no reference labels or measured latency attached. It illustrates the interface. It doesn't establish scoring accuracy.

## Give uncertainty somewhere to go

The successful-call result has two different numbers: a 95% probability on YES and a displayed confidence of 93%. Those aren't interchangeable.

TypeSafe [documents confidence](https://docs.typesafe.ai/confidence) as a statistic derived from the distribution across choices or score levels. A concentrated distribution produces higher confidence. [Noul](https://docs.typesafe.ai/primitives/noul) returns a single yes probability without a separate confidence field.

For a production system, the useful question is what happens when the answer is uncertain.

A possible call-quality workflow could accept routine scores that meet a validated threshold, send ambiguous cases to an evaluator, and always require review for designated high-consequence findings. The thresholds would depend on the question and on observed performance. A missing greeting and a serious policy violation shouldn't automatically share the same decision rule.

And calibration needs to be measured. Among predictions assigned roughly 90% probability, how often does the predicted answer agree with an independently adjudicated outcome? Does that relationship hold for different clients, scorecards, and transcript quality? A convincing percentage on a screen can't answer those questions.

This is where Jev's approach connects to our [Call Criteria work](https://anth.us/blog/call-criteria/). Human expertise defines the scoring criteria, resolves disagreements, and supplies feedback for improvement. Plexus provides the operational setting for evaluating and managing models. A new decision model would be a candidate inside that system.

TypeSafe's critique of preference-trained chat models doesn't make human feedback obsolete. Our interest is in how expert judgment improves the whole workflow: clearer rubrics, better evaluation sets, more useful review queues, and evidence about where automation earns its place. None of that assumes Jev learns from our corrections automatically.

## The price changes what you can afford to ask

TypeSafe's [published price](https://typesafe.ai/) is $42 per billion input tokens, or $0.042 per million. The [launch announcement](https://typesafe.ai/blog/introducing-system-one-models-and-jev) lists output tokens as free and reports response times of 70–500 milliseconds.

Take a hypothetical request with 10,000 billed input tokens, including its state and questions. At that input rate, the model charge is $0.00042. A million such requests would cost $420.

That is an illustration of the rate, not a quote for scoring a million calls. Actual token accounting, transcript lengths, question sets, retries, and additional requests determine the invoice. Transcription, storage, engineering, and human review still belong in the budget.

But the possibility is exciting. A lower price per useful judgment can make it economical to ask questions you previously left off the scorecard. You could check more interactions, investigate more failure categories, or compare competing interpretations before deciding which cases deserve attention.

The constraint then moves. If a million cheap judgments generate an unmanageable review queue, you've found the next expensive part of the system. The metric to track is cost per accepted score at a defined error tolerance, together with human minutes and escalation rate.

There's also a connection to [Never Use Fast](https://anth.us/blog/never-use-fast/). That article challenges paying a premium for speed when nobody benefits from the shorter wait. Jev's proposition combines low latency with low cost. For an overnight scoring job, the economics may be the attraction. For a decision that has to arrive during a live interaction, latency can determine whether the feature is practical at all.

The business requirement decides which advantage matters.

## Read the guarantees at the right level

TypeSafe's [homepage](https://typesafe.ai/) advertises gains of 193.6× in speed and 444.6× in cost. The [announcement](https://typesafe.ai/blog/introducing-system-one-models-and-jev) says these come from its workflow evaluations and are likely toward the high end of real-world gains.

The [evaluation site](https://evals.typesafe.ai/) describes four workflows, including customer service and invoice processing. Reference labels come from an average of two other models' responses. Those results measure agreement with that reference under the chosen harness. They don't establish agreement with a client's expert evaluators or guarantee a saving on our workloads.

The announcement also explains that comparison LLMs use a wrapper that returns compatible decisions with probabilities. That can cost more and take longer than requesting a label alone. It's a relevant comparison for probability-aware workflows, but it needs to stay attached to the headline numbers.

The same care applies to the phrase “zero hallucinations.” TypeSafe grounds its [claim](https://typesafe.ai/blog/introducing-system-one-models-and-jev) in guaranteed schema matching. Constraining the answer space removes a class of integration failures. A model choosing from YES, NO, and NA can't invent a fourth category.

It can still choose the wrong allowed category. It can misunderstand a transcript, encounter an ambiguous rubric, or confidently misclassify an unfamiliar case. Type safety makes the output usable by code. Whether the resulting action is appropriate remains something we have to evaluate.

## Put Jev against a real quality bar

The next useful experiment is a bounded comparison on a scorecard we understand.

Freeze the rubric and reserve a held-out set of calls with expert-adjudicated answers. Include straightforward cases, disputed cases, missing evidence, and requirements that don't apply. Give Jev and the existing scoring approach equivalent information. Then compare errors per question, probability calibration, tail latency, and the total cost of arriving at an accepted result.

Start in shadow mode, where Jev's outputs don't change customer outcomes. Use the results to decide which questions it handles economically and which should keep their current path. Version the questions and record the model used, so an apparent improvement doesn't quietly reflect a changed test.

That evaluation could produce a mixed answer. Jev might be excellent at some scorecard items while another model remains better at the difficult ones. A workflow that routes each item appropriately would be a useful result.

This is what draws us to Jev. It invites us to treat intelligence as an ordinary component of software: bounded, measurable, and cheap enough to use wherever it earns its cost.

A few cents saved on a request is pleasant. A quality check that finally becomes affordable across every interaction changes what a business can do. That's the version of AI progress we want to build on.
