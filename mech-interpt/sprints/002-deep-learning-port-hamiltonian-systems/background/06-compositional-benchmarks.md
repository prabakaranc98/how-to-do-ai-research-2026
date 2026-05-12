# 06 - Compositional And Port-Based Benchmarks

**Sprint:** 002 - Deep Learning for Port-Hamiltonian and Compositional Systems  
**Last updated:** May 2, 2026

This note focuses on the "lego-block" version of the sprint:

**systems built from subsystems, ports, connectors, relations, or reusable components.**

This is different from a generic PDE benchmark. PDEBench and The Well are useful for field dynamics, but compositional/port-Hamiltonian learning needs benchmarks where the system is explicitly made from parts.

## 1. Important Distinction

There does not seem to be one dominant ML benchmark called "the port-Hamiltonian compositional benchmark."

Instead, there are several adjacent benchmark families:

- component-based physical modeling libraries;
- power-grid network benchmarks;
- graph physical simulation benchmarks;
- physical reasoning benchmarks;
- biological network model repositories;
- custom port-Hamiltonian graph generators.

The practical move is to use these as benchmark sources, then add our own port-Hamiltonian diagnostics.

## 2. Native Component-Based Systems

### Modelica Standard Library

Link: https://github.com/modelica/ModelicaStandardLibrary

Why it matters:

- Modelica is built around component-oriented modeling of complex cyber-physical systems.
- The standard library includes reusable components for mechanical, electrical, magnetic, thermal, fluid, control, and state-machine systems.
- This is very close to the "lego-block" idea: components connect through standardized interfaces.

How it could be used:

- choose simple examples from mechanical, electrical, thermal, or fluid sublibraries;
- generate trajectories from connected component models;
- learn component-level or port-level neural surrogates;
- test whether learned components transfer to new interconnections.

Good first candidates:

- translational mass-spring-damper networks;
- rotational inertia/gear/friction networks;
- analog RLC circuits;
- simple thermal networks;
- simple fluid pipe networks.

Research question:

**Can a learned component model trained inside one Modelica circuit/network be reused in another network without retraining?**

### ScalableTestGrids

Link: https://doi.org/10.3384/ecp21181351

Why it matters:

- Open-source benchmark suite for large-scale power-system test cases in Modelica.
- Built from PowerGrids components and designed to test tool performance on large network models.
- Relevant to subsystem composition: buses, lines, generators, loads, controllers, and topology.

How it could be used:

- treat the grid as a compositional network;
- learn surrogates for local components or subnetworks;
- test transfer across grid sizes or topologies;
- evaluate physical constraints such as power balance and stability.

## 3. Power-Grid Network Benchmarks

Power systems are among the most natural real-world compositional systems:

- buses;
- generators;
- loads;
- transmission lines;
- transformers;
- controllers;
- protective actions;
- topology changes.

### pandapower / MATPOWER Test Cases

Links:

- https://pandapower.readthedocs.io/en/develop/networks/power_system_test_cases.html
- https://matpower.org/docs/ref/matpower6.0/case24_ieee_rts.html

Why they matter:

- Provide standard IEEE and large-scale power-network test cases.
- Networks are explicit graphs with components.
- Useful for power-flow, optimal power-flow, contingency, and surrogate-model experiments.

How they could be used:

- start with IEEE 14, 24, 30, or 39 bus systems;
- create perturbations in load, generation, topology, or line status;
- train neural surrogates for power-flow or dynamics;
- add structure metrics: power balance, constraint violation, passivity/stability where applicable.

### Grid2Op / L2RPN

Link: https://github.com/Grid2op/grid2op

Why it matters:

- Testbed for sequential decision making in power-system operation.
- Built with modularity and topology actions in mind.
- Used by the Learning to Run a Power Network community.

How it could be used:

- model the grid as an interacting compositional system;
- learn dynamics/surrogates for topology-aware operation;
- test whether learned representations understand component changes and interventions.

Research question:

**Can a structure-preserving model generalize across topology actions better than a monolithic black-box model?**

## 4. Graph Physical Simulation Benchmarks

These are closer to contemporary ML than classical port-Hamiltonian benchmarks.

### Interaction Networks

Link: https://papers.nips.cc/paper/6418-interaction-networks-for-learning-about-objects-relations-and-physics

Why it matters:

- Early object-relation neural model for physical systems.
- Evaluated on n-body dynamics, rigid-body collision, and non-rigid dynamics.
- Explicitly studies objects and relations rather than one monolithic state vector.

Port-Hamiltonian angle:

- nodes can be subsystems;
- edges can be interactions or ports;
- add energy, momentum, or power-flow diagnostics.

### Graph Networks As Learnable Physics Engines

Link: https://proceedings.mlr.press/v80/sanchez-gonzalez18a.html

Why it matters:

- Uses graph networks as object- and relation-centric physical forward models.
- Studies generalization across parametrically and structurally varied systems.
- Also includes system identification and control.

Port-Hamiltonian angle:

- good benchmark family for structural generalization;
- ask whether learned edge functions can be made power-preserving, dissipative, or interpretable.

### Learning To Simulate Complex Physics With Graph Networks

Link: https://arxiv.org/abs/2002.09405

Why it matters:

- General graph-network simulator for fluids, rigid solids, and deformable materials.
- Uses particles as graph nodes and learned message passing as dynamics.
- Strong contemporary benchmark direction for learned simulators.

Port-Hamiltonian angle:

- local interactions can be interpreted as exchanges;
- add diagnostics for energy drift, stability, and physically impossible behavior.

### MeshGraphNets

Link: https://arxiv.org/abs/2010.03409

Why it matters:

- Mesh-based learned simulation for aerodynamics, structural mechanics, and cloth.
- Very relevant to engineering-scale AI simulation.
- Meshes already provide a compositional structure through cells/nodes/edges.

Port-Hamiltonian angle:

- ports can be approximated as boundary/interface exchanges;
- graph edges can carry learned flow/effort-like quantities;
- useful later, after small graph systems work.

## 5. Physical Reasoning Benchmarks

These are not port-Hamiltonian modeling benchmarks, but they are useful for reasoning about compositional physical systems.

### PHYRE

Link: https://phyre.ai/

Why it matters:

- 2D physical reasoning benchmark with tasks generated from templates.
- Measures sample-efficient generalization across physics puzzles.
- Useful for "agent reasons over components" rather than pure trajectory prediction.

Limit:

- not built around energy, ports, or system identification.

### ComPhy

Link: https://comphyreasoning.github.io/

Why it matters:

- Compositional physical reasoning dataset.
- Focuses on hidden physical properties such as mass and charge inferred through object interactions.
- Very aligned with the idea that structure and hidden components matter.

Limit:

- video reasoning benchmark, not a clean port-Hamiltonian simulation benchmark.

### CLEVRER / CLEVRER-Humans

Links:

- https://research.ibm.com/publications/clevrer-collision-events-for-video-representation-and-reasoning
- https://openreview.net/forum?id=sd1fv0g3UO1

Why it matters:

- Physical and causal reasoning over collision events.
- Useful for causal event graphs, counterfactuals, and reasoning over interactions.

Limit:

- more about reasoning over events than learning reusable physical components.

## 6. Biological Compositional Benchmarks

### BioModels / SBML

Links:

- https://www.ebi.ac.uk/biomodels/
- https://sbml.org/

Why it matters:

- BioModels is a curated repository of quantitative biological models.
- SBML is the standard format for biochemical reaction-network models.
- Models contain explicit species, reactions, parameters, and interaction structure.

How it could be used:

- choose a small curated metabolic or signaling model;
- treat species as states and reactions as flow-like components;
- learn unknown reaction terms or surrogate dynamics;
- test conservation, positivity, stoichiometric constraints, and response to perturbations.

Best first biological use:

**small metabolic or enzyme-kinetic networks, not protein-scale molecular dynamics.**

Why:

- reactions and fluxes are explicit;
- open-system boundaries can be defined;
- the compositional graph is already present.

## 7. Custom Port-Hamiltonian Graph Generator

Because there is no single canonical ML benchmark for port-Hamiltonian lego-block learning, we may need to create a small benchmark generator.

This could be the cleanest first contribution.

### Component Library

Start with components:

- masses;
- springs;
- dampers;
- pendulums;
- capacitors;
- inductors;
- resistors;
- voltage/current sources;
- simple controllers;
- boundary ports.

Each component has:

- state variables;
- stored energy;
- port variables;
- dissipation;
- known equations.

### Generated Tasks

Generate many systems by composing components:

- chains;
- stars;
- grids;
- random graphs;
- modular circuits;
- coupled oscillators;
- mixed mechanical-electrical analogs.

Train/test splits:

- train on small graphs, test on larger graphs;
- train on one topology, test on new topologies;
- train on one component parameter range, test on shifted parameters;
- train on one interconnection pattern, test on recombined modules.

### Metrics

Prediction:

- one-step error;
- rollout error;
- extrapolation error.

Structure:

- energy-balance residual;
- passivity violation;
- dissipation sign violation;
- port power mismatch;
- constraint violation;
- stability/blow-up count.

Compositionality:

- subsystem reuse without retraining;
- zero-shot topology generalization;
- component parameter recovery;
- edge/port interpretability;
- performance under component replacement.

## 8. Recommended Path

For this sprint, the strongest path is:

1. **Custom small port-Hamiltonian graph generator** for clean lego-block experiments.
2. **Modelica Standard Library examples** for real component-based physical systems.
3. **Power-grid network benchmark** using pandapower/MATPOWER/Grid2Op for community relevance.
4. **Graph simulator benchmarks** such as Interaction Networks, GNS, or MeshGraphNets for comparison with contemporary ML.
5. **BioModels/SBML** for the later biological compositional extension.

This gives both:

- mathematical cleanliness;
- external relevance.

The best first compositional research question:

**Can a learned subsystem or port model trained on one family of interconnections generalize to new component compositions while preserving energy, passivity, and port-power consistency?**
