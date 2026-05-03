# 03 - Methods And Experiments

**Sprint:** 002 - Deep Learning for Port-Hamiltonian and Compositional Systems  
**Purpose:** Convert the broad idea into an experiment ladder.

## 1. Experimental Ladder

The sprint should move from simple to compositional.

The benchmark ladder should separate:

- **debugging systems:** clean enough to verify equations and losses;
- **community-facing benchmarks:** relevant to current scientific ML and AI-for-science work;
- **extension systems:** biological or molecular systems that should wait until the method is mature.

### Phase 0: Reproduce The Equations

Goal:

- implement one known dynamical system;
- simulate trajectories;
- compute energy;
- verify the analytical energy balance.

Useful systems:

- mass-spring system;
- mass-spring-damper;
- forced damped oscillator;
- Duffing oscillator;
- RLC circuit.

Output:

- clean simulator;
- plots of state and energy;
- numerical check of `dH/dt`.

### Phase 1: Black-Box Baseline

Goal:

- train a Neural ODE or MLP vector-field model;
- measure short-term and long-term prediction;
- inspect where it violates energy or stability.

This is the baseline that gives the port-Hamiltonian model something meaningful to beat.

### Phase 2: Structure-Preserving Model

Goal:

- implement a simple port-Hamiltonian neural model;
- parameterize `H_theta`, `R_theta`, maybe fixed `J`, and simple `G`;
- train on the same data;
- compare prediction and structure metrics.

Keep the first pHNN simple:

- fixed canonical `J`;
- diagonal positive `R_theta`;
- learned scalar `H_theta`;
- known or learned input matrix `G`.

### Phase 3: Noise And Partial Observability

Goal:

- add noisy observations;
- test derivative-free training through rollout;
- test whether structure helps under noisy state measurements.

Questions:

- Does the black-box model overfit noise?
- Does the structured model preserve plausible dynamics?
- Does output-error style training become necessary?

### Phase 3.5: Community Benchmark Translation

Goal:

- take the model beyond toy systems;
- choose one benchmark that current scientific-ML researchers recognize;
- add structure-aware metrics to that benchmark.
- include a neural-operator baseline when the benchmark is PDE-like or field-valued.

Candidate benchmarks:

- PDEBench reaction-diffusion;
- PDEBench shallow-water;
- PDEBench wave/advection-diffusion;
- a small subset from The Well;
- later, a FluidsBench-style CFD surrogate task.

Core question:

**Can the structured model reveal reliability differences that benchmark MSE alone would miss?**

Neural-operator comparison:

- FNO or DeepONet as the strong PDE surrogate baseline;
- physics-informed neural operator when equation residuals are available;
- port-Hamiltonian or thermodynamic neural operator as the structured variant to explore later.

### Phase 4: Compositional Extension

Goal:

- move from one oscillator to coupled oscillators or a small RLC network;
- define subsystems and ports;
- compare monolithic learning with modular learning.

Core question:

**Can learned subsystem models be reconnected in a new graph and still behave plausibly?**

### Phase 5: Neurosymbolic Interpretation

Goal:

- inspect learned `H`, `R`, `J`, `G`, and port flows;
- attempt symbolic regression on learned energy or vector field;
- create a structured explanation of the model's behavior.

Example explanations:

- "Energy decays because learned dissipation is positive."
- "The external input injects energy through this port."
- "The coupling edge transfers power between subsystem A and B."
- "The learned Hamiltonian approximates quadratic kinetic plus potential energy."

### Phase 6: Biological Candidate Mapping

Goal:

- choose one biological system only after the physical phases are understood;
- map states, flows, ports, stored quantities, dissipation, and regulation;
- identify which parts are physically grounded and which parts are only analogical;
- decide whether the first biological model should be mechanistic, neural, symbolic, or hybrid.

Candidate systems:

- small metabolic pathway;
- enzyme kinetic system;
- protein binding or conformational transition model;
- ion-channel or membrane-potential model;
- simple cellular signaling feedback loop.

Output:

- system map;
- assumptions;
- known equations or constraints;
- data availability;
- reason why port-Hamiltonian or compositional modeling is useful.

### Phase 7: Biological Pilot

Goal:

- build a minimal biological model only after phase 6 produces a clean map;
- compare black-box dynamics with a structured compositional model;
- evaluate whether the structure improves extrapolation, interpretability, or constraint satisfaction.

Possible first biological pilot:

**A small metabolic reaction network with external nutrient input, internal reaction fluxes, ATP-like coupling, dissipation, and waste output.**

This is likely cleaner than starting directly with protein-scale molecular dynamics, because reaction networks have clearer flows and open-system boundaries.

## 2. Candidate First System

A strong first system is the forced mass-spring-damper:

```text
m d2q/dt2 + c dq/dt + k q = u(t)
```

State:

```text
x = [q, p]
p = m dq/dt
```

Hamiltonian:

```text
H(q, p) = p^2 / (2m) + k q^2 / 2
```

Port-Hamiltonian form:

```text
dq/dt =  dH/dp
dp/dt = -dH/dq - c dH/dp + u
```

Why it is useful:

- simple enough to understand;
- includes energy storage;
- includes dissipation;
- includes external forcing;
- has known ground truth;
- can be extended to coupled systems.

## 3. Candidate Model Losses

### Data Loss

```text
loss_data = mean ||x_hat(t) - x(t)||^2
```

Use this for all models.

### Derivative Loss

If derivative estimates are available:

```text
loss_deriv = mean ||f_theta(x, u, t) - dx/dt||^2
```

Use carefully because numerical derivatives can be noisy.

### Rollout Loss

Train by integrating the learned dynamics:

```text
x_hat(t_0:t_T) = ODESolve(f_theta, x_0, u(t), t_0:t_T)
loss_rollout = mean ||x_hat - x||^2
```

This is often closer to the real use case.

### Energy-Balance Loss

For a port-Hamiltonian model:

```text
dH/dt + grad H^T R grad H - y^T u = 0
```

Possible loss:

```text
loss_energy = mean |dH/dt + grad H^T R grad H - y^T u|^2
```

This should be used as a diagnostic even if not used as a training term.

### Structural Constraint Loss

If constraints are not hard-coded:

```text
loss_J = ||J + J^T||^2
loss_R = penalty_if_R_not_psd
```

Prefer hard parameterizations when possible:

- build `J` as `A - A^T`;
- build `R` as `L L^T` or diagonal `softplus(r)`.

## 4. Baselines

First baselines:

1. MLP one-step predictor.
2. Black-box Neural ODE.
3. HNN or controlled Hamiltonian model.
4. Port-Hamiltonian neural model.

Later baselines:

- PINN with known residual;
- FNO, DeepONet, or another neural operator for PDE benchmarks;
- physics-informed neural operator for PDE benchmarks with known residuals;
- SINDy or sparse symbolic model;
- graph neural simulator;
- graph port-Hamiltonian model.

## 5. Ablations

Useful ablations:

- fixed `H` vs learned `H`;
- fixed `R` vs learned `R`;
- known `G` vs learned `G`;
- data loss only vs data plus energy-balance loss;
- short rollout training vs long rollout training;
- clean data vs noisy data;
- interpolation forcing vs extrapolation forcing;
- fixed-resolution neural operator vs super-resolution or new-resolution evaluation;
- data-only neural operator vs physics-informed neural operator;
- monolithic model vs subsystem model.

## 6. Metrics

Prediction:

- one-step MSE;
- rollout MSE;
- final-time error;
- error growth rate.

Structure:

- energy-balance residual;
- passivity violation rate;
- negative dissipation events;
- stability or blow-up count.

Interpretability:

- learned energy shape compared to true energy;
- learned damping compared to true damping;
- learned input mapping compared to true input mapping;
- symbolic fit quality for learned terms.

Compositional:

- transfer to different coupling strength;
- transfer to new graph topology;
- subsystem reuse with frozen weights;
- edge-level power-flow error.

## 7. Failure Taxonomy

Track failures explicitly:

- good short-term fit but unstable rollout;
- low MSE but energy balance violation;
- stable but inaccurate;
- learned energy has wrong shape;
- learned dissipation becomes negative;
- model ignores input;
- model overfits forcing pattern;
- model fails under new coupling;
- symbolic extraction gives misleading equation;
- compositional model works only for the training graph.

## 8. Minimal First Research Question

The first build should answer:

**Can a port-Hamiltonian neural model learn a forced dissipative oscillator with better structural reliability than a black-box Neural ODE?**

Minimal deliverable:

- simulator;
- two models;
- shared training data;
- rollout comparison;
- energy-balance diagnostics;
- short writeup with failure cases.

This is not the whole sprint. It is the first experimental handle.
