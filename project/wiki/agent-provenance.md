# Agent provenance and instruction/data separation

Provenance records where data or instructions originated and which authority they should carry. Passing content between agents must not silently promote untrusted data into trusted instructions. An authorized capability can be misused after such a promotion even when no credential bytes are revealed.

CaMeL separates trusted planning from untrusted data and applies capability policies in a custom interpreter. This is a particular enforcement design, rather than a generic credential vault or a drop-in sandbox for arbitrary hostile native code. The authors' implementation explicitly warns that it may contain bugs and may not be fully secure. [Debenedetti et al., Defeating Prompt Injections by Design](https://arxiv.org/abs/2503.18813), [implementation](https://github.com/google-research/camel-prompt-injection).

James Peters-Gill, Avi Semler, Henning Bartsch, Ilia Shumailov and Christian Schroeder de Witt examine hierarchical composition in *Can CaMeLs Talk?* Their multi-CaMeL work identifies problems when downstream untrusted data becomes trusted instructions and proposes provenance-preserving, separate instruction and data channels. Reported benchmark success is empirical evidence, not a universal theorem for every multi-agent deployment. [Can CaMeLs Talk?, version 1](https://arxiv.org/html/2610.05640v1).

Original example: a worker quotes a malicious instruction from a retrieved page in its report. A supervisor interprets the quotation as a new command and invokes its authorized release capability. Memory isolation can still hold while the instruction boundary fails. The remedy must address provenance and authority interpretation, not merely the location of a key.

Exercise: Does a message from another agent become trustworthy because that agent used a protected broker?

Answer: No. Determine the message's source, data dependencies, authorized instruction channel, and recipient policy. A secure credential boundary does not automatically establish trustworthy content or safe composition.

## Related concepts

[Authority misuse](authority-misuse.md) · [Capability](capability.md) · [Security evidence](security-evidence.md)

[Return to the syllabus](information-flow-security.md)

## Primary reading

[Defeating Prompt Injections by Design](https://arxiv.org/abs/2503.18813), 2025. [Can CaMeLs Talk?](https://arxiv.org/html/2610.05640v1), 2026.
