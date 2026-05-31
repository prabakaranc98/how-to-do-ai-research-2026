# Marr, Control, and Neural Geometry Framework

This is the shared research frame for the mechanistic-interpretability sprint sequence.

The goal is to treat language models less like prompt alchemy and more like engineered, steerable systems. The working synthesis:

**David Marr gives the levels. Neural geometry gives the state-space map. Control gives the intervention discipline.**

## 1. The Three Levels

### Behavioral Level: What The System Does

Question:

**What goals can the model or agent achieve, and where are its empirical bounds?**

This level studies observable trajectories:

- task success and failure;
- answer quality;
- refusal, abstention, retrieval, or clarification behavior;
- planning under constraints;
- sensitivity to evidence, memory, or user pressure;
- robustness under distribution shift;
- stochastic variation across samples.

Artifacts:

- benchmark prompts;
- behavioral evals;
- failure taxonomies;
- calibration plots;
- trajectories and transcripts;
- before/after intervention comparisons.

This level is necessary but insufficient. Behavior alone can make a weak intervention look strong if the model merely changes style.

### Representational Level: What State Space The System Uses

Question:

**What concepts, constraints, memories, and decision states are represented inside the model, and what shape do they have?**

This level studies neural geometry:

- hidden-state clusters;
- manifolds and subspaces;
- concept directions;
- sparse features;
- layerwise separability;
- token-position effects;
- representation shifts under context, memory, or evidence;
- distances, directions, and curvature in activation space.

Artifacts:

- probes;
- PCA/UMAP/t-SNE plots;
- concept activation vectors;
- representation-difference analyses;
- sparse autoencoder feature maps;
- NLA-style activation interpretations treated as hypotheses;
- layerwise representation reports.

This is the level of intentional design. Instead of prompting blindly, we map where concepts like `budget`, `downtown`, `truthfulness`, `uncertainty`, `contradiction`, or `goal progress` appear.

### Computational / Mechanistic Level: How The System Computes

Question:

**What mechanisms transform representations into behavior, and can we causally intervene on them?**

This level studies computation and control:

- attention heads;
- MLP features;
- residual-stream directions;
- circuits;
- activation patching;
- causal tracing;
- steering vectors;
- ReFT-style interventions;
- policy heads and routers;
- feedback loops in agents.

Artifacts:

- causal intervention reports;
- activation patching experiments;
- circuit hypotheses;
- steering studies;
- ReFT/RepE intervention runs;
- ablations;
- closed-loop control experiments.

This is the level of problem solving. A claim becomes stronger when changing the proposed mechanism changes behavior in the predicted direction.

## 2. Control View

Treat the model as a high-dimensional dynamical system:

```text
input/context -> internal state -> computation -> output/action
```

Control asks:

- What state should the model be in?
- What state is it currently in?
- What intervention moves it toward the desired state?
- What side effects does that intervention create?
- Does the system stay controlled under new tasks, contexts, or perturbations?

Possible control actions:

- prompt changes;
- retrieval or clarification;
- steering-vector injection;
- activation patching;
- ReFT-style hidden-state intervention;
- LoRA/adapters;
- agent memory update;
- policy routing;
- self-correction triggered by prediction error.

The key standard:

**A good intervention should be targeted, causal, measurable, and checked for side effects.**

## 3. Research Loop

Use this loop for every sprint:

```text
1. Observe behavior.
2. Map representations.
3. Form a mechanism hypothesis.
4. Intervene on the state or mechanism.
5. Measure behavioral change.
6. Check representation movement.
7. Audit side effects.
8. Decide whether to continue, pivot, pause, or stop.
```

The loop is deliberately recursive. A failed steering result may reveal a better representational hypothesis. A surprising representation cluster may suggest a new behavioral eval. A control failure may expose the wrong mechanistic story.

## 4. Sprint Mapping

### Sprint 000: Internal World Models

Main question:

**Can we find the cognitive map?**

Marr mapping:

- Behavioral: NYC planning, route coherence, affordance-aware choices, memory-conditioned lifestyle simulation.
- Representational: spatial directions, neighborhood clusters, affordance manifolds, persona-conditioned shifts.
- Mechanistic/control: causal steering of concepts like `uptown`, `quiet`, `budget`, `walkable`, or `touristy`.

### Sprint 001: Epistemology And Decision Making

Main question:

**Can we understand how the model uses representations to reason under uncertainty?**

Marr mapping:

- Behavioral: answer/retrieve/abstain/clarify/escalate decisions.
- Representational: uncertainty, answerability, contradiction, evidence support, false-premise status.
- Mechanistic/control: attention flow, circuit hypotheses, activation patching, and interventions that alter epistemic decisions.

### Sprint 002: Representation Engineering / ReFT

Main question:

**Can we edit and steer internal representations efficiently?**

Marr mapping:

- Behavioral: improved reliability, truthfulness, abstention, retrieval triggering, and constraint-following.
- Representational: concept activation vectors, contrastive directions, steering directions, hidden-state movement after intervention.
- Mechanistic/control: RepE, ReFT, targeted activation patching, low-rank interventions, and policy heads.

### Sprint 003: Cognitive Control And Predictive Coding

Main question:

**Can the model dynamically adjust its own representation spaces during complex tasks?**

Marr mapping:

- Behavioral: agent success, recovery from failure, planning quality, self-correction.
- Representational: prediction error, goal progress, memory relevance, failure-risk state.
- Mechanistic/control: feedback loops, memory updates, prediction-error triggers, policy revision, and closed-loop steering.

## 5. What Good Looks Like

A strong sprint should make claims at all three levels:

- **Behavioral claim:** the model or agent does something measurably better.
- **Representational claim:** a relevant internal state, concept, or geometry is identified.
- **Mechanistic/control claim:** intervening on the proposed state or mechanism changes behavior predictably.

If a sprint only has behavior, it is an eval. If it only has geometry, it is descriptive analysis. If it only has steering, it may be a trick. The research contribution appears when all three levels reinforce each other.

