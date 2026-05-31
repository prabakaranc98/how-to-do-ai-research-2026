# 02 - Methods Map

**Sprint:** 002 - Representation Engineering, ReFT, and Meta-Learning in Small Language Models

This note maps the candidate methods for the sprint. The goal is to choose one small experimental ladder, not to implement every method at once.

## 1. Baseline: Prompt-Only Reliability Policy

The simplest baseline asks the model to choose an epistemic action:

```text
answer / retrieve / abstain / clarify / escalate
```

This checks whether prompting alone can produce reasons-responsive behavior. It is also the easiest baseline to beat accidentally, so the evaluation must include false premises, unanswerable questions, contradictory evidence, and user-pressure examples.

## 2. Behavioral Fine-Tuning Baseline

Train a lightweight LoRA or adapter on prompt-completion examples:

```text
question + evidence -> action + answer
```

This is the ordinary fine-tuning baseline. It tests whether the model can imitate the desired behavior from examples.

Risk:

- the model may learn caution phrases without better evidence sensitivity;
- the model may overfit dataset artifacts;
- the hidden states may not become more interpretable or controllable.

This baseline is necessary because ReFT or RepE should beat something simple.

## 3. Probes

Train small classifiers or regressors on hidden states.

Possible probe targets:

- answerable vs unanswerable;
- supported vs contradicted by evidence;
- true premise vs false premise;
- high confidence vs low confidence;
- retrieve-needed vs internally answerable;
- reasons-responsive action label.

Use probes for measurement first. Do not treat a probe as proof that the model uses the feature. For causal evidence, combine probes with steering, patching, or intervention.

## 4. Concept Activation Vectors

Concept activation vectors represent directions in activation space associated with a concept or behavior.

Possible contrast pairs:

- answerable questions vs unanswerable questions;
- supported evidence vs contradictory evidence;
- truthful answer contexts vs misleading-premise contexts;
- budget-constrained planning vs unconstrained planning;
- epistemically careful completions vs overconfident completions.

The sprint should ask:

**Does moving along this direction change model behavior in a coherent and targeted way?**

## 5. Representation Engineering / RepE Steering

RepE-style steering injects or subtracts learned representation directions during inference.

Useful tests:

- add an uncertainty direction and see whether abstention increases on unanswerable cases;
- add a truthfulness direction and see whether false-premise acceptance falls;
- add a constraint direction and see whether planning respects a stated constraint;
- subtract an overconfidence direction and see whether unsupported answers decrease.

Important control:

The direction should not simply make the model refuse everything. Track answered coverage and accuracy among answered cases.

## 6. Targeted Activation Patching

Activation patching tests whether a hidden state is causally involved.

Basic setup:

1. Run a clean prompt where the model behaves correctly.
2. Run a corrupted prompt where the model fails.
3. Patch selected activations from clean into corrupted.
4. Measure whether the decision or answer recovers.

For this sprint, patching can test:

- whether contradiction evidence is represented in a specific layer;
- whether answerability affects the abstain decision;
- whether a constraint representation changes a planning answer;
- whether user-pressure susceptibility appears in specific layers or token positions.

## 7. ReFT-Style Intervention

ReFT trains a compact intervention on hidden states while leaving most model weights frozen.

The sprint version can be simple:

```text
hidden state at selected layer/token
    -> low-rank or small learned transformation
    -> model continues generation
```

Compare against:

- prompt-only;
- LoRA/adapters;
- runtime steering with a fixed vector.

A useful ReFT result would show:

- fewer trainable parameters than LoRA;
- better abstention or retrieval decisions;
- improved calibration;
- retained general QA ability;
- hidden-state movement in the predicted direction.

## 8. Meta-Learned Decision Policy

Meta-learning can be a small policy over signals, not a large training framework.

Inputs:

- probe scores;
- sampling consistency;
- retrieval agreement;
- task family;
- model confidence or log-prob features;
- evidence contradiction score.

Output:

```text
answer / retrieve / abstain / clarify / escalate
```

The held-out test is whether this policy generalizes across task families, not only across examples from the same distribution.

## 9. Recommended First Ladder

Start here:

1. Build a small dataset with answerable, unanswerable, false-premise, and contradictory-evidence examples.
2. Run prompt-only action selection.
3. Train one LoRA or adapter baseline.
4. Extract hidden states and train answerability / contradiction probes.
5. Learn one concept direction.
6. Test runtime RepE steering or targeted activation patching.
7. Train one small ReFT-style intervention if the direction has a real causal effect.
8. Compare behavior, calibration, retention, and representation movement.

The sprint succeeds if it teaches whether representation-level control is more than a cosmetic behavior change.
