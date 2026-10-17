# Undecidability

A checker is sound for a stated property when every program it accepts satisfies that property under the model. It is complete when it accepts every program satisfying that property. A checker may be sound and reject safe programs.

Worked example: A syntactic rule rejecting every expression mentioning secret rejects public = secret - secret, even over mathematical integers where the result is always zero. That is a false alarm relative to a final-value model, not evidence that sound enforcement is impossible.

Here is an original halting reduction showing why a total, sound and complete decision procedure for a broad semantic confidentiality property cannot exist for general unbounded programs. Given an arbitrary machine M and input x, construct:

```text
P(secret):
    simulate M on x
    emit secret
```

The simulation has no secret-dependent behavior. If M never halts, P has the same non-emitting divergent behavior for every secret. If M halts, P emits different outputs for different secrets. Thus P satisfies the specified noninterference property exactly when M does not halt. A decider that always answered correctly for every such P would decide halting by negating its answer.

This assumes an effective general-purpose language with unbounded simulation and at least two possible secrets. Physical computers have finite resources; bounded finite-state models can be decidable, though expensive. Testing finitely many executions is not the same as exhaustively verifying a complete finite model.

Exercise: Does undecidability rule out a proof for one broker or a sound type system?

Answer: No. It rules out the universal terminating, exact classifier in the specified general setting. Restricted languages, conservative analyses, interactive proofs, and finite-state verification remain available.

## Related concepts

[Soundness](soundness.md) · [Completeness](completeness.md) · [Finite state verification](finite-state-verification.md) · [Secret confinement impossibility under unrestricted output](secret-confinement-impossibility.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Alan M. Turing. “On Computable Numbers, with an Application to the Entscheidungsproblem.” Proceedings of the London Mathematical Society, series 2, 42, 230–265, 1936–1937. [Original paper](https://theory.stanford.edu/~trevisan/cs172-07/turing36.pdf).
