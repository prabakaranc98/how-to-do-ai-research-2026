# World Models

This domain is for work on simulation, spatial intelligence, embodied prediction, model-based planning, synthetic worlds, compositional dynamical systems, mechanistic interpretability of world models, and agent-environment dynamics.

Keep future sprints under `sprints/`. Domain-specific datasets, environments, and notes should stay inside this folder when they are created.

This domain should stay connected to the shared [`Integrated AI Research Stack`](../research-craft/integrated-ai-research-stack.md): world models turn representation learning into predictive state, mechanistic interpretability maps that state, and RL/control methods use it for planning and action.

## Initiatives

### Mechanistic Interpretability of World Models

Study whether models that predict environments, trajectories, spatial structure, or latent dynamics contain inspectable internal world models.

The core question:

**Can we locate, represent, and causally steer the state variables a world model uses for prediction, planning, and control?**

This connects the shared [`Marr, Control, and Neural Geometry`](../mech-interpt/framework-marr-control-neural-geometry.md) frame to simulation, agents, spatial intelligence, and dynamical systems.

### Representation Learning for Predictive State

Study the latent spaces that support prediction, rollout, planning, and control.

The core question:

**What representation of state lets a model predict the future and choose useful actions?**

This initiative links world models to RL: a policy can only optimize well if the state representation preserves the variables that matter for reward, uncertainty, and controllability.

### Compositional Dynamical Systems

Study structured systems with states, flows, constraints, ports, and interactions. This includes the preserved Port-Hamiltonian sprint.

## Sprint Index

- [`sprints/000-deep-learning-port-hamiltonian-systems/`](sprints/000-deep-learning-port-hamiltonian-systems/): preserved SciML / compositional-systems sprint originally drafted as mech-interpt Sprint 002.
- [`sprints/001-mechanistic-interpretability-world-models/`](sprints/001-mechanistic-interpretability-world-models/): mechanistic interpretability of learned world models, latent state spaces, predictive representations, and causal steering.
- The NYC spatial-world-model prototype currently lives in [`../mech-interpt/sprints/000-bigcity-small-world-nyc-gemma/`](../mech-interpt/sprints/000-bigcity-small-world-nyc-gemma/).
