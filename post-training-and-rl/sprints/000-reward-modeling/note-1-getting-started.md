# Note 1: Getting Started

**Sprint:** Post-Training-and-RL 000 - Reward Modeling  
**Date:** May 12, 2026  
**Status:** Initial framing

## 1. Starting Point

This sprint begins from a practical question:

**Can we train and evaluate reward models that prefer genuinely better reasoning, not merely more fluent, agreeable, or judge-pleasing answers?**

Post-training is powerful because it shapes model behavior after pretraining. But reward signals are fragile. If the reward model prefers confident style over valid reasoning, the policy will learn the wrong thing.

## 2. Working Research Direction

Possible title:

**Reward Modeling for Reliable Small Language Models**

Working thesis:

**Reward modeling should be evaluated not only by preference accuracy, but by whether it improves truthfulness, reasoning process, uncertainty behavior, and resistance to reward hacking.**

This creates an intersection across:

- **Post-training:** supervised fine-tuning, preference optimization, RL-style policy improvement, and rejection sampling.
- **Reward modeling:** pairwise preference models, process reward models, outcome reward models, and judge models.
- **Cognitive systems:** reasoning, uncertainty, self-correction, and user-pressure resistance.
- **Mechanistic auditing:** hidden-state changes, reward-sensitive directions, sycophancy representations, and steering checks.

## 3. First Minimal Experiment

Start with a small preference dataset.

Include pairs such as:

- correct concise answer vs fluent wrong answer;
- evidence-sensitive answer vs user-pleasing answer;
- calibrated uncertainty vs overconfident answer;
- valid reasoning process vs plausible but invalid chain;
- safe abstention vs unsupported answer.

Train or use:

- one small base/policy model;
- one reward model or preference classifier;
- one simple response-selection loop;
- optional SFT or preference-optimization baseline.

First question:

**Does the reward model prefer epistemically better answers when surface style conflicts with correctness?**

## 4. Metrics

Reward model metrics:

- held-out preference accuracy;
- adversarial preference accuracy;
- calibration;
- sensitivity to style vs correctness;
- process-label agreement.

Policy behavior metrics:

- answer accuracy;
- sycophancy rate;
- false-premise acceptance;
- abstention quality;
- reasoning-process validity;
- reward-model score vs ground truth quality.

Mechanistic metrics:

- hidden-state shifts after post-training;
- probe accuracy for uncertainty or user-pressure sensitivity;
- reward-sensitive representation directions;
- activation or steering checks for reward-hacking behavior.

## 5. What This Is Not

This sprint is not:

- a full RLHF pipeline;
- a leaderboard chase;
- a black-box preference optimization exercise;
- a claim that reward models solve alignment.

This sprint is:

- a small, inspectable post-training laboratory;
- a way to learn reward modeling and RL concepts with scientific controls;
- a bridge from behavior-level preference optimization to representation-level auditing.

## 6. First Next Step

Create `data.md` with the preference-pair taxonomy.

Recommended first experiment:

**Train a small preference classifier on answer pairs where style and correctness conflict, then test whether reward-guided selection improves real answer quality or only reward score.**
