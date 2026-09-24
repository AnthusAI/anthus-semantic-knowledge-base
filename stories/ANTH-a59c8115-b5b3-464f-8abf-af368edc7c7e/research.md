# Research

Primary source: [Xie et al., “LensVLM: Selective Context Expansion for Compressed Visual Representation of Text”](https://arxiv.org/abs/2605.07019), submitted 2026-05-07. [HTML version](https://arxiv.org/html/2605.07019v1) was checked on 2026-09-24.

## Verified claims

- The paper says, “A growing line of work compresses text by rendering it as images for VLMs” (§2). The screenshot is Figure 7, which compares 5×, 10×, and 15× rendering presets. Its caption says the text becomes progressively illegible and the visual-token count decreases.
- LensVLM renders source text to images, scans them, and uses an Expand tool to retrieve selected regions as original text or high-resolution images (§3). The uncompressed source remains available for the tool. It is not recovered from unreadable pixels.
- The authors report accuracy comparable to a full-text upper bound at 4.3× *effective* compression and beating several baselines through 10.1× on seven text QA benchmarks (abstract and §4). Effective compression counts the initial visual tokens and the tokens returned through Expand.
- Their batch-size-1 latency profile is approximately 17 seconds with one Expand call versus 8 seconds for the text baseline (§10). This is a measured research setup, not a deployment guarantee.
- The paper is marked CC BY-NC-ND 4.0. Attribute Figure 7 in the post and preserve the screenshot figure when fitting it into the social image.

## Image

User-supplied Desktop screenshot: `Screenshot 2026-09-24 at 1.12.13 PM.png`, 2680×1040. It contains the whole Figure 7 composite and caption. Create the required 1200×630 cover by scaling the whole screenshot proportionally and padding the margins.
