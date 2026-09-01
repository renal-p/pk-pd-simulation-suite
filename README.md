# Systems Pharmacology & Pharmacokinetics Portfolio (`/pk-pd-simulation-suite`)

This repository contains Python and Julia notebooks tracking drug clearance, multi-compartment distribution, and biochemical reactions.

## Project Frameworks

* **foundational_decay_model.ipynb**: A Jupyter notebook simulating standard 1st-order drug elimination in Python. Tracks baseline clearance curves and half-life decay.
* **linear_3_compartment_model.jl**: A Pluto notebook solving a linear 3-compartment system following an IV bolus with an ODE solver. Uses an arrowhead matrix structure to map the distribution pathways between the central core and peripheral tissues.
* **non_linear_3_compartment_model.jl**: A Pluto notebook utilizing non-linear differential equations with Michaelis-Menten clearance. Scales the system by explicit compartment volume ratios to track mass-balanced drug concentration changes under metabolic saturation thresholds.
* **interactive_ternary_allosteric_model.jl**: A Pluto notebook modeling allosteric ligand-receptor-modulator binding kinetics with a stiff solver (`Rosenbrock23`). Features interactive parameter sliders and a real-time validation check to ensure the binding math follows thermodynamic rules.
* **interactive_huang_ferrell_mapk_model.jl**: A Pluto notebook mapping out the full Huang-Ferrell model using `Catalyst.jl`. Manually codes all ten mass-action reactions for the cascade. Features an interactive slider to control the concentration of E1 so you can visually watch the system flip between on and off states using `Tsit5()`.

### Non-Linear Bayesian Parameter Estimation Suite

A two-part computational pipeline developed in Julia to generate synthetic pharmacokinetic (PK) data with analytical noise and back-calculate true physiological parameters using Markov Chain Monte Carlo (MCMC) sampling.

* **pk_data_generation_engine.jl**: A Pluto notebook that models non-linear Michaelis-Menten drug elimination and 3-compartment lipophilic distribution kinetics (simulating therapeutic compounds like Phenytoin). It injects realistic Gaussian analytical measurement noise into a stiff ODE system (`Rodas5P`) to compile a reproducible, time-series reference dataset using `DataFrames.jl` and `CSV.jl`.
* **pk_bayesian_parameter_estimation.jl**: A Pluto notebook executing an inverse problem workflow. It utilizes `Turing.jl` and the No-U-Turn Sampler (NUTS) algorithm to reconstruct the 9 true hidden physiological parameters (volumes, clearances, and partition coefficients) from the noisy mock dataset. It incorporates custom log-probability penalties (`-Inf` returns) to enforce structural constraints, optimizes convergence via automated finite differencing (`ADTypes.AutoFiniteDiff()`), and validates model accuracy against true trajectories and $R̂$ metrics.
