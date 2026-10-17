# Observation model

An observation model specifies what an attacker can distinguish. It is part of a security claim, not an implementation detail to omit afterward. Two executions can be equivalent for one observer and different for another.

Original example: a worker emits the JSON value 0 for every secret. Under an observer that sees only that value, the executions match. Under an observer that also sees response time, they differ if one secret causes a one-second delay and another causes two seconds. Under an observer that also sees filenames, a secret-dependent artifact name can add another distinction.

Write observations concretely: output bytes, output ordering, sizes, completion or timeout events, timestamps, persistent state, and shared-resource effects. Include only observations available to the specified attacker, but justify important exclusions.

Exercise: A proof excludes wall-clock time. Can it support a claim that no attacker can extract bits using request latency?

Answer: No. It establishes the stated abstraction. To address latency, extend the model and show that the implementation meets the stronger condition, or adopt a justified leakage bound.

## Related concepts

[Noninterference](noninterference.md) · [Covert channel](covert-channel.md) · [Termination sensitivity](termination-sensitivity.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Joseph A. Goguen and José Meseguer. “Security Policies and Security Models.” IEEE Symposium on Security and Privacy, 1982. [Full text](https://www.cs.purdue.edu/homes/ninghui/readings/AccessControl/goguen_meseguer_82.pdf).
