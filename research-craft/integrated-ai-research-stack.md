# Integrated AI Research Stack

This repo should treat RL, post-training, mechanistic interpretability, world models, representation learning, and steering as one connected research program.

They are not separate buzzwords. They are different views of the same problem:

**How do intelligent systems represent the world, choose actions, learn from feedback, and become controllable enough to trust?**

## 1. The Stack

```text
representation learning
    -> world models and latent state spaces
    -> mechanistic interpretability and neural geometry
    -> control, steering, ReFT, and representation engineering
    -> post-training, reward modeling, and RL
    -> agent behavior, planning, and cognitive control
    -> evaluation, causal evidence, and scientific validation
```

## 2. Roles

### Representation Learning

Representation learning is the substrate.

It asks:

- What state variables does the model learn?
- What concepts become linearly or nonlinearly separable?
- What geometry organizes memory, goals, constraints, uncertainty, and environment state?
- Which representations are stable across prompts, tasks, or environments?

Representation learning connects every domain in this repo. Without it, post-training is only behavior shaping, world models are only predictors, and interpretability has no object to inspect.

### World Models

World models are predictive state systems.

They ask:

- What does the model believe will happen next?
- What latent state supports prediction and planning?
- Does the model represent objects, agents, constraints, goals, dynamics, and uncertainty?
- Can the latent state be patched or steered to change rollout behavior?

World models make representation learning operational: a representation matters if it supports prediction, planning, or control.

### Mechanistic Interpretability

Mechanistic interpretability is measurement and causal explanation.

It asks:

- Where is a concept or state represented?
- Which layers, tokens, heads, MLPs, or residual-stream directions matter?
- Is the representation causal or merely correlated?
- Does activation patching, steering, or ablation change behavior as predicted?

Mechanistic interpretability prevents the repo from becoming black-box tuning.

### Control, Steering, and Representation Engineering

Control is intervention discipline.

It asks:

- What internal state do we want?
- How do we move the model toward that state?
- Can we steer the behavior predictably?
- Can a ReFT or RepE intervention make the control persistent?
- What side effects appear under distribution shift?

This is where neural geometry becomes useful for intentional design.

### Post-Training and Reward Modeling

Post-training is behavioral optimization after pretraining.

It asks:

- What reward signal is being optimized?
- Does the reward model prefer genuinely better reasoning or fluent-looking answers?
- Does preference optimization improve reliability or create reward hacking?
- Do hidden states change in the intended direction after SFT, DPO-style training, PPO-style RL, or rejection sampling?

Post-training should be audited mechanistically, not only scored behaviorally.

### Reinforcement Learning

RL is sequential decision optimization.

It asks:

- What is the state, action, reward, policy, and environment?
- What feedback signal drives learning?
- Does the policy learn robust behavior or exploit the reward?
- Can prediction error, value, uncertainty, and goal progress be represented and controlled?

RL connects post-training to agents and world models.

### Agents and Cognitive Control

Agents are closed-loop systems.

They ask:

- Can the system plan, act, observe, and revise?
- Can it detect failure before reward arrives?
- Can it use memory and prediction error to update behavior?
- Can it decide when to answer, retrieve, abstain, clarify, escalate, or call tools?

Agents are where the whole stack becomes visible in behavior.

## 3. Shared Research Loop

Every serious sprint should follow this loop:

```text
1. Define the behavior or decision problem.
2. Identify the relevant representations.
3. Measure the neural geometry.
4. Form a mechanistic hypothesis.
5. Intervene with steering, patching, ReFT, RL, or post-training.
6. Evaluate behavior, representation movement, and side effects.
7. Use evidence to continue, pivot, pause, or stop.
```

## 4. Cross-Domain Map

| Research Area | Primary Role | Typical Artifact |
|---|---|---|
| Representation learning | Learn internal state spaces | probes, embeddings, latent maps, geometry reports |
| World models | Predict and simulate environment state | latent dynamics model, rollout eval, state probe |
| Mechanistic interpretability | Explain and test internal computation | activation patching, circuit notes, SAE features |
| Steering / ReFT / RepE | Intervene on hidden states | steering vectors, ReFT intervention, causal report |
| Post-training | Shape behavior after pretraining | SFT/DPO/RL run, reward-model eval, model card |
| Reward modeling | Define and test optimization signal | preference dataset, reward model, reward-hacking audit |
| RL | Optimize sequential decisions | policy, environment, reward curves, failure analysis |
| Agent control | Close the loop | planner/executor, memory system, self-correction eval |

## 5. What Good Looks Like

A strong project should answer:

- **Behavior:** What does the model or agent do?
- **Representation:** What internal state supports that behavior?
- **Mechanism:** How is the representation transformed into output or action?
- **Optimization:** What training, reward, or feedback signal changes it?
- **Control:** Can we intervene safely and predictably?
- **Evidence:** What would change our mind?

The repo should compound when each sprint leaves behind one reusable object: a dataset, probe, eval, steering method, reward model, environment, or report.

