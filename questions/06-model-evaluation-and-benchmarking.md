# 6. Model Evaluation & Benchmarking

Rigorous validation is the single most important differentiator between a scientifically credible AI drug discovery program and one producing misleading results.

---

### Q: Why is random train/test splitting often misleading for evaluating drug discovery models, and what alternative splitting strategies would you use instead? 🔴

**Why it's asked:** One of the most important, frequently-tested concepts in this field — naive ML evaluation practices can produce dangerously overoptimistic results.

**Answer:**
Random splitting can place near-duplicate or highly similar molecules in both the training and test sets (since real compound datasets are often clustered around a few chemical series, not uniformly distributed across chemical space), which lets a model succeed on the test set largely by memorizing similar structures rather than genuinely generalizing — this inflates apparent performance in a way that doesn't reflect real-world prospective performance, where the model needs to predict properties for genuinely novel chemical matter.

Alternative strategies:
- **Scaffold split:** grouping molecules by their core structural scaffold and ensuring scaffolds in the test set don't appear in training — better simulates generalizing to structurally novel series.
- **Temporal split:** training on data available up to a certain date and testing on data generated afterward, which best mimics the real prospective use case (predicting on molecules that didn't exist yet when the model was built) and can reveal how the model handles genuine distribution shift over a project's timeline.
- **Cluster-based splitting:** clustering the full dataset by structural similarity and assigning entire clusters to either train or test, similar in spirit to scaffold splitting but based on a different similarity metric.

A rigorous evaluation typically reports performance under multiple splitting strategies, since each reveals a different aspect of generalization, and a model that only looks good under random splitting should be treated with real suspicion.

**Follow-ups:**
- If a model performs well under random split but much worse under scaffold split, what does that tell you, and would you still consider deploying it?

---

### Q: What is "applicability domain," and why is it important to define for a deployed drug discovery model? 🟡

**Answer:**
Applicability domain refers to the region of chemical (or, more broadly, feature) space within which a model's predictions can be trusted to have the accuracy observed during validation — outside this domain, a model is extrapolating into a region it wasn't meaningfully trained or validated on, and its predictions should be treated with much lower confidence, even if the model doesn't explicitly signal this.

This matters because ML models will produce a confident-looking numerical output for essentially any input, regardless of whether that input resembles anything in the training data — silently extrapolating without any built-in warning. Defining and communicating an applicability domain (e.g., via similarity to training data, prediction uncertainty estimates, or explicit domain-boundary methods) is essential so that downstream users (chemists, decision-makers) know when to trust a prediction and when to treat it as a rough, low-confidence guess rather than a validated estimate.

**Follow-ups:**
- How would you practically estimate whether a new candidate molecule falls inside or outside your model's applicability domain?

---

### Q: How do you interpret an ROC-AUC or PR-AUC score in the context of a drug discovery classification task, and what are the risks of over-relying on a single aggregate metric? 🟡

**Answer:**
ROC-AUC measures a classifier's ability to rank actives above inactives across all possible thresholds, and PR-AUC does something similar but is generally more informative for highly imbalanced datasets (common in screening data) since it's more sensitive to performance specifically on the minority (active) class.

Risks of over-relying on a single aggregate metric:
- **It hides where the model fails** — a model can have a solid aggregate AUC while still failing badly on the scientifically important cases (e.g., activity cliffs, novel chemical series, borderline potency compounds near a decision threshold that actually matters for triage decisions).
- **It doesn't reflect real-world decision costs** — the actual operational question is usually "if I follow up on my top N predicted compounds, what fraction will be true hits" (enrichment/precision at a specific cutoff), which AUC-style metrics summarize across all thresholds rather than the one that's actually operationally relevant.
- **It can be inflated by the evaluation methodology itself** (see the random vs. scaffold split discussion above) — a high AUC under a lenient/overoptimistic split doesn't mean the model will perform similarly well prospectively.

I'd always pair an aggregate metric with segment-level analysis (performance by chemical series/cluster, performance near the actual decision threshold used in practice) and, where possible, retrospective enrichment analysis matching the actual intended use case.

**Follow-ups:**
- How would you choose between ROC-AUC and PR-AUC for a specific virtual screening evaluation, and why?

---

### Q: What's the difference between retrospective and prospective validation, and why is prospective validation considered the gold standard despite being slower and more expensive? 🔴

**Answer:**
**Retrospective validation** evaluates a model against existing historical data — fast and cheap, but subject to all the biases discussed above (dataset selection bias, potential train/test leakage even with careful splitting, the possibility that historical data itself reflects idiosyncrasies of past experimental campaigns that won't repeat going forward).

**Prospective validation** evaluates the model's real, out-of-sample predictions against experiments run specifically to test it — new compounds are actually designed/selected based on the model's predictions, synthesized, and tested, and the true experimental outcome is compared against what the model predicted, with no possibility of the model having "seen" this exact outcome in any form beforehand.

Prospective validation is the gold standard because it's the only validation method that fully mirrors the real, intended use case of the model (predicting on genuinely novel molecules before they're made) and is immune to the subtle historical dataset biases that can make retrospective validation look better than reality warrants. Its cost and slowness is exactly why retrospective validation (careful, bias-aware retrospective validation) remains valuable as a faster, cheaper filter to decide which models are even worth the investment of prospective testing.

**Follow-ups:**
- How would you design a prospective validation study with a limited experimental budget to maximize what you learn about the model's real-world reliability?

---

### Q: How would you evaluate the quality of molecules produced by a generative model, beyond just checking whether the property predictor scores them highly? 🔴

**Answer:**
- **Validity:** what fraction of generated outputs are even chemically valid molecules (correct valence, sensible structure)? A model producing a high rate of invalid structures has a basic problem before property quality is even relevant.
- **Novelty:** are generated molecules meaningfully different from the training set, or is the model largely memorizing/interpolating trivially close analogs of known compounds?
- **Diversity:** does the generated set cover a reasonably diverse region of chemical space, or does it collapse to a narrow set of similar structures (mode collapse)?
- **Synthesizability:** can generated molecules realistically be synthesized? Automated synthesizability scores (e.g., retrosynthesis-based feasibility estimates) are a useful proxy, but expert medicinal chemist review remains important since these scores are themselves imperfect.
- **Property predictor reliability itself:** since generative optimization typically targets a learned property predictor as its objective, a generative model can effectively learn to exploit weaknesses/blind spots in that predictor (producing molecules that score artificially well according to the predictor without genuinely having the desired real-world property) — this is a real risk analogous to adversarial or reward-hacking behavior, and is a reason a generative pipeline's final outputs need independent validation, not just a high score from the same predictor used to guide generation.

**Follow-ups:**
- How would you detect whether your generative model is "gaming" its property predictor rather than producing genuinely good molecules?
