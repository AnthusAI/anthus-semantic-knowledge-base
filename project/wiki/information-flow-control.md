# Information flow control

Information-flow control, abbreviated IFC, enforces restrictions on how information propagates through computation. It concerns dependencies between values and observations rather than just whether a particular resource may be opened.

Original example: suppose customer A's invoice is Secret and a public report is Public. Copying the invoice, compressing it, encrypting it to an attacker-controlled key, or encoding it in filenames can all violate the same intended flow restriction. A pattern filter for the original bytes recognizes only one representation.

IFC can be enforced through a restricted language, static analysis, runtime labels, operating-system mechanisms, or combinations. Each concrete mechanism needs a theorem or argument about a precise semantics and observation model. A type system sound for final public values does not automatically handle real-time measurements or an unmodeled native extension.

Exercise: Must an IFC system reject every program that reads a secret?

Answer: No. A program can process Secret data and keep all its results in Secret storage. The prohibited step is an unauthorized flow to an observation or destination. Useful intentional release needs an explicit declassification policy.

## Related concepts

[Explicit information flow](explicit-flow.md) · [Implicit information flow](implicit-flow.md) · [Security lattice](security-lattice.md) · [Declassification](declassification.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Dorothy E. Denning. “A Lattice Model of Secure Information Flow.” Communications of the ACM 19(5), 236–243, 1976. [DOI](https://doi.org/10.1145/360051.360056).
