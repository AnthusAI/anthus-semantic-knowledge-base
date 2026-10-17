# Credential proxies and complete mediation

A credential-injecting proxy can keep a credential out of the worker's original request. Its security depends on the entire request and response path: destination identity, operation authorization, credential attachment, redirects, responses, errors, logging, and alternative routes.

Authorize the effective destination and operation before attaching credentials. A 302 response does not by itself guarantee that a client forwards authentication material: redirect handling varies. Denying redirects is a simple policy. Otherwise, revalidate each hop and prevent credentials from being carried into a different protection space. [RFC 9110 §§11.5 and 15.4](https://www.rfc-editor.org/rfc/rfc9110.html#section-15.4).

Original example: an allowed service reflects an injected authorization header in an error response. The worker never supplied the credential but can now observe it. A response-release policy must account for useful derivatives, returned tokens, errors, and repeated queries. Restricting the response to an allowed string field leaves a channel if that string can contain encoded data.

A one-byte response cap can release one secret byte per call. A yes/no answer can release one bit. Output caps and query budgets may bound bandwidth or cost without establishing zero disclosure. These counterexamples do not establish that every restricted output policy fails; they identify claims requiring a more precise argument.

A semantic broker can expose a meaningful operation such as publishing an approved manifest instead of arbitrary authenticated requests. It may have CLI or HTTP frontends usable from native scripts. The interface protocol, including MCP, does not itself establish confinement. The enforcement boundary and admitted operations determine the guarantee.

Exercise: A worker's proxy environment variables and CA certificate are configured correctly. Has every hostile request been mediated?

Answer: Not necessarily. Establish an enforced route and account for direct sockets, DNS, IPv6, alternate protocols, and credential-service destinations. Configuration that cooperative clients follow does not prove arbitrary worker code must follow it.

## Related concepts

[Reference monitor](reference-monitor.md) · [Credential broker](credential-broker.md) · [Declassification](declassification.md) · [Container isolation](container-isolation.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

[RFC 9110: HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110.html). Andrew C. Myers, Andrei Sabelfeld and Steve Zdancewic. [Enforcing Robust Declassification](https://www.cs.cornell.edu/andru/papers/csfw04.pdf), 2004.
