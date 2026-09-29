# Report: responsibility after deployment

## Recommended claim

The model does not own the decision. The organization that turns it into a
decision system owns the model choice, task, threshold, access, marketing
claims, deployment context, and response to known failures.

## Why publish now

FTC Chair Andrew Ferguson's Reuters remarks supply a timely, plain-language
entry point: he rejects treating agents as independent actors that escaped human
control. The useful engineering extension is not a prediction of liability. It
is a warning that a deployed model's observed behavior can be carried into
customer escalation, public advocacy, regulatory attention, or litigation.

## Evidence that carries the story

1. **Ferguson:** Reuters reports the chair's instruction-and-audit-trail
   framing. It is a conference remark, not a new rule.
2. **Meta/Facebook:** advocacy challenges over discriminatory delivery of
   housing, employment, and credit ads; product changes; HUD charge; DOJ
   settlement requiring withdrawal of an ad tool and a variance-reduction
   system. This is the lead real-world case.
3. **HireVue:** EPIC's FTC complaint plus public scrutiny; HireVue later
   removed visual analysis. Do not claim the complaint compelled the change.
4. **Agency doctrine:** EEOC's Workday position on screening/referral vendors;
   DOJ/HUD's SafeRent statement addressing providers and tenant-screening
   companies; CFPB's rule that black-box complexity does not excuse an adverse
   action without accurate reasons; FTC substantiation enforcement in Workado.
5. **Anthus measurement:** Biased-Decisions' causal counterfactuals and control
   floors show the sort of evidence that travels. They do not make a legal
   determination.

## Article promise

Give the engineer a concrete mental model for the day after deployment. The
question is not whether they meant to discriminate. It is whether they can
identify the exact system that made the decision, show how it was tested, state
what it was allowed to do, and explain what they did after a credible problem
appeared.

## Do not say

- "The FTC says all AI makers are liable."
- "A flip-rate benchmark proves discrimination."
- "HireVue withdrew visual analysis because the FTC made it."
- "Activist criticism is itself evidence of wrongdoing."

## Suggested title and social hook

**Title:** Who Is Responsible After You Deploy a Decision Model?

**Social hook:** Who is responsible for the decisions after you deploy a
decision model? Federal agencies are converging on one answer: not the model.
