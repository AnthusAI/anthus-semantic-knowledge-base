# Execution monitor

An execution monitor observes a running system and can stop an operation before a policy violation. Schneider studies a specific class of mechanisms that observe execution steps and terminate the target when necessary. The model's information restrictions are essential to its conclusions.

Original example: a monitor can prevent every delete operation by checking an event before it commits. When it sees public output 42, however, the current trace alone does not establish whether that output depends on a secret. The same bytes can be harmless in one program and a disclosure in another.

This does not prove runtime IFC impossible. A runtime can maintain labels, instrument computation, constrain permitted operations, and enforce a conservative rule. Static analysis can contribute information about alternative paths. A combined design must explain what it tracks and how bypasses are excluded.

Exercise: Is blocking every public write after reading Secret data a perfect semantic classifier?

Answer: No. It may reject harmless constant output. It can still support a useful sound policy under appropriate assumptions. Precision and security are separate evaluation axes.

## Related concepts

[Hyperproperty](hyperproperty.md) · [Soundness](soundness.md) · [Reference monitor](reference-monitor.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Fred B. Schneider. “Enforceable Security Policies.” ACM Transactions on Information and System Security 3(1), 30–50, 2000. [Full text](https://www.cs.cornell.edu/fbs/publications/EnfSecPols.pdf).
