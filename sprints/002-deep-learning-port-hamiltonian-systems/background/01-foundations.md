# 01 - Foundations

**Sprint:** 002 - Deep Learning for Port-Hamiltonian and Compositional Systems  
**Purpose:** Build the first-principles mental model before experiments.

## 1. Why This Problem Space Matters

Many important real-world systems are dynamical systems:

- machines move;
- circuits exchange electrical energy;
- power grids synchronize and destabilize;
- fluids move through boundaries;
- robots interact with the world;
- biological systems regulate flows;
- organizations and markets move resources, incentives, and information.

A plain neural network can learn patterns in these systems, but it may ignore what makes the system physically or structurally meaningful:

- conservation;
- dissipation;
- stability;
- passivity;
- controllability;
- interconnection;
- constraints;
- modularity.

Port-Hamiltonian systems are interesting because they give a general language for **open systems**: systems that exchange energy, matter, signals, or generalized power with their environment.

Deep learning is interesting because many real systems are partially unknown, high-dimensional, noisy, nonlinear, and difficult to model fully by hand.

The research opportunity is the intersection:

**Can neural models learn the unknown parts while the port-Hamiltonian scaffold preserves structure?**

## 2. Research Progression

The sprint should develop in two stages.

Stage 1:

**Physical systems first.**

Start with systems where the modeling language is clean:

- mechanical oscillators;
- forced and damped systems;
- RLC circuits;
- coupled oscillators;
- small networked physical systems.

These systems are useful because energy, ports, dissipation, and interconnection can be written down explicitly. That makes them good for learning the craft:

- simulate known equations;
- train black-box and structured models;
- measure energy-balance violations;
- inspect learned components;
- understand failure modes.

Stage 2:

**Biological systems later.**

Once the physical modeling discipline is clear, extend the lens to biological systems:

- metabolic reaction networks;
- enzyme kinetics;
- protein/molecular interaction systems;
- cellular signaling and regulation;
- membrane and ion-channel dynamics;
- physiological flow systems.

The biological extension is attractive because biological systems are also open, dissipative, adaptive, and compositional. But it needs more care than physical toy systems because the stored quantities, ports, and conservation laws may be indirect, approximate, or scale-dependent.

## 3. The Port-Hamiltonian Mental Model

Classical Hamiltonian systems are often used for closed conservative systems. Port-Hamiltonian systems extend this idea to open, dissipative, controlled, and interconnected systems.

The core ingredients:

- **State `x`:** the variables needed to describe the system.
- **Hamiltonian `H(x)`:** stored energy or a generalized storage function.
- **Interconnection `J(x)`:** structure that moves energy around without creating or destroying it.
- **Dissipation `R(x)`:** structure that removes stored energy, often as heat, friction, resistance, or loss.
- **Port matrix `G(x)`:** how external inputs enter the system.
- **Input `u`:** external actuation, forcing, boundary input, or control.
- **Output `y`:** what the system exposes back to the environment.

A standard finite-dimensional form is:

```text
dx/dt = (J(x) - R(x)) grad H(x) + G(x) u
y     = G(x)^T grad H(x)
```

Typical structural constraints:

```text
J(x) = -J(x)^T
R(x) = R(x)^T
R(x) >= 0
```

The energy balance is the key:

```text
dH/dt = - grad H(x)^T R(x) grad H(x) + y^T u
```

This says:

- dissipation can only remove stored energy;
- external ports can inject or remove energy;
- the model has a built-in accounting system.

## 4. Why Ports Matter

Ports are the boundary between a subsystem and the world.

A port usually has paired variables:

- **flow:** how much is moving;
- **effort:** the driving intensity;
- **power:** effort times flow.

Examples:

- electrical circuit: voltage and current;
- mechanical translation: force and velocity;
- rotation: torque and angular velocity;
- fluid system: pressure and flow rate;
- thermal system: temperature difference and entropy or heat flow, depending on formulation.

Ports matter because they make systems compositional. If two subsystems exchange power through compatible ports, they can be connected while preserving the larger balance law.

This is why port-Hamiltonian models are attractive for:

- robotics;
- circuits;
- power systems;
- mechatronics;
- multi-physics simulation;
- networked physical systems;
- modular learned simulators.

## 5. Compositional Systems

A compositional system is built from parts.

Each part can have:

- its own state;
- its own storage function;
- its own dissipation;
- its own input/output ports;
- its own unknown learned components.

The full system is created by interconnecting ports. The valuable point is that the full system is not merely a large black-box function. It has a structure:

```text
subsystem A <-> port/interconnection <-> subsystem B
```

This suggests a graph view:

- nodes are subsystems;
- edges are interconnections;
- node attributes include state and energy;
- edge attributes include flows, efforts, and coupling;
- global behavior emerges from local interactions.

This is the bridge to graph neural networks and neurosymbolic modeling.

## 6. Why Deep Learning Enters

In many real systems, the exact equations are incomplete or unknown:

- the energy function may not be known;
- damping may be nonlinear;
- external forcing may be unobserved;
- friction may be hard to model;
- parameters may change over time;
- interactions may be too complex to write manually;
- measurements may be noisy or partial.

Deep learning can learn these unknown pieces:

- `H_theta(x)` for energy;
- `R_theta(x)` for dissipation;
- `J_theta(x)` for interconnection;
- `G_theta(x)` for input structure;
- `f_theta(x, u, t)` for residual unknown dynamics;
- neural operators for solution maps from inputs, coefficients, forcing, or boundary conditions to full spatiotemporal fields;
- graph message functions for couplings;
- symbolic approximations after learning.

But without structure, deep models may fit short trajectories while failing at long-horizon behavior, extrapolation, stability, or physical plausibility.

The research tension:

**How much structure should be hard-coded, and how much should be learned?**

Neural operators are especially relevant once the sprint moves from low-dimensional ODE systems to PDE fields. They are strong candidates for reaction-diffusion, shallow-water, wave, fluid, and multi-physics benchmarks because they learn an operator over a family of problem instances rather than one trajectory at a time.

The port-Hamiltonian question for neural operators is:

**Can we make learned solution operators respect energy, dissipation, boundary ports, conservation laws, or thermodynamic structure, instead of only optimizing field prediction error?**

## 7. Biological Systems As A Later Extension

Biological systems are a promising but harder target.

The port-Hamiltonian/compositional lens may help when a biological system can be expressed through:

- state variables such as concentrations, conformational states, membrane potentials, or population levels;
- stored quantities such as chemical free energy, electrochemical potential, mechanical energy, or biomass;
- flows such as reaction fluxes, ion currents, transport rates, or signaling rates;
- ports such as nutrient input, waste output, ligand binding, membrane exchange, neural input, or mechanical force;
- dissipation through irreversible reactions, friction, heat, degradation, diffusion, or entropy production;
- regulation through enzymes, feedback loops, allosteric control, gene expression, or external signals.

Good biological candidates:

- **Metabolic networks:** reaction fluxes, chemical potentials, ATP/NADH coupling, nutrient ports, waste ports, and dissipation.
- **Protein/molecular systems:** conformational states, binding/unbinding, free-energy landscapes, reaction pathways, and coupling to cellular context.
- **Ion channels and neurons:** membrane voltage, ion currents, conductances, capacitance, synaptic inputs, and dissipative flow.
- **Physiological flow systems:** blood flow, pressure, compliance, resistance, and organ-level exchange.
- **Cell signaling:** input/output regulation and feedback, though the energy interpretation may be less direct.

The important caution:

**Biological modeling should not force every interaction into an energy metaphor.**

For some biological systems, the port-Hamiltonian language may be mathematically meaningful. For others, it may be a useful compositional analogy but not a valid physical formalism. The research discipline is to state which case we are in.

## 8. How To Think About "Any Event"

It is tempting to say every event can be modeled as a port-Hamiltonian system. That is too strong.

A more rigorous version:

**Any event can be investigated with a port-Hamiltonian lens if we can identify state, stored quantity, flows, boundaries, dissipation, and interconnection.**

This lens is strongest for physical systems with real balance laws. It becomes more speculative for social, economic, cognitive, or organizational systems.

For non-physical systems, the useful question is:

- What is stored?
- What flows?
- What is conserved, approximately conserved, or dissipated?
- What enters through ports?
- What leaves through ports?
- What are the coupling rules?
- What breaks the analogy?

The last question is important. Good research should know where the metaphor stops.

## 9. Neurosymbolic Interpretation

In this sprint, "neurosymbolic" should mean something precise.

The symbolic part:

- state variables;
- graph structure;
- port connections;
- energy balance;
- known physical constraints;
- equations and loss terms;
- interpretable components like `H`, `R`, `J`, and `G`.

The neural part:

- unknown functions;
- nonlinear energy landscapes;
- damping laws;
- coupling functions;
- residual dynamics;
- latent state embeddings;
- learned observation maps.

A good neurosymbolic model should allow both:

- numerical prediction;
- structured reasoning.

Structured reasoning examples:

- Which subsystem injected energy?
- Which edge carried the largest power flow?
- Did the model violate passivity?
- Which learned damping term explains decay?
- Can the learned energy be approximated symbolically?
- Does a new interconnection remain stable?

## 10. The Key Research Taste

The sprint should avoid two extremes.

Extreme 1:

**Pure black-box modeling.**

This may produce good short-term prediction but weak explanation, weak extrapolation, and weak guarantees.

Extreme 2:

**Over-symbolic modeling.**

This may require exact equations and assumptions that are unavailable in real systems.

The useful middle:

**Learn unknown pieces inside a structure-preserving model.**

That is the north star for this sprint.
