# Proof of possession and token replay

A bearer credential is usable by a party that possesses it, subject to server policy. Proof-of-possession mechanisms require an additional proof involving a key. Token theft and access to the proof key are therefore different events.

OAuth tokens are not all bearer tokens. DPoP binds tokens to a public key and uses signed proofs in requests. Mutual-TLS certificate-bound access tokens require the corresponding client certificate authentication. These mechanisms can reduce replay of a stolen token without its associated key; they do not make the token's bytes invisible. [RFC 9449 §2](https://www.rfc-editor.org/rfc/rfc9449.html#section-2), [RFC 8705 §3](https://www.rfc-editor.org/rfc/rfc8705.html#section-3).

Original example: an agent cannot read a non-exportable proof key but may invoke its signing operation for arbitrary requests. It may still use the bound token through that operation. The architecture needs a policy governing permitted uses of the key, in addition to protection against export. DPoP's security discussion addresses the importance of the execution environment and key protection. [RFC 9449 §11](https://www.rfc-editor.org/rfc/rfc9449.html#section-11).

Short lifetimes and limited scopes can reduce damage when enforced. A refresh token may restore longer-term access. Passwords, cookies, signing keys, and delegated credentials all belong in the authority inventory: analyze what possession or permitted use of each object can cause.

Exercise: Does replacing a bearer token with a key-bound token prove that a compromised worker cannot call the API?

Answer: No. Determine whether the worker can use the associated key or request proofs from a service, and whether the server verifies the binding. Key binding addresses a replay threat under specified conditions; it does not implement the entire operation-authorization policy.

## Related concepts

[Capability](capability.md) · [Credential surrogation](credential-surrogation.md) · [Authority misuse](authority-misuse.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

[RFC 9449: OAuth 2.0 Demonstrating Proof of Possession](https://www.rfc-editor.org/rfc/rfc9449.html). [RFC 8705: OAuth 2.0 Mutual-TLS Client Authentication and Certificate-Bound Access Tokens](https://www.rfc-editor.org/rfc/rfc8705.html).
