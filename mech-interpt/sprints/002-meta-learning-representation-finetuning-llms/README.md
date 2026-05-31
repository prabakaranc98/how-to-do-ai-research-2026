# Representation Engineering, ReFT, and Meta-Learning in Small Language Models

## Sprint 002

### Working project document

This sprint studies whether small language models can be made more reliable by editing, steering, or fine-tuning the internal representations that support uncertainty, abstention, retrieval, truthfulness, and self-correction.

The point is not to fine-tune a model until it sounds better. The point is to ask a mechanistic question:

> Once mechanistic interpretability helps us locate a concept or decision state, can representation engineering or Representation Fine-Tuning (ReFT) edit, steer, or bake that control into the model efficiently?

## Why This Sprint Exists

The mech-interpt sequence now has a clearer arc:

1. **Sprint 000 - Internal world models:** Do small models contain structured representations of places, affordances, memory, and spatial concepts?
2. **Sprint 001 - Reasons-responsive epistemic behavior:** Can small models decide when to answer, abstain, retrieve, clarify, or escalate?
3. **Sprint 002 - Representation engineering / ReFT:** Can we edit, steer, or fine-tune the internal representations that drive that behavior?
4. **Sprint 003 - Cognitive control in language agents:** Can an agent dynamically adjust its own representation spaces through prediction error, memory, and self-monitoring?

Sprint 002 is the intervention bridge. Sprint 000 and 001 mostly ask what is inside the model and how the model behaves. Sprint 002 asks whether we can change the model in a targeted, measurable, interpretable way using ReFT, Representation Engineering (RepE), concept activation vectors, steering vectors, and targeted activation patching.

## Core Thesis

Small language models should not only be fine-tuned on final answers. They should be trained and evaluated around internal decision states: uncertainty, evidence sensitivity, contradiction detection, abstention, truthfulness, retrieval need, and task adaptation.

Mechanistic interpretability gives us candidate locations and directions. Representation engineering asks whether those discovered representations can be steered at inference time or turned into lightweight, persistent interventions.

## First Minimal Question

**Can ReFT-style representation interventions improve a small language model's answer/retrieve/abstain policy under uncertainty better than ordinary instruction fine-tuning or LoRA on prompt-completion pairs?**

This question is small enough to execute, but it teaches the important research skills:

- build a dataset with labels for epistemic actions;
- inspect hidden states before training;
- fine-tune with a simple behavioral baseline;
- test a ReFT / RepE / steering-vector intervention;
- compare behavior, calibration, and internal probes;
- check whether the improvement is real or just polished overconfidence.

## Candidate Methods

Start with a frozen small language model and compare:

1. **Prompt-only baseline:** ask the model to answer, abstain, retrieve, or clarify.
2. **Behavioral fine-tuning baseline:** train LoRA/adapters on labeled prompt-completion or epistemic-action examples.
3. **RepE / steering baseline:** derive concept or behavior directions and inject them at selected layers.
4. **Concept activation vectors:** learn directions for states such as truthfulness, uncertainty, contradiction, and budget constraint.
5. **Targeted activation patching:** test whether replacing or editing selected activations causally changes the decision.
6. **ReFT-style intervention:** train a small intervention on hidden states while keeping most model weights frozen.
7. **Meta-learned decision head:** learn a small policy that maps uncertainty signals to answer/retrieve/abstain actions across tasks.

The first sprint does not need all seven. A good first version compares prompt-only, LoRA/adapters, and one ReFT or RepE intervention.

## Evaluation

Behavioral metrics:

- answer accuracy;
- answered coverage;
- error among answered cases;
- abstention precision;
- retrieval trigger quality;
- false-premise acceptance rate;
- contradiction sensitivity;
- user-pressure or sycophancy rate.

Mechanistic metrics:

- linear probe accuracy for uncertainty, contradiction, and answerability;
- layerwise separability of epistemic states;
- representation shift before vs after fine-tuning;
- causal intervention tests on a learned direction, activation patch, or ReFT intervention;
- permanence check: does the control require runtime steering, or can it be baked into a lightweight intervention?
- retention checks on unrelated capabilities.

Research-craft checks:

- compare against a simple baseline;
- keep a held-out evaluation set;
- include negative and ambiguous examples;
- inspect failures manually;
- report when fine-tuning makes the model more confident but not more correct.

## Planned Artifacts

- `note-1-getting-started.md`: sprint framing and first experiment shape.
- `background/`: focused notes on ReFT, RepE, concept vectors, activation patching, meta-learning, probes, and epistemic evaluation.
- `hypothesis.md`: concrete falsifiable claims before training.
- `data.md`: dataset design, labels, leakage risks, and examples.
- `methods.md`: models, layers, adapters, losses, baselines, and controls.
- `evals.md`: metrics, calibration, failure taxonomy, and decision criteria.
- `research-log.md`: session notes, surprises, decisions, and dead ends.
- `runs/`: configs, checkpoints, outputs, plots, and failed attempts.
- `report.md`: final result, limitations, and next-step recommendation.

## Start Here

Begin with [`note-1-getting-started.md`](note-1-getting-started.md).
