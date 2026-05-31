# Note 1: Getting Started

**Sprint:** World-Models 001 - Mechanistic Interpretability of World Models  
**Date:** May 12, 2026  
**Status:** Initial framing

## 1. Starting Point

This sprint begins from a direct question:

**Can we inspect and control the latent state space of a learned world model?**

A world model should represent enough about an environment to support prediction or planning. But prediction accuracy alone does not tell us whether the model has a useful internal map, whether it tracks the right variables, or whether its representations can be trusted under intervention.

## 2. Working Research Direction

Possible title:

**Mechanistic Interpretability of Learned World Models**

Working thesis:

**A useful world model should expose internal representations of state, dynamics, uncertainty, and goal progress that can be measured and causally steered.**

This creates an intersection across:

- **World models:** latent dynamics, simulation, rollout prediction, planning, and environment modeling.
- **Mechanistic interpretability:** probes, representation geometry, activation patching, steering, and circuit hypotheses.
- **Control:** state estimation, intervention, stability, trajectory correction, and closed-loop planning.
- **Evaluation:** prediction error, planning success, counterfactual validity, and intervention side effects.

## 3. First Minimal Experiment

Start with a known-state toy environment.

Candidate setup:

- train a small latent dynamics model on gridworld or toy 2D trajectories;
- keep ground truth factors such as position, velocity, object identity, goal direction, obstacle state, and reward;
- extract latent states during rollouts;
- train probes for ground truth factors;
- visualize latent trajectories;
- patch or steer one factor and observe rollout effects.

First question:

**Can a simple probe recover state variables from the model's latent space, and does steering one recovered direction causally alter future rollout behavior?**

## 4. Metrics

Behavioral metrics:

- next-state prediction error;
- long-horizon rollout error;
- planning success;
- goal-reaching rate;
- failure under counterfactual changes.

Representational metrics:

- probe accuracy for state variables;
- layerwise or timestep-wise decodability;
- latent trajectory clustering;
- geometry of goal, obstacle, or uncertainty states.

Mechanistic/control metrics:

- activation patch recovery rate;
- steering effect size;
- off-target behavior changes;
- rollout stability after intervention;
- whether intervention improves planning or merely distorts prediction.

## 5. What This Is Not

This sprint is not:

- a benchmark chase for video prediction;
- a vague claim that every latent model has a human-readable world model;
- a full robotics project;
- a large-scale foundation world-model training run.

This sprint is:

- a focused interpretability study of learned environment representations;
- a way to connect neural geometry to model-based control;
- a small reproducible bridge from toy world models to larger agent systems.

## 6. First Next Step

Choose one environment and one model.

Recommended first version:

**MiniGrid or a custom gridworld with a small latent dynamics model, state probes, one activation patching experiment, and one steering experiment.**
