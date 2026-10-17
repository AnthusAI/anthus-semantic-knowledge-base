# Hyperproperty

A hyperproperty classifies sets of execution traces. An ordinary trace property classifies each execution individually. This distinction matters because many confidentiality claims compare what could happen across different secret inputs.

Original example: the single observed trace “output 0” is compatible with both an always-zero program and a program that outputs a secret bit. The first program may satisfy noninterference; the second does not. The trace alone cannot establish their difference.

Hyperproperties provide a general framework for these relational requirements. The term does not mean that every hyperproperty is noninterference or that every verification problem needs the same technique. Different definitions quantify over different sets of traces, schedules, attackers, or probabilities.

Exercise: Why is “we observed no suspicious output in a thousand runs” weaker than a noninterference proof?

Answer: Those runs cover only selected executions. A relational property requires the relevant comparisons across the whole modeled behavior set. Testing remains useful for finding counterexamples, checking implementation assumptions, and validating finite models; its evidential role should be named accurately.

## Related concepts

[Noninterference](noninterference.md) · [Execution monitor](execution-monitor.md) · [Finite state verification](finite-state-verification.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Michael R. Clarkson and Fred B. Schneider. “Hyperproperties.” Journal of Computer Security 18(6), 1157–1210, 2010. [DOI](https://doi.org/10.3233/JCS-2009-0393).
