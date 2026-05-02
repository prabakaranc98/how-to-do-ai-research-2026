# 04 - Related Work Map

This note maps the areas to study. It is a guide for reading and reference collection.

Do not try to read everything before experimenting. Use this map to identify what is relevant to the next hypothesis.

For concrete paper links and short takeaways, see `05-paper-reading-list.md`.

## 1. LLM Calibration

Core question:

**Do model confidence scores match empirical correctness?**

What to look for:

- verbalized confidence;
- token probability confidence;
- calibration of multiple-choice vs open-ended answers;
- overconfidence and underconfidence;
- calibration under distribution shift;
- calibration after instruction tuning or RLHF.

Why it matters:

If confidence is not calibrated, the system cannot safely decide when to answer.

Possible gap:

Many studies focus on large models or classification-like settings. Small open models and open-ended QA may need more practical evaluation.

## 2. Selective Prediction And Abstention

Core question:

**Can the model choose not to answer when likely wrong?**

What to look for:

- selective classification;
- risk-coverage curves;
- abstention policies;
- answer-or-abstain systems;
- thresholds chosen on validation/calibration data.

Why it matters:

Real systems often need reliable answers more than maximum answer rate.

Possible gap:

Many LLM products still optimize answer generation, not principled answer refusal or escalation.

## 3. Conformal Prediction For LLMs

Core question:

**Can conformal methods provide error-control style wrappers for language-model outputs?**

What to look for:

- split conformal prediction;
- conformal sets for classification;
- conformal generation;
- conformal risk control;
- conformal abstention;
- assumptions such as exchangeability.

Why it matters:

Conformal prediction offers a bridge between statistical guarantees and LLM uncertainty.

Possible gap:

Open-ended language generation makes nonconformity scores difficult. The useful contribution may be practical score design rather than new theory.

## 4. Semantic Uncertainty

Core question:

**How uncertain is the model over meanings, not just strings?**

What to look for:

- sampling multiple answers;
- clustering answers by semantic equivalence;
- entropy over semantic clusters;
- disagreement in final claims;
- hallucination detection through semantic variation.

Why it matters:

Small changes in wording can hide large differences in meaning.

Possible gap:

Semantic clustering can depend on another model, which creates a second layer of uncertainty.

## 5. Self-Consistency

Core question:

**Does repeated generation reveal uncertainty or improve correctness?**

What to look for:

- majority vote;
- chain-of-thought self-consistency;
- answer diversity;
- sample agreement;
- relation between consistency and correctness.

Why it matters:

Self-consistency is cheap enough to test and often works as a practical uncertainty signal.

Possible gap:

Consistency can be confidently wrong. The key question is when consistency actually predicts correctness.

## 6. Hallucination And Factuality

Core question:

**When does the model produce unsupported or false claims?**

What to look for:

- factual QA;
- attribution and citation faithfulness;
- unsupported answer detection;
- hallucination under weak retrieval;
- hallucination under prompt pressure.

Why it matters:

Epistemic reliability fails when the model gives unsupported claims with high confidence.

Possible gap:

Many hallucination evaluations measure final factuality but not whether the model should have abstained or asked for evidence.

## 7. Retrieval-Augmented Generation Reliability

Core question:

**Does retrieval make the answer more reliable, or just more confident?**

What to look for:

- retrieval quality;
- answer faithfulness;
- context relevance;
- contradiction handling;
- retrieval noise;
- source attribution;
- RAG evaluation.

Why it matters:

Retrieval is a common fix for model uncertainty, but bad retrieval can worsen reliability.

Possible gap:

Need clear measurement of when retrieval should be used, when it helps, and when it should be distrusted.

## 8. Model Routing And Cascades

Core question:

**When should a small model handle the task, and when should a larger system take over?**

What to look for:

- small-to-large model cascades;
- cost-aware routing;
- confidence-based routing;
- verifier-based routing;
- fallback systems;
- escalation policies.

Why it matters:

Small models become much more useful when embedded inside a larger decision system.

Possible gap:

Many routing systems optimize cost/accuracy, but fewer inspect epistemic behavior and failure categories.

## 9. Small Language Model Reliability

Core question:

**What reliability problems are specific to small models?**

What to look for:

- Phi, Gemma, SmolLM, Qwen, Mistral/Ministral style small models;
- on-device deployment;
- quantization effects;
- domain adaptation;
- small model reasoning limits;
- small model calibration.

Why it matters:

Small models are likely to be deployed in settings where cost, privacy, and latency matter.

Possible gap:

Open small models are often benchmarked for capability, but less often studied for epistemic decision behavior.

## 10. Epistemic Humility And "Knowing When Not To Answer"

Core question:

**Can a model know when it should not make a claim?**

What to look for:

- refusal behavior;
- uncertainty expression;
- asking clarifying questions;
- abstention;
- admitting insufficient evidence;
- answerability detection.

Why it matters:

Reliable systems should not answer every query.

Possible gap:

Refusal and abstention are often evaluated through safety lenses, but less often through statistical correctness and knowledge limits.

## 11. Conformity, Suggestibility, And Prompt Pressure

Core question:

**Does the model agree with false premises or social pressure?**

What to look for:

- sycophancy;
- user-pressure prompts;
- false premise questions;
- majority influence;
- contradiction handling;
- instruction hierarchy.

Why it matters:

An epistemically reliable model should not simply conform to the user.

Possible gap:

Small-model conformity under uncertainty may be underexplored, especially when combined with retrieval and abstention decisions.

## 12. Cognitive Science, Surprise, And Belief Updating

Core question:

**How should evidence change behavior?**

What to look for:

- surprise;
- prediction error;
- belief updating;
- metacognition;
- confidence revision;
- cognitive control;
- active information seeking.

Why it matters:

These concepts can inspire better behavioral tests without requiring claims about consciousness.

Possible gap:

There is room for simple, behavior-grounded experiments that test whether LLMs update under contradiction or uncertainty.

## 13. What Might Be Missing

Potential gap for this sprint:

**A focused, small-model study of answer/abstain/retrieve behavior using uncertainty signals, calibration, and failure analysis.**

More specific possible contribution:

- Show which simple uncertainty signals predict correctness for small models.
- Compare self-confidence vs consistency vs retrieval agreement.
- Build a small selective-prediction benchmark for small-model QA.
- Analyze when retrieval helps, hurts, or creates overconfidence.
- Study conformity under false-premise prompts.
- Produce a failure taxonomy for small-model epistemic behavior.

## 14. Reading Notes Template

For each paper, post, or reference:

- Citation / link:
- Area:
- Main question:
- Method:
- Dataset / setup:
- Key result:
- What it measures well:
- What it misses:
- Relevance to this sprint:
- Follow-up idea:
