# Variational Monte Carlo for Quantum Many-Body Systems

A C++ implementation of Variational Monte Carlo (VMC) for simulating quantum many-body systems, specifically designed for atoms with up to six electrons. This project was developed as part of Computational Physics coursework in 2014/2015.

## Overview

This code implements the Variational Monte Carlo method to study quantum mechanical systems using importance sampling and the Metropolis algorithm. The simulation focuses on calculating ground state energies and wave functions for multi-electron atoms in harmonic oscillator potentials.

## Features

- **Variational Monte Carlo** implementation with importance sampling
- **Parallel computation** using OpenMP for efficient Monte Carlo sampling
- **Quantum many-body systems** with up to 6 electrons
- **Importance sampling** using quantum forces for improved convergence
- **Trial wave functions** including Jastrow factors and Slater determinants
- **Numerical differentiation** for local energy calculations
- **Harmonic oscillator potential** with configurable frequency

## Physics Background

The code solves the Schrödinger equation for N-electron systems using:
- **Trial wave function**: Ψ_T = Jastrow factor × Unperturbed wave function
- **Jastrow factor**: Accounts for electron-electron correlations
- **Slater determinants**: For antisymmetrization of fermionic wave functions
- **Importance sampling**: Uses quantum forces to guide Monte Carlo sampling

## Mathematical Equations

The code implements the Variational Monte Carlo method to solve the Schrödinger equation:

### Hamiltonian
The system Hamiltonian for N electrons in a harmonic oscillator potential:
```
H = T + V = -½Σ∇²_i + ½ω²Σr_i² + Σ_{i<j} 1/|r_i - r_j|
```

### Trial Wave Function
The trial wave function combines a Jastrow correlation factor with Slater determinants:
```
Ψ_T(r₁, r₂, ..., r_N) = exp(Σ_{i<j} a_{ij}r_{ij}/(1 + βr_{ij})) × det[φ_k(r_i)]
```

Where:
- `r_{ij} = |r_i - r_j|` is the electron-electron distance
- `a_{ij} = 1/3` for parallel spins, `a_{ij} = 1` for antiparallel spins
- `φ_k(r_i)` are single-particle orbitals using Hermite polynomials

### Single-Particle Orbitals
The single-particle wave functions use harmonic oscillator eigenstates:
```
φ_{n_x,n_y}(r) = exp(-½αωr²) × H_{n_x}(√(αω)x) × H_{n_y}(√(αω)y)
```

Where `H_{n}` are Hermite polynomials:
- `H₀(x) = 1`
- `H₁(x) = 2x`
- `H₂(x) = 4x² - 2`

### Local Energy
The local energy is calculated using numerical differentiation:
```
E_L(r) = HΨ_T(r)/Ψ_T(r) = [ -½Σ∇²Ψ_T/Ψ_T + V ]
```

### Variational Parameters
- `α`: Controls the width of the single-particle orbitals
- `β`: Controls the strength of electron-electron correlations in the Jastrow factor
- `ω`: Harmonic oscillator frequency

## Build Requirements

- C++ compiler with C++11 support
- [Armadillo](http://arma.sourceforge.net/) linear algebra library
- BLAS and LAPACK libraries
- OpenMP for parallel computation

## Building

```bash
make
```

This will compile the code and produce the `main.out` executable.

## Running the Simulation

```bash
./main.out
```

The simulation will:
1. Perform Monte Carlo sampling with 100,000 cycles
2. Test variational parameters α and β
3. Calculate ground state energies and wave functions
4. Output results to `vmc.dat`

## Key Parameters

- **α (alpha)**: Variational parameter controlling wave function width
- **β (beta)**: Variational parameter for Jastrow factor strength
- **ω (omega)**: Harmonic oscillator frequency (default: 1.0)
- **Number of particles**: Configurable up to 6 electrons
- **Dimensionality**: 2D or 3D systems

## Output

The program generates `vmc.dat` containing:
- Variational parameters (α, β)
- Ground state energy estimates
- Kinetic and potential energy components
- Statistical variance and error estimates

## Code Structure

- `src/main.cpp`: Main program with Monte Carlo sampling
- `src/manybody.h/cpp`: Many-body wave function implementations
- `src/singleparticle.h/cpp`: Single particle wave functions
- `src/Random.h/cpp`: Random number generation
- `src/lib.h/cpp`: Utility functions

## Mathematical Methods

1. **Metropolis Algorithm**: For Monte Carlo sampling
2. **Importance Sampling**: Using quantum forces for efficient sampling
3. **Numerical Differentiation**: For calculating local energies
4. **Slater Determinants**: For fermionic antisymmetrization
5. **Jastrow Factors**: For electron correlation effects

## Performance Optimization

- Parallel computation with OpenMP
- Efficient matrix operations using Armadillo
- Optimized numerical algorithms for wave function evaluation
