# Finite state verification

Finite-state verification reasons about a model with finitely many states and transitions. Exhaustive exploration can establish properties of that model, subject to the correctness of the exploration algorithm and the relationship between model and implementation.

Original example: a toy language has one one-bit Secret input, one one-bit Public variable, four control locations, and no unbounded allocation. An explorer can enumerate reachable states and compare observations for both secrets. This is fundamentally different from running a few hand-selected test cases.

A bounded model is useful only when its bounds are stated. Limiting a loop to ten iterations proves nothing about iteration eleven in an implementation that allows it. Replacing an unbounded queue with a two-item queue changes the modeled system. Abstraction techniques can justify broader conclusions, but they require their own soundness argument.

Exercise: Does the undecidability of general noninterference make this finite verification impossible?

Answer: No. Undecidability concerns a universal procedure over the general unbounded class. A finite instance can be decidable yet computationally expensive. The hard engineering task is often validating the abstraction, not merely running the explorer.

## Related concepts

[Undecidability](undecidability.md) · [Hyperproperty](hyperproperty.md) · [Soundness](soundness.md) · [Security evidence](security-evidence.md)

## A recent agent-plan example

CaMeLoT translates restricted Python plans into finite-state CTL models checked with nuXmv. Its abstraction treats conditions as nondeterministic choices and tools as black boxes. This illustrates verification relative to an explicit model; it does not verify arbitrary Python or the internals of a tool provider. [Nikolaou et al., CaMeLoT, version 1](https://arxiv.org/html/2609.18674v1).

[Return to the syllabus](information-flow-security.md)

## Primary reading

Michael R. Clarkson and Fred B. Schneider. “Hyperproperties.” Journal of Computer Security 18(6), 1157–1210, 2010. [DOI](https://doi.org/10.3233/JCS-2009-0393).
