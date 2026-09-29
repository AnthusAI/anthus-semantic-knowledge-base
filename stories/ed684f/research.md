# Research: when a bias finding becomes an operating problem

## Working conclusion

An advocacy organization, affected person, employee, journalist, or customer
can turn evidence about an automated decision into litigation, agency attention,
customer pressure, or a product change. A public benchmark does not itself prove
a statutory violation; it can establish a system property that someone must
explain.

## FTC signal

Reuters reported on 25 September 2026 that FTC Chair Andrew Ferguson rejected
the idea that an AI agent causing harm is an independent actor with its own
will. His concrete formulation: if someone tells a tool to do something and it
does it, the question is not what to do about the tool. Reuters characterized
the remarks as pointing to developers who instruct agents and reported his view
that audit trails had shown ostensibly uncontrolled agents carrying out given
instructions.

- Source: Reuters reprint, 2026-09-25:
  https://www.marketscreener.com/news/ftc-chair-suggests-ai-developers-should-be-liable-for-conduct-of-agents-ce785adfd18ef423
- Read: conference remarks, not a new FTC rule or decided case.
- Editorial use: responsibility follows the instructed system and the people
  who choose its objective, access, and controls. Do not write that every base
  model maker automatically owns all downstream uses.

## Federal positions that reject the "the algorithm did it" defense

### Employment

In *Mobley v. Workday*, the EEOC argued in an amicus brief that Workday's
algorithmic screening tools could perform the same screening and referral
functions as a traditional employment agency. This is an agency litigation
position, not a final merits judgment, but it directly supports examining a
vendor's actual role.

- EEOC amicus brief:
  https://www.eeoc.gov/sites/default/files/2024-04/Mobley%20v%20Workday%20NDCal%20am-brf%2004-24%20sjw.pdf
- EEOC filing rule: an organization or agency may file a charge on behalf of an
  aggrieved person to protect that person's identity:
  https://www.eeoc.gov/filing-charge-discrimination

### Housing

DOJ and HUD's SafeRent statement says housing providers **and tenant screening
companies** using algorithms and data are not absolved from liability when their
practices disproportionately deny people of color housing opportunities.

- DOJ, 2023-01-09:
  https://www.justice.gov/archives/opa/pr/justice-department-files-statement-interest-fair-housing-act-alleging-unlawful-algorithm

### Credit

The CFPB says creditors using AI or complex algorithms must give accurate,
specific reasons for adverse credit decisions. A creditor's inability to
understand its own method is not a defense to that obligation.

- CFPB Circular 2022-03:
  https://www.consumerfinance.gov/compliance/circulars/circular-2022-03-adverse-action-notification-requirements-in-connection-with-credit-decisions-based-on-complex-algorithms/

### FTC product claims

The FTC's Workado matter alleged that a company advertised its AI detector as
98% accurate without adequate substantiation. The resulting order requires
competent and reliable evidence for future effectiveness claims. This is not an
employment-bias matter; it establishes the route by which a vendor's "fair,"
"validated," or quantified accuracy claims can become the FTC issue.

- FTC, 2025-04-28:
  https://www.ftc.gov/news-events/news/press-releases/2025/04/ftc-order-requires-workado-back-artificial-intelligence-detection-claims

## Documented escalation cases

### Meta/Facebook: outside groups → product changes → federal settlement

The National Fair Housing Alliance, ACLU, Communications Workers of America,
and other plaintiffs challenged discriminatory housing, employment, and credit
ad targeting. Facebook announced 2019 settlements and changes to the platform.
HUD later pursued a Fair Housing Act charge; DOJ's 2022 settlement required
Meta to stop using its Special Ad Audience tool for housing ads and develop a
Variance Reduction System to address disparities produced by personalization
algorithms.

- ACLU settlement context:
  https://www.aclu.org/press-releases/facebook-agrees-sweeping-reforms-curb-discriminatory-ad-targeting-practices
- Meta's account of 2019 changes:
  https://about.fb.com/news/2019/03/protecting-against-discrimination-in-ads/
- Federal outcome:
  https://www.justice.gov/crt/case/united-states-v-meta-platforms-inc-fka-facebook-inc-sdny

**Use:** the strongest case of outside evidence becoming business changes, a
federal case, and a mandated engineering response. It is advertising delivery,
not a small text classifier; the article must say that plainly.

### HireVue: FTC complaint and scrutiny → feature withdrawal

EPIC filed an FTC complaint in 2019 alleging that HireVue's recruiting product
engaged in unfair and deceptive practices and falsely denied using facial
recognition. In January 2021 HireVue said it had removed visual analysis from
new assessment models earlier in 2020, attributing its decision to research on
predictive value and improved NLP. The sources do not establish that the FTC
complaint legally compelled removal. The accurate sequence is public scrutiny,
an FTC complaint, and a product retreat.

- EPIC filing: https://epic.org/documents/in-re-hirevue/
- HireVue announcement:
  https://www.hirevue.com/blog/hiring/industry-leadership-new-audit-results-and-decision-on-visual-analysis

**Use:** closest to the reader — an employment-assessment vendor whose feature
set became a public accountability issue.

## Whistleblower route

An employee who reasonably and in good faith opposes a perceived unlawful
employment practice can have retaliation protection under EEO law. This is not
a general licence to disclose confidential data or a generic AI-whistleblower
statute. It matters because an engineer, recruiter, reviewer, or analyst can
raise concerns about an employment decision system internally or participate in
an EEOC matter.

- EEOC retaliation Q&A:
  https://www.eeoc.gov/laws/guidance/questions-and-answers-enforcement-guidance-retaliation-and-related-issues

## Biased-Decisions: what it supplies

The project measures causal counterfactual sensitivity: the same bio, one
protected cue changed, plus a matched trivial-edit control floor. It reports
verdict flips, probability shifts, or shortlist ratios with model and record
provenance. It does **not** decide whether a deployer or vendor has violated a
statute. It makes the accountability question concrete: which version changed
its result, by how much, relative to which floor, and who knew before release.

- https://github.com/AnthusAI/Biased-Decisions
- https://github.com/AnthusAI/Biased-Decisions/blob/main/RESULTS.md

## Provisional structure

1. The question after launch: who owns a decision when the model has made it?
2. The FTC chair's answer: tools do not become independent actors.
3. The evidence that travels: a reproducible result is not a legal verdict, but
   it starts an accountability event.
4. Three routes: Meta/Facebook, HireVue, and the federal positions in
   employment, housing, and credit.
5. What an engineer owns: selection, rubric, threshold, access, version,
   monitoring, marketing claims, and the response to known findings.
