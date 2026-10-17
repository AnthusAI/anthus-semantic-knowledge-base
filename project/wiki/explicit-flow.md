# Explicit information flow

An explicit flow transfers information through a data dependency. In Python, public = secret & 1 directly makes the public result depend on a secret operand. A complete copy is not required: a single bit is information.

Original worked example:

```python
encoded = bytes(value ^ 0x55 for value in secret)
write_public(encoded)
```

The output is reversibly transformed, so hiding the literal secret string does not remove the flow. The same reasoning applies to base64, compression, reordered fragments, and a file format carrying attacker-decodable bytes. A hash is not generally reversible, but it can still reveal equality or enable guessing when the input space is small.

An explicit flow may be allowed when the destination has an appropriate confidentiality label. The existence of a dependency is not itself a policy violation; the policy says which dependencies are permitted.

Exercise: Does public = len(secret) disclose the secret?

Answer: It reveals length if length varies and is not already public. Whether that is permitted depends on the policy. “It does not print the key” is insufficient to settle the question.

## Related concepts

[Implicit information flow](implicit-flow.md) · [Information flow control](information-flow-control.md) · [Noninterference](noninterference.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Andrei Sabelfeld and Andrew C. Myers. “Language-Based Information-Flow Security.” IEEE Journal on Selected Areas in Communications 21(1), 2003. [Full text](https://www.cs.cornell.edu/andru/papers/jsac/sm-jsac03.pdf).
