# How to Do Statistical Research on LLMs

This note is the working background guide for turning statistics into LLM research.

The core move:

**Do not start by asking "which model is better?" Start by asking "what quantity am I trying to estimate, under what uncertainty, from what biased evidence, for what decision?"**

That is the statistician's advantage in LLM research.

## 1. The Statistical Research Loop

Every statistical LLM project should follow this loop:

```text
1. Define the decision problem.
2. Define the estimand.
3. Identify the data-generating process.
4. Identify uncertainty and bias.
5. Choose a measurement or training method.
6. Estimate with controls and uncertainty intervals.
7. Validate on held-out, shifted, and tail cases.
8. Decide whether the evidence changes the next experiment.
```

For LLMs, the "data" is not only benchmark examples. It can include prompts, generations, logits, hidden states, preference labels, reward-model scores, activations, tool traces, user feedback, synthetic data, and model-to-model supervision.

## 2. Start With an Estimand

An estimand is the thing you are trying to know.

Weak framing:

**Does model B transfer knowledge from model A?**

Better statistical framing:

**Does uncertainty-weighted teacher supervision reduce target-domain error in a small student model without increasing inherited teacher errors on ambiguous and false-premise prompts?**

Possible LLM estimands:

- probability that an answer is correct;
- probability that the model should abstain;
- selective risk among answered cases;
- calibration error;
- teacher-student error overlap;
- reward-model validity;
- preference-disagreement structure;
- probability of a harmful behavior under a prompt distribution;
- representation similarity between teacher and student;
- causal effect of a data mixture on a target capability;
- subgroup reliability under a deployment distribution.

If the estimand is vague, the experiment will become vague.

## 3. Name the Unit of Analysis

In ordinary statistics, the unit might be a person, patient, school, or transaction. In LLM research, it must be explicit.

Possible units:

- prompt;
- prompt-template family;
- generated answer;
- token;
- reasoning step;
- preference pair;
- user-model interaction;
- tool call;
- hidden-state vector;
- activation feature;
- model checkpoint;
- training data source;
- teacher-student transfer example;
- deployment task.

The unit determines what can be averaged, bootstrapped, clustered, or causally interpreted.

## 4. Treat Models as Measurement Instruments

An LLM can be:

- a predictor;
- a labeler;
- a judge;
- a teacher;
- a generator of synthetic data;
- a reward model;
- a noisy measurement device for hidden capability or preference.

This means every model-produced signal needs a measurement question:

- Is the model calibrated?
- Does it have systematic bias?
- Does it fail differently across domains?
- Does it produce correlated errors?
- Does it preserve uncertainty?
- Does it collapse minority or rare modes?
- Does it leak benchmark knowledge?

This is why teacher-student transfer is statistical: the teacher is not ground truth. The teacher is a noisy proxy for the target.

## 5. Common Statistical LLM Problem Templates

### Uncertainty and Selective Prediction

Question:

**When should an LLM answer, abstain, retrieve, clarify, or escalate?**

Statistical objects:

- calibration;
- Brier score;
- expected calibration error;
- selective risk;
- conformal thresholds;
- confidence intervals;
- uncertainty under distribution shift.

Minimal project:

Build a QA eval where the model can answer or abstain. Compare self-confidence, sample consistency, verifier disagreement, and retrieval agreement as predictors of correctness.

### Evaluation Science

Question:

**Is a benchmark score or safety eval result strong enough evidence for a deployment decision?**

Statistical objects:

- benchmark variance;
- bootstrap confidence intervals;
- prompt sensitivity;
- judge disagreement;
- leakage risk;
- power for rare failures;
- false positive and false negative rates.

Minimal project:

Take one benchmark or custom eval and report score uncertainty, prompt-template variance, judge variance, and failure slices instead of one headline score.

### Preference and Alignment Statistics

Question:

**What does it mean to align to human preferences when humans disagree?**

Statistical objects:

- pairwise preference models;
- annotator heterogeneity;
- preference cycles;
- reward-model calibration;
- minority preference preservation;
- preference collapse after RLHF/DPO.

Minimal project:

Build a small preference dataset with annotator clusters or simulated preference types. Test whether a reward model preserves disagreement or collapses to one average preference.

### Cross-Model Knowledge Transfer

Question:

**Which teacher signals should a small student copy, and when should it distrust the teacher?**

Statistical objects:

- teacher reliability;
- measurement error;
- bias-variance tradeoff;
- teacher-student error overlap;
- fidelity vs generalization;
- uncertainty propagation;
- representation alignment.

Minimal project:

Compare naive distillation against uncertainty-weighted distillation. Measure not only accuracy, but inherited teacher errors, calibration, and tail performance.

### Heterogeneous Feature Transport

Question:

**Can a small student absorb selected latent knowledge from a larger, differently trained teacher model?**

Statistical objects:

- latent target knowledge;
- proxy teacher measurements;
- representation geometry;
- CKA / CCA / Procrustes alignment;
- optimal transport;
- Gromov-Wasserstein alignment;
- concept probes;
- target-domain risk.

Minimal project:

Transfer an answerability, contradiction, or refusal feature from a larger teacher to a smaller student. Compare output-only distillation with representation-geometry transfer.

### Data Mixtures and Causal Attribution

Question:

**Which data source caused a model capability or failure to change?**

Statistical objects:

- ablation design;
- causal estimands;
- mixture weights;
- confounding by data quality;
- sample efficiency;
- scaling behavior;
- distribution shift.

Minimal project:

Train or fine-tune small models on different synthetic/real mixtures and estimate the causal effect of mixture ratio on accuracy, calibration, and tail diversity.

### Watermarking and Detection

Question:

**Can we detect or verify model-generated text at a useful false-positive rate?**

Statistical objects:

- hypothesis testing;
- type I / type II error;
- ROC curves;
- robustness under paraphrase;
- adversarial distribution shift;
- deployment base rates.

Minimal project:

Evaluate a detector under clean, paraphrased, edited, and translated text. Report false positives by domain, not only average detection accuracy.

## 6. What Statistics Adds That Generic ML Often Misses

Statistics adds discipline around:

- **estimands:** what quantity is being estimated;
- **identification:** whether the data can answer the question;
- **uncertainty:** how much evidence supports the claim;
- **bias:** what systematic error may be present;
- **sampling:** what population the result applies to;
- **measurement:** whether labels, judges, teachers, or evals are reliable;
- **validity:** whether the metric measures the intended construct;
- **causality:** whether an intervention caused the observed change;
- **tail risk:** whether rare failures are hidden by averages.

For frontier AI, this matters because many claims are overconfident:

- "the model is safer";
- "the reward model aligns with humans";
- "the teacher transferred reasoning";
- "the benchmark improved";
- "the detector works";
- "the synthetic data helps";
- "the small model inherited the large model's capability".

A statistician asks: under which distribution, with what uncertainty, compared to what baseline, and what would falsify the claim?

## 7. A First Research Sprint That Is Actually Doable

Strong first sprint:

**Uncertainty-Aware Knowledge Transfer from a Large Teacher to a Small Student**

Problem statement:

**Can uncertainty-weighted teacher supervision improve a small LLM's answerability and calibration better than naive output-only distillation?**

Setup:

- teacher: larger model or API model;
- student: small open-weight model;
- task: answerable, unanswerable, false-premise, and ambiguous QA;
- teacher signals: answer, confidence proxy, multiple samples, rationale, optional hidden states;
- baselines: student zero-shot, naive distillation, rationale distillation;
- statistical method: teacher reliability scoring plus uncertainty-weighted transfer;
- evaluation: accuracy, calibration, selective risk, inherited-error rate, tail/OOD slices.

Why this is good:

- it is small enough to run;
- it uses statistics directly;
- it connects to frontier-lab needs;
- it produces a reusable eval harness;
- it can later expand into representation transport, preference transfer, or safety-policy transfer.

## 8. Research Questions Worth Keeping

Good questions:

- When is a teacher model a reliable source of supervision?
- Can teacher uncertainty be transferred rather than collapsed?
- Which divergence objective best transfers LLM behavior: forward KL, reverse KL, JS, ranking loss, or optimal transport?
- Does distillation preserve reasoning fidelity or only final-answer accuracy?
- Can representation geometry transfer better than output imitation?
- Does synthetic teacher data erase rare modes?
- Can conformal or selective prediction create better answer/retrieve/abstain policies?
- How much benchmark movement is real after accounting for prompt and judge variance?
- How do preference models behave when annotators disagree?
- Can statistical tests detect reward hacking or preference collapse?

Weak questions:

- Which model is best?
- Can I improve the benchmark?
- Does distillation work?
- Is the model aligned?
- Is the model confident?

The weak questions can become good questions only after defining an estimand, distribution, baseline, and uncertainty measure.

## 9. Reading Map

Use the existing notes as layers:

- [`01-web-pass-research-map.md`](01-web-pass-research-map.md): broad map of statistics for LLM reliability, evaluation, alignment, and frontier safety.
- [`02-cross-model-knowledge-transfer.md`](02-cross-model-knowledge-transfer.md): teacher-student transfer, distillation, weak supervision, synthetic data, and model collapse.
- [`03-heterogeneous-feature-transport.md`](03-heterogeneous-feature-transport.md): large-to-small transfer across different layers, tokenizers, architectures, and representation spaces.

Read them in this order:

```text
01 for the field
02 for the transfer problem
03 for the feature-transport research angle
04 for the statistical research method
```

## 10. The North Star

The research identity here is not "statistics applied to LLMs" in a generic way.

The sharper identity:

**Build statistical methods that make LLM reliability, alignment, evaluation, and knowledge transfer measurable under uncertainty.**

For knowledge transfer specifically:

**Estimate what a large model knows, decide which parts are reliable, align that signal into a smaller model, and prove the student improved without inheriting hidden teacher failures.**

