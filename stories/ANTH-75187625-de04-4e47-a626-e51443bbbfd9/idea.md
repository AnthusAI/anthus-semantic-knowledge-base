# Idea

## Pitch

A new class of models is arriving that answers bounded questions with a decision and a confidence instead of prose. Jev is the first one we've worked with in depth. They are cheap, fast, and frozen: the vendor serves one set of weights to everyone, so the usual move of fine-tuning to your data is off the table. Everything that makes such a model fit your business has to live in the harness around it: the questions you ask, the calibration of its confidence, the fitted head on top, the gate that routes uncertain or biased slices to a human, the retraining of whatever you distil out of it, and the record of what changed and whether it got better.

Plexus is that harness. The Jev series on the site already proved each stage separately, on labelled data with checkable numbers. This piece is the one that shows them as one loop running in one system, and argues that as decision models spread, the lab that aligns them is the product, not the model.

Sequenced after the general classifier-lab piece (ANTH-f7e800). That one is the offer for any classification task; this one is the why-now for teams looking at decision models specifically. Write from studies already run and what Plexus does today, not the roadmap.

## Audience

Engineering and product leads evaluating decision models for high-volume judgment tasks, who have realised they cannot fine-tune what they're buying. Secondary: readers of the Jev series who want to know what to do with the results.

## Working title

Plexus as the lab for the new decision models
