# Building an agent harness whose boundaries survive smarter agents

## NotebookLM brief

Create one coherent, technically substantial video explainer from the outline below. The audience is an experienced software or ML engineer who understands processes, APIs, credentials, and ordinary application security but is learning formal information-flow security. Teach the argument itself. Define unfamiliar terms at the moment they become useful, connect each chapter to the next, and use the running examples to make the abstractions concrete. The material below is the explanatory content, not merely a list of production instructions.

The central question is: how do we build an execution harness intended to last five or six years when the agents inside it may become far more capable than the people who designed it? The answer should move from trying to anticipate every clever attack toward specifying and enforcing boundaries that quantify over arbitrary permitted programs. Explain the limitations of that answer just as carefully as its strengths.

Use a calm, precise teaching voice. Include the important qualifications without burying the main argument. Do not add citations, researcher names, paper titles, publication dates, bibliographies, unsupported product claims, or an implementation tutorial. Diagrams and small pseudocode examples can support the explanation. The conclusion should give viewers a practical way to reason about designs, not promise an absolutely leak-free production system.

## 1. Start with the future-agent problem

### 1.1 A harness can outlive the models it was designed around

Imagine a coding agent that can write Python, invoke command-line tools, install dependencies, inspect a workspace, call services, and produce downloadable artifacts. Today, its designers may be able to anticipate its favorite ways to misuse these powers. Over the lifetime of the harness, that may change dramatically.

The agent might eventually discover attack paths the designers never considered. It might combine individually harmless operations into a harmful chain. A defense based on recognizing suspicious intent, maintaining forbidden-command lists, or making the agent promise good behavior has to keep winning an open-ended contest.

The more durable question is different: what can every program admitted by this harness do, regardless of who wrote it or how clever its strategy is?

### 1.2 Distinguish structural guarantees from predictions

A correctly enforced memory boundary does not depend on guessing what an adversary will try. Its claim is about which memory operations are possible under specified assumptions. Likewise, a narrowly authorized service can reject requests outside its policy without understanding the requesting agent's intentions.

This does not make the design timeless or infallible. A smarter agent may find implementation defects, unmodeled outputs, overly broad permissions, or a false assumption. The aim is a clear guarantee with explicit dependencies, so that stronger attackers challenge the boundary rather than the designer's imagination.

Transition: before choosing a sandbox or proxy, identify one security promise precisely enough that we can reason about it.

## 2. Turn “keep the credential safe” into explicit claims

### 2.1 Define the threat model

Use a running example: an agent prepares a release, while a protected credential can authorize publishing that release. The worker controls all its own code and tool arguments. It may be malicious even if the model itself is ordinarily helpful, because repository contents or retrieved material can influence its behavior.

Name the protected asset, attacker control, attacker observations, permitted operations, and trusted components. Does the worker see stdout, errors, files, snapshots, network responses, completion status, or timing? Can it change dependencies or launch child processes? Which kernel, broker, cloud configuration, and administrative components are assumed correct?

A threat model is not a claim that everything outside scope is safe. It describes where the argument stops.

### 2.2 Separate three meanings of protection

First, the worker must not learn the credential's literal bytes. Second, it may be required not to learn unauthorized information about the credential, including individual bits or reversible encodings. Third, it must not obtain or misuse the authority that the credential represents.

These are different properties. A worker might learn one bit without being able to authenticate. It might obtain a replacement access token without recovering the original key. Or it might ask a protected service to perform a destructive operation without obtaining any credential at all.

Use two headings throughout the video: confidentiality, meaning what the attacker can learn; and authorization, meaning which actions the attacker can cause. Success under one heading does not establish success under the other.

### 2.3 Access control does not automatically control propagation

An access check answers whether a principal may read a file or invoke an API. Suppose the worker is allowed to read a credential and allowed to write a report. Each operation can pass its access check while their combination copies the credential into the report.

Read-only access to the original file does not prevent disclosure. Disabling internet access does not prevent a returned artifact from becoming the output channel. This is why information-flow security asks where information can go after it has been read.

## 3. The short impossibility argument

### 3.1 Three requirements cannot all hold

Suppose a system promises:

1. The worker may execute arbitrary code that reads a secret K.
2. The worker may choose arbitrary bytes on an attacker-visible output.
3. No permitted worker can disclose K.

Consider the program “read K, then write K to that output.” The first two requirements admit this program. Its behavior contradicts the third requirement. That is the whole argument.

This is not an argument about how intelligent the agent is. It does not need undecidability, cryptography, or a sophisticated exploit. It is a contradiction between permissions and the requested guarantee.

### 3.2 Every successful defense changes an assumption

A design can deny secret reads, constrain outputs, restrict the permitted programming language, enforce information-flow rules, or trust the code that handles the secret. A system that blocks all outputs also changes the assumptions, though it may be useless for the intended task.

The question is not whether any useful secure system can exist. It is which combinations of usefulness and protection are achievable once the permissions are stated honestly.

Visual idea: draw secret storage, worker, and public output as three boxes. Highlight the two permitted arrows. Then erase or constrain one arrow for each architectural alternative.

Transition: perhaps an output filter could preserve both arrows while recognizing leaks. To evaluate that idea, we need to understand what information flow includes.

## 4. A leak is a dependency, not a recognizable string

### 4.1 Explicit flows survive changes of representation

Printing K is an explicit flow because the output value directly depends on K. Base64-encoding it, XORing it with a known constant, splitting it across files, or hiding bytes in an image changes its representation while preserving recoverable information.

A literal-string filter can remove the literal string and still miss the same secret encoded differently. This failure already follows from simple counterexamples; it does not require an undecidability theorem.

A hash needs separate reasoning. It is not generally reversible, but it can reveal equality or permit guessing when the secret comes from a small candidate set. Even a length field can disclose information if length was meant to remain private.

### 4.2 Implicit flows travel through control decisions

Consider a public variable initially set to zero. If the secret's low bit is one, the program assigns one to the public variable. The assignment contains only a public constant, yet the result reveals the secret bit.

This is a control dependency, also called an implicit flow. Tracking only values copied from secret memory misses it. One conservative enforcement technique labels the current control context as Secret while executing a secret-dependent branch and forbids public writes in that context.

But dependency tracking is a mechanism, not the definition of leakage. If both branches assign seven, their final values may be identical. A conservative checker may still reject the program. Whether this is a false alarm also depends on what else the observer sees.

### 4.3 Termination and timing are observations too

Suppose both branches return the same text, but one waits much longer. A timing observer can distinguish them. Suppose one branch completes and the other loops forever. A caller observing completion or a timeout can learn something.

Do not dismiss these channels because they may be noisy or inconvenient. Noise changes an attack's reliability or bandwidth, not automatically its existence. Conversely, an inaccessible resource is not automatically an attacker-visible channel. Specify the observer and explain why a distinction is available to it.

A timing-insensitive or termination-insensitive result can be useful, but its name describes a deliberate limitation. It cannot be restated as a guarantee against latency or timeout extraction.

## 5. Noninterference: compare two possible worlds

### 5.1 The basic definition

Hold public inputs fixed. Run the same program with secret H1 and then with secret H2. Extract exactly the observations available to the attacker. A simple deterministic noninterference property requires those observations to be equal for every allowed pair of secrets and every allowed public input.

The intuition is that changing only the secret must not change the public view. A program that always emits “done” satisfies this simple condition if it has no other observable differences. A program that emits the secret's parity does not: choose two secrets with different parity and the public views differ.

This is a relationship between executions. Seeing one execution without an obvious secret string does not establish it.

### 5.2 The observation model determines the theorem

Two runs may match under an observer that sees only a final integer, yet differ under an observer that also sees filenames, output ordering, response sizes, persistent state, or elapsed time. State which observer the claim concerns before judging the program.

For randomized systems, equal outputs in two sampled runs are not the right universal requirement. Randomness can make harmless runs differ. One must compare appropriately defined distributions or another explicit probabilistic relation. Cryptographic security may instead bound what feasible attackers can distinguish under stated assumptions.

### 5.3 “Proven” always has a scope

A proof for a small mathematical language does not automatically cover Python native extensions, runtime bugs, shared caches, or a cloud control plane. A proof can be correct while the deployed system fails to implement its assumptions.

Model-relative security is not an embarrassing escape clause. It is the discipline of stating what was established and what connects it to reality. Strong engineering adds implementation and configuration evidence instead of silently expanding the theorem.

## 6. What enforcement can and cannot decide

### 6.1 Labels and conservative enforcement

Introduce Public and Secret labels. Public information can enter Secret storage. A Secret-to-Public transfer needs explicit permission. Combining inputs ordinarily produces an output label that retains their restrictions.

Policies can be richer than a single hierarchy. Customer A's private information and customer B's private information need not be interchangeable. A combined result must preserve both restrictions unless an authorized release rule says otherwise.

A static type system, restricted interpreter, runtime monitor, or operating-system mechanism may enforce such rules. Each needs an argument covering the operations and observations it actually models.

### 6.2 Soundness and completeness mean different things

A checker is sound when every accepted program satisfies the specified property. It is complete when every program satisfying that property is accepted.

A checker that rejects everything can be sound but impractical. A checker that accepts everything can be complete in this narrow sense but insecure. Under mathematical-integer, final-value semantics, “public equals secret minus secret” always produces zero. A checker rejecting every mention of secret rejects this secure program and is incomplete.

Real language behavior matters: overloaded operators, exceptions, or evaluation side effects can invalidate that toy example's assumptions. Preserve the model when explaining the example.

### 6.3 The careful undecidability argument

Now ask for a checker that always terminates and classifies every general unbounded program exactly. Given any machine M and input x, construct a program that first simulates M on x and, only if that simulation finishes, emits its secret input.

The simulation itself does not depend on the secret. If M never halts, every secret produces the same non-emitting divergent behavior. If M halts, different secrets produce different emitted values. For this specified trace-based noninterference property, the constructed program is secure exactly when M does not halt.

An always-correct, always-terminating security decider could therefore decide whether arbitrary computations halt. That universal exact decider cannot exist in this general setting.

The result does not mean every analysis is useless, every particular program is unprovable, or every bounded system is undecidable. Restricted languages, conservative sound analyses, proofs for specific programs, and exhaustive finite-state verification remain possible. Physical finiteness also does not make exhaustive analysis practically cheap.

Transition: instead of classifying arbitrary secret-handling worker code, change the architecture so the worker never receives the credential.

## 7. The broker pattern changes the problem

### 7.1 Move the secret behind a narrow operation

The worker requests an operation. A trusted broker authenticates the caller, checks policy, uses the credential inside its protected environment, and returns an allowed result. The worker continues running arbitrary code, but that code no longer reads the protected key directly.

For the release example, expose “publish this approved manifest to this release destination” rather than “sign any byte string” or “send any authenticated HTTP request.” The broker can bind identity, resource, operation, payload, audience, expiration, budget, and required approval.

A schema makes inputs well formed; it does not by itself make an operation authorized. A perfectly valid request can still request the wrong resource or a dangerous action.

### 7.2 Hiding the key does not hide the power

An isolated signer can keep its key confidential while signing an attacker-selected release. A database broker can hide its password while executing a destructive query. These are authority failures even if the protected bytes never leave trusted memory.

Returning a replacement bearer token may also transfer the power the design was supposed to contain. Evaluate the original secret, useful derivatives, delegated credentials, and remote operations together.

A capability is an unforgeable reference granting specified authority. Narrow capabilities reduce ambient power, but a capability to read secrets plus a capability to publish arbitrary output can still permit disclosure. Capability design and information-flow reasoning complement one another.

### 7.3 Useful results require controlled release

Strict information-theoretic noninterference is often too strong for useful services. A password check intentionally releases a match result. A signer intentionally returns a signature whose value depends on its private key.

Declassification names the permitted release: what information, to whom, under whose authority, and under which conditions. Robust release also constrains how attacker-controlled inputs can expand that disclosure. An “aggregate” query that lets the attacker choose a one-person cohort is not protected merely because its output is called an aggregate.

For signing, the appropriate confidentiality claim is typically computational key protection under the scheme's assumptions, alongside a separate signing-authorization policy. Do not claim that signatures are mathematically independent of the key. Repeated queries and cumulative observations belong in the argument.

## 8. Inspect every boundary around the broker

### 8.1 Complete mediation and the trusted computing base

Every protected operation must pass through checks the worker cannot bypass or modify. This is complete mediation. A beautifully implemented broker is insufficient if the worker can obtain equivalent cloud credentials from another endpoint.

The trusted computing base is everything whose correct behavior the claim requires: perhaps the kernel, runtime, broker, parser, cloud identity configuration, log collector, and administrative interfaces. “Trusted” means the argument depends on it, not that it has been proved flawless.

Draw data paths and authority paths separately. Include memory, environment variables, files, inherited handles, debug interfaces, errors, logs, snapshots, and response channels. Persistence can turn yesterday's supposedly internal file into tomorrow's public artifact.

### 8.2 A proxy is an architecture to analyze, not a magic filter

A credential-injecting proxy can keep credentials out of the worker's original request. But the request destination, TLS identity, redirect behavior, response body, and logging paths all matter. A remote service might reflect an injected credential in an error. An allowed host can expose many operations with radically different authority.

A strict response schema can narrow a channel, but an allowed string can still carry encoded information. Length limits can bound some output capacity without making it zero. Entropy heuristics are detectors, not proofs of secrecy.

A semantic broker often makes the intended operation easier to specify than a transparent general-purpose proxy. That is a design advantage, not automatic evidence that its implementation is correct. Compatibility and security claims must be examined separately.

### 8.3 Deployment facts must be verified

A second container is not automatically an independent identity or an adequate trust boundary. Shared networking, task-scoped credentials, volumes, process visibility, debugging permissions, and administrative access can reconnect the compartments.

For a managed container platform, verify the actual service API and configuration semantics. Do not assume a task-scoped role can be assigned independently to sibling containers. Do not confuse an orchestration system's process-namespace field with another platform's similarly named feature. Shared process visibility alone also does not prove arbitrary memory access; permissions and tracing controls matter.

Likewise, do not assume the platform permits privileged packet interception, custom kernel instrumentation, or nested microVM construction. These are deployment questions requiring evidence. The conceptual recommendation is to establish a verified isolation boundary, not to prescribe an unverified sidecar recipe.

## 9. Credentials have different replay and delegation properties

### 9.1 Bearer secrets and renewable sessions

A bearer API token is usable by whoever possesses it, subject to scope and server policy. A short-lived access token reduces the duration of misuse but does not prevent misuse during its lifetime. Refresh tokens can restore longer-term access and may undermine a design that protects only short-lived tokens.

Passwords and session cookies belong in the authority inventory too. Protecting a password while handing the worker an equally powerful session can preserve the wrong property.

### 9.2 Private keys and proof of possession

A protected signing key can avoid export while still exposing a signing oracle. Bind the permitted signatures to meaningful operations. Proof-of-possession mechanisms make a token alone insufficient for replay by requiring another key or proof, but their value depends on where that key lives and who can invoke it.

If the worker can use the proof key freely, it may exercise the authority without stealing it. Certificate-bound credentials similarly require correct server enforcement and protection of the corresponding private key. Avoid describing any token category as inherently safe.

### 9.3 Downscoping limits damage when enforced

Exchanging broad authority for a narrower, shorter-lived credential can reduce blast radius when the provider actually enforces audience, resource, action, lifetime, and delegation limits. The exchange mechanism does not guarantee meaningful narrowing by itself.

Ask the same question for every credential: what can possession or permitted use of this object cause? This keeps the focus on authority rather than credential branding.

## 10. Combine theory, deployment review, and adversarial evidence

### 10.1 Tests look for counterexamples

Probe direct and encoded disclosure, secret-dependent branches, timeout behavior, reflected errors, redirected requests, token-minting operations, persistence, and alternative identity paths. Test cumulative queries as well as one request. Check unauthorized operations separately from credential extraction.

Use synthetic credentials and controlled services so the tests themselves do not expose production secrets. Inspect what each attacker can actually observe. A failed extraction attempt establishes that the attempt failed; it does not prove the secret never existed in worker memory.

### 10.2 Different evidence answers different questions

A test can demonstrate a bug. A proof can establish a stated property of a model. Exhaustive model exploration can establish a property within justified finite bounds. Deployment review can check whether real identities, permissions, networks, and storage satisfy the model's assumptions.

None automatically substitutes for the others. A clean red-team exercise is valuable evidence, but it does not quantify over every future program. A correct theorem is valuable evidence, but it does not verify an unrelated cloud configuration.

## 11. Close with a design exercise and the durable lesson

### 11.1 Specify one useful operation

Ask viewers to design the release broker from the running example. Write the request, authorization predicate, allowed response, query budget, persistence policy, observation model, and trusted dependencies. Then ask how an adversarial worker could manipulate each input or observe each output.

Can it change the destination? Obtain a substitute credential? Turn errors into a channel? Reuse approval for another manifest? Reach the protected operation by another path? Cause a destructive action while preserving key secrecy?

The exercise is complete only when the team can say what is guaranteed, under which assumptions, and what evidence supports each part.

### 11.2 The final takeaway

A long-lived harness should not depend on staying more cunning than every agent it will host. Begin with definitions, remove contradictory permissions, minimize trusted secret-handling code, constrain authority at semantic boundaries, and verify the implementation against the stated model.

Stronger agents remain dangerous: they can discover bugs and challenge assumptions. The structural advantage is that the intended guarantee no longer rests on predicting their next trick. It rests on a boundary whose meaning is explicit enough to test, review, and, where feasible, prove.
