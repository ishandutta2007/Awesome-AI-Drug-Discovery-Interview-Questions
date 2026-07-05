# 8. Behavioral & Case Studies

Cross-functional collaboration with medicinal chemists, biologists, and structural biologists is central to this role. Interviewers probe whether you can bridge computational and experimental worlds.

---

### Q: Tell me about a time your model's predictions were met with skepticism from an experimental (wet-lab) colleague. How did you handle it? 🟡

**What a strong answer includes:**
- Specific understanding of *why* the skepticism existed (often well-founded — experimentalists have seen many overhyped computational claims) rather than treating it as an obstacle to overcome by force.
- A concrete approach to building credibility: proposing a small, well-scoped test where the model's prediction could be checked against real experimental data, rather than asking for blind trust.
- Evidence of genuine two-way collaboration — incorporating the colleague's domain expertise into refining the model or its application, not just trying to convince them the model was right.
- An honest outcome, including if the model was wrong in that specific instance and what was learned from it.

**Follow-ups:**
- How did that experience change how you communicate model predictions and uncertainty going forward?

---

### Q: Describe a project where a model performed well in retrospective validation but failed to deliver value prospectively. What did you learn? 🔴

**What a strong answer includes:**
- Honesty that this is a common, expected occurrence in this field (ties to section 6's discussion of retrospective vs. prospective validation) — interviewers are wary of candidates who claim uniform prospective success.
- A specific diagnostic process for understanding the gap (was the retrospective evaluation methodology flawed — e.g., random split leakage? Did the prospective compounds fall outside the model's applicability domain? Was there a distribution shift between the historical data and the new chemical series being explored?).
- Concrete changes made to evaluation practices or model scope as a result (e.g., adopting scaffold-split validation going forward, being more conservative about applicability domain claims).
- Evidence that the experience made you a more rigorous, appropriately humble scientist rather than defensive about the model's limitations.

**Follow-ups:**
- How did you communicate this gap between expected and actual performance to the project team and to leadership?

---

### Q: Walk me through how you'd approach a new drug discovery program where you're told "we want to use AI to accelerate this," starting from day one. 🟡

**Case-study structure — a strong candidate would:**
1. **Understand the specific bottleneck first** — is the team struggling with hit identification, lead optimization throughput, ADMET failures, or something else? "Use AI" is not a specific enough goal, and the right approach differs a lot depending on the actual bottleneck.
2. **Assess data availability** — what historical data exists for this target/program (or closely related ones), and is it sufficient to train a meaningful model, or does the program need to start by generating an initial dataset (e.g., an initial small screen) before ML can add much value?
3. **Start with a scoped, validated pilot** rather than promising a comprehensive AI-driven pipeline immediately — propose a specific, measurable near-term goal (e.g., "improve enrichment in the next round of compound selection by X%") that can build credibility incrementally.
4. **Plan the evaluation approach upfront**, including how prospective validation will be measured, before the first round of model-guided decisions is made — not as an afterthought once results come in.
5. **Establish the collaboration process** with the chemistry/biology team early — how will model suggestions be reviewed and incorporated into actual DMTA decisions, and what does the feedback loop look like.

**Follow-ups:**
- The program lead wants a fully autonomous compound-selection pipeline within the first month. How would you manage this expectation?

---

### Q: Tell me about a time you had to explain a technical limitation of your model to a non-computational stakeholder (e.g., a program lead or clinician) who was hoping for a more definitive answer. 🟡

**What a strong answer includes:**
- A specific example of translating a genuine model limitation (uncertainty, applicability domain boundary, known failure mode) into terms a non-computational audience could use to make a better-informed decision, rather than either overstating confidence or being so hedged as to be unhelpful.
- Evidence you calibrated the level of technical detail appropriately to the audience.
- A concrete outcome showing the stakeholder was able to make a better decision as a result of your honest framing, even if the "answer" wasn't as clean as they initially hoped for.

**Follow-ups:**
- How do you avoid being perceived as constantly hedging or unhelpful when a large part of your job involves communicating genuine uncertainty?

---

### Q: Describe a time you had to decide between a faster, simpler modeling approach and a more sophisticated one under real project time pressure. 🟡

**What a strong answer includes:**
- A clear articulation of the actual tradeoff considered (development time, data requirements, interpretability, expected accuracy gain) rather than reflexively reaching for the most sophisticated available technique.
- Evidence of validating the choice against the specific project's needs (e.g., "the team needed a decision this week, and a well-tuned classical model on fingerprints was good enough and could be delivered in time, versus a more complex approach that wouldn't have been ready").
- An honest reflection on whether, in hindsight, the choice was right, and what you'd do differently with more time or data.

**Follow-ups:**
- How do you decide when it's worth investing in a more sophisticated modeling approach later, after shipping the simpler one first?

---

### Q: Tell me about a time you disagreed with a computational colleague or supervisor about the right validation methodology for a model before it was used to guide real experimental decisions. 🔴

**What a strong answer includes:**
- A specific, technically grounded disagreement (e.g., about splitting strategy, appropriate metrics, or applicability domain claims) rather than a vague interpersonal conflict.
- Evidence of resolving the disagreement through evidence (running both validation approaches and comparing results, or finding an external benchmark/precedent) rather than through seniority or persistence alone.
- A clear articulation of what was ultimately decided, why, and the outcome — including whether the extra validation rigor caught a real issue or turned out to be unnecessary caution, and what that taught you about calibrating validation effort to actual risk going forward.

**Follow-ups:**
- How do you decide how much validation rigor is "enough" given that more rigorous validation always takes more time, and project timelines are often tight?
