# Termination sensitivity

Termination sensitivity concerns whether termination and divergence count as distinguishable behavior. If a program completes for one secret and loops for another, an observer who can distinguish those behaviors can learn something.

Original example:

```python
if secret_bit:
    while True:
        pass
emit("done")
```

A completion observer distinguishes the two cases. A termination-insensitive formulation may abstract away the problematic comparison involving divergence. Such a result must not be presented as eliminating termination channels. Formal definitions differ; always inspect the paper's actual execution and trace relation.

Real systems often impose a timeout. This turns indefinite execution into a finite observation such as “done before the deadline” versus “timed out.” The timeout can preserve a useful distinguishing channel rather than solve the confidentiality problem. Exceptions, cancellation, and partial outputs need similar care.

Exercise: Does returning the same error message after every timeout eliminate the channel?

Answer: Not if some secrets produce a successful response and others time out, or if the attacker can distinguish their timing. Uniform text is only one part of the observable behavior.

## Related concepts

[Observation model](observation-model.md) · [Noninterference](noninterference.md) · [Robust declassification](robust-declassification.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Andrew C. Myers, Andrei Sabelfeld and Steve Zdancewic. “Enforcing Robust Declassification.” IEEE Computer Security Foundations Workshop, 2004. [Full text](https://www.cs.cornell.edu/andru/papers/csfw04.pdf).
