# Information flow security and secret confinement for AI agents

This syllabus is a connected textbook for engineers who know Python, Rails, or ML systems and want the formal security concepts needed to design agent execution environments. Each linked term has its own explanation, worked example, exercise with an answer, related concepts, and primary-source reading.

The core result is elementary: arbitrary code that may read a secret and choose arbitrary public output can output the secret. The design task is to change those assumptions while preserving useful computation. A credential broker can keep a key outside untrusted code and expose a narrow operation, but key confidentiality and authorization must be evaluated separately.

A long-lived harness may eventually run agents much more capable than those used to design it. Structural boundaries and model-relative proofs are more durable than a catalog of today's tricks. They still depend on implementation correctness, explicit observations, and the assumptions named in the claim. No absolute guarantee about every physical channel or future implementation follows.

# Learning path

## 1 Frame the security question

1. [Threat model](threat-model.md)
2. [Confidentiality](confidentiality.md)
3. [Access control](access-control.md)
4. [Confinement](confinement.md)
5. [Secret confinement impossibility under unrestricted output](secret-confinement-impossibility.md)

## 2 Follow information through computation

1. [Information flow control](information-flow-control.md)
2. [Security lattice](security-lattice.md)
3. [Explicit information flow](explicit-flow.md)
4. [Implicit information flow](implicit-flow.md)

## 3 Specify what the attacker can distinguish

1. [Noninterference](noninterference.md)
2. [Observation model](observation-model.md)
3. [Covert channel](covert-channel.md)
4. [Termination sensitivity](termination-sensitivity.md)

## 4 Understand proof and enforcement limits

1. [Hyperproperty](hyperproperty.md)
2. [Execution monitor](execution-monitor.md)
3. [Soundness](soundness.md)
4. [Completeness](completeness.md)
5. [Undecidability](undecidability.md)
6. [Finite state verification](finite-state-verification.md)

## 5 Design a constrained credential service

1. [Declassification](declassification.md)
2. [Robust declassification](robust-declassification.md)
3. [Capability](capability.md)
4. [Reference monitor](reference-monitor.md)
5. [Trusted computing base](trusted-computing-base.md)
6. [Credential broker](credential-broker.md)
7. [Authority misuse](authority-misuse.md)

# Read the original papers

Start with Lampson for confinement intuition, Denning for label structure, Goguen and Meseguer for noninterference, and Sabelfeld and Myers for a readable survey. Then study Volpano and colleagues for type-system soundness; Schneider and Clarkson with Schneider for enforcement and hyperproperties; Myers and colleagues for robust declassification; and Wedge and Capsicum for concrete compartmentalization.

The source attached to a term supports its formal or historical background. Original examples in these pages illustrate the ideas; they are not claims that a cited paper evaluated a particular agent harness. The halting reduction is a modern teaching construction, not a quotation of Turing's original paper.

# Capstone exercise

Specify a broker operation for one real development task without giving the worker the underlying credential.

Write its request schema, authorization predicate, allowed response, query budget, persistence policy, trusted computing base, observation model, and deliberately excluded observations. Try to break the design using encoded output, errors, returned credentials, redirects, repeated queries, and authority misuse.

A satisfactory answer identifies what is guaranteed, under which assumptions, and what evidence establishes it. Tests can find counterexamples; a proof can establish a property of a model; deployment verification checks whether the implementation and configuration satisfy the assumptions.

# Prospective article sequence

- The short exfiltration proof and the permissions it forces us to reconsider
- Explicit and implicit flows in a tiny Python interpreter
- Two-run tests and the gap between examples and noninterference
- A careful halting reduction and a finite model that remains verifiable
- A broker with a written release policy and separate authority policy
- An end-to-end review of outputs, persistence, identity, and trusted components

Each article should state one claim, one model, one worked example, and what would strengthen the evidence.

# Complete reading edition

[NotebookLM ready reading edition](information-flow-security-notebooklm.md) contains the course as one self-contained document.

# Bibliography

1. Butler W. Lampson. “A Note on the Confinement Problem.” Communications of the ACM 16(10), 613–615, 1973. [Full text](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/acrobat-24.pdf).
2. Dorothy E. Denning. “A Lattice Model of Secure Information Flow.” Communications of the ACM 19(5), 236–243, 1976. [DOI](https://doi.org/10.1145/360051.360056).
3. Andrei Sabelfeld and Andrew C. Myers. “Language-Based Information-Flow Security.” IEEE Journal on Selected Areas in Communications 21(1), 2003. [Full text](https://www.cs.cornell.edu/andru/papers/jsac/sm-jsac03.pdf).
4. Joseph A. Goguen and José Meseguer. “Security Policies and Security Models.” IEEE Symposium on Security and Privacy, 1982. [Full text](https://www.cs.purdue.edu/homes/ninghui/readings/AccessControl/goguen_meseguer_82.pdf).
5. Michael R. Clarkson and Fred B. Schneider. “Hyperproperties.” Journal of Computer Security 18(6), 1157–1210, 2010. [DOI](https://doi.org/10.3233/JCS-2009-0393).
6. Fred B. Schneider. “Enforceable Security Policies.” ACM Transactions on Information and System Security 3(1), 30–50, 2000. [Full text](https://www.cs.cornell.edu/fbs/publications/EnfSecPols.pdf).
7. Andrew C. Myers, Andrei Sabelfeld and Steve Zdancewic. “Enforcing Robust Declassification.” IEEE Computer Security Foundations Workshop, 2004. [Full text](https://www.cs.cornell.edu/andru/papers/csfw04.pdf).
8. Robert N. M. Watson, Jonathan Anderson, Ben Laurie and Kris Kennaway. “Capsicum: Practical Capabilities for UNIX.” USENIX Security Symposium, 2010. [Full text](https://www.usenix.org/legacy/events/sec10/tech/full_papers/Watson.pdf).
9. Andrea Bittau, Petr Marchenko, Mark Handley and Brad Karp. “Wedge: Splitting Applications into Reduced-Privilege Compartments.” USENIX NSDI, 2008. [Full text](https://www.usenix.org/legacy/event/nsdi08/tech/full_papers/bittau/bittau_html/index.html).
10. Jerome H. Saltzer and Michael D. Schroeder. “The Protection of Information in Computer Systems.” Proceedings of the IEEE 63(9), 1278–1308, 1975. [Full text](https://web.cs.wpi.edu/~cs557/f14/papers/saltzer1975_alt.html).
11. Dennis Volpano, Cynthia Irvine and Geoffrey Smith. “A Sound Type System for Secure Flow Analysis.” Journal of Computer Security 4(2–3), 1996. [DOI](https://doi.org/10.3233/JCS-1996-42-304).
12. Alan M. Turing. “On Computable Numbers, with an Application to the Entscheidungsproblem.” Proceedings of the London Mathematical Society, series 2, 42, 230–265, 1936–1937. [Original paper](https://theory.stanford.edu/~trevisan/cs172-07/turing36.pdf).
