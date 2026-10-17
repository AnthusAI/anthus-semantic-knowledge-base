# Trusted computing base

The trusted computing base, or TCB, is the collection of components whose correct behavior is required for the security claim. “Trusted” describes a dependency of the argument; it does not mean the component is known to be flawless.

Original example: a worker cannot directly read a broker's key. The claim may nevertheless depend on the kernel's memory isolation, cloud identity configuration, broker implementation, request parser, logging system, administrative interfaces, and any process capable of debugging the broker. If a privileged log collector records the key and makes logs visible to the worker, memory isolation alone is insufficient.

The TCB is claim-specific. A component required for availability may not be required for a narrowly stated confidentiality result, and vice versa. List dependencies for the property actually being asserted.

Exercise: Does moving a key into a second container necessarily shrink the TCB?

Answer: No. Shared identities, writable volumes, task-level privileges, administrative access, and response channels can preserve or introduce dependencies. Draw the complete authority and data graph before evaluating the boundary.

## Related concepts

[Reference monitor](reference-monitor.md) · [Credential broker](credential-broker.md) · [Observation model](observation-model.md) · [Container isolation](container-isolation.md) · [Security evidence](security-evidence.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Andrea Bittau, Petr Marchenko, Mark Handley and Brad Karp. “Wedge: Splitting Applications into Reduced-Privilege Compartments.” USENIX NSDI, 2008. [Full text](https://www.usenix.org/legacy/event/nsdi08/tech/full_papers/bittau/bittau_html/index.html).
