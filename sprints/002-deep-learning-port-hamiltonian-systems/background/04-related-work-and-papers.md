# 04 - Related Work And Papers

**Sprint:** 002 - Deep Learning for Port-Hamiltonian and Compositional Systems  
**Last updated:** May 2, 2026

This file collects papers and references for the sprint.

The purpose is not to read everything first. The purpose is to understand the map well enough to choose a sharp first experiment.

Shorthand used below: `pHNN` means port-Hamiltonian neural network.

## 1. First Reading Order

Start with these:

1. [Neural Ordinary Differential Equations](https://papers.nips.cc/paper/7892-neural-ordinary-differential-equations)
2. [Hamiltonian Neural Networks](https://arxiv.org/abs/1906.01563)
3. [Physics-informed neural networks: A deep learning framework for solving forward and inverse problems involving nonlinear partial differential equations](https://doi.org/10.1016/j.jcp.2018.10.045)
4. [Fourier Neural Operator for Parametric Partial Differential Equations](https://openreview.net/forum?id=c8P9NQVtmnO)
5. [Learning nonlinear operators via DeepONet](https://doi.org/10.1038/s42256-021-00302-5)
6. [Physics-Informed Neural Operator for Learning Partial Differential Equations](https://arxiv.org/abs/2111.03794)
7. [Port-Hamiltonian neural networks for learning explicit time-dependent dynamical systems](https://doi.org/10.1103/PhysRevE.104.034312)
8. [Port-Hamiltonian Systems on Graphs](https://doi.org/10.1137/110840091)
9. [Stable Port-Hamiltonian Neural Networks](https://doi.org/10.48550/arXiv.2502.02480)
10. [Port-metriplectic neural networks: thermodynamics-informed machine learning of complex physical systems](https://doi.org/10.1007/s00466-023-02296-w)

This gives the minimum context for:

- continuous-time neural dynamics;
- energy-preserving neural models;
- physics-informed residual learning;
- neural operators for PDE solution maps;
- open dissipative systems;
- graph and compositional port-Hamiltonian structure;
- stability and thermodynamic constraints.

## 2. Core Port-Hamiltonian Foundations

### Port-Hamiltonian Systems on Graphs

Link: https://doi.org/10.1137/110840091

Why it matters:

- Presents port-Hamiltonian systems as a geometric and compositional framework for network dynamics.
- Uses open graphs and Dirac structures to formalize interconnection.
- Directly supports this sprint's idea that compositional systems can be modeled through ports.

Questions for this sprint:

- Can we represent learned subsystems as graph nodes with ports?
- Can a graph interconnection preserve power balance?
- What is the simplest graph system to implement first?

### Interconnection of Port-Hamiltonian Systems and Composition of Dirac Structures

Link: https://doi.org/10.1016/j.automatica.2006.08.014

Why it matters:

- Explains how interconnecting port-Hamiltonian systems yields another port-Hamiltonian system.
- Gives the theoretical basis for modular composition.
- Important for the neurosymbolic direction: learned components should compose through explicit rules.

Questions for this sprint:

- What interconnection rule can we implement in a simple coupled oscillator?
- How do we test whether composition preserves passivity?

### From Dirac Structures to Port-Hamiltonian Partial Differential Equations, a Tutorial Introduction

Link: https://www.mdpi.com/1099-4300/28/3/292

Why it matters:

- Tutorial-style introduction to Dirac structures and port-Hamiltonian PDEs.
- Useful for building intuition without jumping directly into abstract geometry.
- Helps clarify when the structure gives conservation and when existence/well-posedness still matters.

Questions for this sprint:

- Which finite-dimensional concepts transfer cleanly?
- Which PDE/discretization issues should be postponed?

## 3. Deep Learning For Dynamical Systems

### Neural Ordinary Differential Equations

Link: https://papers.nips.cc/paper/7892-neural-ordinary-differential-equations

Why it matters:

- Core reference for learning continuous-time dynamics with differentiable ODE solvers.
- Useful baseline for all structure-preserving models.
- Gives the training machinery needed for pHNN rollout training.

Questions for this sprint:

- How strong is a black-box Neural ODE on the first toy system?
- Does it fail through energy drift, instability, or poor extrapolation?

### Hamiltonian Neural Networks

Link: https://arxiv.org/abs/1906.01563

Why it matters:

- Foundational neural inductive-bias paper for learning Hamiltonian dynamics.
- Shows why learning a scalar energy can improve generalization.
- Good contrast with port-Hamiltonian systems: closed conservative vs open dissipative.

Questions for this sprint:

- Where does HNN fail on forced or damped dynamics?
- Can we show why pHNN is needed beyond HNN?

### Symplectic ODE-Net: Learning Hamiltonian Dynamics with Control

Link: https://arxiv.org/abs/1909.12077

Why it matters:

- Adds controlled Hamiltonian structure to Neural ODE-style learning.
- Useful bridge between HNNs and port-Hamiltonian models.
- Gives an interpretable path for controlled systems before full dissipation/interconnection.

Questions for this sprint:

- Should SymODEN be a baseline for controlled but low-dissipation cases?
- How does it compare to pHNN when damping is present?

### Lagrangian Neural Networks

Link: https://arxiv.org/abs/2003.04630

Why it matters:

- Learns dynamics through a Lagrangian instead of a Hamiltonian.
- Useful when coordinates and velocities are natural.
- Helps compare physics-inspired inductive biases.

Questions for this sprint:

- Is the first system easier in Hamiltonian or Lagrangian form?
- Do LNNs help with interpretability or add unnecessary complexity for sprint 002?

## 4. Physics-Informed Learning

### Physics-Informed Neural Networks

Link: https://doi.org/10.1016/j.jcp.2018.10.045

Why it matters:

- Standard reference for using equation residuals as training constraints.
- Useful when part of the governing equation is known.
- A good contrast to pHNNs: residual enforcement vs structural parameterization.

Questions for this sprint:

- Should the first PINN baseline use the exact oscillator residual?
- Does residual loss improve prediction but still fail interpretability?

### Quantifying Total Uncertainty in Physics-Informed Neural Networks

Link: https://doi.org/10.1016/j.jcp.2019.07.048

Why it matters:

- Adds uncertainty framing to PINNs for forward and inverse stochastic problems.
- Useful if the sprint later studies noisy trajectories and uncertainty.

Questions for this sprint:

- Do we need uncertainty in phase 1, or should it wait?
- How should model uncertainty interact with physical-structure violations?

## 5. Neural Operators

### Fourier Neural Operator for Parametric Partial Differential Equations

Link: https://openreview.net/forum?id=c8P9NQVtmnO

Why it matters:

- Foundational neural-operator architecture for PDE surrogate modeling.
- Learns mappings between function spaces, such as PDE parameters or initial conditions to solution fields.
- Strong baseline for PDEBench-style tasks.

Questions for this sprint:

- Should FNO be the main baseline on reaction-diffusion or shallow-water?
- Does it get good field MSE while violating energy, conservation, or dissipation diagnostics?
- Can structure-aware losses or parameterizations improve its reliability?

### Learning Nonlinear Operators via DeepONet

Link: https://doi.org/10.1038/s42256-021-00302-5

Why it matters:

- Foundational DeepONet paper for learning nonlinear operators.
- Useful alternative to FNO, especially when the input/output representation is not naturally spectral.
- Gives a broader operator-learning perspective beyond one architecture.

Questions for this sprint:

- Is DeepONet easier to adapt to irregular sensors, sparse observations, or biological systems?
- Should it be a later baseline after FNO?

### Physics-Informed Neural Operator for Learning Partial Differential Equations

Link: https://arxiv.org/abs/2111.03794

Why it matters:

- Combines neural operators with PDE constraints.
- More relevant to this sprint than data-only operator learning because we care about structure, not just prediction.
- Useful bridge between PINNs and neural operators.

Questions for this sprint:

- Can a physics-informed neural operator be extended with energy-balance or port-Hamiltonian residuals?
- Does residual structure improve extrapolation across forcing, boundary conditions, or resolution?

### Learning Physical Operators Using Neural Operators

Link: https://arxiv.org/abs/2602.23113

Why it matters:

- Recent direction that decomposes physical PDEs into modular learned and fixed operators.
- Relevant to compositional modeling and interpretable physical components.
- Connects operator learning to continuous-time neural ODE-style prediction.

Questions for this sprint:

- Is operator splitting a practical bridge between neural operators and compositional port-Hamiltonian thinking?
- Can learned operators be checked against known energy or dissipation behavior?

## 6. Port-Hamiltonian Neural Networks

### Port-Hamiltonian Neural Networks for Learning Explicit Time-Dependent Dynamical Systems

Link: https://doi.org/10.1103/PhysRevE.104.034312

Why it matters:

- Directly embeds port-Hamiltonian formalism into neural networks.
- Targets nonautonomous systems with time-dependent forcing and energy dissipation.
- Shows recovery of Hamiltonian, force, and dissipative coefficient in nonlinear systems.

Questions for this sprint:

- Can we reproduce a simplified version on a forced oscillator?
- Which parameters should be learned first: `H`, `R`, or forcing?

### Port-Hamiltonian Neural Networks With Output Error Noise Models

Link: https://doi.org/10.1016/j.automatica.2026.112892

Why it matters:

- Extends pHNNs toward practical system identification with noisy measurements.
- Integrates port-Hamiltonian theory with output-error noise modeling.
- Important for real-world data beyond clean synthetic derivatives.

Questions for this sprint:

- Should noisy measurement handling be phase 2?
- Can output-error training avoid unreliable numerical derivatives?

### Stable Port-Hamiltonian Neural Networks

Link: https://doi.org/10.48550/arXiv.2502.02480

Why it matters:

- Focuses on global Lyapunov stability in learned port-Hamiltonian dynamics.
- Directly addresses a common failure of neural dynamics: unstable rollouts.
- Useful for building metrics around stability and blow-up.

Questions for this sprint:

- Can we measure stability violations in the baseline?
- Should stability be hard-coded or only evaluated at first?

### Port-Metriplectic Neural Networks

Link: https://doi.org/10.1007/s00466-023-02296-w

Why it matters:

- Extends port-Hamiltonian ideas toward thermodynamic consistency.
- Enforces conservation of energy and non-negative entropy production.
- Supports learning complex systems by parts and composing them through energy ports.

Questions for this sprint:

- Is thermodynamic consistency relevant to our first mechanical/circuit examples?
- Could this become a later sprint on dissipative multi-physics systems?

### Learning Subsystem Dynamics in Nonlinear Systems via Port-Hamiltonian Neural Networks

Link: https://arxiv.org/abs/2411.05730

Why it matters:

- Directly relevant to learning subsystem dynamics.
- Supports the compositional direction where parts can be learned separately.

Questions for this sprint:

- Can we learn one oscillator subsystem and reuse it in a coupled system?
- What data is needed to identify subsystem ports?

### Controlled Oscillation Modeling Using Port-Hamiltonian Neural Networks

Link: https://arxiv.org/abs/2602.15704

Why it matters:

- Recent work on controlled oscillatory systems using pHNNs.
- Close to the proposed first experiment.

Questions for this sprint:

- What oscillator setup do they use?
- Can we design a smaller version for our first implementation?

### Stochastic Port-Hamiltonian Neural Networks: Universal Approximation With Passivity Guarantees

Link: https://arxiv.org/abs/2603.10078

Why it matters:

- Extends pHNNs to stochastic forcing and passivity in expectation.
- Relevant for noisy real systems and uncertainty-aware modeling.

Questions for this sprint:

- Should stochasticity be postponed until the deterministic case is solid?
- What would passivity mean under noisy forcing?

### Structure- And Stability-Preserving Learning Of Port-Hamiltonian Systems

Link: https://arxiv.org/abs/2604.13297

Why it matters:

- Recent work on preserving Hamiltonian structure and stability properties.
- Useful for thinking about expressiveness vs stability constraints.

Questions for this sprint:

- Do stability constraints hurt or help expressiveness on simple systems?
- Can multiple equilibria appear in later examples?

## 7. Graph, Compositional, And Symbolic Directions

### Symplectic Graph Neural Networks

Link: https://doi.org/10.1016/j.neunet.2025.107397

Why it matters:

- Combines symplectic structure with graph neural networks.
- Targets high-dimensional many-body Hamiltonian systems.
- Useful reference for scaling from small systems to graph-structured physical systems.

Questions for this sprint:

- Should our compositional model be graph-based or explicit port-based first?
- What does graph structure preserve that vanilla GNNs miss?

### A Physics-Informed Graph Neural Network Conserving Linear And Angular Momentum For Dynamical Systems

Link: https://www.nature.com/articles/s41467-025-67802-5

Why it matters:

- Shows how graph models can encode conservation and improve physical consistency.
- Relevant to compositional modeling and interpretable edge-level interactions.

Questions for this sprint:

- Which conservation laws are analogous to port power balance?
- How do edge-wise interactions support interpretability?

### Discovering Symbolic Laws Directly From Trajectories With Hamiltonian Graph Neural Networks

Link: https://arxiv.org/abs/2307.05299

Why it matters:

- Connects learned Hamiltonian graph models with symbolic law discovery.
- Closest to the neurosymbolic direction of this sprint.

Questions for this sprint:

- Can symbolic regression recover the energy or damping law from learned components?
- What would count as a useful symbolic explanation?

## 8. What Seems Already Worked

The literature already has strong work on:

- Neural ODEs for continuous-time learning;
- HNNs for conservative systems;
- PINNs for equation-constrained learning;
- neural operators for PDE surrogate modeling;
- pHNNs for forced and dissipative systems;
- stable pHNN variants;
- output-error pHNNs for noisy measurements;
- graph Hamiltonian models for many-body systems;
- port-Hamiltonian systems on graphs and compositional interconnection.

So the sprint should not claim the broad topic is new.

## 9. What May Still Be Missing

Potential gap:

**A clean, educational, research-sprint-style comparison of modeling choices for the same open compositional system, focused not only on prediction but also on energy balance, passivity, interpretability, and symbolic/component-level reasoning.**

More specific missing angles:

- Many papers compare accuracy but not always the whole modeling craft.
- Neural-operator benchmarks often emphasize field error, but structure violations may be under-measured.
- The bridge from pHNNs to neurosymbolic reasoning is still underdeveloped.
- Compositional generalization is harder than fitting one fixed system.
- Symbolic extraction from learned pH components could be made more practical.
- Good teaching artifacts are scarce: equations, code, diagnostics, and failure cases in one place.

## 10. Possible Workshop-Level Contribution

A focused contribution could be:

**A small experimental framework comparing Neural ODE, PINN, neural-operator, HNN/SymODEN, and pHNN-style models on forced dissipative, PDE, and coupled systems, evaluating prediction, energy-balance consistency, passivity, and interpretability of learned components.**

Minimal claims:

- Does pH structure improve long-horizon reliability?
- Does it reduce impossible energy behavior?
- Does it help under sparse or noisy data?
- Can learned components be inspected meaningfully?
- Can subsystem models compose better than monolithic models?

This is small enough to execute and rich enough to teach real research craft.
