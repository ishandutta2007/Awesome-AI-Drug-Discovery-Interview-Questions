# 5. Data, Validation & Experimental Design

Data quality and the wet-lab/dry-lab feedback loop are where most real-world drug discovery ML projects succeed or fail.

---

### Q: What are common sources of noise or bias in experimental assay data, and how should this affect how you build and trust models trained on it? 🟡

**Answer:**
Common sources of noise/bias:
- **Assay variability:** the same compound tested in the same assay on different days/batches can give somewhat different readouts due to experimental noise — meaning the "ground truth" label itself has inherent uncertainty, not just the model's predictions.
- **Batch/plate effects:** systematic variation introduced by which experimental batch or plate a compound was tested in, which can be mistaken for a real structure-activity signal if not properly controlled for.
- **Assay-specific artifacts:** compounds can show apparent activity due to assay interference (e.g., fluorescence interference, aggregation-based false positives) rather than genuine biological activity against the intended target.
- **Selection bias in historical datasets:** compounds that were tested were themselves selected based on prior beliefs (chemist intuition, earlier model predictions), meaning historical datasets are rarely a random or representative sample of chemical space — this affects both training data and how you interpret a model's apparent performance.

Implication: I'd treat single-measurement labels with appropriate skepticism, prioritize datasets with replicate measurements or documented assay quality control, and be cautious about a model's apparent precision when the underlying label noise itself is significant — a model claiming to distinguish a 2x potency difference is not meaningful if the assay's own reproducibility is worse than that.

**Follow-ups:**
- How would you estimate the noise floor of an assay, and why does that matter for interpreting your model's performance metrics?

---

### Q: How do you decide what to prioritize for synthesis when your model produces a ranked list of candidate molecules, given limited synthesis capacity? 🟡

**Answer:**
- **Don't just take the top-N by predicted score** — factor in prediction uncertainty (a molecule with a high point-estimate score but very high uncertainty may be a worse bet than a slightly lower-scoring molecule with tighter, more reliable uncertainty).
- **Balance exploitation and exploration:** include some diversity/exploration picks (structurally distinct from what's already been tested) even if they don't top the predicted-score ranking, since pure exploitation can get stuck in a local optimum and miss better regions of chemical space (this connects directly to active learning strategies in section 3).
- **Factor in synthesizability and cost**, not just predicted biological properties — a top-ranked molecule that's extremely difficult or slow to synthesize may not be worth prioritizing over a slightly lower-ranked but readily synthesizable one, especially under time pressure.
- **Involve the medicinal chemistry team in the final decision**, since they often have tacit knowledge (synthetic feasibility, red flags from experience) not captured in the model's inputs — treat the model's ranking as a strong input to a human decision, not an automatic, unreviewed synthesis order.

**Follow-ups:**
- How would you quantify and communicate prediction uncertainty to a chemist who's used to single-number potency predictions?

---

### Q: What is the "cliff" problem (activity cliffs) in structure-activity relationships, and how does it affect model training and evaluation? 🟡

**Answer:**
An activity cliff occurs when two structurally very similar molecules (by standard similarity metrics like Tanimoto similarity) have dramatically different biological activity — violating the general assumption underlying most QSAR-style models that structurally similar molecules should have similar activity (the "similarity property principle").

This affects modeling because: models that rely heavily on smooth structure-based similarity (e.g., nearest-neighbor-style predictions, or models implicitly assuming smoothness) tend to perform poorly exactly at these cliffs, which are often the most scientifically and practically important regions (since they can reveal precisely which small structural change drives a big potency change — valuable medicinal chemistry insight). It also affects evaluation: standard aggregate accuracy metrics can look good overall while systematically failing on activity cliff examples, so a rigorous evaluation should specifically check performance on known or suspected cliff pairs, not just average performance across a dataset.

**Follow-ups:**
- How would you specifically test whether your model captures (or fails to capture) known activity cliffs in your data?

---

### Q: How would you design the feedback loop between your ML predictions and the experimental (wet-lab) team to maximize the model's real-world value? 🔴

**Answer:**
- **Make the loop fast and tight** — the value of active learning and iterative model improvement depends on getting real experimental results back into the model promptly, so I'd advocate for prioritizing turnaround time on the assay/synthesis side for actively-modeled programs where feasible, not just optimizing the model itself.
- **Track prediction-vs-actual systematically** for every synthesized/tested compound, not just in aggregate — building a living record that lets you retrain, recalibrate, and diagnose specific failure patterns over time.
- **Build trust incrementally** — chemists and biologists are (rightly) skeptical of a new model until it's demonstrated real prospective value; I'd start with a modest, well-scoped pilot where the model's suggestions are one input among several (not a replacement for chemist judgment), and let a track record of genuinely good prospective picks build credibility over time.
- **Make model reasoning as transparent as possible** — e.g., highlighting which substructural features are driving a prediction, or flagging when a prediction is in a low-confidence/out-of-domain region — since chemists are more likely to act on and trust suggestions they can sanity-check against their own expertise.
- **Regularly revisit and retrain**, since a static model trained once at project kickoff will become progressively less relevant as the DMTA cycle explores new regions of chemical space that a stale model wasn't trained on.

**Follow-ups:**
- A medicinal chemist on your project is skeptical of your model's suggestions and keeps deprioritizing them in favor of their own intuition. How do you handle this?

---

### Q: How do you handle highly imbalanced datasets in drug discovery (e.g., a screening campaign where only 0.1% of tested compounds are active)? 🟡

**Answer:**
- **Use appropriate metrics** — plain accuracy is nearly meaningless on a severely imbalanced dataset (a model predicting "inactive" for everything would score ~99.9% accuracy); use metrics like precision-recall AUC, enrichment factor, or hit rate in the top-ranked fraction, which are more relevant to the actual downstream use case (finding a small number of true actives worth following up).
- **Consider resampling or class-weighting techniques** (oversampling actives, undersampling inactives, weighted loss functions) during training, while being cautious about overfitting to a small number of duplicated/upsampled active examples.
- **Think about what the negative class actually represents** — "inactive" in a screening dataset often just means "not confirmed active in this specific assay under these conditions," not a definitive biological negative, which has implications for how confidently a model should treat these as true negatives during training.
- **Frame the practical goal correctly:** in most real virtual screening use cases, the actual goal is efficient enrichment (finding a disproportionate share of true actives within a manageable follow-up budget), not perfectly calibrated probability estimates across the whole dataset — evaluation should reflect that framing.

**Follow-ups:**
- How would you choose and justify a specific "top-N to follow up experimentally" cutoff, given a fixed experimental budget?
