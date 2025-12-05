# Quaternionic-Harmonic Transformer (QHT) – Fractal & Laser-Interaction Sandbox

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.17171112.svg)](https://doi.org/10.5281/zenodo.17171112)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](LICENSE)
[![Python ≥3.8](https://img.shields.io/badge/Python-≥3.8-3776ab)](https://www.python.org)

A lightweight research sandbox that couples **quaternionic-harmonic vector spaces** with **laser-pulse scattering** to probe self-similar structures.  
Use it to:

* Generate arbitrary 2-D/3-D fractals from Iterated Function Systems (IFS)  
* Estimate fractal dimension *D* via box-counting **and** spectral (Fourier-power) estimators  
* Simulate femtosecond Gaussian pulses interacting with the fractal substrate  
* Extract scattering exponents that correlate with *D* (validation loop)  

The code is deliberately self-contained (NumPy/SciPy/Matplotlib only) so you can copy one file into a notebook and start experimenting.

---

## Quick-start

```bash
git clone https://github.com/your-handle/QHT-Fractal-Laser.git
cd QHT-Fractal-Laser
python sandbox.py          # runs the demo → three plots + console report

Requirements
python >= 3.8
numpy
scipy
matplotlib

What the demo does

    Builds a Sierpiński triangle with 50 k points (IFS, chaos-game).
    Computes D two ways:
        Box-counting on 20 dyadic grids →  D<sub>
        Radial power-spectrum fit P(k) ∝ k^{–β} →  D<sub> = (7–β)/2
    Compares with theoretical D = log 3 / log 2 ≈ 1.585.
    Fires a chirped Gaussian pulse (λ = 800 nm, τ = 1 ps, w₀ = 10 µm) across the attractor and records the complex scattering matrix.
    Repeats the spectral fit on the scattered field to show the exponent is preserved (validating the quaternionic-harmonic assumption).

Typical console output
==================================================
  FRACTAL ANALYSIS REPORT
==================================================
Fractal: Sierpinski Triangle
Theoretical Dimension: 1.5850
Dimension (Box-Counting): 1.581 ± 0.015
Dimension (Spectral Analysis): 1.589 ± 0.020
==================================================


API cheat-sheet
from sandbox import FractalGenerator, LaserPulseSimulator

# 1. Build any IFS you like
frac = FractalGenerator(dim=2)
frac.add_transform([0.5, 0, 0, 0.5, 0, 0])        # contraction + translation
frac.add_transform([0.5, 0, 0, 0.5, 0.5, 0])
frac.add_transform([0.5, 0, 0, 0.5, 0.25, 0.5])
pts = frac.generate(n_points=100_000)

# 2. Dimension estimators
D_box  = frac.calculate_fractal_dimension('boxcount')
D_spec = frac.calculate_fractal_dimension('spectral')

# 3. Laser scattering
laser = LaserPulseSimulator(I0=1.0, omega=2*np.pi, beta=0.05)
field, t_vals, x_vals = laser.interact_with_fractal(pts)
D_scatter = laser.analyze_response(field)[0]   # should be ≈ D_spec


Klenio Araujo Padilha.  
"Reformulating Transformers for LLMs: A Quaternionic-Harmonic Framework with Empirical Validation".  
Zenodo, 2025. https://doi.org/10.5281/zenodo.17171112


@software{padilha2025qht,
  author  = {Padilha, Klenio A.},
  title   = {Quaternionic-Harmonic Transformer: Fractal & Laser-Interaction Sandbox},
  year    = {2025},
  doi     = {10.5281/zenodo.17171112},
  url     = {https://doi.org/10.5281/zenodo.17171112},
  license = {GPL-3.0}
}

Contributing & roadmap

    3-D spectral dimension estimator (currently 2-D only)
    GPU-accelerated box-counting with CuPy
    Replace Matplotlib plotting with interactive Plotly widgets
    Add quaternion-valued transformer blocks that learn IFS parameters end-to-end

Pull-requests welcome—open an issue first to discuss design.
