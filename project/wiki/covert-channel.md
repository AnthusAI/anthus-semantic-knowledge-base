# Covert channel

A covert channel uses a mechanism not intended for information transfer. Ordinary outputs such as stdout are overt channels; both can disclose secrets.

Consider a worker that emits identical JSON in every run but sleeps for one or two seconds depending on a secret bit. A timestamp observer sees a leak; a final-value-only observer does not. A worker that loops forever for one bit value can communicate through completion or timeout behavior.

Termination-insensitive reasoning deliberately abstracts away some differences involving divergence. It must not be reported as a guarantee against timeout-based extraction. Likewise, a model that excludes CPU contention establishes no claim about an observer measuring shared CPU contention.

Lampson's examples include persistent storage and resource-based communication. This is why confinement requires considering the environment around a process, including information retained between calls. [Lampson 1973](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/acrobat-24.pdf)

Worked example: A disconnected worker writes a secret bit by creating either a short-lived file or no file. A later snapshot exposes its existence. “No network during execution” did not remove the later output.

Exercise: List five channels in an agent system beyond HTTP.

Answer: User-visible stdout, error messages, generated artifacts, persistent workspace snapshots, and operation timing are five. Depending on access, metrics labels, crash dumps, DNS, shared caches, and broker responses may add more. A channel inventory should name its observer and trust boundary; an inaccessible file is not automatically an attacker channel.

## Related concepts

[Observation model](observation-model.md) · [Termination sensitivity](termination-sensitivity.md) · [Confinement](confinement.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Butler W. Lampson. “A Note on the Confinement Problem.” Communications of the ACM 16(10), 613–615, 1973. [Full text](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/acrobat-24.pdf).
