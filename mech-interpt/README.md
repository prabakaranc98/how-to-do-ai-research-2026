# Mechanistic Interpretability and Small-Model Research

This domain contains research sprints about small language models, mechanistic interpretability, internal world models, epistemic behavior, representation geometry, representation engineering, representation fine-tuning, and language-agent control.

The shared operating frame is [`Marr, Control, and Neural Geometry`](framework-marr-control-neural-geometry.md): use behavior to discover empirical bounds, neural geometry to map internal state spaces, and control methods to steer or fine-tune mechanisms.

This domain is one part of the broader [`Integrated AI Research Stack`](../research-craft/integrated-ai-research-stack.md), where representation learning, world models, mechanistic interpretability, steering, post-training, reward modeling, RL, and agents are treated as one cohesive research program.

## Sprint Index

- [`000-bigcity-small-world-nyc-gemma/`](sprints/000-bigcity-small-world-nyc-gemma/): mechanistic interpretability plus internal world models. Can we find the cognitive map through probing, geometry, NLA-style interpretation, and causal checks?
- [`001-epistemic-reliability-small-language-models/`](sprints/001-epistemic-reliability-small-language-models/): mechanistic interpretability plus reasons-responsive decision making. Can we understand how representations support uncertainty, evidence use, attention, circuits, and logical flow?
- [`002-meta-learning-representation-finetuning-llms/`](sprints/002-meta-learning-representation-finetuning-llms/): mechanistic interpretability plus representation engineering / ReFT. Can we edit, steer, and fine-tune internal representations with ReFT, RepE, concept activation vectors, and targeted activation patching?
- [`003-predictive-coding-small-language-model-agents/`](sprints/003-predictive-coding-small-language-model-agents/): mechanistic interpretability plus cognitive control. Can a language agent dynamically adjust representation spaces using prediction error, memory, and self-correction?

## Sprint Arc

```text
000: find internal world-model representations
001: measure epistemic and reasons-responsive behavior
002: edit, steer, and fine-tune representations with ReFT / RepE
003: build agentic cognitive control from prediction error and memory
```

## Cross-Domain Initiatives

- [`Mechanistic Interpretability of World Models`](../world-models/sprints/001-mechanistic-interpretability-world-models/): applies the Marr/control/neural-geometry frame to latent states, predictive representations, simulators, and model-based agents.
- [`Post-Training and RL`](../post-training-and-rl/): studies reward models, preference data, RL-style optimization, process supervision, and reward hacking with mechanistic audits.

## Domain Standard

Each substantial sprint should keep its artifacts inside its own sprint folder:

- `hypothesis.md`: the research question and falsifiable predictions.
- `data.md`: data sources, licensing, preprocessing, leakage risks, and known biases.
- `methods.md`: models, baselines, ablations, and implementation choices.
- `evals.md`: metrics, statistical checks, failure modes, and decision criteria.
- `research-log.md`: session notes, surprises, decisions, and dead ends.
- `runs/`: raw configs, outputs, logs, plots, and failed attempts.
- `report.md`: final technical note, limitations, and next-step recommendation.

Use the shared [`Research Sprint Review Checklist`](../research-craft/review-checklist.md) at the end of each sprint.

Use the shared [`Research Sprint Template`](../research-craft/sprint-template.md) when starting a new sprint.
