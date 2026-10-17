# Credential broker

A broker pattern changes the problem:

1. The worker sends a structured request describing a permitted operation.
2. A trusted broker checks identity, resource, operation, payload constraints, budget and any required approval.
3. The broker uses the credential in its own protected environment.
4. It returns only the response allowed by the release policy.

Wedge provides a concrete precedent: it separates sensitive state into compartments and exposes callgates, including an OpenSSH worker without direct access to private keys or user credentials. Its Apache discussion also distinguishes hiding an RSA key from preventing abuse of decryption-related operations. [Bittau and colleagues 2008](https://www.usenix.org/legacy/event/nsdi08/tech/full_papers/bittau/bittau_html/index.html)

Original broker review checklist: Can an error echo authorization headers? Can redirects forward credentials? Can a returned token replace the original key? Can an attacker select a destination embedded inside an otherwise approved request? Can the worker read broker memory, storage, environment, logs, or cloud identity? Can the interface become an oracle across repeated calls?

Exercise: A key never leaves the broker, but the worker can ask it to delete every production record. Is the system secure?

Answer: Key confidentiality may hold while authority control fails catastrophically. State the two claims separately. A protected credential facility must prevent disclosure of useful derivatives and constrain permitted uses; neither follows merely from process separation.

## Related concepts

[Trusted computing base](trusted-computing-base.md) · [Authority misuse](authority-misuse.md) · [Declassification](declassification.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Andrea Bittau, Petr Marchenko, Mark Handley and Brad Karp. “Wedge: Splitting Applications into Reduced-Privilege Compartments.” USENIX NSDI, 2008. [Full text](https://www.usenix.org/legacy/event/nsdi08/tech/full_papers/bittau/bittau_html/index.html).
