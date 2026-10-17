# Capability

A capability is an unforgeable reference carrying authority to perform specified operations on a resource. Its value is the authority it grants, not whether it looks like a conventional password.

Capsicum demonstrates capability-oriented compartmentalization using operating-system support, including capability mode and rights-limited file descriptors. This illustrates a concrete alternative to giving every component broad ambient access.

Original example: a worker receives a handle permitting append-only writes to one staging log. It receives no handle for reading production secrets or changing access permissions. Compromise of that worker can misuse the granted append authority, but should not automatically grant unrelated operations if the boundary is enforced.

A bearer token returned by a broker can itself be a capability. Returning a broadly privileged token may therefore undermine a design even if the original private key never leaves the broker.

Exercise: Can a capability system prevent a worker from leaking a secret it legitimately reads to a public handle it legitimately possesses?

Answer: Those two authorities can permit the flow. Capability confinement and information-flow confinement must be distinguished; reducing authority can help, but the desired policy still needs analysis.

## Related concepts

[Access control](access-control.md) · [Credential broker](credential-broker.md) · [Authority misuse](authority-misuse.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Robert N. M. Watson, Jonathan Anderson, Ben Laurie and Kris Kennaway. “Capsicum: Practical Capabilities for UNIX.” USENIX Security Symposium, 2010. [Full text](https://www.usenix.org/legacy/events/sec10/tech/full_papers/Watson.pdf).
