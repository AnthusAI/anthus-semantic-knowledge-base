# Threat model

A threat model states what is protected, who may attack it, which parts the attacker controls, what the attacker observes, and what assumptions remain trusted. It turns “secure against agents” into a question that can be answered.

Original example: protect a signing key against a worker that controls every Python instruction and every tool argument. The worker can see stdout, returned files, and broker responses. The key is outside worker memory. The kernel and broker are trusted. The policy permits signing only one approved release manifest. Each sentence constrains a different part of the argument.

For a harness expected to operate for five or six years or more, potentially with much more capable agents, structural boundaries matter. A proof quantifying over every permitted worker program does not depend on predicting today's favorite attack. It still depends on correct implementation, adequate observations, and any cryptographic or physical assumptions.

Exercise: Does a stronger model invalidate a correct isolation theorem?

Answer: Intelligence alone does not invalidate a theorem within its assumptions. It may expose a missing channel, bug, overly broad capability, or assumption that never held. State a model-relative guarantee rather than an absolute promise about every future attacker.

## Related concepts

[Trusted computing base](trusted-computing-base.md) · [Observation model](observation-model.md) · [Secret confinement impossibility under unrestricted output](secret-confinement-impossibility.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Jerome H. Saltzer and Michael D. Schroeder. “The Protection of Information in Computer Systems.” Proceedings of the IEEE 63(9), 1278–1308, 1975. [Full text](https://web.cs.wpi.edu/~cs557/f14/papers/saltzer1975_alt.html).
