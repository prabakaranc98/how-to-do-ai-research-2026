# 02 - Notation And Metrics

This note defines the symbols and measurements for the sprint.

## 1. Basic Objects

Let:

- `x` = input question or task.
- `e` = external evidence, retrieved context, tool output, or supporting document.
- `M` = small language model.
- `a` = model answer.
- `y` = ground-truth answer or accepted reference answer.
- `z` = correctness label, where `z = 1` if `a` is correct and `z = 0` otherwise.
- `c` = model confidence or confidence-like signal.
- `u` = uncertainty score, where higher means more uncertain.
- `s` = selection score, where higher means safer to answer.
- `d` = system decision.
- `p` = prompt-pressure condition, such as neutral, false premise, user certainty, or approval pressure.
- `g` = evidence-grounding label, such as supported, contradicted, or insufficient evidence.

Decision set:

`d in {answer, retrieve, abstain, clarify, escalate}`

For the first sprint, keep the decision set smaller:

`d in {answer, abstain}`

or:

`d in {answer, retrieve, abstain}`

## 2. Dataset Splits

Use three splits if possible:

- **Development set:** used to design prompts, inspect failures, and iterate.
- **Calibration set:** used to choose thresholds for abstention or conformal methods.
- **Test set:** used only for final evaluation.

If the dataset is small, keep the split simple and document the limitation.

Important:

- Do not tune thresholds on the test set.
- Do not inspect test examples repeatedly while designing the method.
- Keep a record of every prompt and threshold change.

## 3. Basic Accuracy

Accuracy:

`accuracy = correct answers / total examples`

For a QA task:

`accuracy = mean(z_i)`

Accuracy alone is not enough because the system may choose not to answer some questions.

## 4. Selective Prediction

Selective prediction studies systems that answer only on selected examples.

Let:

- `S` = set of examples where the system chooses to answer.
- `n` = total number of examples.
- `|S|` = number of answered examples.

Coverage:

`coverage = |S| / n`

Risk among answered examples:

`risk = wrong answered examples / answered examples`

Answered accuracy:

`answered_accuracy = correct answered examples / answered examples`

A good abstaining system should reduce risk while maintaining useful coverage.

Tradeoff:

- high coverage can increase risk;
- low risk can reduce coverage too much.

## 5. Abstention Metrics

Let the system abstain when uncertainty is high.

Useful metrics:

- **abstention rate:** fraction of examples where the system abstains.
- **answered accuracy:** accuracy on examples the system answered.
- **missed opportunity rate:** fraction of abstained examples the model would have answered correctly.
- **unsafe answer rate:** fraction of answered examples that were wrong.
- **abstention precision:** among abstentions, how many were actually hard or would have been wrong.

There is no single perfect metric. The right metric depends on the cost of wrong answers versus abstentions.

## 6. Confidence And Calibration

Calibration asks whether confidence matches correctness.

If the model says confidence is 0.8, then roughly 80% of such answers should be correct.

### Reliability Diagram

Group predictions into confidence bins:

- 0.0-0.1;
- 0.1-0.2;
- ...
- 0.9-1.0.

For each bin, compare:

- average confidence;
- empirical accuracy.

Well-calibrated systems have confidence close to empirical accuracy.

### Expected Calibration Error

Expected Calibration Error (ECE) summarizes calibration gap across bins.

For bins `B_m`:

`ECE = sum_m (|B_m| / n) * |accuracy(B_m) - confidence(B_m)|`

Lower ECE is better.

Limitation:

ECE depends on binning and can hide important patterns.

### Brier Score

For binary correctness:

`Brier = mean((c_i - z_i)^2)`

Lower is better.

Brier score rewards both accuracy and calibrated probability estimates.

### Negative Log Likelihood

If confidence is interpreted as probability:

`NLL = -mean(z_i * log(c_i) + (1 - z_i) * log(1 - c_i))`

NLL heavily penalizes confident wrong predictions.

## 7. Correctness Prediction

We can treat uncertainty estimation as predicting whether the answer is correct.

Given score `s_i`, evaluate:

- AUROC: can the score rank correct answers above incorrect answers?
- AUPRC: useful when errors are rare or imbalanced.
- thresholded precision/recall: useful for deciding when to answer.

This is important because a routing policy needs a score that predicts correctness.

## 8. Conformal Prediction Framing

Conformal prediction uses a calibration set to create thresholds with statistical coverage guarantees under exchangeability assumptions.

For this sprint, use the intuition first:

1. Define a nonconformity score `r_i`.
2. Higher `r_i` means the example looks less trustworthy.
3. Compute scores on a calibration set.
4. Choose a quantile threshold.
5. On test examples, answer only when `r_i` is below the threshold.

Possible nonconformity scores:

- `1 - confidence`;
- answer inconsistency;
- retrieval disagreement;
- verifier disagreement;
- semantic entropy;
- contradiction score.

Important caution:

Conformal methods give guarantees only under assumptions. If calibration and test data differ, the guarantee may not hold.

## 9. Retrieval Agreement Metrics

When evidence `e` is available, measure whether answer `a` is supported by evidence.

Possible signals:

- evidence contains the answer;
- answer is entailed by evidence;
- answer contradicts evidence;
- retrieved passages agree with each other;
- model answer changes after seeing evidence;
- model cites unsupported evidence.

A simple first-pass score:

`retrieval_agreement = 1` if answer is supported by retrieved evidence, else `0`.

Later, this can become a graded score.

## 10. Decision-Theoretic View

Every decision has a cost.

Let:

- `C_wrong` = cost of wrong answer.
- `C_abstain` = cost of abstaining.
- `C_retrieve` = cost of retrieval.
- `C_escalate` = cost of escalation.
- `C_latency` = latency cost.

A simple policy:

Answer if expected cost of answering is lower than abstaining or escalating.

Example:

`answer if P(correct | signals) * value_correct - P(wrong | signals) * C_wrong > -C_abstain`

The first sprint does not need a complex utility model, but this view helps interpret thresholds.

## 11. Sycophancy And Preference-Pressure Metrics

When studying false premises or user pressure, measure whether the model follows evidence or follows the user.

False-premise acceptance rate:

`false_premise_acceptance = accepted_false_premise / false_premise_examples`

Sycophancy rate:

`sycophancy_rate = user_aligned_wrong_answers / user_pressure_examples`

Confidence shift under pressure:

`confidence_shift = confidence_pressure_prompt - confidence_neutral_prompt`

Evidence override rate:

`evidence_override_rate = cases_where_evidence_corrects_user_pressure / user_pressure_with_evidence_examples`

Unsupported helpfulness rate:

`unsupported_helpfulness = polished_unsupported_answers / total_answers`

These metrics are rough, but useful. They treat sycophancy and reward-shaped "cheap tricks" as measurable epistemic behavior:

**Does the model answer according to evidence, or according to what the user or rater seems to want?**

## 12. Minimal Metric Set For First Sprint

Use:

- direct accuracy;
- self-confidence calibration;
- answered coverage;
- answered accuracy;
- risk among answered cases;
- ECE or Brier score;
- false-premise acceptance rate if pressure prompts are included;
- sycophancy/conformity rate if pressure prompts are included;
- error categories;
- latency/cost if easy to measure.

Avoid adding too many metrics before the first experiment is stable.
