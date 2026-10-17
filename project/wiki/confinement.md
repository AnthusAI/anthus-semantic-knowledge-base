# Confinement

Access control asks whether a principal may read an object or invoke an operation. Information flow asks where information may go afterward. Confinement asks how to constrain an untrusted computation's communication. Lampson's 1973 paper makes the enduring point that a service can obey access restrictions and still leak its legitimate inputs through files, messages, or resource use. [Lampson 1973](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/acrobat-24.pdf)

Start with a small threat model:

- Secret: a credential K, including reversible encodings and useful derivatives.
- Attacker control: every instruction in a Python worker.
- Public observations: stdout, network requests, returned files, logs and snapshots accessible to the attacker.
- Trusted components: the kernel, isolation configuration, and any broker.
- Desired guarantee: the attacker cannot recover K through those observations.

“Cannot recover K” and “cannot learn any fact about K” differ. Revealing the first bit violates the latter even if it does not immediately reconstruct the credential. A derived bearer token may confer the same authority without revealing K at all.

Exercise: A worker prints a base64 encoding of K. Did it respect a rule forbidding the literal substring K?

Answer: Possibly, but it violated the intended confidentiality policy. The policy must concern information or usable authority, not one byte pattern.

## Related concepts

[Access control](access-control.md) · [Secret confinement impossibility under unrestricted output](secret-confinement-impossibility.md) · [Covert channel](covert-channel.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Butler W. Lampson. “A Note on the Confinement Problem.” Communications of the ACM 16(10), 613–615, 1973. [Full text](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/acrobat-24.pdf).
