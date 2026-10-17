# Credential surrogation and substitute authority

Credential surrogation lets a trusted component act with a credential without delivering that credential to the worker. The worker supplies an operation request or a surrogate reference; the trusted component resolves it and performs the authorized action. The security argument must cover both what the worker learns and what the worker can cause.

Meta's published Sentinel architecture describes host-side authority and credential surrogation. Treat this as the provider's architecture description, with its stated controls and assumptions, rather than an independently established noninterference theorem. [Meta, How We Built Safety Into Muse](https://research.meta.ai/blog/security-and-safety-for-ai-agents-our-approach-with-muse).

Original example: a broker keeps a database password private but returns a session cookie that permits the same database operations. The original bytes remain confidential while equivalent authority has crossed the boundary. Refresh tokens, presigned URLs, newly created credentials, and token-exchange results deserve the same analysis.

Token exchange does not inherently produce a narrowly scoped, short-lived result. RFC 8693 describes the exchange protocol and allows a refresh token in some deployments. Provider support and enforced audience, resource, action, lifetime, and delegation restrictions determine whether an exchange actually reduces authority. [RFC 8693 §2.2.1](https://www.rfc-editor.org/rfc/rfc8693.html#section-2.2.1).

The engineering implication is to authorize permitted operations and outputs. A blacklist of familiar token-endpoint paths cannot establish that every other operation is harmless. This conclusion follows from the substitute-authority counterexample; it is not a universal claim about one OAuth deployment.

Exercise: A worker cannot export the original key but can create unrestricted API tokens. What property has the broker failed to preserve?

Answer: It has failed to constrain delegated authority. Preventing disclosure of one representation does not prevent the worker from obtaining an equivalent capability.

## Related concepts

[Authority misuse](authority-misuse.md) · [Credential broker](credential-broker.md) · [Proof of possession](proof-of-possession.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

[RFC 8693: OAuth 2.0 Token Exchange](https://www.rfc-editor.org/rfc/rfc8693.html). [Meta's Sentinel architecture description](https://research.meta.ai/blog/security-and-safety-for-ai-agents-our-approach-with-muse).
