# 1. Drug Discovery Fundamentals

Core vocabulary and process knowledge every AI drug discovery scientist needs, regardless of which side (ML or life sciences) they came from.

---

### Q: Walk me through the drug discovery pipeline from target identification to clinical candidate, and where AI/ML tools typically get applied at each stage. 🟢

**Answer:**
A simplified pipeline:
1. **Target identification/validation:** identifying a biological target (usually a protein) whose modulation is believed to affect disease. ML applications: mining omics data, literature, and genetic association data to prioritize targets.
2. **Hit identification:** finding initial molecules that show some activity against the target, via high-throughput screening (HTS), virtual screening, or fragment-based approaches. ML applications: virtual screening models, generative models proposing novel candidate structures.
3. **Hit-to-lead:** improving initial hits' potency and basic drug-like properties. ML applications: QSAR models predicting activity, structure-based design tools.
4. **Lead optimization:** iteratively improving potency, selectivity, and ADMET (absorption, distribution, metabolism, excretion, toxicity) properties through many rounds of design-make-test-analyze (DMTA). ML applications: multi-property optimization models, ADMET prediction, active learning to prioritize which molecules to synthesize next.
5. **Preclinical development:** in vivo studies, safety/toxicology testing before human trials.
6. **Clinical trials (Phase I-III):** testing in humans for safety and efficacy.

ML/AI is applied throughout, but its role and reliability vary a lot by stage — it's generally most mature and impactful in the early discovery stages (virtual screening, lead optimization) and much less mature (and much more heavily regulated/validated) closer to the clinic.

**Follow-ups:**
- Which stage of this pipeline do you think has benefited most from recent deep learning advances, and why?

---

### Q: What is the DMTA (Design-Make-Test-Analyze) cycle, and how does an AI/ML scientist fit into it? 🟢

**Answer:**
DMTA is the iterative loop at the heart of medicinal chemistry: **Design** a set of candidate molecules (traditionally by a chemist's intuition, increasingly assisted by generative/predictive models), **Make** them (synthesize in the lab), **Test** them (run assays to measure potency, selectivity, ADMET properties), and **Analyze** the results to inform the next round of design.

An AI/ML scientist typically contributes to the Design and Analyze steps: building models that predict properties before synthesis (to prioritize which molecules are worth making), and analyzing test results to update models and suggest the next round of candidates (active learning). A core part of the job is understanding that **each cycle takes real time and money** (synthesis and assays aren't instant or free), so the value of a model is measured by how much it improves the efficiency of this loop — fewer wasted synthesis cycles, faster convergence on a good candidate — not by an abstract accuracy number.

**Follow-ups:**
- How would you measure whether your models are actually accelerating the DMTA cycle in practice, versus just producing good offline metrics?

---

### Q: What does ADMET stand for, and why do these properties matter as much as raw potency in drug discovery? 🟢

**Answer:**
ADMET stands for **Absorption, Distribution, Metabolism, Excretion, and Toxicity** — the pharmacokinetic and safety properties that determine whether a potent molecule can actually become a safe, effective drug in a living organism.

A molecule can be extremely potent against its target in a test tube (in vitro) and still fail as a drug if it's poorly absorbed, doesn't reach the target tissue, is broken down too quickly by the liver, accumulates to toxic levels, or has off-target toxicity. This is why the vast majority of drug candidates fail in development — not from lack of potency, but from poor ADMET or safety profiles discovered later (often in preclinical or clinical stages, which is far more costly than catching the issue early). This is a major reason multi-property optimization (potency + ADMET together, not potency alone) is central to how ML models are used in lead optimization.

**Follow-ups:**
- Why is ADMET generally considered harder to model with ML than potency, and what does that imply about how much you'd trust a model's ADMET predictions?

---

### Q: What's the difference between phenotypic screening and target-based screening, and how does this affect the kind of ML models you'd build for each? 🟡

**Answer:**
- **Target-based screening** starts from a known, specific molecular target (e.g., a particular enzyme) and screens/designs molecules to modulate that target directly — the mechanism of action is defined upfront.
- **Phenotypic screening** starts from an observable cellular or organismal outcome (e.g., "does this molecule kill cancer cells" or "does it reduce inflammation in this assay") without necessarily knowing the specific molecular target — the mechanism may be discovered later, if at all.

For ML: target-based approaches often lend themselves to structure-based modeling (docking, structure-based generative design) since you have a defined binding site to model. Phenotypic screening data is often noisier and more indirect (the readout is several biological steps removed from the direct molecular interaction), so models trained on phenotypic data need to be more cautious about overfitting to assay-specific artifacts rather than the true underlying biology, and target deconvolution (figuring out the actual mechanism) is itself sometimes an ML-assisted problem.

**Follow-ups:**
- If you had a phenotypic hit with no known target, how might ML help identify a plausible mechanism of action?

---

### Q: Why does drug discovery have such a high failure rate, and what does that imply about how you should think about the role of AI in the process? 🟡

**Answer:**
Failure happens at every stage for different reasons: many initial hits don't have drug-like ADMET properties; preclinical safety issues emerge that weren't predicted; efficacy in animal models doesn't translate to humans; and even molecules that pass Phase I/II safety and early efficacy signals often fail in larger Phase III trials due to more subtle efficacy or safety issues, or simply because human disease biology is enormously complex and imperfectly represented by preclinical models.

The implication for AI's role: **ML models are trained on necessarily imperfect proxies for the real question** (does this drug work safely in humans), several steps removed from the ultimate outcome. This means a healthy amount of humility is warranted — most current AI contributions are about **improving the efficiency and hit rate of early discovery stages** (fewer wasted synthesis/assay cycles, better prioritization of candidates) rather than a claim that AI has "solved" downstream clinical failure risk, which stems from much deeper biological uncertainty that data alone may not resolve.

**Follow-ups:**
- How would you communicate the actual, realistic impact of an ML model to a computational chemistry team or leadership without overstating what it can do?

---

### Q: What is the difference between a "hit," a "lead," and a "clinical candidate" in drug discovery terminology? 🟢

**Answer:**
- **Hit:** a molecule showing some measurable activity against the target of interest, typically identified from a screen — usually still far from drug-like in overall properties.
- **Lead:** a hit that has been chemically optimized to have a reasonable balance of potency and initial drug-like properties, and represents a validated starting point for more extensive optimization (lead optimization).
- **Clinical candidate:** a molecule that has been optimized across potency, selectivity, and ADMET/safety sufficiently to be nominated for preclinical development and eventual human clinical trials — the culmination of the discovery phase.

This progression roughly matches the DMTA cycle stages and reflects increasing confidence and increasing property requirements at each stage, which is important context for understanding what "good enough" means for a model's predictions at each stage — the bar for a hit-identification model is different from the bar for a clinical-candidate-nomination decision.

**Follow-ups:**
- Which stage's ML models do you think carry the highest cost of a false positive, and why?
