# Idea

## Pitch

Plexus is a classifier lab. It takes a business decision that people currently make by reading things (is this call compliant, is this ticket urgent, is this claim complete) and turns it into a classifier that runs at scale, with a confidence gate that routes the uncertain cases to a human, and a feedback loop that retrains from what those humans decide. Anthus has operated that loop in production for years and knows how.

Why now: the site has strong research on classification (distillation, confidence, gating, fine-tuning) and no piece that says to a buyer "this is the operational method, and here is what running it looks like." The cost-collapse and commodity-receipts articles already make the why-now argument for cheap models; this is the piece that says what to do with them.

Shape: open with a business problem, not a model. Walk one classifier through the loop: labelled examples, evaluation, confidence gating, human review, retraining, controlled rollout. Show the scorecard at month one versus month six. Reuse published, checkable figures from the distillation article and Classification with Confidence rather than inventing new ones. Cite Call Criteria for scale. Give away the whole method, then say Anthus will run it for you. One call to action, on the engagement page.

## Audience

Operations and product leaders with a high-volume judgment task and a QA or review team that cannot keep up. Secondary: the engineer they will ask to evaluate us.

## Working title

Plexus is a classifier lab: operationalizing classifiers at scale
