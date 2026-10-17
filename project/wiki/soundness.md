# Soundness

A checker is sound for a specified security property when every program it accepts satisfies that property under the stated model. The direction matters: acceptance implies security. Soundness alone does not promise that every secure program will be accepted.

Original example: a checker rejects all Public assignments executed under a Secret branch condition. It rejects a program whose two branches both assign the same constant. Under final-value-only observations, that program can be secure. The rejection does not refute soundness; it demonstrates a conservative approximation.

A soundness theorem should name the language, operational semantics, security property, permitted observations, and trusted assumptions. A result about a small deterministic language does not automatically cover Python native extensions, network stacks, scheduling, or reflective evaluation.

Volpano, Irvine and Smith provide a foundational type-system treatment of secure flow analysis. Study the theorem alongside the typing rules rather than treating “typed” as a universal guarantee.

Exercise: A checker accepts one insecure program and rejects ten secure programs. Which properties have failed?

Answer: Soundness has failed because of the accepted insecure program. Completeness has failed because secure programs were rejected.

## Related concepts

[Completeness](completeness.md) · [Implicit information flow](implicit-flow.md) · [Undecidability](undecidability.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Dennis Volpano, Cynthia Irvine and Geoffrey Smith. “A Sound Type System for Secure Flow Analysis.” Journal of Computer Security 4(2–3), 1996. [DOI](https://doi.org/10.3233/JCS-1996-42-304).
