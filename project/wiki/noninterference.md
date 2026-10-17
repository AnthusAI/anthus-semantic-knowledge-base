# Noninterference

For a deterministic toy system, write run(P, L, H) for execution with public input L and secret input H. Let observe select exactly what the attacker can see. A simple noninterference condition is:

Here run denotes the complete modeled execution, including divergent executions; observe returns its public trace under the chosen treatment of termination.

For every L, H1 and H2, observe(run(P, L, H1)) equals observe(run(P, L, H2)).

The secret may change; the public view must not. Goguen and Meseguer's foundational model expresses noninterference using the effect of one domain's actions on another domain's observations. The equation above is a simplified input-output teaching model, not a quotation of their entire formalism. [Goguen and Meseguer 1982](https://www.cs.purdue.edu/homes/ninghui/readings/AccessControl/goguen_meseguer_82.pdf)

Worked example: P always outputs “done.” For secret inputs 0 and 1, its public traces match. Q outputs the parity of the secret; choosing 0 and 1 produces different traces and witnesses a failure.

The observation function is part of the claim. Final database values, a stream of events, response sizes, wall-clock timestamps, and termination status define different properties. For randomized programs, comparing a single sampled output is inadequate; one commonly compares distributions or uses another explicitly defined probabilistic relation.

Exercise: Can a passing test with two random seeds establish probabilistic noninterference?

Answer: No. It checks those samples. The property quantifies over the specified behavior distribution, inputs, and attacker observations.

## Related concepts

[Observation model](observation-model.md) · [Hyperproperty](hyperproperty.md) · [Declassification](declassification.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Joseph A. Goguen and José Meseguer. “Security Policies and Security Models.” IEEE Symposium on Security and Privacy, 1982. [Full text](https://www.cs.purdue.edu/homes/ninghui/readings/AccessControl/goguen_meseguer_82.pdf).
