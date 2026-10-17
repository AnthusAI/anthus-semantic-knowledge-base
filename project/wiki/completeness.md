# Completeness

For a security checker, completeness means every program satisfying the specified property is accepted. Completeness is relative to that property and language. It is not synonymous with checking many examples or supporting many syntax features.

Original example: under mathematical-integer, final-value semantics, public = secret - secret always assigns zero. A checker that rejects every expression mentioning secret rejects this secure program and is incomplete. In a real language, inspect possible overloaded operators, exceptions, and evaluation effects before importing that conclusion.

A checker that accepts everything is complete in this narrow sense but generally unsound. A checker that rejects everything is sound but generally incomplete. Useful systems seek a practical balance while preserving the guarantee they claim.

For sufficiently expressive general programs, an always-terminating exact classifier is unavailable for semantic noninterference; the undecidability page gives a reduction. That limitation does not prevent precise decisions in restricted sublanguages or proofs about particular programs.

Exercise: Should an engineering team weaken soundness to reduce false alarms?

Answer: That is a policy and product decision, not a theorem. If it does, the resulting tool must no longer claim that every accepted program satisfies the original property.

## Related concepts

[Soundness](soundness.md) · [Undecidability](undecidability.md) · [Finite state verification](finite-state-verification.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Dennis Volpano, Cynthia Irvine and Geoffrey Smith. “A Sound Type System for Secure Flow Analysis.” Journal of Computer Security 4(2–3), 1996. [DOI](https://doi.org/10.3233/JCS-1996-42-304).
