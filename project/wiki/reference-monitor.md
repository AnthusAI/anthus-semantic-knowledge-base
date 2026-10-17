# Reference monitor

A reference monitor mediates security-relevant operations between a subject and protected resources. The practical design question is whether every relevant path passes through the policy check and whether the subject can bypass or alter that enforcement.

Original example: an API wrapper checks permissions before invoking a credentialed operation. If the worker can read the wrapper's environment or contact an alternate endpoint with the same role credentials, the wrapper is not the complete boundary. A correct check on one path does not mediate another path.

Keep the enforcement mechanism small and identify every entry point. Complete mediation, least privilege, and economy of mechanism are classic design principles, but citing them is not a proof that a particular deployment implements them.

Exercise: A broker validates every request it receives. What remains to show?

Answer: That the worker cannot access the protected operation without the broker; that policy inputs and identities are authentic; that validation covers the actual operation; and that responses, errors, and persistent state obey the release policy. Also identify the trusted components on which these claims depend.

## Related concepts

[Execution monitor](execution-monitor.md) · [Trusted computing base](trusted-computing-base.md) · [Credential broker](credential-broker.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Jerome H. Saltzer and Michael D. Schroeder. “The Protection of Information in Computer Systems.” Proceedings of the IEEE 63(9), 1278–1308, 1975. [Full text](https://web.cs.wpi.edu/~cs557/f14/papers/saltzer1975_alt.html).
