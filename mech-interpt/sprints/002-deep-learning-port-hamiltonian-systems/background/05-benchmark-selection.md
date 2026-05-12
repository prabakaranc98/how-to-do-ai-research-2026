# 05 - Benchmark Selection

**Sprint:** 002 - Deep Learning for Port-Hamiltonian and Compositional Systems  
**Last updated:** May 2, 2026

The benchmark should not be chosen only because it is mathematically convenient. It should also connect to what people in contemporary AI/ML, scientific machine learning, neural operators, graph simulators, and AI-for-engineering are actively studying.

For explicitly compositional, lego-block-style systems, see `06-compositional-benchmarks.md`.

The practical rule:

**Use toy physical systems to debug the method, but use community-relevant physical benchmarks to make the work matter.**

## 1. Selection Criteria

A good benchmark for this sprint should satisfy most of these:

- **Contemporary relevance:** connected to active scientific ML, neural operator, graph simulation, robotics, CFD, energy, or AI-for-science work.
- **Port-Hamiltonian fit:** has meaningful state, stored quantity, flow, dissipation, external input, boundary, or interconnection structure.
- **Compositional structure:** can be decomposed into subsystems, graph nodes, mesh cells, ports, or interacting components.
- **Executable scale:** small enough to run locally before scaling up.
- **Diagnostic richness:** supports more than MSE: energy balance, passivity, stability, conservation, dissipation, rollout error, and extrapolation.
- **Bridge to biology:** ideally has a later path toward reaction networks, metabolism, cellular dynamics, or molecular systems.

## 2. Benchmark Ladder

### Tier 0: Method Debugging Systems

Purpose:

- verify equations;
- test training code;
- inspect energy and dissipation;
- build intuition.

Candidate systems:

- forced mass-spring-damper;
- forced Duffing oscillator;
- RLC circuit;
- two coupled oscillators;
- small mechanical network.

These are not enough as the final benchmark. They are unit tests for the research method.

### Tier 1: Community Scientific-ML Benchmarks

Purpose:

- connect the sprint to neural operators, PINNs, PDE learning, and AI-for-science evaluation.

Relevant benchmark families:

- [PDEBench](https://github.com/pdebench/PDEBench): broad scientific-ML benchmark suite for time-dependent PDEs, with baselines such as FNO, U-Net, and PINN.
- [The Well](https://proceedings.neurips.cc/paper_files/paper/2024/hash/4f9a5acd91ac76569f2fe291b1f4772b-Abstract-Datasets_and_Benchmarks_Track.html): large-scale collection of diverse physics simulations for machine learning, with many spatiotemporal systems and a PyTorch interface.
- [RealPDEBench](https://huggingface.co/papers/2601.01829): real-world physical-system benchmark focused on the sim-to-real gap.

Most relevant PDE-style candidates:

- reaction-diffusion systems;
- shallow-water equations;
- Burgers equation;
- wave equations;
- Navier-Stokes subsets;
- advection-diffusion systems.

Why these matter:

- they are recognizable in the SciML community;
- they are natural for neural operators and PINNs;
- some have conservation, dissipation, or port-Hamiltonian/PDE structure;
- reaction-diffusion gives a bridge toward biological systems.

Neural-operator role:

- FNO or DeepONet should be treated as strong baselines for PDE-style benchmarks.
- Physics-informed neural operators should be considered when the governing residual is available.
- A later structured variant could ask whether energy, dissipation, boundary-port, or thermodynamic constraints improve reliability beyond field MSE.

### Tier 2: Graph And Mesh Physical Simulators

Purpose:

- connect the sprint to compositional learning and graph-based physical simulation.

Relevant references:

- [Learning to Simulate Complex Physics with Graph Networks](https://arxiv.org/abs/2002.09405): particle/graph simulator for fluids, rigid solids, and deformable materials.
- [Learning Mesh-Based Simulation with Graph Networks](https://arxiv.org/abs/2010.03409): MeshGraphNets for aerodynamics, structural mechanics, and cloth.
- [Meshgraphnets informed locally by thermodynamics](https://doi.org/10.1186/s40323-025-00311-8): recent thermodynamics-informed graph-network direction with port-metriplectic structure.

Candidate systems:

- cloth or deformable materials;
- mesh-based structural mechanics;
- flow around objects;
- particles/fluids with learned interactions;
- graph-coupled oscillator systems.

Why these matter:

- graph simulators are a major contemporary ML direction;
- compositional structure is explicit;
- edge/node interactions can be interpreted as local exchanges;
- port-Hamiltonian or thermodynamic constraints can be layered onto graph models.

### Tier 3: AI-For-Engineering Benchmarks

Purpose:

- connect the sprint to applied industrial and frontier AI-for-science relevance.

Relevant areas:

- CFD surrogate modeling;
- digital twins;
- robotics and control;
- power grids and grid-forming inverters;
- thermal systems;
- mechanical design and simulation;
- multi-physics simulation.

Relevant benchmark/watch item:

- [FluidsBench](https://fluidsbench.org/): emerging benchmark effort for CFD AI models and surrogates.

Why these matter:

- this is where structure-preserving learned simulators can have real value;
- long-horizon stability and physical plausibility matter;
- speedups over numerical solvers matter;
- interpretability is more useful than leaderboard MSE alone.

### Tier 4: Biological Bridge Benchmarks

Purpose:

- extend the physical/compositional lens toward biological systems after the method is mature.

Candidate systems:

- reaction-diffusion as a biological-pattern bridge;
- enzyme kinetic networks;
- small metabolic pathways;
- membrane and ion-channel systems;
- protein binding or conformational transition toy models.

Why not start here:

- biological systems often have hidden states, noisy measurements, multi-scale dynamics, and approximate rather than clean conservation laws;
- protein/molecular systems add free-energy landscapes, stochasticity, and scale separation;
- metabolism is probably the cleaner first biological extension because reaction fluxes and open-system boundaries are easier to define.

## 3. Recommended Sprint Path

The best path is not to choose only one benchmark. Use a ladder.

### Step 1: Debugging Benchmark

Use:

**Forced mass-spring-damper or forced Duffing oscillator.**

Why:

- clean port-Hamiltonian structure;
- easy energy-balance diagnostics;
- fast local experiments;
- good for debugging Neural ODE vs port-Hamiltonian neural model.

Claim level:

- method sanity check;
- not a major benchmark contribution.

### Step 2: First Community-Facing Benchmark

Use one of:

**PDEBench reaction-diffusion, shallow-water, or wave/advection-diffusion subset.**

Why:

- recognizable scientific-ML benchmark;
- relevant to PINNs, FNOs, neural operators, and PDE learning;
- richer than a toy oscillator;
- reaction-diffusion bridges naturally toward biology;
- shallow-water/wave systems have clearer conservation and port-Hamiltonian PDE interpretations.

Claim level:

- structured model evaluation on a known benchmark;
- compare not only prediction but also structure violations and extrapolation.

### Step 3: Compositional Benchmark

Use:

**Coupled oscillator/RLC network first, then MeshGraphNet-style graph or mesh system.**

Why:

- coupled oscillator/RLC gives clean ports;
- graph/mesh systems connect to modern physical simulation ML;
- allows subsystem and edge-level reasoning.

Claim level:

- can learned components compose better than monolithic dynamics?

### Step 4: Biological Pilot

Use:

**Small metabolic reaction network before protein-scale molecular dynamics.**

Why:

- reaction fluxes and open boundaries are clearer;
- bridges from reaction-diffusion/PDE work;
- easier to state what is stored, what flows, what dissipates, and what is regulated.

Claim level:

- exploratory extension, not immediate benchmark SOTA.

## 4. What Would Make The Benchmark Interesting

The sprint should not aim to beat every model on a leaderboard.

The more interesting question:

**Can structure-preserving models expose failures that standard benchmark metrics hide?**

Useful claims:

- a black-box model gets low MSE but violates energy or passivity;
- a structured model has similar MSE but better rollout stability;
- a structured model extrapolates better to new forcing or boundary conditions;
- learned energy/dissipation/coupling components are inspectable;
- subsystem models transfer to new interconnections better than monolithic models;
- biological extension is possible only when the balance variables are carefully defined.

## 5. Current Shortlist

Best immediate sequence:

1. **Forced mass-spring-damper:** unit test for port-Hamiltonian learning.
2. **PDEBench reaction-diffusion or shallow-water:** first community-facing scientific-ML benchmark.
3. **Coupled oscillator or RLC network:** clean compositional benchmark.
4. **MeshGraphNet-style flow/deformation system:** modern graph simulation benchmark.
5. **Small metabolic network:** biological extension once the method is mature.

The strongest first research question becomes:

**Can a port-Hamiltonian or thermodynamic structure-preserving neural model improve long-horizon reliability and structural consistency on a benchmark that contemporary scientific ML researchers already care about?**
