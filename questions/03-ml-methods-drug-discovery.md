# 3. ML Methods for Drug Discovery

Modeling approaches specific to this domain: QSAR, graph neural networks, generative design, virtual screening, and docking.

---

### Q: What is QSAR (Quantitative Structure-Activity Relationship) modeling, and how has it evolved with modern deep learning? 🟢

**Answer:**
QSAR modeling predicts a biological or chemical property (activity, toxicity, solubility) as a function of a molecule's structure, traditionally using hand-crafted molecular descriptors/fingerprints fed into classical ML models (linear regression, random forests, support vector machines).

Modern deep learning has extended this in a few directions: **graph neural networks** learn structural representations directly from the molecular graph rather than relying on fixed hand-crafted descriptors; **pretrained molecular foundation models** (trained via self-supervised learning on large unlabeled compound libraries, then fine-tuned on smaller labeled property datasets) aim to transfer general chemical knowledge to data-scarce tasks; and **multi-task learning** trains a single model to predict many properties simultaneously, which can improve performance on data-scarce properties by sharing structure with data-rich ones.

Despite the hype around deep learning, classical descriptor-based models remain strong, often superior baselines on small/medium datasets (a common finding in the field), so a good candidate treats deep learning as one tool among several, not an automatic upgrade.

**Follow-ups:**
- When would you expect a graph neural network to meaningfully outperform a classical fingerprint-based model, and when would you expect it not to?

---

### Q: How do generative models for molecule design typically work, and what's the difference between optimizing in a learned latent space versus directly generating structures (e.g., autoregressive SMILES/graph generation)? 🟡

**Answer:**
Generative approaches broadly fall into a few families:
- **Autoregressive sequence generation:** generating SMILES strings token-by-token (or graphs node/edge-by-edge), often trained with reinforcement learning or fine-tuning to bias generation toward desired properties.
- **Latent space optimization:** using a variational autoencoder (VAE) or similar model to learn a continuous latent representation of molecules, then optimizing within that latent space (e.g., via Bayesian optimization or gradient-based methods) toward a target property, and decoding the optimized latent point back into a molecule.
- **Diffusion-based generation:** increasingly used for both 2D graph and 3D structure generation, generating molecules by iteratively denoising from random noise.

The latent-space approach can make optimization smoother/more tractable since the latent space is (ideally) continuous and differentiable, but a real risk is that the decoder may fail to reconstruct a valid, synthesizable molecule from an "optimized" latent point that lies outside the region the decoder was well-trained on. Direct/autoregressive generation avoids this decode-failure risk but can make property-directed optimization less smooth. In practice, most production generative pipelines combine generation with heavy downstream filtering (synthesizability checks, property prediction, novelty/diversity checks) rather than trusting raw generative output directly.

**Follow-ups:**
- What would you do if a generative model proposes molecules that score well on your property predictor but look chemically implausible or unsynthesizable to an expert chemist?

---

### Q: What is molecular docking, and what are its main strengths and limitations for virtual screening? 🟡

**Answer:**
Molecular docking predicts how a small molecule (ligand) binds to a protein target's binding site, by searching over possible poses/orientations and scoring them (usually with a physics-inspired or empirical scoring function) to estimate binding affinity and geometry.

Strengths: doesn't require any prior activity data for the specific target (unlike QSAR, which needs labeled training data) — useful for screening large virtual libraries against a target with a known/modeled structure but limited experimental activity data. Provides an interpretable, structural hypothesis for binding (which atoms interact with which residues).

Limitations: docking scoring functions are known to be **imperfect estimators of true binding affinity** — they're often good at distinguishing binders from clear non-binders at a coarse level but unreliable for fine-grained ranking of similar, active compounds. They also generally don't fully account for protein flexibility (most docking treats the protein as rigid or with limited flexibility), solvent effects, and entropic contributions to binding, all of which limits accuracy. As a result, docking is typically used as a **triage/filtering step** to narrow a huge virtual library down to a more manageable, enriched subset for further computational or experimental follow-up — not as a final, standalone affinity prediction to trust directly.

**Follow-ups:**
- How would you validate whether a docking protocol is actually adding value for a specific target, before relying on it for a large-scale virtual screen?

---

### Q: What is active learning, and why is it particularly valuable in a drug discovery context (versus a typical ML setting with abundant labeled data)? 🟡

**Answer:**
Active learning is an iterative modeling strategy where the model itself identifies which unlabeled examples would be most informative to label next (rather than labeling data randomly or exhaustively), and that selected subset is prioritized for actual experimental testing.

This is especially valuable in drug discovery because **each label (assay result) is expensive and slow to obtain** (real synthesis and wet-lab experiments, unlike many ML domains where more labels can be acquired cheaply/automatically). Active learning directly targets the core bottleneck of the DMTA cycle: instead of synthesizing/testing molecules based purely on chemist intuition or exhaustive screening, the model prioritizes molecules expected to be maximally informative (e.g., highest uncertainty, or highest expected improvement toward a design goal), reducing the number of expensive DMTA cycles needed to reach a good candidate.

**Follow-ups:**
- What's the difference between uncertainty-based active learning and Bayesian optimization-style "expected improvement" active learning, and when would you prefer one over the other in lead optimization?

---

### Q: What is multi-parameter/multi-objective optimization in drug discovery, and why is it fundamentally harder than optimizing for potency alone? 🟡

**Answer:**
Real drug candidates need to satisfy many, often competing properties simultaneously: potency, selectivity (not hitting unintended targets), and a range of ADMET properties (solubility, metabolic stability, low toxicity, etc.). Multi-parameter optimization explicitly models and balances these tradeoffs rather than optimizing a single property in isolation.

It's harder because: **properties often trade off against each other** (a chemical modification that improves potency might worsen solubility), so there's rarely a single molecule that's simultaneously best on every axis — the goal is usually to find a good Pareto-optimal balance, not a single maximum. It also requires **combining predictions from multiple different models** (each potentially trained on a different, imperfectly overlapping dataset with its own reliability profile), and communicating this multi-dimensional tradeoff clearly to the medicinal chemists who ultimately decide what to synthesize, rather than collapsing it into a single opaque score that hides the underlying tradeoffs.

**Follow-ups:**
- How would you design a scoring function that combines multiple property predictions into a single ranking, while still being transparent about the underlying tradeoffs to a chemist reviewing the output?

---

### Q: What role do protein language models (e.g., models trained on large protein sequence databases) play in modern drug discovery, distinct from small-molecule-focused models? 🔴

**Answer:**
Protein language models are trained via self-supervised learning on large databases of protein sequences (analogous to how large language models are trained on text), learning representations that capture evolutionary and structural information without explicit labels. They're used for tasks like: predicting protein structure or structural features, predicting the effect of mutations on protein function/stability, and generating novel protein sequences (e.g., for antibody or enzyme design) — extending "drug discovery" beyond small molecules into biologics and protein engineering.

Distinct from small-molecule models: proteins are large, and their function depends heavily on 3D folding and long-range structural interactions, which is a different modeling challenge than a small molecule's much smaller, more locally-structured space. A drug discovery ML scientist working on biologics/protein-based therapeutics needs at least conceptual familiarity with this separate line of models (e.g., structure prediction tools, protein language models) distinct from the cheminformatics/small-molecule toolkit described elsewhere in this repo.

**Follow-ups:**
- How would you evaluate whether a protein language model's learned representation is actually useful for a specific downstream task (e.g., predicting a mutation's effect on binding), versus just being a generically impressive but task-irrelevant embedding?
