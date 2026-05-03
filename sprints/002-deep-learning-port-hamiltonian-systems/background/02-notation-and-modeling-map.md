# 02 - Notation And Modeling Map

**Sprint:** 002 - Deep Learning for Port-Hamiltonian and Compositional Systems  
**Purpose:** Make the problem measurable and compare modeling choices cleanly.

## 1. Basic Variables

Use this notation consistently:

- `t`: time.
- `x(t)`: system state.
- `u(t)`: external input, actuation, forcing, or control.
- `y(t)`: system output.
- `H(x)`: Hamiltonian, energy, or storage function.
- `grad H(x)`: gradient of the Hamiltonian with respect to state.
- `J(x)`: skew-symmetric interconnection matrix.
- `R(x)`: positive semidefinite dissipation matrix.
- `G(x)`: input or port matrix.
- `theta`: learned model parameters.
- `x_hat(t)`: predicted state.
- `D`: dataset of trajectories.
- `tau`: one trajectory.
- `E(t)`: observed or computed energy when available.
- `e`: effort variable at a port.
- `f`: flow variable at a port.
- `P = e^T f`: power through a port.
- `s`: spatial coordinate or mesh point for field-valued systems.
- `a(s)`: coefficient, material field, parameter field, or input function.
- `v(t, s)`: spatiotemporal state field.
- `O_theta`: learned operator mapping one function or field to another.

## 2. Standard Dynamics Families

### Black-Box Dynamics

```text
dx/dt = f_theta(x, u, t)
```

What it learns:

- arbitrary vector field.

Strength:

- expressive and simple to implement.

Weakness:

- no built-in conservation, dissipation, passivity, or compositional meaning.

### Neural ODE

```text
x(t_1) = ODESolve(f_theta, x(t_0), t_0, t_1)
```

What it learns:

- continuous-time dynamics trained through an ODE solver.

Strength:

- natural for irregular time series and continuous-time modeling.

Weakness:

- still black-box unless structure is added.

### Hamiltonian Neural Network

For canonical coordinates `x = (q, p)`:

```text
dq/dt =  dH/dp
dp/dt = -dH/dq
```

Or:

```text
dx/dt = J grad H_theta(x)
```

What it learns:

- energy function for a closed conservative system.

Strength:

- preserves Hamiltonian structure and often improves long-term behavior.

Weakness:

- not enough for dissipation, inputs, or open systems unless extended.

### Lagrangian Neural Network

For coordinates `q` and velocities `dq/dt`, learn:

```text
L_theta(q, dq/dt)
```

Then use Euler-Lagrange structure to derive dynamics.

Strength:

- natural when the system is expressed in generalized coordinates and velocities.

Weakness:

- training can be unstable; constraints, dissipation, and inputs need careful handling.

### PINN

Given a known residual:

```text
N[x](t) = 0
```

Train a neural model to fit observations and reduce the residual:

```text
loss = data_loss + lambda * residual_loss
```

Strength:

- useful when equations are known but data is sparse or noisy.

Weakness:

- depends on the right equations and can struggle with stiffness, multi-scale behavior, and loss balancing.

### Neural Operator

For a PDE or field-valued system, learn an operator:

```text
O_theta: a(s) -> v(t, s)
```

Examples:

```text
initial condition -> future solution field
forcing field -> response field
coefficient field -> PDE solution
boundary condition -> spatiotemporal state
```

What it learns:

- a map between function spaces, not just a finite-dimensional vector field.

Strength:

- strong fit for PDE benchmarks, surrogate simulation, neural operators, and AI-for-science workloads.
- can generalize across parameter fields, initial conditions, boundary conditions, and resolutions when designed well.

Weakness:

- standard neural operators may still violate conservation, energy balance, passivity, boundary physics, or long-horizon stability unless structure is added.

### Port-Hamiltonian Neural Network

```text
dx/dt = (J_theta(x) - R_theta(x)) grad H_theta(x) + G_theta(x) u
y     = G_theta(x)^T grad H_theta(x)
```

Structural constraints:

```text
J_theta = -J_theta^T
R_theta = R_theta^T
R_theta >= 0
```

Strength:

- supports open systems, dissipation, control, and input/output structure.

Weakness:

- more design choices; needs careful parameterization and fair baselines.

### Graph / Compositional Port-Hamiltonian Model

Each subsystem `i` has:

```text
x_i, H_i, J_i, R_i, ports_i
```

Interconnections define how port variables exchange power:

```text
sum_i e_i^T f_i = 0
```

Strength:

- supports modularity, subsystem reuse, and transfer to new interconnections.

Weakness:

- graph design, port matching, and data requirements can become complex.

### Energy-Based Model

Learn an energy or score:

```text
E_theta(x)
```

Strength:

- useful for potentials, constraints, probabilistic modeling, and implicit structure.

Weakness:

- an energy function alone is not a full dynamics model.

### Symbolic Regression / Sparse Discovery

Fit compact equations from observed or learned dynamics:

```text
dx/dt = sum_k c_k phi_k(x, u)
```

Strength:

- can produce human-readable equations.

Weakness:

- sensitive to noise, library choice, derivative estimation, and missing variables.

## 3. Comparison Table

| Model family | Best for | Main inductive bias | Main risk |
| --- | --- | --- | --- |
| Black-box dynamics | flexible forecasting | none beyond architecture | physically impossible rollouts |
| Neural ODE | continuous-time dynamics | ODE flow | black-box vector field |
| HNN | closed conservative systems | energy conservation | weak for open/dissipative systems |
| LNN | coordinate-based mechanics | variational mechanics | training and constraints |
| PINN | known equations with sparse data | equation residuals | loss balancing and equation misspecification |
| Neural operator | PDE solution operators and surrogate simulation | function-space mapping | may violate physics without constraints |
| pHNN | open dissipative controlled systems | energy, ports, dissipation, passivity | parameterization complexity |
| Graph pHNN | interconnected systems | subsystem and port composition | graph and port design |
| Energy-based model | potentials and constraints | scalar energy landscape | not sufficient for dynamics |
| Symbolic discovery | interpretable equations | sparse equation library | noise and missing variables |

## 4. What To Measure

Prediction metrics:

- one-step error;
- rollout error;
- out-of-distribution forcing error;
- sparse-data performance;
- noisy-data performance.

Structure metrics:

- energy-balance residual;
- passivity violation;
- symmetry or skew-symmetry violation;
- positive-semidefinite dissipation violation;
- constraint violation;
- stability or blow-up rate.

Interpretability metrics:

- recovery of known energy form;
- recovery of damping coefficient;
- recovery of forcing or input matrix;
- quality of symbolic approximation;
- subsystem-level attribution of energy flow.

Compositional metrics:

- generalization to new coupling strength;
- generalization to new graph topology;
- subsystem reuse without retraining;
- port compatibility under interconnection;
- local-to-global prediction accuracy.

## 5. Minimal Experimental Variables

For the first experiment, keep the variable set small:

- choose one system;
- choose one dataset generator;
- choose two or three models;
- choose four metrics;
- choose one noise setting;
- choose one extrapolation setting.

A good first comparison:

```text
System: forced damped oscillator
Models: Neural ODE vs port-Hamiltonian neural network
Metrics: rollout error, energy-balance residual, passivity violation, extrapolation to new forcing
```

This gives a concrete start without losing the larger direction.
