# NeoCore

Universal spectral evolution engine for Resonance Field Theory (RFT).

## What is NeoCore?

NeoCore is a minimal, dimension-agnostic spectral solver that evolves a complex scalar field ψ under the Resonance Field Theory (RFT) dynamical equation.

It implements the core nonlinear evolution of RFT using a fully spectral (FFT-based) method with semi-implicit time stepping for stability. The engine is contained in a single clean Python file (NeoCoreFinal.py) and its companion notebook.

This is the reference implementation of the field equation derived from Resonance Field Theory. It is not a general-purpose library — it is purpose-built for RFT simulations.

## The Equation

NeoCore evolves:

∂ψ/∂t = -η |ψ|⁴ ψ - λ (Yukawa ⊗ (|ψ|² - 1)) ψ + ∇²ψ

- Local quartic repulsion term (η |ψ|⁴ ψ)
- Non-local Yukawa-mediated attraction on density fluctuations (λ term)
- Linear Laplacian diffusion (handled implicitly for unconditional stability)

The code runs in any dimension (1D, 2D, 3D, 4D, …) on cubic/hypercubic grids.

## Usage

import torch
from NeoCoreFinal import evolve

# Initial condition: complex scalar field on an N^D grid
N = 128
psi0 = torch.randn(N, N, dtype=torch.cfloat)  # or any shape you want

# Run the evolution
psi_final = evolve(
    psi0,
    eta=1e-7,      # local |ψ|⁴ repulsion strength
    lam=10.0,      # non-local Yukawa attraction strength
    ell=0.15,      # Yukawa screening length
    dt=0.004,      # time step
    steps=5000,    # number of steps
    dealias=0.666, # 2/3-rule dealiasing (or False to disable)
    progress=True  # shows tqdm progress bar
)

print(psi_final.shape)   # (N, N) or whatever input shape you gave

The function returns the final field ψ in real space. Everything else (FFT transforms, dealiasing, Yukawa kernel, semi-implicit update) is handled internally.

## Files

- NeoCoreFinal.py — the core engine (single function evolve())
- NeoCoreFinal.ipynb — interactive notebook for exploration and visualization

## Requirements

- PyTorch
- tqdm (only if progress=True)

## License

MIT — do what you want with it.

Resonance Field Theory (RFT) reference implementation  
Rooster Murphy  
March 2026
