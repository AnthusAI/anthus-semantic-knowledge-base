# Implicit information flow

An implicit flow carries information through control dependence. The destination need not mention the secret value.

```python
public = 0
if secret & 1:
    public = 1
```

This original example releases the low bit exactly as public = secret & 1 would. Tracking only explicit assignments from secret values misses it. A program-counter label can record that execution is currently controlled by a Secret condition; a checker can then reject a Public assignment in that context.

This is a conservative enforcement strategy, not a definition of every semantic leak. If both branches assign the same value, their final values may be indistinguishable. If one branch takes longer, a timing observer may still distinguish them. The observer matters before deciding whether a particular control dependency violates confidentiality.

Exercise: Both branches assign public = 7. A checker rejects the program. Is the checker necessarily unsound?

Answer: No. Soundness prohibits accepting insecure programs. Rejecting a secure program is a completeness limitation. Under a final-value-only model, this example can be secure despite the conservative rejection.

## Related concepts

[Explicit information flow](explicit-flow.md) · [Observation model](observation-model.md) · [Soundness](soundness.md) · [Completeness](completeness.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Dennis Volpano, Cynthia Irvine and Geoffrey Smith. “A Sound Type System for Secure Flow Analysis.” Journal of Computer Security 4(2–3), 1996. [DOI](https://doi.org/10.3233/JCS-1996-42-304).
