# Robust declassification

Robust declassification addresses how intentional information release interacts with attacker-controlled code or data. A release mechanism should not let the attacker expand what is revealed beyond the policy's intended authority.

Original example: a trusted service may release a fixed aggregate for an approved cohort. If an untrusted worker can replace the cohort with a single person's record, the same “aggregate” operation can reveal much more than intended. The issue is control of the release, not just the existence of a declassify function.

The formal property is more precise than this example. Myers, Sabelfeld and Zdancewic prove enforcement results for a particular imperative language and type system. Their weak indistinguishability is timing- and termination-insensitive. A production service needs its own argument covering its actual observations, interface, and attacker influence.

Exercise: Is a comment stating “this output is declassified” sufficient?

Answer: No. Identify who authorizes release, what information is permitted, which inputs the attacker controls, and how the implementation enforces the decision. A label change without those constraints can simply name the leak.

## Related concepts

[Declassification](declassification.md) · [Termination sensitivity](termination-sensitivity.md) · [Authority misuse](authority-misuse.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Andrew C. Myers, Andrei Sabelfeld and Steve Zdancewic. “Enforcing Robust Declassification.” IEEE Computer Security Foundations Workshop, 2004. [Full text](https://www.cs.cornell.edu/andru/papers/csfw04.pdf).
