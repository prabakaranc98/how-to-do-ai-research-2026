# Note 1: Getting Started

**Sprint:** 002 - Representation Engineering, ReFT, and Meta-Learning in Small Language Models  
**Date:** May 12, 2026  
**Status:** Reframed sprint direction

## 1. Starting Point

This sprint begins from a practical research question:

**Can we improve a small language model's epistemic behavior by editing, steering, or fine-tuning the representations and decision policies that sit between hidden states and final answers?**

Sprint 001 asks whether small language models can behave reliably under uncertainty. Sprint 002 asks what to do next if the answer is "not reliably enough."

The target is not generic helpfulness. The target is reasons-responsive behavior:

- answer when evidence is sufficient;
- abstain when the question is not answerable;
- retrieve when internal knowledge is weak;
- ask for clarification when the user underspecifies the task;
- resist false premises and user pressure;
- update when evidence contradicts the first answer;
- preserve calibration instead of merely sounding confident.

## 2. Working Research Direction

Possible title:

**Representation Engineering and ReFT for Reasons-Responsive Small Language Models**

Working thesis:

**Small language models can be made more reliable by locating, steering, and fine-tuning internal representations of uncertainty, answerability, contradiction, truthfulness, and evidence sensitivity, not only by fine-tuning final output text.**

This creates an intersection across:

- **Mechanistic interpretability:** probes, layerwise representations, circuits, attention flow, activation directions, causal interventions, and representation geometry.
- **Epistemology:** reasons-responsiveness, evidence sensitivity, answerability, defeaters, testimony, uncertainty, and justified refusal.
- **Representation engineering:** RepE, concept activation vectors, steering vectors, contrastive directions, and targeted activation patching.
- **Representation fine-tuning:** ReFT-style hidden-state interventions, supervised fine-tuning, LoRA/adapters, preference data, and retention checks.
- **Meta-learning:** task adaptation, few-shot reliability, learned decision policies, and uncertainty-conditioned routing.
- **Evaluation:** calibration, selective prediction, abstention, retrieval triggering, sycophancy, and false-premise resistance.

## 3. Why This Belongs Between Sprint 001 and Sprint 003

The sequence should teach one research move at a time.

Sprint 000 asks:

**Do small models contain structured internal world representations?**

Sprint 001 asks:

**Can small models behave well when their knowledge or evidence is limited?**

Sprint 002 asks:

**Can we edit, steer, or fine-tune internal representations to improve that behavior?**

Sprint 003 asks:

**Can an agent use prediction error, memory, and cognitive control to revise behavior over time?**

So the four-sprint arc becomes:

```text
find representations -> understand epistemic behavior -> edit/steer representations -> agentic control
```

That is a stronger "how to do AI research in 2026" sequence than mixing in a separate SciML dynamical-systems sprint at slot 002.

## 4. What We Mean By Representation Engineering / ReFT

Ordinary fine-tuning changes model behavior by training on input/output examples. That can help, but it can also hide the mechanism. The model may learn to produce better-looking refusals or uncertainty phrases without becoming more evidence-sensitive.

Representation Engineering (RepE) and Representation Fine-Tuning (ReFT) make the hidden state the object of intervention:

- Which layers separate answerable from unanswerable questions?
- Is contradiction represented before the final answer?
- Can a linear probe predict whether the model should abstain?
- Does fine-tuning move examples in representation space in the intended direction?
- Can a concept activation vector or steering vector shift behavior in a targeted way?
- Can targeted activation patching prove that a representation is causally involved?
- Can a small ReFT intervention bake useful control into the model without broadly damaging it?

For this sprint, representation engineering can mean several concrete implementation styles:

1. **Adapter/LoRA with mechanistic auditing:** train a lightweight adapter, then inspect hidden-state changes.
2. **Probe-guided training:** train an auxiliary head for answerability or uncertainty and use it to guide routing.
3. **Concept activation vectors:** learn directions for concepts such as truthfulness, uncertainty, contradiction, or budget constraint.
4. **RepE / steering vectors:** inject or subtract behavior directions at selected layers during inference.
5. **Targeted activation patching:** patch activations from clean/corrupted examples to test causal effect.
6. **ReFT-style intervention:** train a low-rank or localized hidden-state intervention while leaving most model weights frozen.

The first implementation should choose one ReFT or RepE method, not all of them.

## 5. What We Mean By Meta-Learning

Meta-learning should stay modest in the first version.

Do not start with a large MAML-style training system. Start with a small learned decision policy that generalizes across task types.

Example:

```text
uncertainty signals + task metadata + hidden-state probe scores
    -> answer / retrieve / abstain / clarify / escalate
```

The meta-learning question is:

**Can the system learn when to use each epistemic action across related tasks, instead of hard-coding one threshold for everything?**

This can be studied with:

- several small QA/task families;
- held-out task families;
- calibration splits;
- few-shot adaptation;
- a small policy head or classifier;
- simple baselines with fixed thresholds.

## 6. First Hypotheses

Initial hypotheses:

1. **Self-rated confidence is weak.** Direct confidence reports will be less reliable than hidden-state or sampling-based signals.
2. **Answerability is linearly visible.** Some layers will contain separable information about whether a question is answerable.
3. **Runtime steering can change policy.** A concept or behavior direction should shift answer/retrieve/abstain behavior in predictable ways.
4. **ReFT-style interventions can be more efficient than LoRA.** A localized hidden-state intervention should improve calibration and abstention quality with fewer trainable parameters than output-only fine-tuning.
5. **Fine-tuning can create fake reliability.** Some interventions will make the model sound more cautious while not reducing error among answered cases.

## 7. First Minimal Sprint Shape

Keep the first experiment small.

Candidate setup:

- one small open language model;
- one small benchmark made from answerable, unanswerable, false-premise, and contradictory-evidence examples;
- one prompt-only baseline;
- one LoRA or adapter behavioral fine-tuning baseline;
- one ReFT / RepE / concept-vector intervention;
- one held-out evaluation split.

Conditions to compare:

1. Prompt-only direct answer.
2. Prompt-only answer/retrieve/abstain instruction.
3. Supervised fine-tuning or LoRA on epistemic-action labels.
4. Concept-vector or RepE steering at selected layers.
5. ReFT-style hidden-state intervention.
6. Probe-guided or meta-learned decision policy using hidden states.

Metrics:

- accuracy;
- answered coverage;
- error among answered cases;
- abstention precision;
- calibration;
- false-premise acceptance;
- contradiction sensitivity;
- retrieval trigger quality;
- sycophancy or user-pressure acceptance;
- retention on a small general QA/control set.

## 8. Mechanistic Analysis To Require

The sprint should not stop at a benchmark table.

At minimum:

- extract hidden states for each condition;
- train a probe for answerability or contradiction;
- report layerwise probe performance;
- visualize representation clusters before and after steering or fine-tuning;
- test at least one concept activation vector or contrastive representation direction;
- run one targeted activation patching or steering experiment;
- inspect examples where the behavior improved but the representation did not;
- inspect examples where the representation moved but behavior did not improve.

The strongest version includes one causal test:

**If we modify the representation along an uncertainty, truthfulness, budget-constraint, or answerability direction, does the model change its decision in a coherent way?**

## 9. What This Is Not

This sprint is not:

- generic instruction tuning;
- LoRA on prompt-completion pairs with no representation audit;
- a leaderboard chase;
- a prompt-engineering exercise;
- a claim that hidden states are human beliefs;
- a full RLHF or preference-optimization project;
- a large-scale post-training system.

This sprint is:

- a focused bridge between mechanistic interpretability, representation engineering, and fine-tuning;
- a way to learn ReFT, RepE, concept vectors, activation patching, probes, calibration, and intervention discipline;
- a test of whether small models can be trained to become more evidence-sensitive;
- a public artifact for showing how to turn a philosophical reliability question into experiments.

## 10. Artifacts To Keep In This Sprint Folder

All artifacts for this sprint should stay under:

`mech-interpt/sprints/002-meta-learning-representation-finetuning-llms/`

Planned artifacts:

- `note-1-getting-started.md`: this framing note.
- `background/`: focused notes on ReFT, RepE, concept vectors, activation patching, meta-learning, probes, and epistemic evaluation.
- `hypothesis.md`: refined research question and falsifiable predictions.
- `data.md`: dataset design, labeling policy, leakage risks, and examples.
- `methods.md`: base model, adapters/interventions, probes, losses, and baselines.
- `evals.md`: metrics, calibration, failure taxonomy, and decision criteria.
- `research-log.md`: daily notes, decisions, surprises, and dead ends.
- `runs/`: configs, outputs, checkpoints, plots, and failed attempts.
- `report.md`: sprint writeup and next-step recommendations.

## 11. First Next Step

The next note should narrow this to one concrete experiment.

Candidate:

**Can a ReFT or RepE intervention outperform prompt-only and LoRA baselines on answerable, unanswerable, false-premise, and contradictory-evidence questions?**

This is small enough to run, but it still tests the central thesis: reliable behavior should be connected to measurable internal representations, not only to better-sounding text.
