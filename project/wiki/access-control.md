# Access control

Access control determines which principals may perform which operations on which resources. A Rails authorization check that permits one account to read one invoice is an access-control decision. Its success does not establish that the reader will preserve the invoice's confidentiality afterward.

Original example: a worker is legitimately allowed to read a production credential to contact an API. It is also allowed to write reports. Every read and write can be individually authorized while the worker copies the credential into a report. A policy about the relationship between the inputs and outputs is needed to forbid that propagation.

This distinction does not make access control unimportant. Denying unnecessary secret reads can eliminate an information-flow problem before it begins. The architectural question is whether a worker needs the credential itself or only a narrowly authorized operation performed with it.

Exercise: If a worker can read a file but cannot change it, can it still leak the file?

Answer: Yes. Read-only describes permitted changes to that file. It says nothing about copying its contents into another output. Identify the output observer before claiming a disclosure.

## Related concepts

[Information flow control](information-flow-control.md) · [Capability](capability.md) · [Confinement](confinement.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

Jerome H. Saltzer and Michael D. Schroeder. “The Protection of Information in Computer Systems.” Proceedings of the IEEE 63(9), 1278–1308, 1975. [Full text](https://web.cs.wpi.edu/~cs557/f14/papers/saltzer1975_alt.html).
