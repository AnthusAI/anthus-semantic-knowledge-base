# Report

## Finding

Rendering text as an image can reduce the visual-token budget for a long input. The Figure 7 screenshot makes the price of that reduction obvious: at 15×, the text is no longer reliably legible. LensVLM does not ask the model to read that tiny text perfectly. It uses the compressed image to choose which part to expand, then supplies the original text for the answer.

## Recommended form

A brief link post, about three compact paragraphs after the image: hook, mechanism, engineering tradeoff. Use the user's “Strange but true” framing and link directly to the paper in the first sentence. Name the 4.3× effective compression result only with attribution and note the 17s-versus-8s latency example.

## Risks

- Do not confuse visual-token compression with guaranteed end-to-end cost savings.
- Do not imply the model reconstructs text from pixels after detail is lost; Expand reads retained source text.
- The screenshot's full aspect ratio is wider than 1200×630; preserve the complete figure and caption via letterboxing.
