# 3D SSFM Solver for Driven GPE (Floquet Dynamics)

> 🚧 **Active Master's Thesis Project** 🚧
> *This repository contains the ongoing 3D computational framework for my M.S. thesis on the Floquet dynamics of Bose-Einstein Condensates. The codebase is actively evolving.*

## Overview
This repository implements a high-performance numerical solver for the 3D Time-Dependent Gross-Pitaevskii Equation (GPE). It is specifically designed to simulate the non-equilibrium dynamics of Bose-Einstein Condensates (BECs) subjected to a time-periodic square-wave Floquet drive.

The solver utilizes the **Split-Step Fourier Method (SSFM)**, which efficiently handles the non-linear interaction terms in real space and evaluates the kinetic energy operator in momentum space via 3D Fast Fourier Transforms (FFT).

## Theoretical Framework

The dynamics of the driven BEC are governed by the following non-linear Schrödinger equation (GPE) in three dimensions:

$$i\hbar \frac{\partial \Psi(\mathbf{r},t)}{\partial t} = \left[ -\frac{\hbar^2}{2m}\nabla^2 + V(\mathbf{r})\Gamma_D \text{Sgn}(\cos(\omega t)) - \mu + g\vert{}\Psi(\mathbf{r},t)\vert{}^2 \right] \Psi(\mathbf{r},t)$$

Where:
*   $\Gamma_D$: Drive Strength
*   $\mu$: Chemical Potential
*   $V(\mathbf{r})$: Static Isotropic Harmonic Trap Potential ($\frac{1}{2}m\omega_{trap}^2 \mathbf{r}^2$)
*   $\text{Sgn}(\cos(\omega t))$: Periodic Square-Wave Drive
*   $g$: Mean-field interaction strength

## Computational Methodology

### 1. Imaginary Time Evolution (Ground State Preparation)
Before applying the Floquet drive, the absolute ground state is found by evolving the GPE in imaginary time ($\tau = it$). The SSFM operator acts as a relaxation method, continuously damping out higher-energy excited states until the chemical potential ($\mu$) converges to a stable minimum within a specified tolerance.

### 2. Real-Time SSFM Propagation
To propagate the wavefunction forward in real time steps ($\Delta t$), the non-commuting kinetic ($\hat{T}$) and potential/interaction ($\hat{V}$) operators are factorized using the Strang splitting scheme to minimize local truncation errors. 

For each time step, the solver:
1. Applies half a step of the real-space potential and non-linear interactions.
2. Transforms the wavefunction to 3D momentum space via FFT.
3. Applies a full step of the kinetic operator using the exact $K^2$ dispersion relation.
4. Transforms back to real space via IFFT.
5. Applies the remaining half step of the potential.

### 3. Observables Tracking
During real-time propagation, the solver continuously evaluates and records period-averaged observables via numerical integration (trapezoidal rule):
*   Trap Energy Expectation: $\langle V(\mathbf{r}, t) \rangle$
*   Kinetic Energy Expectation: $\langle T \rangle$ (calculated natively in momentum space via Parseval's theorem)
*   Interaction Energy: $E_0(t)$

## Current Capabilities (Ongoing)
- [x] Full 3D Cartesian grid generation and $K$-space mapping.
- [x] Imaginary-time evolution for initial ground state preparation.
- [x] Real-time 3D SSFM propagation.
- [x] Implementation of the square-wave Floquet operator.
- [x] Continuous observable tracking and period-averaging.

## Dependencies
*   `numpy` (Core matrix operations and FFT)
*   `scipy` (Numerical integration via `scipy.integrate.trapezoid`)
*   `matplotlib` (Observable tracking visualization)
