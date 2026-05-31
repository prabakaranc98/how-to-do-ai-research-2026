# Mechanistic Interpretability of World Models

## World-Models Sprint 001

### Working project document

This sprint studies whether learned world models contain internal state spaces that can be inspected, mapped, and causally steered.

The goal is not only to ask whether a model predicts the next observation. The goal is to ask:

> What latent variables, object states, spatial maps, temporal dynamics, goals, and controllable factors does the model use to predict and plan?

## Why This Sprint Exists

World models are central to agents, robotics, video prediction, planning, simulation, and embodied AI. But a model that predicts well may still be hard to trust or control.

Mechanistic interpretability gives this domain a sharper research surface:

- locate latent state variables;
- test whether concepts are linearly or nonlinearly represented;
- map neural geometry of trajectories and environments;
- patch or steer internal states;
- connect representation changes to prediction and planning behavior.

## Marr-Level Framing

### Behavioral Level

What the system does:

- predicts future observations;
- rolls out latent trajectories;
- plans actions in an environment;
- adapts to changed goals or constraints;
- fails under distribution shift or counterfactual changes.

### Representational Level

What the system represents:

- object identity and position;
- velocity, force, contact, affordance, or obstacle state;
- map-like spatial structure;
- latent dynamics;
- goal progress;
- uncertainty and prediction error.

### Mechanistic / Control Level

How the system computes and how we intervene:

- probe latent states;
- patch clean/corrupted trajectories;
- steer directions such as `near obstacle`, `goal closer`, `higher uncertainty`, or `moving left`;
- identify circuits or modules that update state;
- test whether intervention changes rollout or planning behavior predictably.

## First Minimal Question

**Can we identify and causally steer a small learned world model's latent representation of environment state?**

A first version can use a simple environment:

- gridworld;
- MiniGrid;
- toy 2D physics;
- simple video prediction;
- latent dynamics model trained on synthetic trajectories.

The sprint should start small enough that ground truth state is known.

## Candidate Methods

- train or use a small world model with latent states;
- collect trajectories with ground truth factors;
- train probes for position, velocity, object identity, goal direction, or obstacle state;
- visualize latent trajectories with PCA/UMAP;
- patch activations between clean and corrupted rollouts;
- steer a latent direction and measure rollout change;
- evaluate whether intervention improves or breaks planning.

## Planned Artifacts

- `note-1-getting-started.md`: framing and first experiment shape.
- `background/`: focused notes on world models, latent dynamics, probing, geometry, and steering.
- `hypothesis.md`: falsifiable claims before training or probing.
- `data.md`: environment, trajectory generation, state labels, and splits.
- `methods.md`: model, probes, steering, patching, baselines, and controls.
- `evals.md`: prediction, planning, representation, and intervention metrics.
- `research-log.md`: session notes, surprises, failures, and next experiments.
- `runs/`: configs, rollouts, plots, checkpoints, and failed attempts.
- `report.md`: results, limitations, and next-step recommendation.

## Start Here

Begin with [`note-1-getting-started.md`](note-1-getting-started.md).
