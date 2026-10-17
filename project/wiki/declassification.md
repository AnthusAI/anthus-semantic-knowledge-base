# Declassification

Strict noninterference disallows useful secret-dependent answers, including a password check's success bit. Declassification identifies a permitted release: what information, to whom, under whose authority, and under what conditions.

Robust declassification asks whether attacker-controlled parts can manipulate release beyond the intended policy. Myers, Sabelfeld and Zdancewic provide a type-based enforcement result for a formal language. Their paper's weak indistinguishability is timing- and termination-insensitive, so that theorem does not certify an arbitrary production service against those channels. [Myers, Sabelfeld and Zdancewic 2004](https://www.cs.cornell.edu/andru/papers/csfw04.pdf)

Original example: A service may reveal whether a submitted password matches, but unrestricted repeated queries enable guessing. The release policy needs a query budget and authentication context. “Only one bit per call” does not bound leakage across unlimited calls.

A signing broker intentionally returns key-dependent signatures. That is not strict information-theoretic noninterference of all outputs with respect to the key. Its claim may instead be computational key confidentiality under a specified cryptographic scheme, combined with authorization restrictions on signing.

Exercise: Is permitting the worker to choose any message to sign equivalent to permitting only a particular release transaction?

Answer: No. A narrow transaction interface can bind resource, audience, operation, expiry, and approval. An arbitrary-message signing interface exposes much broader authority.

## Related concepts

[Robust declassification](robust-declassification.md) · [Authority misuse](authority-misuse.md) · [Noninterference](noninterference.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Andrew C. Myers, Andrei Sabelfeld and Steve Zdancewic. “Enforcing Robust Declassification.” IEEE Computer Security Foundations Workshop, 2004. [Full text](https://www.cs.cornell.edu/andru/papers/csfw04.pdf).
