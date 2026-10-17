# Secret confinement impossibility under unrestricted output

Suppose an execution environment promises all three:

1. Arbitrary worker code may read K.
2. Worker code may choose arbitrary bytes on an attacker-visible output.
3. No permitted worker can disclose K.

Choose the program below.

```python
def worker(secret, public_output):
    public_output.write(secret)
```

Permissions 1 and 2 permit this program, and its output contradicts 3. That is the entire proof. Replacing write with an HTTP request, an artifact, or a log changes the channel rather than the contradiction.

A wrapper can block this execution by denying reads, denying or constraining outputs, transforming the program under enforced semantics, or trusting the program. Each changes an assumption. A wrapper that blocks every output is confidential in this simple model but usually useless.

This argument is separate from undecidability, Gödel's incompleteness theorems, and the difficulty of detecting malicious intent. It also says nothing against useful restricted systems.

Worked example: A Rails job receives an API key through ENV and can create downloadable reports. Blocking outbound internet leaves reports as a disclosure channel. Excluding .env from reports does not prevent the job from copying the value into report.csv.

Exercise: Does a command allowlist solve the contradiction if it allows Python scripts and arbitrary script bodies?

Answer: No. The permitted interpreter still implements the counterexample. An allowlist can reduce attack surface, but its actual semantics determine the guarantee.

## Related concepts

[Confinement](confinement.md) · [Undecidability](undecidability.md) · [Credential broker](credential-broker.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Butler W. Lampson. “A Note on the Confinement Problem.” Communications of the ACM 16(10), 613–615, 1973. [Full text](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/acrobat-24.pdf).
