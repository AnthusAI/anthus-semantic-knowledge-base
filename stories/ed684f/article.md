# Who Is Responsible After You Deploy a Decision Model?

An engineer ships a model that sorts job applications. A few weeks later, someone runs a paired test: same résumé, same experience, one protected cue changed. The recommendation changes more often than ordinary noise checks say it should.

The model did not choose its job. It did not choose the applicant pool, the question, the score that moved someone forward, or the production threshold. It cannot answer a customer, a reporter, an applicant, or a regulator. The engineer and company that put it between a résumé and a decision can.

That is the practical force behind a remark FTC Chair Andrew Ferguson made at a Reuters event in September. Ferguson rejected the idea that an AI agent which causes harm has become an independent actor with its own will. If someone tells a tool to do something and it does it, he said, the question is not what to do about the tool. Reuters reported that he pointed to the people who instruct the agent, and to audit trails that showed supposedly uncontrolled systems carrying out the instructions they had been given.

It was a chair's remark, not a new FTC rule. But it names a problem engineers already have: deployment does not move responsibility into the model.

## The decision system is bigger than the model

A decision model can classify a support ticket, rank a candidate, route a claim, flag an account, or choose who gets a closer look. The model may be one API call. The decision system around it is not.

Someone chose the model version. Someone wrote the rubric or prompt. Someone selected the data and features. Someone set the confidence threshold, shortlist size, escalation path, and permission to act. Someone described the product to buyers. And someone kept the system running after the first strange result.

Those choices are why "the model made the decision" is a thin answer. It describes a mechanism. It does not identify the person or organization that made the mechanism consequential.

Federal agencies do not all regulate the same workflow or use the same legal test. But they have repeatedly refused the idea that software complexity creates a responsibility-free zone.

The [CFPB says](https://www.consumerfinance.gov/compliance/circulars/circular-2022-03-adverse-action-notification-requirements-in-connection-with-credit-decisions-based-on-complex-algorithms/) a creditor using a complex model still has to give accurate, specific reasons for an adverse credit decision. Not understanding the model is not an excuse. The [EEOC argued](https://www.eeoc.gov/sites/default/files/2024-04/Mobley%20v%20Workday%20NDCal%20am-brf%2004-24%20sjw.pdf) that a vendor whose software screens and refers candidates can be performing the work of an employment agency, rather than merely selling a neutral tool. And DOJ and HUD have said that housing providers and tenant-screening companies are not absolved when algorithmic screening disproportionately denies people housing opportunities.

The statutes differ. The engineering lesson does not: if your system makes a regulated decision, "it was an algorithm" does not end the inquiry.

## How an outside finding becomes your operating problem

The nightmare version is easy to picture. A civil-rights organization publishes a report. A customer forwards it to compliance. Someone posts two nearly identical applications landing on different sides of a screen. The post travels faster than the model card.

That sequence does not require a group to prove a legal case on social media. It needs a credible artifact: a repeatable input change, an observed output change, a named version, and a plain description of what the system was allowed to decide. Affected people can bring it to an agency. Organizations can help file or support charges. Buyers can ask a vendor to account for a claim that sounded settled in a demo.

There are already two useful precedents.

### Meta had to change the machinery

Civil-rights groups including the ACLU, the National Fair Housing Alliance, and the Communications Workers of America challenged Facebook's systems for targeting housing, employment, and credit ads. Facebook announced settlements and product changes in 2019. The issue did not stay in a press cycle. HUD filed a Fair Housing Act charge, and in 2022 DOJ's settlement required Meta to stop using its Special Ad Audience tool for housing ads and develop a Variance Reduction System to address disparities caused by personalization algorithms.

That is a real engineering consequence. The company did not merely publish a better explanation. It had to retire a tool and build a system that changed how ads were delivered. The [DOJ case record](https://www.justice.gov/crt/case/united-states-v-meta-platforms-inc-fka-facebook-inc-sdny) is a reminder that a consequential decision often lives in the surrounding optimization system, not in an obvious checkbox a buyer clicked.

### HireVue lost a feature before an enforcement order arrived

In 2019, the Electronic Privacy Information Center filed an FTC complaint over HireVue's use and description of facial analysis in automated hiring. HireVue later said it had removed visual analysis from new assessment models in 2020, announcing the change in 2021 and attributing it to research on predictive value and improving natural-language processing.

The available record does not say the FTC compelled that change. It shows a different, common kind of pressure: a product feature becomes difficult to defend once an advocacy organization, applicants, researchers, and customers are all asking what it measures, whether it is fair, and why it is necessary. For a hiring-tech founder, that is not an abstract reputational concern. It can change the roadmap.

## "Fair" is a factual claim

The FTC has a direct route to the company marketing a decision product. Claims such as "bias-free," "validated," "fair," "compliant," or "98% accurate" are factual claims about what a system does. They need evidence that matches them.

The FTC's [Workado action](https://www.ftc.gov/news-events/news/press-releases/2025/04/ftc-order-requires-workado-back-artificial-intelligence-detection-claims) was not a hiring case. It concerned an AI-content detector advertised as 98% accurate. The agency alleged the claim was unsupported and required competent, reliable evidence for future effectiveness claims. The principle reaches every product page that turns a limited benchmark, a friendly pilot, or an internal audit into a broad promise.

Don't say a system is fair because nobody has yet found the failure mode. Say what you measured: which version, which decision, which population, which counterfactual, which control, and which result.

## Build the record before someone else does

Anthus's [Biased-Decisions](https://github.com/AnthusAI/Biased-Decisions) benchmark changes one protected signal on the same text, keeps everything else fixed, and compares the effect with an equally trivial edit that changes no protected signal. It reports the result in decision currency: a verdict flip, a probability shift, or a shortlist ratio.

That does not decide whether a hiring, housing, lending, or benefits system has broken a law. But it can establish something more useful than a generic promise of responsible AI: a versioned, replayable record of what the system did when a cue changed.

If the effect clears the control floor, the next question is operational. Can you name the model version? Can you reproduce the input and output? Can you show the threshold that turned a score into a decision? Can you show who reviewed the result, what the system was allowed to do while it was under review, and what changed afterward?

Those are not compliance decorations. They are the difference between learning about a failure mode early and learning about it from a viral screenshot, a customer escalation, or an agency demand.

The model is not the party that has to answer. The people who deployed the decision system are.

## Sources

- [Reuters reprint of Ferguson's remarks](https://www.marketscreener.com/news/ftc-chair-suggests-ai-developers-should-be-liable-for-conduct-of-agents-ce785adfd18ef423)
- [DOJ settlement record: United States v. Meta Platforms](https://www.justice.gov/crt/case/united-states-v-meta-platforms-inc-fka-facebook-inc-sdny)
- [EPIC complaint: In re HireVue](https://epic.org/documents/in-re-hirevue/)
- [HireVue announcement on visual analysis](https://www.hirevue.com/blog/hiring/industry-leadership-new-audit-results-and-decision-on-visual-analysis)
- [EEOC amicus brief in Mobley v. Workday](https://www.eeoc.gov/sites/default/files/2024-04/Mobley%20v%20Workday%20NDCal%20am-brf%2004-24%20sjw.pdf)
- [DOJ/HUD statement on algorithmic tenant screening](https://www.justice.gov/archives/opa/pr/justice-department-files-statement-interest-fair-housing-act-alleging-unlawful-algorithm)
- [CFPB Circular 2022-03](https://www.consumerfinance.gov/compliance/circulars/circular-2022-03-adverse-action-notification-requirements-in-connection-with-credit-decisions-based-on-complex-algorithms/)
- [FTC's Workado action](https://www.ftc.gov/news-events/news/press-releases/2025/04/ftc-order-requires-workado-back-artificial-intelligence-detection-claims)
