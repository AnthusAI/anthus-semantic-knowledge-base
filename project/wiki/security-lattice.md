# Security lattice

A security lattice organizes confidentiality labels and permitted flows. In a two-label teaching system, Public information may flow into Secret storage, while a Secret-to-Public flow requires a permitted release. More complex policies need not rank all labels on one line.

Original example: let labels represent the sets of customers whose information a value contains. Data labeled {A} and {B} are incomparable: neither may be sent into storage reserved solely for the other. Combining the two produces {A, B}. Set inclusion orders the labels, and union supplies their least upper bound, or join.

The lattice expresses policy structure. It does not by itself track a Python program, prove a compiler correct, or guarantee that the operating system respects labels. Those are separate enforcement obligations.

Exercise: A calculation combines a public constant, customer A's private data, and customer B's private data. What label should its result receive under the set model?

Answer: {A, B}. The public constant adds no customer restriction. Releasing the combined result to A alone would require another policy decision rather than silently dropping B's restriction.

## Related concepts

[Information flow control](information-flow-control.md) · [Explicit information flow](explicit-flow.md) · [Declassification](declassification.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Dorothy E. Denning. “A Lattice Model of Secure Information Flow.” Communications of the ACM 19(5), 236–243, 1976. [DOI](https://doi.org/10.1145/360051.360056).
