# SiC-LGAD
**Defect-Aware Silvaco TCAD Modeling of Irradiated 4H-SiC LGADs for HL-LHC 4D Tracking**

[![Silvaco TCAD](https://img.shields.io/badge/Simulator-Silvaco_Victory_Device-blue)]()
[![Material](https://img.shields.io/badge/Material-4H--SiC-green)]()
[![Target](https://img.shields.io/badge/Target-HL--LHC_Timing-orange)]()
[![Status](https://img.shields.io/badge/Status-Calibrating_SICAR-red)]()

---

# 📑 Executive Summary

This repository contains the **Silvaco Victory TCAD** simulation decks, numerical solver strategies, and Python analysis pipelines for modeling thin and ultra-thin **4H-SiC Low-Gain Avalanche Detectors (LGADs)**. The project aims to establish a physically credible, multi-foundry calibrated TCAD model capable of predicting the radiation hardness and timing performance of 4H-SiC detectors operating in high-luminosity collider environments with fluences exceeding **10¹⁵ n<sub>eq</sub>/cm²**.

**Current Focus:** Rigorous electrostatic and transient calibration against the **SICAR (IHEP China)** experimental datasets using a custom-built wide-bandgap physics stack and decoupled numerical solver strategies.

---

# 🔬 1. Scientific Motivation & The TCAD Challenge

Silicon LGADs currently serve as the baseline technology for precision 4D tracking (space + time) in experiments such as **ATLAS** and **CMS**. However, prolonged irradiation results in severe acceptor removal, increased leakage current, and progressive gain degradation.

The superior material properties of **4H-SiC**, including its wide bandgap (~3.25 eV), high critical electric field (~3 MV/cm), excellent thermal conductivity, and inherently low leakage current, make it one of the strongest candidates for future radiation-hard timing detectors.

## The Wide-Bandgap TCAD Challenge

Conventional silicon TCAD workflows fail when directly applied to 4H-SiC. This repository implements a rigorously validated physics stack to overcome the primary numerical and physical challenges associated with wide-bandgap device simulation:

1. **Extremely Low Intrinsic Carrier Concentration**

   At 300 K,

   > nᵢ ≈ 10⁻⁹ cm⁻³

   Empirical intrinsic carrier concentration models combined with Boltzmann statistics frequently produce divide-by-zero failures.

2. **Floating Point Dynamic Range**

   The ratio

   > N<sub>D</sub> / nᵢ ≈ 10²³

   exceeds the practical numerical stability of conventional double precision during breakdown simulations.

3. **Incomplete Ionization**

   Aluminum acceptors possess deep ionization energies (≈0.20–0.32 eV). Only a fraction of implanted dopants become electrically active at room temperature, making incomplete ionization essential for accurate electric field prediction.

4. **Anisotropic Impact Ionization**

   Avalanche multiplication strongly depends on crystal orientation and therefore requires anisotropic impact ionization models rather than silicon-derived isotropic approximations.

---

# ⚙️ 2. Physics Stack

Every physical model implemented in this repository is directly traceable to peer-reviewed literature or the Silvaco Victory Device documentation. Arbitrary parameter tuning is intentionally avoided.

## 2.1 Statistics & Bandgap Narrowing

- **Fermi–Dirac Statistics (`fermidirac ni.fermi`)**
  - Self-consistent intrinsic carrier concentration calculation.
  - Prevents numerical instability caused by empirical nᵢ models.

- **Lindefelt Bandgap Narrowing (`bgn.lindefelt`)**
  - Wide-bandgap calibrated replacement for the silicon Slotboom model.
  - Independent treatment of n-type and p-type bandgap narrowing.

## 2.2 Incomplete Ionization

- **Two-Level Incomplete Ionization (`inc.two_level`)**
  - Models aluminum acceptors occupying cubic and hexagonal lattice sites.
  - Includes nitrogen donor activation energies.

- **Bound Trap Formalism (`bound.trap`)**
  - Couples bound states with SRH recombination for physically consistent carrier statistics.

## 2.3 Impact Ionization

- **Hatakeyama Anisotropic Model (`impact aniso sic4h0001`)**

- **Important Options**
  - `gradqfl`
    - Uses quasi-Fermi gradients instead of electric field alone.
    - Improves avalanche gain prediction under carrier transport.
  - `e.side`
    - Evaluates electric fields at element interfaces.
    - Improves convergence near high-field regions.

## 2.4 Deep-Level Defects & Interface

Current implementation includes:

- Z₁/₂ defect center
- EH₆/₇ defect center
- Trap-assisted tunneling (Hurkx model)
- SiO₂/4H-SiC interface charge
- Interface recombination
- As-oxidized interface models without post-oxidation annealing

---

# 🧮 3. Numerical Solver Strategies

Accurate 4H-SiC simulations require significantly greater numerical care than conventional silicon TCAD.

The repository incorporates validated numerical strategies including:

| Numerical Challenge | Implemented Solution |
|---------------------|----------------------|
| Slow equilibrium convergence | Two-stage initialization |
| Breakdown instability | 256-bit precision arithmetic |
| Newton divergence | Adaptive solver tolerances |
| Trap-assisted tunneling convergence | Decoupled simulation decks |
| Post-breakdown instability | Multi-stage bias stepping |
| Gain-layer punch-through | Deferred impact ionization activation |

---

# 🎯 4. Calibration Workflow

## Active Calibration Target

**SICAR (IHEP China)**

Current work focuses on reproducing the published pre-irradiation electrical characteristics of the SICAR 4H-SiC LGAD structure, providing a reliable baseline for future irradiation studies.

## Secondary Validation

**ONSEMI**

Published first-generation ONSEMI devices are used as an additional validation benchmark following successful SICAR calibration.

## Calibration Sequence

1. Geometry validation
2. Mesh convergence
3. Electrostatic calibration (C–V)
4. Leakage current calibration (I–V)
5. Avalanche calibration
6. Charge collection efficiency
7. Transient timing simulations

---

# 🚀 5. Running Simulations

## Requirements

- Silvaco Victory Device
- Victory Process
- DeckBuild
- TonyPlot
- Python 3.8+
- NumPy
- SciPy
- Pandas
- Matplotlib

Production simulations are intended to be executed using high-precision arithmetic with parallel solver support.

---

# 🖥️ 6. Hardware Notes

The simulation workflow has been optimized for workstation-class hardware.

Typical development platform:

- Intel Core i5-14500
- 32 GB RAM

Large two-dimensional simulations involving trap-assisted tunneling and edge termination structures may require significant memory. Quasi-one-dimensional approximations are therefore employed during early-stage parameter optimization.

---

# 📚 7. Key References

### Experimental Datasets

- **SICAR (IHEP China)** — Current primary calibration dataset.
- **ONSEMI** — First-generation 4H-SiC LGAD validation.

### Physics Models

- Hatakeyama et al. — Anisotropic impact ionization in 4H-SiC.
- Castaldini et al. — Radiation-induced deep-level defects.
- Cartiglia et al. — Ultra-Fast Silicon Detector timing theory.
- Carulla et al. — Compensated LGAD gain layer design.

---

# 📖 Citation

If this repository contributes to your research, please cite it using the repository's **CITATION.cff** metadata or GitHub's built-in citation feature.

---

# 📄 License

This project is licensed under the **GNU General Public License v3.0 (GPL-3.0)**.

---

# ⚠️ Disclaimer

This repository is an active research project. Simulation models, calibration parameters, numerical methods, and documentation are continuously refined as additional validation data and experimental measurements become available.
```

