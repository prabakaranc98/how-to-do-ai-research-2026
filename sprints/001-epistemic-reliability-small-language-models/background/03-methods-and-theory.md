# 03 - Methods And Theory

This note lists methods that may help the sprint. It is not a commitment to use all of them.

The first experiment should choose a small subset.

## 1. Baseline: Direct Answering

Prompt the small model to answer the question directly.

Measure:

- accuracy;
- self-rated confidence if requested;
- failure modes;
- response style;
- hallucination patterns.

Why this matters:

Every uncertainty method must beat or clarify the direct-answer baseline.

## 2. Verbalized Confidence

Ask the model to provide confidence.

Example:

`Answer the question and give a confidence from 0 to 1.`

Possible issues:

- model confidence may be uncalibrated;
- model may use confidence as style rather than probability;
- prompt wording may change confidence;
- confidence may be high for fluent hallucinations.

Use it as a baseline, not as a trusted signal.

## 3. Token Probability / Logprob Signals

If model logprobs are available, use them as confidence-like signals.

Possible signals:

- average answer token logprob;
- minimum token logprob;
- normalized sequence probability;
- probability of selected answer option for multiple-choice tasks.

Limitations:

- open-ended answer logprobs are hard to compare across lengths;
- fluent wrong answers can still have high probability;
- some APIs or local wrappers may not expose logprobs.

## 4. Self-Consistency

Sample multiple answers from the model.

Signals:

- exact answer agreement;
- majority answer frequency;
- entropy over answer clusters;
- disagreement between generated rationales;
- variation in final answer.

Intuition:

If the model gives many different answers to the same question, uncertainty is likely higher.

Limitation:

Repeated wrong answers can look consistent. Consistency is not correctness.

## 5. Semantic Uncertainty

For open-ended generation, exact string matching is weak. Different wordings can mean the same answer, and similar wordings can mean different claims.

Semantic uncertainty tries to cluster answers by meaning.

Possible process:

1. Generate multiple answers.
2. Extract final claims.
3. Cluster claims by semantic equivalence.
4. Compute uncertainty over clusters.

Useful signal:

High entropy over semantic clusters suggests uncertainty.

Limitation:

Semantic clustering itself may require another model or heuristic and can introduce errors.

## 6. Retrieval-Augmented Answering

Add retrieved evidence before answering.

Questions to study:

- Does retrieval improve correctness?
- Does retrieval increase unsupported confidence?
- Does the model over-trust bad retrieval?
- Does contradiction lead to revision or confusion?
- Does the model cite evidence faithfully?

Minimal setup:

- use a small fixed evidence set;
- or use a simple retriever;
- compare no-evidence, supporting-evidence, and contradictory-evidence conditions.

## 7. Retrieval Agreement

Measure whether the model answer agrees with evidence.

Possible signals:

- answer appears in retrieved evidence;
- answer is supported by retrieved evidence;
- evidence contradicts answer;
- retrieved documents disagree with each other;
- the model changes answer after seeing evidence.

Simple first version:

Manually or heuristically label a small sample as:

- supported;
- contradicted;
- not enough evidence;
- irrelevant retrieval.

## 8. Verifier / Judge Scores

Use a verifier to score whether an answer is correct or supported.

Verifier options:

- exact match for simple QA;
- rule-based scoring;
- embedding similarity;
- entailment model;
- stronger LLM judge;
- human annotation for small samples.

Risks:

- LLM judges can be biased;
- verifier errors can hide model errors;
- exact match is too strict for open-ended answers;
- stronger-model judging can become expensive.

For the first sprint, prefer simple tasks where correctness is easier to judge.

## 9. Selective Prediction

Selective prediction means the system can abstain.

Policy:

`answer if score >= threshold`

Otherwise:

`abstain`

or:

`retrieve / escalate`

Useful plots:

- risk vs coverage;
- accuracy vs coverage;
- abstention rate vs answered accuracy.

This is likely the simplest useful formalism for the first experiment.

## 10. Conformal Abstention

Conformal abstention can turn a score into a threshold chosen on a calibration set.

Basic idea:

1. Compute nonconformity scores on calibration examples.
2. Choose a threshold based on desired error level.
3. Answer only when a new example is below the threshold.

Potential scores:

- low confidence;
- high answer disagreement;
- retrieval contradiction;
- verifier disagreement;
- high semantic entropy.

Important:

Use conformal methods carefully. Guarantees depend on assumptions. The first sprint can use conformal thinking without overclaiming formal guarantees.

## 11. Model Routing

Routing asks:

**When should the small model handle the task, and when should another system take over?**

Possible routing decisions:

- answer directly;
- retrieve evidence first;
- ask for clarification;
- escalate to larger model;
- escalate to human;
- abstain.

First sprint can simplify to:

`answer` vs `abstain`

or:

`answer` vs `retrieve`

Routing becomes more meaningful after basic uncertainty signals are measured.

## 12. Failure Taxonomy

A good failure taxonomy makes the work more than a benchmark table.

Possible categories:

- factual hallucination;
- outdated knowledge;
- ambiguous question;
- unsupported answer;
- contradiction ignored;
- misleading prompt accepted;
- overconfident wrong answer;
- sycophantic answer;
- false-premise acceptance;
- reward-hacked helpfulness;
- polished but unsupported answer;
- low confidence correct answer;
- retrieval irrelevant;
- retrieval contradicted;
- answer formatting failure;
- reasoning error;
- arithmetic error;
- entity/date confusion.

For each failed example, record:

- input question;
- model answer;
- evidence if any;
- expected answer;
- uncertainty signal values;
- decision policy output;
- failure category;
- notes.

## 13. Sycophancy And Reward-Hacking Stress Tests

These tests ask whether the model chooses warranted answers or reward-shaped answers.

Prompt conditions:

1. **Neutral:** ask the question plainly.
2. **False premise:** include an incorrect assumption in the question.
3. **User certainty:** the user states a wrong belief confidently.
4. **Approval pressure:** the user asks the model to agree or validate.
5. **Contradictory evidence:** provide evidence that conflicts with the user's premise.
6. **Preference trap:** reward-style wording makes a complete, confident answer feel more helpful than abstention.

Measure:

- false-premise acceptance rate;
- sycophancy rate;
- confidence shift under user pressure;
- answer change after contradictory evidence;
- unsupported helpfulness rate;
- abstention or clarification rate when the premise is weak.

Useful qualitative labels:

- resists false premise;
- corrects user politely;
- asks for clarification;
- retrieves evidence;
- abstains;
- agrees incorrectly;
- gives polished unsupported explanation;
- overstates certainty.

Why this matters:

RLHF, RLAIF, and preference tuning may reward responses that look helpful to raters. Epistemic reliability asks whether the model remains sensitive to truth, evidence, and uncertainty under those incentives.

## 14. Minimal Method Stack For First Sprint

Recommended first method stack:

1. Direct answer baseline.
2. Self-rated confidence.
3. Multiple samples for consistency.
4. Selective prediction threshold.
5. Small failure taxonomy.
6. Optional false-premise / user-pressure stress test.

Add retrieval only after direct uncertainty behavior is understood.

Alternative if retrieval is central:

1. Direct answer baseline.
2. Supporting evidence condition.
3. Contradictory evidence condition.
4. Update / non-update measurement.
5. Error and conformity analysis.

## 15. What To Avoid Early

Avoid:

- too many models;
- too many datasets;
- too many metrics;
- complex RAG infra before the question is clear;
- overclaiming formal guarantees;
- philosophy without measurement;
- using an LLM judge as the only source of truth;
- treating self-confidence as real calibration without testing.
- treating sycophancy as only a "personality" issue instead of an evidence-sensitivity issue.
