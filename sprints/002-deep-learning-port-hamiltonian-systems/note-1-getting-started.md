# Note 1: Getting Started

**Sprint:** 002 - Deep Learning for Port-Hamiltonian and Compositional Systems  
**Date:** May 2, 2026  
**Status:** Framing note

## 1. Starting Point

This sprint begins from a broad but concrete question:

**How can port-Hamiltonian and compositional system structure help us model, understand, and reason about real-world dynamical systems with deep learning?**

The research arc should be staged:

1. first, learn the method cleanly on physical systems where energy, ports, dissipation, and interconnection are explicit;
2. then, once the modeling discipline is solid, explore biological systems such as metabolism, protein/molecular interaction systems, and cellular regulation where the same language may help but must be used more carefully.

The goal is not just to predict trajectories. The goal is to understand how different modeling choices create different kinds of insight:

- black-box dynamics give flexibility;
- Neural ODEs give continuous-time dynamics;
- Hamiltonian and Lagrangian models give energy-based structure;
- port-Hamiltonian models add inputs, outputs, dissipation, and interconnection;
- PINNs add known equation residuals and constraints;
- graph and compositional models represent interacting subsystems;
- symbolic and neurosymbolic methods can turn learned behavior into human-readable mechanisms.

The central belief is that many real systems are not isolated objects. They are open, coupled, dissipative, controlled, and composed from parts. Port-Hamiltonian systems give a disciplined language for this: stored energy, flow, effort, ports, interconnection, dissipation, and power balance.

## 2. Working Research Direction

Possible title:

**Deep Learning for Port-Hamiltonian and Compositional Systems**

Working thesis:

**Deep learning models for real-world dynamical systems should not only fit trajectories. They should preserve useful structure: energy, dissipation, passivity, ports, constraints, and compositional interconnection.**

This creates a research intersection across:

- **Dynamical Systems:** state-space models, ODEs, stability, control, nonlinearity, chaos, and long-horizon rollout.
- **Port-Hamiltonian Systems:** energy storage, interconnection, dissipation, passivity, inputs, outputs, and open systems.
- **Deep Learning for Scientific ML:** Neural ODEs, PINNs, neural operators, Hamiltonian neural networks, graph neural networks, and structure-preserving models.
- **Compositional Modeling:** subsystems, graphs, ports, modularity, interconnection rules, and reusable learned components.
- **Neurosymbolic Reasoning:** neural function learning inside a symbolic scaffold of equations, constraints, graphs, and conservation laws.
- **Interpretability:** learning models whose pieces mean something: energy, damping, coupling, forcing, control, or transfer between subsystems.

## 3. What We Mean By Port-Hamiltonian Thinking

A port-Hamiltonian system describes dynamics using:

- a state `x`;
- a Hamiltonian `H(x)`, often interpreted as stored energy;
- an interconnection structure `J(x)`, usually skew-symmetric;
- a dissipation structure `R(x)`, usually positive semidefinite;
- input/output ports `u` and `y`;
- a power balance that constrains how energy changes.

A common finite-dimensional form is:

```text
dx/dt = (J(x) - R(x)) grad H(x) + G(x) u
y     = G(x)^T grad H(x)
```

With suitable assumptions:

```text
dH/dt = - grad H(x)^T R(x) grad H(x) + y^T u
```

So the model is not just saying "the next state is this." It is saying:

- what is stored;
- what flows;
- what is dissipated;
- what enters from the environment;
- what exits to the environment;
- how subsystems can be connected without breaking the balance law.

That is the core attraction for real-world systems.

## 4. The Two Big Questions

This sprint should hold two questions together.

### Question 1: Modeling

**How can port-Hamiltonian and compositional systems in the real world be modeled in different ways?**

Candidate systems:

- mass-spring-damper systems;
- driven pendulum or Duffing oscillator;
- RLC circuits;
- coupled oscillators;
- robot joints;
- power electronics or grid-forming inverter toy models;
- thermal or fluid-flow toy systems;
- metabolic reaction networks;
- protein/molecular interaction systems;
- cellular signaling or regulation systems;
- biological or economic analog systems only when the balance quantities are carefully defined.

Important caution:

**Not every event is literally a port-Hamiltonian system.**

But port-Hamiltonian thinking can still be a useful lens when we can define state, stored quantity, flows, dissipation, external ports, and interconnection. If those quantities cannot be defined, the analogy should stay informal.

### Question 2: Reasoning

**How can different modeling techniques help us understand, infer, and reason in a more neurosymbolic way?**

This means:

- learn unknown functions from data;
- keep the symbolic skeleton explicit;
- expose energy, damping, coupling, forcing, and ports as inspectable objects;
- reason over subsystem graphs;
- test whether learned components obey balance laws;
- extract simpler equations or symbolic approximations where possible.

## 5. First Hypotheses

Initial hypotheses:

1. **Inductive bias improves generalization.** A port-Hamiltonian neural model should need less data and extrapolate better than a black-box Neural ODE on open, dissipative, forced systems.
2. **Structure improves physical plausibility.** Models that preserve energy balance, dissipation, and passivity should produce fewer impossible long-horizon rollouts.
3. **Compositional structure improves transfer.** Learning subsystem dynamics with ports should generalize better to new interconnections than learning one monolithic dynamics model.
4. **Interpretability comes from named structure.** A model is more interpretable when learned components can be inspected as energy, damping, coupling, forcing, or port flow.
5. **Neurosymbolic reasoning needs both sides.** Pure symbolic models are brittle when the physics is incomplete; pure neural models are hard to trust. The useful middle is a symbolic system skeleton with learned neural components.

## 6. Modeling Techniques To Compare

The sprint can compare these modeling families:

- **Black-box dynamics model:** learn `dx/dt = f_theta(x, u, t)`.
- **Neural ODE:** learn continuous-time dynamics and train through an ODE solver.
- **Hamiltonian neural network:** learn `H_theta(x)` for closed conservative systems.
- **Lagrangian neural network:** learn dynamics from a learned Lagrangian, often useful when coordinates and velocities are natural.
- **PINN:** enforce known equations, constraints, residuals, initial conditions, and boundary conditions in the loss.
- **Neural operator:** learn maps between function spaces, such as initial condition, forcing, coefficient field, or boundary condition to a solution field.
- **Port-Hamiltonian neural network:** learn `H`, `J`, `R`, `G`, and input/output behavior while preserving structure.
- **Graph or compositional model:** represent parts as nodes/subsystems and ports/interactions as edges.
- **Energy-based model:** learn scalar energies or potentials that shape dynamics, constraints, or distributions.
- **Symbolic regression / sparse discovery:** extract compact equations from learned or observed dynamics.

The first sprint should not implement all of them. It should build the comparison map, then choose a minimal experimental ladder.

## 7. First Minimal Sprint Shape

Keep the first implementation small.

The benchmark strategy should distinguish debugging from relevance:

- use toy physical systems to debug equations, training, and diagnostics;
- use contemporary scientific-ML benchmarks to make the work externally meaningful;
- choose systems where structural metrics reveal something beyond standard prediction error.

Candidate setup:

- one open dissipative system, such as a forced mass-spring-damper or forced Duffing oscillator;
- one compositional extension, such as two coupled oscillators or an RLC circuit network;
- synthetic trajectories generated from known equations;
- noisy and sparse-data variants;
- a small number of models compared fairly.

First model comparison:

1. Black-box Neural ODE.
2. Hamiltonian neural network or SymODEN-style controlled Hamiltonian model.
3. PINN with known residual terms.
4. Neural operator for PDE-style benchmark cases.
5. Port-Hamiltonian neural network.
6. Optional graph/compositional port-Hamiltonian model for the coupled case.

Metrics:

- one-step prediction error;
- long-horizon rollout error;
- energy-balance residual;
- passivity violation rate;
- stability or blow-up rate;
- data efficiency;
- robustness to noise;
- extrapolation to new forcing;
- parameter or component recovery;
- interpretability of learned `H`, `R`, `J`, `G`, and ports.

Later biological extension:

- metabolic reaction network as an open compositional system;
- protein/molecular interaction system as a graph of states, transformations, binding, and energy or free-energy-like quantities;
- cellular signaling system as a regulated input/output network;
- explicit checks for where the port-Hamiltonian analogy is mathematically valid and where it is only a modeling metaphor.

Benchmark ladder:

1. forced mass-spring-damper or Duffing oscillator as a debugging system;
2. PDEBench reaction-diffusion, shallow-water, or wave/advection-diffusion as the first community-facing benchmark;
3. coupled oscillator or RLC network as the clean compositional benchmark;
4. MeshGraphNet-style flow/deformation system as the modern graph-simulation benchmark;
5. small metabolic reaction network as the biological extension.

For the lego-block version of the sprint, the best benchmark path is:

1. custom port-Hamiltonian graph generator for clean subsystem-reuse experiments;
2. Modelica Standard Library examples for real component-based physical systems;
3. power-grid benchmarks such as pandapower/MATPOWER/Grid2Op for community relevance;
4. graph simulator benchmarks such as Interaction Networks, Graph Network-based Simulators, and MeshGraphNets;
5. BioModels/SBML for later biological compositional systems.

## 8. What This Is Not

This sprint is not:

- a vague metaphor that calls every event "energy";
- a full theory of all real-world systems;
- a large physics simulator project from day one;
- a benchmark table without structural analysis;
- a symbolic AI project without numerical experiments;
- a proof that one modeling family is always best.

This sprint is:

- a focused study of structure-preserving deep learning for open systems;
- a way to compare inductive biases;
- a bridge between dynamical systems, scientific ML, control, and neurosymbolic reasoning;
- a practice ground for interpretable modeling of compositional systems.

## 9. Artifacts To Keep In This Sprint Folder

All artifacts for this sprint should stay under:

`sprints/002-deep-learning-port-hamiltonian-systems/`

Planned artifacts:

- `note-1-getting-started.md`: this framing note.
- `background/`: foundations, notation, methods, experiments, and related work.
- `hypothesis.md`: refined research question and hypotheses.
- `systems.md`: candidate systems, equations, simulation setup, and assumptions.
- `methods.md`: model classes, losses, constraints, and training design.
- `evals.md`: metrics, baselines, ablations, and failure taxonomy.
- `research-log.md`: daily notes, decisions, surprises, and dead ends.
- `runs/`: configs, generated trajectories, trained models, logs, and plots.
- `report.md`: sprint writeup and next-step recommendations.

## 10. First Next Step

The next practical step is to narrow the first experiment to one system and one question.

Candidate:

**Can a port-Hamiltonian neural model learn a forced dissipative oscillator more robustly than a black-box Neural ODE, while preserving energy-balance structure and providing interpretable learned components?**

This is small enough to execute, but it still touches the real thesis: structure-preserving learning should improve reliability, interpretability, and compositional reasoning.
