# Information flow security and secret confinement for AI agents: complete reading edition

This syllabus is a connected textbook for engineers who know Python, Rails, or ML systems and want the formal security concepts needed to design agent execution environments. Each linked term has its own explanation, worked example, exercise with an answer, related concepts, and primary-source reading.

The core result is elementary: arbitrary code that may read a secret and choose arbitrary public output can output the secret. The design task is to change those assumptions while preserving useful computation. A credential broker can keep a key outside untrusted code and expose a narrow operation, but key confidentiality and authorization must be evaluated separately.

A long-lived harness may eventually run agents much more capable than those used to design it. Structural boundaries and model-relative proofs are more durable than a catalog of today's tricks. They still depend on implementation correctness, explicit observations, and the assumptions named in the claim. No absolute guarantee about every physical channel or future implementation follows.


# Course reading edition

This self-contained edition collects all 26 term pages in syllabus order, with their worked examples, exercises, answers, and primary readings.

## 1 Threat model

A threat model states what is protected, who may attack it, which parts the attacker controls, what the attacker observes, and what assumptions remain trusted. It turns “secure against agents” into a question that can be answered.

Original example: protect a signing key against a worker that controls every Python instruction and every tool argument. The worker can see stdout, returned files, and broker responses. The key is outside worker memory. The kernel and broker are trusted. The policy permits signing only one approved release manifest. Each sentence constrains a different part of the argument.

For a harness expected to operate for five or six years or more, potentially with much more capable agents, structural boundaries matter. A proof quantifying over every permitted worker program does not depend on predicting today's favorite attack. It still depends on correct implementation, adequate observations, and any cryptographic or physical assumptions.

Exercise: Does a stronger model invalidate a correct isolation theorem?

Answer: Intelligence alone does not invalidate a theorem within its assumptions. It may expose a missing channel, bug, overly broad capability, or assumption that never held. State a model-relative guarantee rather than an absolute promise about every future attacker.

### Primary reading

Jerome H. Saltzer and Michael D. Schroeder. “The Protection of Information in Computer Systems.” Proceedings of the IEEE 63(9), 1278–1308, 1975. [Full text](https://web.cs.wpi.edu/~cs557/f14/papers/saltzer1975_alt.html).

## 2 Confidentiality

Confidentiality restricts what an unauthorized observer can learn about protected information. A precise claim identifies the information, observer, permitted release, and observation model.

Original example: a worker does not reveal a complete API key but reveals its first bit. This violates a policy permitting no information about the key, although one bit may not enable immediate impersonation. A different policy might allow length or a public key to be known while protecting the secret value. These are distinct requirements.

Computational confidentiality depends on assumptions about feasible attackers and cryptographic hardness. Information-theoretic independence is a stronger kind of condition. A signature intentionally depends on a private key, yet a suitable signing scheme is designed to avoid efficient key recovery. Calling both properties “the output contains no secrets” obscures their different meanings.

Exercise: A broker returns a short-lived bearer credential that grants all the original key's permissions. Has it preserved the intended protection?

Answer: Protecting the original bytes may be insufficient. The returned credential can confer equivalent usable authority. Decide whether the policy protects one representation, information about the key, delegated authority, or all three.

### Primary reading

Joseph A. Goguen and José Meseguer. “Security Policies and Security Models.” IEEE Symposium on Security and Privacy, 1982. [Full text](https://www.cs.purdue.edu/homes/ninghui/readings/AccessControl/goguen_meseguer_82.pdf).

## 3 Access control

Access control determines which principals may perform which operations on which resources. A Rails authorization check that permits one account to read one invoice is an access-control decision. Its success does not establish that the reader will preserve the invoice's confidentiality afterward.

Original example: a worker is legitimately allowed to read a production credential to contact an API. It is also allowed to write reports. Every read and write can be individually authorized while the worker copies the credential into a report. A policy about the relationship between the inputs and outputs is needed to forbid that propagation.

This distinction does not make access control unimportant. Denying unnecessary secret reads can eliminate an information-flow problem before it begins. The architectural question is whether a worker needs the credential itself or only a narrowly authorized operation performed with it.

Exercise: If a worker can read a file but cannot change it, can it still leak the file?

Answer: Yes. Read-only describes permitted changes to that file. It says nothing about copying its contents into another output. Identify the output observer before claiming a disclosure.

### Primary reading

Jerome H. Saltzer and Michael D. Schroeder. “The Protection of Information in Computer Systems.” Proceedings of the IEEE 63(9), 1278–1308, 1975. [Full text](https://web.cs.wpi.edu/~cs557/f14/papers/saltzer1975_alt.html).

## 4 Confinement

Access control asks whether a principal may read an object or invoke an operation. Information flow asks where information may go afterward. Confinement asks how to constrain an untrusted computation's communication. Lampson's 1973 paper makes the enduring point that a service can obey access restrictions and still leak its legitimate inputs through files, messages, or resource use. [Lampson 1973](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/acrobat-24.pdf)

Start with a small threat model:

- Secret: a credential K, including reversible encodings and useful derivatives.
- Attacker control: every instruction in a Python worker.
- Public observations: stdout, network requests, returned files, logs and snapshots accessible to the attacker.
- Trusted components: the kernel, isolation configuration, and any broker.
- Desired guarantee: the attacker cannot recover K through those observations.

“Cannot recover K” and “cannot learn any fact about K” differ. Revealing the first bit violates the latter even if it does not immediately reconstruct the credential. A derived bearer token may confer the same authority without revealing K at all.

Exercise: A worker prints a base64 encoding of K. Did it respect a rule forbidding the literal substring K?

Answer: Possibly, but it violated the intended confidentiality policy. The policy must concern information or usable authority, not one byte pattern.

### Primary reading

Butler W. Lampson. “A Note on the Confinement Problem.” Communications of the ACM 16(10), 613–615, 1973. [Full text](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/acrobat-24.pdf).

## 5 Secret confinement impossibility under unrestricted output

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

### Primary reading

Butler W. Lampson. “A Note on the Confinement Problem.” Communications of the ACM 16(10), 613–615, 1973. [Full text](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/acrobat-24.pdf).

## 6 Information flow control

Information-flow control, abbreviated IFC, enforces restrictions on how information propagates through computation. It concerns dependencies between values and observations rather than just whether a particular resource may be opened.

Original example: suppose customer A's invoice is Secret and a public report is Public. Copying the invoice, compressing it, encrypting it to an attacker-controlled key, or encoding it in filenames can all violate the same intended flow restriction. A pattern filter for the original bytes recognizes only one representation.

IFC can be enforced through a restricted language, static analysis, runtime labels, operating-system mechanisms, or combinations. Each concrete mechanism needs a theorem or argument about a precise semantics and observation model. A type system sound for final public values does not automatically handle real-time measurements or an unmodeled native extension.

Exercise: Must an IFC system reject every program that reads a secret?

Answer: No. A program can process Secret data and keep all its results in Secret storage. The prohibited step is an unauthorized flow to an observation or destination. Useful intentional release needs an explicit declassification policy.

### Primary reading

Dorothy E. Denning. “A Lattice Model of Secure Information Flow.” Communications of the ACM 19(5), 236–243, 1976. [DOI](https://doi.org/10.1145/360051.360056).

## 7 Security lattice

A security lattice organizes confidentiality labels and permitted flows. In a two-label teaching system, Public information may flow into Secret storage, while a Secret-to-Public flow requires a permitted release. More complex policies need not rank all labels on one line.

Original example: let labels represent the sets of customers whose information a value contains. Data labeled {A} and {B} are incomparable: neither may be sent into storage reserved solely for the other. Combining the two produces {A, B}. Set inclusion orders the labels, and union supplies their least upper bound, or join.

The lattice expresses policy structure. It does not by itself track a Python program, prove a compiler correct, or guarantee that the operating system respects labels. Those are separate enforcement obligations.

Exercise: A calculation combines a public constant, customer A's private data, and customer B's private data. What label should its result receive under the set model?

Answer: {A, B}. The public constant adds no customer restriction. Releasing the combined result to A alone would require another policy decision rather than silently dropping B's restriction.

### Primary reading

Dorothy E. Denning. “A Lattice Model of Secure Information Flow.” Communications of the ACM 19(5), 236–243, 1976. [DOI](https://doi.org/10.1145/360051.360056).

## 8 Explicit information flow

An explicit flow transfers information through a data dependency. In Python, public = secret & 1 directly makes the public result depend on a secret operand. A complete copy is not required: a single bit is information.

Original worked example:

```python
encoded = bytes(value ^ 0x55 for value in secret)
write_public(encoded)
```

The output is reversibly transformed, so hiding the literal secret string does not remove the flow. The same reasoning applies to base64, compression, reordered fragments, and a file format carrying attacker-decodable bytes. A hash is not generally reversible, but it can still reveal equality or enable guessing when the input space is small.

An explicit flow may be allowed when the destination has an appropriate confidentiality label. The existence of a dependency is not itself a policy violation; the policy says which dependencies are permitted.

Exercise: Does public = len(secret) disclose the secret?

Answer: It reveals length if length varies and is not already public. Whether that is permitted depends on the policy. “It does not print the key” is insufficient to settle the question.

### Primary reading

Andrei Sabelfeld and Andrew C. Myers. “Language-Based Information-Flow Security.” IEEE Journal on Selected Areas in Communications 21(1), 2003. [Full text](https://www.cs.cornell.edu/andru/papers/jsac/sm-jsac03.pdf).

## 9 Implicit information flow

An implicit flow carries information through control dependence. The destination need not mention the secret value.

```python
public = 0
if secret & 1:
    public = 1
```

This original example releases the low bit exactly as public = secret & 1 would. Tracking only explicit assignments from secret values misses it. A program-counter label can record that execution is currently controlled by a Secret condition; a checker can then reject a Public assignment in that context.

This is a conservative enforcement strategy, not a definition of every semantic leak. If both branches assign the same value, their final values may be indistinguishable. If one branch takes longer, a timing observer may still distinguish them. The observer matters before deciding whether a particular control dependency violates confidentiality.

Exercise: Both branches assign public = 7. A checker rejects the program. Is the checker necessarily unsound?

Answer: No. Soundness prohibits accepting insecure programs. Rejecting a secure program is a completeness limitation. Under a final-value-only model, this example can be secure despite the conservative rejection.

### Primary reading

Dennis Volpano, Cynthia Irvine and Geoffrey Smith. “A Sound Type System for Secure Flow Analysis.” Journal of Computer Security 4(2–3), 1996. [DOI](https://doi.org/10.3233/JCS-1996-42-304).

## 10 Noninterference

For a deterministic toy system, write run(P, L, H) for execution with public input L and secret input H. Let observe select exactly what the attacker can see. A simple noninterference condition is:

Here run denotes the complete modeled execution, including divergent executions; observe returns its public trace under the chosen treatment of termination.

For every L, H1 and H2, observe(run(P, L, H1)) equals observe(run(P, L, H2)).

The secret may change; the public view must not. Goguen and Meseguer's foundational model expresses noninterference using the effect of one domain's actions on another domain's observations. The equation above is a simplified input-output teaching model, not a quotation of their entire formalism. [Goguen and Meseguer 1982](https://www.cs.purdue.edu/homes/ninghui/readings/AccessControl/goguen_meseguer_82.pdf)

Worked example: P always outputs “done.” For secret inputs 0 and 1, its public traces match. Q outputs the parity of the secret; choosing 0 and 1 produces different traces and witnesses a failure.

The observation function is part of the claim. Final database values, a stream of events, response sizes, wall-clock timestamps, and termination status define different properties. For randomized programs, comparing a single sampled output is inadequate; one commonly compares distributions or uses another explicitly defined probabilistic relation.

Exercise: Can a passing test with two random seeds establish probabilistic noninterference?

Answer: No. It checks those samples. The property quantifies over the specified behavior distribution, inputs, and attacker observations.

### Primary reading

Joseph A. Goguen and José Meseguer. “Security Policies and Security Models.” IEEE Symposium on Security and Privacy, 1982. [Full text](https://www.cs.purdue.edu/homes/ninghui/readings/AccessControl/goguen_meseguer_82.pdf).

## 11 Observation model

An observation model specifies what an attacker can distinguish. It is part of a security claim, not an implementation detail to omit afterward. Two executions can be equivalent for one observer and different for another.

Original example: a worker emits the JSON value 0 for every secret. Under an observer that sees only that value, the executions match. Under an observer that also sees response time, they differ if one secret causes a one-second delay and another causes two seconds. Under an observer that also sees filenames, a secret-dependent artifact name can add another distinction.

Write observations concretely: output bytes, output ordering, sizes, completion or timeout events, timestamps, persistent state, and shared-resource effects. Include only observations available to the specified attacker, but justify important exclusions.

Exercise: A proof excludes wall-clock time. Can it support a claim that no attacker can extract bits using request latency?

Answer: No. It establishes the stated abstraction. To address latency, extend the model and show that the implementation meets the stronger condition, or adopt a justified leakage bound.

### Primary reading

Joseph A. Goguen and José Meseguer. “Security Policies and Security Models.” IEEE Symposium on Security and Privacy, 1982. [Full text](https://www.cs.purdue.edu/homes/ninghui/readings/AccessControl/goguen_meseguer_82.pdf).

## 12 Covert channel

A covert channel uses a mechanism not intended for information transfer. Ordinary outputs such as stdout are overt channels; both can disclose secrets.

Consider a worker that emits identical JSON in every run but sleeps for one or two seconds depending on a secret bit. A timestamp observer sees a leak; a final-value-only observer does not. A worker that loops forever for one bit value can communicate through completion or timeout behavior.

Termination-insensitive reasoning deliberately abstracts away some differences involving divergence. It must not be reported as a guarantee against timeout-based extraction. Likewise, a model that excludes CPU contention establishes no claim about an observer measuring shared CPU contention.

Lampson's examples include persistent storage and resource-based communication. This is why confinement requires considering the environment around a process, including information retained between calls. [Lampson 1973](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/acrobat-24.pdf)

Worked example: A disconnected worker writes a secret bit by creating either a short-lived file or no file. A later snapshot exposes its existence. “No network during execution” did not remove the later output.

Exercise: List five channels in an agent system beyond HTTP.

Answer: User-visible stdout, error messages, generated artifacts, persistent workspace snapshots, and operation timing are five. Depending on access, metrics labels, crash dumps, DNS, shared caches, and broker responses may add more. A channel inventory should name its observer and trust boundary; an inaccessible file is not automatically an attacker channel.

### Primary reading

Butler W. Lampson. “A Note on the Confinement Problem.” Communications of the ACM 16(10), 613–615, 1973. [Full text](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/acrobat-24.pdf).

## 13 Termination sensitivity

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

### Primary reading

Andrew C. Myers, Andrei Sabelfeld and Steve Zdancewic. “Enforcing Robust Declassification.” IEEE Computer Security Foundations Workshop, 2004. [Full text](https://www.cs.cornell.edu/andru/papers/csfw04.pdf).

## 14 Hyperproperty

A hyperproperty classifies sets of execution traces. An ordinary trace property classifies each execution individually. This distinction matters because many confidentiality claims compare what could happen across different secret inputs.

Original example: the single observed trace “output 0” is compatible with both an always-zero program and a program that outputs a secret bit. The first program may satisfy noninterference; the second does not. The trace alone cannot establish their difference.

Hyperproperties provide a general framework for these relational requirements. The term does not mean that every hyperproperty is noninterference or that every verification problem needs the same technique. Different definitions quantify over different sets of traces, schedules, attackers, or probabilities.

Exercise: Why is “we observed no suspicious output in a thousand runs” weaker than a noninterference proof?

Answer: Those runs cover only selected executions. A relational property requires the relevant comparisons across the whole modeled behavior set. Testing remains useful for finding counterexamples, checking implementation assumptions, and validating finite models; its evidential role should be named accurately.

### Primary reading

Michael R. Clarkson and Fred B. Schneider. “Hyperproperties.” Journal of Computer Security 18(6), 1157–1210, 2010. [DOI](https://doi.org/10.3233/JCS-2009-0393).

## 15 Execution monitor

An execution monitor observes a running system and can stop an operation before a policy violation. Schneider studies a specific class of mechanisms that observe execution steps and terminate the target when necessary. The model's information restrictions are essential to its conclusions.

Original example: a monitor can prevent every delete operation by checking an event before it commits. When it sees public output 42, however, the current trace alone does not establish whether that output depends on a secret. The same bytes can be harmless in one program and a disclosure in another.

This does not prove runtime IFC impossible. A runtime can maintain labels, instrument computation, constrain permitted operations, and enforce a conservative rule. Static analysis can contribute information about alternative paths. A combined design must explain what it tracks and how bypasses are excluded.

Exercise: Is blocking every public write after reading Secret data a perfect semantic classifier?

Answer: No. It may reject harmless constant output. It can still support a useful sound policy under appropriate assumptions. Precision and security are separate evaluation axes.

### Primary reading

Fred B. Schneider. “Enforceable Security Policies.” ACM Transactions on Information and System Security 3(1), 30–50, 2000. [Full text](https://www.cs.cornell.edu/fbs/publications/EnfSecPols.pdf).

## 16 Soundness

A checker is sound for a specified security property when every program it accepts satisfies that property under the stated model. The direction matters: acceptance implies security. Soundness alone does not promise that every secure program will be accepted.

Original example: a checker rejects all Public assignments executed under a Secret branch condition. It rejects a program whose two branches both assign the same constant. Under final-value-only observations, that program can be secure. The rejection does not refute soundness; it demonstrates a conservative approximation.

A soundness theorem should name the language, operational semantics, security property, permitted observations, and trusted assumptions. A result about a small deterministic language does not automatically cover Python native extensions, network stacks, scheduling, or reflective evaluation.

Volpano, Irvine and Smith provide a foundational type-system treatment of secure flow analysis. Study the theorem alongside the typing rules rather than treating “typed” as a universal guarantee.

Exercise: A checker accepts one insecure program and rejects ten secure programs. Which properties have failed?

Answer: Soundness has failed because of the accepted insecure program. Completeness has failed because secure programs were rejected.

### Primary reading

Dennis Volpano, Cynthia Irvine and Geoffrey Smith. “A Sound Type System for Secure Flow Analysis.” Journal of Computer Security 4(2–3), 1996. [DOI](https://doi.org/10.3233/JCS-1996-42-304).

## 17 Completeness

For a security checker, completeness means every program satisfying the specified property is accepted. Completeness is relative to that property and language. It is not synonymous with checking many examples or supporting many syntax features.

Original example: under mathematical-integer, final-value semantics, public = secret - secret always assigns zero. A checker that rejects every expression mentioning secret rejects this secure program and is incomplete. In a real language, inspect possible overloaded operators, exceptions, and evaluation effects before importing that conclusion.

A checker that accepts everything is complete in this narrow sense but generally unsound. A checker that rejects everything is sound but generally incomplete. Useful systems seek a practical balance while preserving the guarantee they claim.

For sufficiently expressive general programs, an always-terminating exact classifier is unavailable for semantic noninterference; the undecidability page gives a reduction. That limitation does not prevent precise decisions in restricted sublanguages or proofs about particular programs.

Exercise: Should an engineering team weaken soundness to reduce false alarms?

Answer: That is a policy and product decision, not a theorem. If it does, the resulting tool must no longer claim that every accepted program satisfies the original property.

### Primary reading

Dennis Volpano, Cynthia Irvine and Geoffrey Smith. “A Sound Type System for Secure Flow Analysis.” Journal of Computer Security 4(2–3), 1996. [DOI](https://doi.org/10.3233/JCS-1996-42-304).

## 18 Undecidability

A checker is sound for a stated property when every program it accepts satisfies that property under the model. It is complete when it accepts every program satisfying that property. A checker may be sound and reject safe programs.

Worked example: A syntactic rule rejecting every expression mentioning secret rejects public = secret - secret, even over mathematical integers where the result is always zero. That is a false alarm relative to a final-value model, not evidence that sound enforcement is impossible.

Here is an original halting reduction showing why a total, sound and complete decision procedure for a broad semantic confidentiality property cannot exist for general unbounded programs. Given an arbitrary machine M and input x, construct:

```text
P(secret):
    simulate M on x
    emit secret
```

The simulation has no secret-dependent behavior. If M never halts, P has the same non-emitting divergent behavior for every secret. If M halts, P emits different outputs for different secrets. Thus P satisfies the specified noninterference property exactly when M does not halt. A decider that always answered correctly for every such P would decide halting by negating its answer.

This assumes an effective general-purpose language with unbounded simulation and at least two possible secrets. Physical computers have finite resources; bounded finite-state models can be decidable, though expensive. Testing finitely many executions is not the same as exhaustively verifying a complete finite model.

Exercise: Does undecidability rule out a proof for one broker or a sound type system?

Answer: No. It rules out the universal terminating, exact classifier in the specified general setting. Restricted languages, conservative analyses, interactive proofs, and finite-state verification remain available.

### Primary reading

Alan M. Turing. “On Computable Numbers, with an Application to the Entscheidungsproblem.” Proceedings of the London Mathematical Society, series 2, 42, 230–265, 1936–1937. [Original paper](https://theory.stanford.edu/~trevisan/cs172-07/turing36.pdf).

## 19 Finite state verification

Finite-state verification reasons about a model with finitely many states and transitions. Exhaustive exploration can establish properties of that model, subject to the correctness of the exploration algorithm and the relationship between model and implementation.

Original example: a toy language has one one-bit Secret input, one one-bit Public variable, four control locations, and no unbounded allocation. An explorer can enumerate reachable states and compare observations for both secrets. This is fundamentally different from running a few hand-selected test cases.

A bounded model is useful only when its bounds are stated. Limiting a loop to ten iterations proves nothing about iteration eleven in an implementation that allows it. Replacing an unbounded queue with a two-item queue changes the modeled system. Abstraction techniques can justify broader conclusions, but they require their own soundness argument.

Exercise: Does the undecidability of general noninterference make this finite verification impossible?

Answer: No. Undecidability concerns a universal procedure over the general unbounded class. A finite instance can be decidable yet computationally expensive. The hard engineering task is often validating the abstraction, not merely running the explorer.

### Primary reading

Michael R. Clarkson and Fred B. Schneider. “Hyperproperties.” Journal of Computer Security 18(6), 1157–1210, 2010. [DOI](https://doi.org/10.3233/JCS-2009-0393).

## 20 Declassification

Strict noninterference disallows useful secret-dependent answers, including a password check's success bit. Declassification identifies a permitted release: what information, to whom, under whose authority, and under what conditions.

Robust declassification asks whether attacker-controlled parts can manipulate release beyond the intended policy. Myers, Sabelfeld and Zdancewic provide a type-based enforcement result for a formal language. Their paper's weak indistinguishability is timing- and termination-insensitive, so that theorem does not certify an arbitrary production service against those channels. [Myers, Sabelfeld and Zdancewic 2004](https://www.cs.cornell.edu/andru/papers/csfw04.pdf)

Original example: A service may reveal whether a submitted password matches, but unrestricted repeated queries enable guessing. The release policy needs a query budget and authentication context. “Only one bit per call” does not bound leakage across unlimited calls.

A signing broker intentionally returns key-dependent signatures. That is not strict information-theoretic noninterference of all outputs with respect to the key. Its claim may instead be computational key confidentiality under a specified cryptographic scheme, combined with authorization restrictions on signing.

Exercise: Is permitting the worker to choose any message to sign equivalent to permitting only a particular release transaction?

Answer: No. A narrow transaction interface can bind resource, audience, operation, expiry, and approval. An arbitrary-message signing interface exposes much broader authority.

### Primary reading

Andrew C. Myers, Andrei Sabelfeld and Steve Zdancewic. “Enforcing Robust Declassification.” IEEE Computer Security Foundations Workshop, 2004. [Full text](https://www.cs.cornell.edu/andru/papers/csfw04.pdf).

## 21 Robust declassification

Robust declassification addresses how intentional information release interacts with attacker-controlled code or data. A release mechanism should not let the attacker expand what is revealed beyond the policy's intended authority.

Original example: a trusted service may release a fixed aggregate for an approved cohort. If an untrusted worker can replace the cohort with a single person's record, the same “aggregate” operation can reveal much more than intended. The issue is control of the release, not just the existence of a declassify function.

The formal property is more precise than this example. Myers, Sabelfeld and Zdancewic prove enforcement results for a particular imperative language and type system. Their weak indistinguishability is timing- and termination-insensitive. A production service needs its own argument covering its actual observations, interface, and attacker influence.

Exercise: Is a comment stating “this output is declassified” sufficient?

Answer: No. Identify who authorizes release, what information is permitted, which inputs the attacker controls, and how the implementation enforces the decision. A label change without those constraints can simply name the leak.

### Primary reading

Andrew C. Myers, Andrei Sabelfeld and Steve Zdancewic. “Enforcing Robust Declassification.” IEEE Computer Security Foundations Workshop, 2004. [Full text](https://www.cs.cornell.edu/andru/papers/csfw04.pdf).

## 22 Capability

A capability is an unforgeable reference carrying authority to perform specified operations on a resource. Its value is the authority it grants, not whether it looks like a conventional password.

Capsicum demonstrates capability-oriented compartmentalization using operating-system support, including capability mode and rights-limited file descriptors. This illustrates a concrete alternative to giving every component broad ambient access.

Original example: a worker receives a handle permitting append-only writes to one staging log. It receives no handle for reading production secrets or changing access permissions. Compromise of that worker can misuse the granted append authority, but should not automatically grant unrelated operations if the boundary is enforced.

A bearer token returned by a broker can itself be a capability. Returning a broadly privileged token may therefore undermine a design even if the original private key never leaves the broker.

Exercise: Can a capability system prevent a worker from leaking a secret it legitimately reads to a public handle it legitimately possesses?

Answer: Those two authorities can permit the flow. Capability confinement and information-flow confinement must be distinguished; reducing authority can help, but the desired policy still needs analysis.

### Primary reading

Robert N. M. Watson, Jonathan Anderson, Ben Laurie and Kris Kennaway. “Capsicum: Practical Capabilities for UNIX.” USENIX Security Symposium, 2010. [Full text](https://www.usenix.org/legacy/events/sec10/tech/full_papers/Watson.pdf).

## 23 Reference monitor

A reference monitor mediates security-relevant operations between a subject and protected resources. The practical design question is whether every relevant path passes through the policy check and whether the subject can bypass or alter that enforcement.

Original example: an API wrapper checks permissions before invoking a credentialed operation. If the worker can read the wrapper's environment or contact an alternate endpoint with the same role credentials, the wrapper is not the complete boundary. A correct check on one path does not mediate another path.

Keep the enforcement mechanism small and identify every entry point. Complete mediation, least privilege, and economy of mechanism are classic design principles, but citing them is not a proof that a particular deployment implements them.

Exercise: A broker validates every request it receives. What remains to show?

Answer: That the worker cannot access the protected operation without the broker; that policy inputs and identities are authentic; that validation covers the actual operation; and that responses, errors, and persistent state obey the release policy. Also identify the trusted components on which these claims depend.

### Primary reading

Jerome H. Saltzer and Michael D. Schroeder. “The Protection of Information in Computer Systems.” Proceedings of the IEEE 63(9), 1278–1308, 1975. [Full text](https://web.cs.wpi.edu/~cs557/f14/papers/saltzer1975_alt.html).

## 24 Trusted computing base

The trusted computing base, or TCB, is the collection of components whose correct behavior is required for the security claim. “Trusted” describes a dependency of the argument; it does not mean the component is known to be flawless.

Original example: a worker cannot directly read a broker's key. The claim may nevertheless depend on the kernel's memory isolation, cloud identity configuration, broker implementation, request parser, logging system, administrative interfaces, and any process capable of debugging the broker. If a privileged log collector records the key and makes logs visible to the worker, memory isolation alone is insufficient.

The TCB is claim-specific. A component required for availability may not be required for a narrowly stated confidentiality result, and vice versa. List dependencies for the property actually being asserted.

Exercise: Does moving a key into a second container necessarily shrink the TCB?

Answer: No. Shared identities, writable volumes, task-level privileges, administrative access, and response channels can preserve or introduce dependencies. Draw the complete authority and data graph before evaluating the boundary.

### Primary reading

Andrea Bittau, Petr Marchenko, Mark Handley and Brad Karp. “Wedge: Splitting Applications into Reduced-Privilege Compartments.” USENIX NSDI, 2008. [Full text](https://www.usenix.org/legacy/event/nsdi08/tech/full_papers/bittau/bittau_html/index.html).

## 25 Credential broker

A broker pattern changes the problem:

1. The worker sends a structured request describing a permitted operation.
2. A trusted broker checks identity, resource, operation, payload constraints, budget and any required approval.
3. The broker uses the credential in its own protected environment.
4. It returns only the response allowed by the release policy.

Wedge provides a concrete precedent: it separates sensitive state into compartments and exposes callgates, including an OpenSSH worker without direct access to private keys or user credentials. Its Apache discussion also distinguishes hiding an RSA key from preventing abuse of decryption-related operations. [Bittau and colleagues 2008](https://www.usenix.org/legacy/event/nsdi08/tech/full_papers/bittau/bittau_html/index.html)

Original broker review checklist: Can an error echo authorization headers? Can redirects forward credentials? Can a returned token replace the original key? Can an attacker select a destination embedded inside an otherwise approved request? Can the worker read broker memory, storage, environment, logs, or cloud identity? Can the interface become an oracle across repeated calls?

Exercise: A key never leaves the broker, but the worker can ask it to delete every production record. Is the system secure?

Answer: Key confidentiality may hold while authority control fails catastrophically. State the two claims separately. A protected credential facility must prevent disclosure of useful derivatives and constrain permitted uses; neither follows merely from process separation.

### Primary reading

Andrea Bittau, Petr Marchenko, Mark Handley and Brad Karp. “Wedge: Splitting Applications into Reduced-Privilege Compartments.” USENIX NSDI, 2008. [Full text](https://www.usenix.org/legacy/event/nsdi08/tech/full_papers/bittau/bittau_html/index.html).

## 26 Authority misuse

Authority misuse occurs when a component performs an operation outside the intended authorization, even if no credential bytes are disclosed. Secret confidentiality and authority control are distinct claims.

Original example: an isolated signer never returns its key, but it signs any attacker-selected message. If those signatures authorize production releases, an attacker can exploit the service without extracting the key. Another broker may safely hide a database password while accepting an unrestricted query that deletes every table.

A narrower interface can bind resource, operation, audience, payload schema, expiry, budget, and approval. These checks belong to an authorization policy. Hiding a secret from worker memory does not automatically implement them.

Conversely, a system can correctly reject unauthorized operations yet leak a credential in a debug error. Evaluate both directions rather than using “secure broker” as an undifferentiated label.

Exercise: Is an allowed hostname enough to constrain authority?

Answer: No. A single host may expose many operations and resources, redirects, uploads, or reflected output. Network policy can support an architecture, but semantic authorization and information-flow analysis remain separate requirements.

### Primary reading

Jerome H. Saltzer and Michael D. Schroeder. “The Protection of Information in Computer Systems.” Proceedings of the IEEE 63(9), 1278–1308, 1975. [Full text](https://web.cs.wpi.edu/~cs557/f14/papers/saltzer1975_alt.html).

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


# Full bibliography

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
