# LensVLM: text as an image

## Pitch

Short Anth.us link post on the strange but concrete idea in [LensVLM](https://arxiv.org/abs/2605.07019): render text into small images so a vision-language model initially consumes fewer tokens, then let it expand the relevant portion to readable source text. The author's Figure 7 makes the fidelity tradeoff visible at 5×, 10×, and 15× rendering presets.

## Audience

Engineers dealing with long context, agent memory budgets, and the accuracy/latency tradeoff in question answering.

## Positioning

- The visual rendering is a compression knob for the initial context, but tiny text becomes unreadable.
- LensVLM treats the image as a map: scan for the relevant part, then recover the original text via an Expand tool.
- The practical question is whether the saved input tokens and KV cache justify the extra inference turn.
Text as an Image, Then a Magnifying Glass

## Constraints

One short post, tags only `posts`. Use the user's newest Desktop screenshot of Figure 7 for a 1200×630 cover. Attribute the screenshot to the paper. Link the paper in the opening paragraph. Keep study findings attributed to the authors, and include the latency tradeoff.
## Working title
