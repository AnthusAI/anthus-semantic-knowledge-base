# Security evidence: tests, proofs, and deployment claims

Security evidence answers a specific question. An adversarial test can exhibit a counterexample. A theorem establishes a property of a model under assumptions. Exhaustive finite-state exploration establishes a property of the modeled state space. Deployment review checks whether actual permissions, identities, networks, and storage satisfy those assumptions. None automatically substitutes for the others.

Original example: a malicious dependency finds no credential in five tested locations. The result covers those executions and locations; it does not prove that the credential never appears anywhere accessible. Use probes as regressions and counterexample searches, with synthetic credentials and controlled services. Account for reflected or encoded responses, redirect chains, substitute credentials, metadata, shared mounts, logs, and snapshot/restore.

Recent research illustrates distinct kinds of evidence:

- Rafid Ahmed, Joseph Fioresi, Mubarak Shah and Yuzhang Shang's *CredLeakBench* evaluates phishing and legitimate authentication/inbox workflows using actual submissions. It is evidence about credential leakage and utility in those workflows, not arbitrary-code confinement or proof of a proxy design. [CredLeakBench](https://arxiv.org/abs/2610.08871).
- Patrick Kenney and colleagues report 16 controlled vault-boundary probes and describe the study as small, purposive, and not certification. Those observations cannot establish absence of secrets across all process state or all executions. [Vault boundary evaluation](https://arxiv.org/abs/2609.33371).
- Nikolaou and colleagues' *CaMeLoT* translates restricted Python plans into finite-state CTL models checked with nuXmv. Conditions are abstracted as nondeterministic choices and tools are black boxes. The result concerns that model, rather than arbitrary Python or tool-provider internals. [CaMeLoT, version 1](https://arxiv.org/html/2609.18674v1).
- Meta's Sentinel description includes a host-side authority, credential surrogation, eBPF cgroup network attribution, and added LSM taint hooks. Its disclosed policy taints a process after it reads user data and removes auto-allow. This is an architecture description, not a demonstrated complete byte-level account of JITs, heaps, garbage collection, or an independently established noninterference proof. [How We Built Safety Into Muse](https://research.meta.ai/blog/security-and-safety-for-ai-agents-our-approach-with-muse).

Record the model, attacker, observations, trusted components, coverage, and limits beside the result. Distinguish an author's reported benchmark from a reproduced experiment and an architecture claim from a proved property.

Exercise: A benchmark reports no leaks. What must a reviewer ask before using it to support a deployment claim?

Answer: Which workloads, attackers, observations, secrets, and authorization failures were tested; which components were trusted; whether the tested system matches the deployment; and which claim the evidence can support. A passing benchmark need not quantify over every future worker program.

## Related concepts

[Finite state verification](finite-state-verification.md) · [Observation model](observation-model.md) · [Agent provenance](agent-provenance.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

The linked papers and provider account are primary evidence for the bounded descriptions above. Their different methods should remain visible in any synthesis.
