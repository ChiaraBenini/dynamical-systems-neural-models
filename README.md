# Neural Dynamics & Computational Methods

A collection of computational neuroscience assignments exploring neural dynamics through numerical integration, phase plane analysis, and biologically-inspired models.

## Overview

This repository contains two complementary Jupyter notebooks that demonstrate core techniques for analyzing neural dynamical systems:

1. **Numerical Methods & Decision-Making Models** — Comparative implementation of ODE solvers (Euler, Heun, RK4) with phase plane analysis and a Wilson-Cowan style decision-making circuit.

2. **Adaptive Exponential (AdEx) Neuron Model** — Simulation and analysis of a 2D nonlinear integrate-and-fire neuron, including F-I curves, firing pattern classification, and phase-plane nullcline analysis.

---

## Notebook 1: Numerical Integration & Phase Plane Analysis

### Topics Covered

| Topic | Description |
|-------|-------------|
| **Numerical Integration** | Euler, Heun (improved Euler), and 4th-order Runge-Kutta (RK4) methods implemented and compared against analytical solutions |
| **Phase Plane Analysis** | Nullcline calculation, vector field visualization, fixed point classification via eigenvalue analysis (real/complex cases) |
| **Linear 2D Systems** | Systematic classification of nodes, saddles, foci, and spirals through eigenvalue sign and magnitude |
| **Wilson-Cowan Decision Model** | Two-population competitive network with self-excitation, cross-inhibition, and stimulus-dependent dynamics exhibiting winner-take-all behavior |

### Key Results

- RK4 achieves significantly higher accuracy than Euler/Huen for nonlinear systems
- Phase plane analysis confirms eigenvalue-based fixed point predictions
- The decision model displays hysteresis and coherence-dependent switching — lower coherence produces probabilistic outcomes, high coherence drives deterministic winners

### Code Structure

| Function | Purpose |
|----------|---------|
| `euler_solve()` | Forward Euler integration |
| `heun_solve()` | Improved Euler (Heun's method) |
| `rk4_solve()` | 4th order Runge-Kutta |
| `phaseplane_2D()` | Phase plane with nullclines and vector fields |
| `wilson_cowan_model()` | Decision-making dynamics |

---

## Notebook 2: Adaptive Exponential (AdEx) Neuron

### Model Equations

The AdEx model combines a nonlinear exponential spike initiation mechanism with a slow adaptation variable:

```
C_m dV/dt = -G_L(V - E_L) + G_L·Δ_T·exp((V - V_T)/Δ_T) - w + I_stim
τ_w dw/dt = -w + a(V - E_L)
```

- **Spike mechanism:** When V reaches V_peak, voltage resets to V_reset and w increases by b (spike-triggered adaptation)

### Topics Covered

| Topic | Description |
|-------|-------------|
| **Neuron Simulation** | Euler integration with spike-reset logic and adaptation dynamics |
| **Firing Patterns** | Classification into regular-adaptive, transient, and bursting/delayed-tonic regimes through parameter variation |
| **F-I Curves** | Frequency-current relationships computed for multiple parameter sets |
| **Phase Plane Analysis** | V-nullcline and w-nullcline calculation; fixed point identification |
| **Trajectory Analysis** | Phase-space trajectories for single-spike and tonic spiking regimes |

### Parameter Sets Explored

| Regime | Key Parameters | Behavior |
|--------|---------------|----------|
| **Regular-Adaptive** | a = 0.5 nS, b = 7 pA | Gradual spike frequency adaptation |
| **Transient** | τ_w = 250 ms, b = 100 pA | Initial burst followed by quiescence |
| **Bursting/Delayed Tonic** | a = -0.5 nS, b = 20 pA, V_reset = -58 mV | Delayed onset of sustained spiking |

### Key Results

- The AdEx model reproduces diverse firing patterns observed in cortical neurons by varying adaptation strength (a) and spike-triggered adaptation (b)
- F-I curves reveal threshold currents (rheobase) and gain differences across parameter regimes
- Phase plane nullclines explain spiking initiation and termination: the V-nullcline's exponential shape creates a "knee" that enables regenerative spiking, while the w-nullcline's slope (determined by a) controls adaptation strength

### Code Structure

| Function | Purpose |
|----------|---------|
| `adex_sim()` | AdEx simulation with Euler integration and spike handling |
| `plot_nullclines()` | Phase plane with both nullclines for given input current |
| F-I curve calculation | Firing rate extraction across 0-500 pA input range |

---

## Technologies Used

- **Python 3.9+**
- **NumPy** — numerical arrays, linear algebra, exponential/trigonometric functions
- **SciPy** — ODE solvers (for validation), eigenvalue computation
- **Matplotlib** — phase planes, time series, nullcline visualization, F-I curves


---

## Usage Notes

- Both notebooks are self-contained with explanatory markdown cells
- Parameters can be modified interactively within the notebooks
- The AdEx simulation uses Euler integration — for production use, consider smaller dt or adaptive methods



## Author

Chiara Benini

**Course:** Neural Computation | Radboud University

