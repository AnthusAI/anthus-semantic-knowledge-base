# Authority misuse

Authority misuse occurs when a component performs an operation outside the intended authorization, even if no credential bytes are disclosed. Secret confidentiality and authority control are distinct claims.

Original example: an isolated signer never returns its key, but it signs any attacker-selected message. If those signatures authorize production releases, an attacker can exploit the service without extracting the key. Another broker may safely hide a database password while accepting an unrestricted query that deletes every table.

A narrower interface can bind resource, operation, audience, payload schema, expiry, budget, and approval. These checks belong to an authorization policy. Hiding a secret from worker memory does not automatically implement them.

Conversely, a system can correctly reject unauthorized operations yet leak a credential in a debug error. Evaluate both directions rather than using “secure broker” as an undifferentiated label.

Exercise: Is an allowed hostname enough to constrain authority?

Answer: No. A single host may expose many operations and resources, redirects, uploads, or reflected output. Network policy can support an architecture, but semantic authorization and information-flow analysis remain separate requirements.

## Related concepts

[Credential broker](credential-broker.md) · [Capability](capability.md) · [Declassification](declassification.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Jerome H. Saltzer and Michael D. Schroeder. “The Protection of Information in Computer Systems.” Proceedings of the IEEE 63(9), 1278–1308, 1975. [Full text](https://web.cs.wpi.edu/~cs557/f14/papers/saltzer1975_alt.html).
