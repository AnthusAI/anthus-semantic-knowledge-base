# Confidentiality

Confidentiality restricts what an unauthorized observer can learn about protected information. A precise claim identifies the information, observer, permitted release, and observation model.

Original example: a worker does not reveal a complete API key but reveals its first bit. This violates a policy permitting no information about the key, although one bit may not enable immediate impersonation. A different policy might allow length or a public key to be known while protecting the secret value. These are distinct requirements.

Computational confidentiality depends on assumptions about feasible attackers and cryptographic hardness. Information-theoretic independence is a stronger kind of condition. A signature intentionally depends on a private key, yet a suitable signing scheme is designed to avoid efficient key recovery. Calling both properties “the output contains no secrets” obscures their different meanings.

Exercise: A broker returns a short-lived bearer credential that grants all the original key's permissions. Has it preserved the intended protection?

Answer: Protecting the original bytes may be insufficient. The returned credential can confer equivalent usable authority. Decide whether the policy protects one representation, information about the key, delegated authority, or all three.

## Related concepts

[Noninterference](noninterference.md) · [Observation model](observation-model.md) · [Authority misuse](authority-misuse.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Joseph A. Goguen and José Meseguer. “Security Policies and Security Models.” IEEE Symposium on Security and Privacy, 1982. [Full text](https://www.cs.purdue.edu/homes/ninghui/readings/AccessControl/goguen_meseguer_82.pdf).
