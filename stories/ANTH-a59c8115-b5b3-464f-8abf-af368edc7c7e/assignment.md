# Assignment

Write a short Anth.us **post** linking [LensVLM](https://arxiv.org/abs/2605.07019), focused on the surprising mechanics of text-as-image compression and selective expansion.

## Spine

1. Open with the user's “Strange but true” observation, with the paper link.
2. Explain why Figure 7's 5×, 10×, and 15× images become harder to read as visual tokens shrink.
3. Explain the model's escape hatch: scan compressed visual context, expand the relevant image into original text, answer from readable evidence.
4. Close on the engineering tradeoff: fewer prompt tokens and KV-cache bytes, but another inference turn and greater latency.

## Required checks

- The Figure 7 screenshot is the newest Desktop screenshot from 2026-09-24 at 1:12:13 PM, and must be formatted as a 1200×630 cover without clipping the figure.
- Attribute benchmark and latency numbers to the paper; avoid presenting them as general production results.
- Use a recognizable excerpt with no emoji and `posts` as the only tag.
