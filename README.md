Here's your complete README extended to include all four assignments:

---

# Neural Dynamics & Computational Methods

A collection of computational neuroscience assignments exploring neural dynamics through numerical integration, synaptic transmission models, short-term memory mechanisms, and biologically-inspired learning rules.

## Repository Structure

| Notebook | Topic | Key Techniques |
|----------|-------|----------------|
| **Assignment 1** | Numerical Methods & Decision-Making | ODE solvers, phase plane analysis, Wilson-Cowan model |
| **Assignment 2** | Adaptive Exponential (AdEx) Neuron | F-I curves, firing pattern classification, nullcline analysis |
| **Assignment 3** | Synaptic Transmission | Exponential/alpha synapses, convolution, model fitting, BCM plasticity |
| **Assignment 4** | Short-Term Memory | AdEx adaptation, autocorrelation, Markram-Tsodyks plasticity |

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

- RK4 achieves significantly higher accuracy than Euler/Heun for nonlinear systems
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

## Notebook 3: Synaptic Transmission & Plasticity

### Topics Covered

| Topic | Description |
|-------|-------------|
| **Single-Exponential Synapse** | Instantaneous rise with exponential decay; implementation via direct integration |
| **Convolution-Based Synaptic Response** | Using DSP convolution to compute responses to multiple spikes efficiently |
| **Alpha Synapse** | Biologically realistic kernel `g(t) ∝ t·exp(-t/τ)` for synaptic conductance |
| **Current-Based vs. Conductance-Based Synapses** | PSC and EPSP calculation with fixed vs. voltage-dependent driving force |
| **Double-Exponential Synapse** | Separate rise and decay time constants (τ_rise = 1.7ms, τ_decay = 15ms) |
| **Model Fitting** | Using `LsqFit.curve_fit()` to fit synapse models to experimental EPSC data |
| **BCM Learning Rule** | Bienenstock-Cooper-Munro plasticity with fixed and adaptive thresholds |

### Synaptic Models Implemented

| Model | Kernel | Parameters |
|-------|--------|------------|
| Single-Exponential | `exp(-t/τ)` | τ = 1.7ms |
| Alpha | `(t/τ)·exp(-t/τ)` | τ = 1.7ms |
| Double-Exponential | `exp(-t/τ_decay) - exp(-t/τ_rise)` | τ_rise = 1.7ms, τ_decay = 15ms |

### Experimental Data Fitting

- Extracted EPSC traces from `test_trace.csv` by aligning around local minima (< -200 pA)
- Computed mean ± standard deviation across aligned traces
- Fitted both alpha and double-exponential models using nonlinear least squares

**Fit Results:**
- Alpha synapse: R² = 75.22% (τ = 12.2ms)
- Double-exponential: R² = 75.22% (τ_rise = 5.8ms, τ_decay = 53.8ms)

Both models explain ~75% of variance; alpha has fewer parameters (Occam's razor favors it).

### BCM Learning Rule

Implemented the Bienenstock-Cooper-Munro plasticity rule:

```
Δw = φ·x·(y - θ_M)
```

- Fixed threshold version recreating LTP/LTD crossover
- Adaptive threshold: `dθ_M/dt = (y² - θ_M)/τ_θ`
- Competition simulation: 4 inputs → 1 output with selective potentiation

### Key Results

- Convolution approach efficiently handles multiple spike inputs via linear superposition
- Both alpha and double-exponential capture EPSC dynamics equally well for this dataset
- BCM with adaptive threshold demonstrates synaptic competition — one input selectively potentiates when its rate increases

### Code Structure

| Function | Purpose |
|----------|---------|
| `single_exponential_synapse()` | Direct time-domain conductance calculation |
| Kernel definitions | Exponential, alpha, and double-exponential kernels |
| `fit_synapse_model()` | Curve fitting to experimental data using LsqFit |
| `bcm_update()` | Bienenstock-Cooper-Munro weight update |
| `bcm_competition()` | 4→1 network simulation with adaptive threshold |

---

## Notebook 4: Short-Term Memory & Neuronal Adaptation

### Topics Covered

| Topic | Description |
|-------|-------------|
| **Autocorrelation Analysis** | Quantifying memory in time series; MA(q) processes vs. oscillatory signals |
| **Poisson Spike Generation** | Generating random spike trains at 5 Hz across 100 input channels |
| **Synaptic Integration** | Single-exponential synapses (τ = 10ms) with random Gamma-distributed weights |
| **AdEx with Refractory Period** | 2ms absolute refractory period added to AdEx neuron model |
| **Adaptation Time Constant Sweep** | Testing τ_w = 50, 100, 250, 500 ms effects on membrane potential autocorrelation |
| **Multi-Seed Analysis** | Running simulations with 10 different random seeds to quantify consistency |
| **Markram-Tsodyks Short-Term Plasticity** | Phenomenological model of synaptic facilitation and depression |

### Autocorrelation Analysis

- **MA(q) process:** Moving average with θ = [0.8, 0.6] gives autocorrelation cutoff at lag 4
- **Oscillatory process:** Sine wave (5 Hz) yields periodic autocorrelation with non-zero asymptote
- **Neuronal memory:** Adaptation time constant τ_w determines how long past activity influences future firing

### Network Architecture

```
100 Poisson inputs (5 Hz) → Gamma-distributed weights → Single AdEx neuron → Adaptation current w
```

- Synaptic time constant: τ_syn = 10 ms
- Weight scaling: tuned to achieve 20-35 Hz firing rate (optimal scaling factor = 73)
- Refractory period: 2 ms absolute refractory period

### Adaptation Time Constant Effects

| τ_w (ms) | Expected Memory | Observed Autocorrelation |
|----------|----------------|--------------------------|
| 50 | Short | Fast decay |
| 100 | Medium | Moderate decay |
| 250 | Long | Slower decay |
| 500 | Very long | Slowest decay |

### Markram-Tsodyks Synaptic Plasticity

Model equations:

```
du/dt = -u/τ_f + U·(1-u)·δ(t - t_spike)     # facilitation (release probability)
dx/dt = (1-x)/τ_d - u·x·δ(t - t_spike)      # depression (resource depletion)
I_syn = A·u·x                               # postsynaptic current
```

**Parameter sets:**

| Plasticity Type | τ_f (ms) | τ_d (ms) | U | Behavior |
|-----------------|----------|----------|---|----------|
| Facilitation | 750 | 50 | 0.15 | Increasing EPSCs over train |
| Depression | 50 | 750 | 0.45 | Decreasing EPSCs over train |

**Frequency dependence (10-50 Hz):**
- Facilitation: EPSC ratio (I₁₀/I₁) increases with frequency
- Depression: EPSC ratio decreases with frequency

### Key Results

- Adaptation current `w` integrates spike history, creating a memory trace
- Autocorrelation decay time scales with τ_w — longer adaptation constants produce slower decay
- Stronger adaptation (higher a or b parameters) produces more consistent single-simulation results
- Short-term plasticity exhibits frequency-dependent gain control — facilitation amplifies high frequencies, depression attenuates them

### Code Structure

| Function | Purpose |
|----------|---------|
| `poisson_spike_gen()` | Generate Poisson spike trains at specified rate |
| `synaptic_current_response()` | Single-exponential synapse convolution |
| `adex_sim()` | AdEx with refractory period and variable τ_w |
| `simulate_stp()` | Markram-Tsodyks short-term plasticity model |
| Frequency sweep | Computing EPSC ratio across 10-50 Hz inputs |

---

## Technologies Used

- **Python 3.9+**
- **NumPy**  numerical arrays, linear algebra, exponential/trigonometric functions
- **SciPy**  ODE solvers (for validation), eigenvalue computation, curve fitting
- **Matplotlib** phase planes, time series, nullcline visualization, F-I curves, raster plots
- **LsqFit.jl** (Notebook 3)  nonlinear least squares fitting of synapse models
- **DSP.jl** (Notebook 3) convolution for synaptic kernel processing
- **StatsBase.jl** (Notebook 3)  statistical operations on EPSC traces
- **Peaks.jl** (Notebook 3)  local minima detection for EPSC alignment

---

## Key Biological Insights

### From Synaptic Transmission (Notebook 3)
- Single-exponential synapses produce fast, transient currents; alpha synapses have smoother rise
- Convolution implements linear superposition — total synaptic input = sum of individual spike contributions
- Experimental data fitting reveals that simple kinetic models capture ~75% of EPSC variance
- BCM rule with adaptive threshold implements homeostatic plasticity — threshold tracks squared firing rate

### From Short-Term Memory (Notebook 4)
- Adaptation current `w` integrates firing history, creating a low-pass filter on neural activity
- Autocorrelation quantifies memory — the rate of decay indicates how long past activity influences present
- Short-term plasticity acts as a frequency-dependent gain control mechanism
- Facilitating synapses amplify high-frequency inputs; depressing synapses attenuate them
- Refractory periods enforce minimum spike intervals, affecting maximum firing rate

---

## Usage Notes

- All notebooks are self-contained with explanatory markdown cells
- Parameters can be modified interactively within the notebooks
- Notebooks 1-2 are Python-based; Notebooks 3-4 are Julia-based (requires Julia 1.6+ with IJulia)
- For Julia notebooks, install required packages: `Pkg.add(["Plots", "DSP", "CSV", "DataFrames", "Peaks", "LsqFit", "StatsBase", "Distributions", "Random", "Printf"])`
- The experimental data file `test_trace.csv` is required for Notebook 3

---

## Author

**Chiara Benini**

*Course:* Neural Computation | Radboud University



---

## License

This repository is for educational purposes as part of coursework in computational neuroscience.
