# Note 1: Getting Started

**Sprint:** 003 - Predictive Coding, Natural Intelligence, and Small Language-Model Agents  
**Date:** May 3, 2026  
**Status:** Initial direction

## 1. Starting Point

This sprint is not yet a committed experiment. It is an initial research direction to keep alive while Sprint 001 remains active.

The broad interest:

**Can predictive-coding ideas help small language-model agents learn, plan, and correct themselves more efficiently?**

This sits at the intersection of:

- predictive coding and predictive processing;
- reinforcement learning;
- small language models;
- language-grounded agents;
- surprise, prediction error, memory, and self-correction;
- natural intelligence as a source of useful hypotheses, not loose analogy.

## 2. Working Research Direction

Possible title:

**Predictive Coding for Small Language-Model Agents**

Working thesis:

**Small language-model agents may become more reliable and sample-efficient if they learn not only which action to take, but also what future observations, outcomes, failures, or goal progress to expect.**

The useful predictive-coding pattern:

1. predict what should happen next;
2. act;
3. observe what actually happened;
4. compute prediction error;
5. use that error to update behavior, memory, planning, uncertainty, or exploration.

This keeps the idea concrete. We do not need to claim that transformers are brains. We can ask whether prediction-error machinery improves agent behavior.

## 3. Why Smaller Language Models

Small language models are a better first target than frontier-scale models because they are:

- cheaper to run;
- easier to fine-tune;
- easier to inspect;
- more likely to expose failure modes;
- practical for local agents, routing systems, and lightweight decision policies.

They also have a clear weakness:

**Small models often lack robust world knowledge, planning depth, and self-correction.**

That makes predictive coding interesting. A small model may not know everything, but it might become more useful if it can predict, check, and revise.

## 4. Why Reinforcement Learning

RL gives the agent setting:

- observations;
- actions;
- goals;
- rewards;
- failures;
- exploration;
- delayed consequences.

Predictive coding gives a possible learning signal beyond sparse reward:

- predict next observation;
- predict goal progress;
- predict reward or failure;
- predict whether an action will be valid;
- predict whether a plan will need correction.

This may be useful when rewards are sparse, delayed, or noisy.

## 5. Possible First Experimental Shape

This should stay tentative for now.

A later concrete experiment could use a small transformer or small language model in a text-based RL environment.

Candidate environments:

- TextWorld;
- BabyAI / MiniGrid;
- ALFWorld as a later-stage environment.

At each step, the model receives:

```text
history + current observation + goal
```

It predicts:

```text
action
next observation
goal progress
reward or failure risk
```

Then the environment returns:

```text
actual next observation
actual reward
done / not done
```

The predictive-coding signal:

```text
prediction error = actual outcome - predicted outcome
```

Possible uses:

- auxiliary training loss;
- prioritized replay;
- exploration bonus;
- uncertainty estimate;
- self-correction trigger;
- memory update signal.

## 6. Candidate Questions

Keep these as possible directions, not commitments:

- Does future-observation prediction improve small language-model agents in sparse-reward text environments?
- Does prediction error help decide when the agent should revise its plan?
- Can surprise predict failure earlier than reward?
- Does auxiliary prediction improve sample efficiency?
- Can a small model learn better state representations by predicting future observations?
- Does prediction error improve memory selection or replay?
- Can this connect to epistemic reliability from Sprint 001?

## 7. What This Is Not

This sprint is not:

- a claim that language models are brains;
- a broad consciousness project;
- a vague neuroscience metaphor;
- a full RLHF/post-training project;
- a frontier-scale training project.

This sprint is:

- an initial direction at the intersection of predictive coding, RL, and small language-model agents;
- a possible future experiment about prediction error, planning, and self-correction;
- a bridge between natural intelligence ideas and practical agent learning.

## 8. Status

For now:

- Sprint 001 remains active.
- Sprint 002 is to be started later.
- Sprint 003 is an exploratory direction, not yet narrowed.

The next step, when this becomes active, is to choose one small environment and one measurable question.
