# 7. Regulatory, IP & Responsible AI

Increasingly relevant given growing regulatory attention to AI in healthcare/pharma and genuine dual-use concerns in this specific domain.

---

### Q: What is "dual-use" risk in the context of AI-driven drug/molecule design, and how would you think about mitigating it in your own work? 🔴

**Why it's asked:** A serious, field-specific safety topic — generative molecule design models can, in principle, be redirected toward designing harmful compounds (e.g., toxins) rather than therapeutics, and this has been a documented concern raised in the scientific literature.

**Answer:**
Dual-use risk refers to the fact that the same generative modeling techniques used to design beneficial therapeutic molecules could, with a different objective function (e.g., optimizing for toxicity rather than therapeutic activity), be redirected toward proposing harmful compounds. This has been explicitly discussed in the scientific literature as a genuine, demonstrated risk with this class of models, not a hypothetical one.

Practical mitigations a scientist can advocate for: **restricting broad publication of readily-weaponizable model weights/training details** where risk is high, while still supporting appropriate scientific transparency for safety-relevant findings; **screening generated outputs against known toxin/harmful-compound databases** as a safeguard layer; **institutional review processes** for particularly sensitive generative design work (similar in spirit to biosafety review processes in wet-lab biology); and **being thoughtful about who has access to powerful generative design tools** and under what oversight, rather than assuming open access is automatically the right default. This is a genuinely evolving area of policy and scientific norms, and a good answer shows awareness of the tension between open science and this specific risk, rather than a dismissive "this isn't my problem" stance.

**Follow-ups:**
- How would you balance open scientific publication norms (which the field generally values) against dual-use risk mitigation for a specific piece of generative modeling work?

---

### Q: How do FDA (or other regulatory bodies') expectations around AI/ML use in drug development affect how a computational scientist should build and document their models? 🔴

**Answer:**
Regulatory bodies have been increasingly issuing guidance specifically addressing AI/ML use in drug development, generally emphasizing the need for **model transparency, validation rigor, and documentation** proportional to how directly the model's output feeds into a regulatory decision or clinical claim. A model used purely for early-stage internal hit prioritization faces much lighter practical scrutiny than one whose output is used to support a specific regulatory claim or trial design decision.

Practical implications for a computational scientist: maintain **rigorous documentation of model development, training data provenance, and validation methodology** (not just a final performance number) from early on, since retrofitting this documentation later is far harder; understand that as a model's role moves closer to directly informing a regulatory-facing decision, the validation bar rises significantly (prospective validation, robustness/sensitivity analysis, clear articulation of the applicability domain); and stay aware that regulatory guidance in this specific area is evolving relatively quickly, so a scientist working close to the regulatory-facing end of a pipeline should check current agency guidance rather than relying on older assumptions.

**Follow-ups:**
- How would your model documentation and validation practices differ for a purely internal hit-triage model versus a model whose predictions are cited in a regulatory submission?

---

### Q: How do intellectual property (IP) considerations affect how AI-generated molecules are handled in drug discovery programs? 🟡

**Answer:**
Patentability and IP strategy are central to pharma/biotech business models (a drug's commercial viability depends heavily on patent protection), and AI-generated molecule design intersects with this in a few specific ways: **novelty and inventive step** requirements for patents mean generated molecules need genuine structural/functional novelty, not just minor variations of existing patented compounds (a real risk if a generative model is trained heavily on existing patent literature and tends to produce close analogs); questions around **inventorship** when an AI system substantially contributes to designing a novel compound are a genuinely unsettled and evolving area of patent law in various jurisdictions; and practical **freedom-to-operate analysis** (checking whether a promising generated molecule infringes on existing patents) needs to happen before significant investment in optimizing and advancing a specific compound, which is a legal/IP function a computational scientist should be aware of and loop in early, not something to assume is automatically handled downstream.

**Follow-ups:**
- If your generative model repeatedly proposes molecules very close to already-patented compounds, what would you investigate about the model or the training data?

---

### Q: How would you handle a situation where your model's data comes from a mix of public and proprietary/licensed sources, and you need to decide what can be published or shared externally? 🟡

**Answer:**
- **Understand data provenance and licensing terms clearly before starting**, not after a paper/model is ready to share — different data sources often carry different restrictions on redistribution, commercial use, or publication of results derived from them.
- **Separate what can be published about methodology from what can be published about specific results/data** — often the modeling approach itself can be shared openly even when the underlying proprietary training data or specific compound structures cannot.
- **Loop in legal/IP and data-licensing stakeholders early** when planning any external publication or open-sourcing of a model trained on mixed data sources, rather than assuming a scientist's default instinct toward openness overrides contractual/licensing obligations.
- **Consider synthetic or public benchmark validation** as a way to demonstrate a method's value externally without exposing proprietary training data directly.

**Follow-ups:**
- Your team wants to publish a paper showcasing a new modeling method, but the training data is under a restrictive license. How would you navigate this?

---

### Q: How should a drug discovery ML scientist think about patient safety implications of their models, given that most of them work several steps removed from actual patients? 🟡

**Answer:**
Even though most drug discovery ML work happens far upstream of any patient exposure (early screening, lead optimization), the ultimate purpose of the entire pipeline is producing a safe, effective medicine for patients — so a scientist should maintain awareness that **model errors compound downstream** and that overconfident or poorly validated predictions at an early stage can waste resources chasing a candidate destined to fail later for reasons the early model couldn't or didn't account for, delaying good candidates from getting appropriate attention.

Practically, this means: being honest and calibrated about model uncertainty and limitations rather than overselling results to secure resources/attention for a project; flagging when a model is being asked to make decisions outside its validated scope (e.g., a potency-focused model being used to make an implicit safety-related decision it was never validated for); and maintaining the broader perspective that computational predictions are one input supporting, not replacing, the more rigorous experimental and clinical safety evaluation that follows.

**Follow-ups:**
- Describe a scenario where an overconfident computational prediction early in a program could lead to a meaningful downstream cost or risk, and how you'd guard against it.
