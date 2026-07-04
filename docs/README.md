# Documentation & Literature

This directory contains the foundational literature, project planning documents, and weekly progress presentations that guide the physics stack and calibration targets for the 4H-SiC LGAD TCAD simulations.

## 📂 Directory Structure

### `literature/`
Core research papers and experimental datasets used to build and validate the TCAD models.
*   **Experimental Targets:** SICAR (IHEP China) and ONSEMI 4H-SiC LGAD characterization papers (I-V, C-V, and transient data).
*   **Physics Models:** Hatakeyama (anisotropic impact ionization), Lindefelt (Bandgap Narrowing), and Castaldini (deep-level defects like $Z_{1/2}$ and $EH_{6/7}$).
*   **Timing Theory:** Cartiglia et al. (UFSD timing performance, Shockley-Ramo, and jitter/Landau budgets).

### `presentations/`
Weekly group meeting slides documenting the progression of the internship.
*   Tracks the debugging journey (e.g., the transition from Silicon baseline to 4H-SiC, the Week 5 physics stack rebuild).
*   Documents numerical artifact mitigation (e.g., 256-bit precision, two-step initialization, decoupled avalanche/defect decks).

### `project_context/`
High-level project scoping and execution guidelines.
*   **Abstract:** The Heavy Flavour Meet 2026 submission outlining the compensated gain-layer and defect-aware modeling goals.
*   **Research Plan:** The staged workflow from baseline PIN to irradiated ultra-thin LGADs and JTE design.
*   **Internship Context:** Strict execution guidelines, model-selection hierarchy, and computing constraints.

---
*Note: All PDFs in this directory are tracked via Git LFS (`.gitattributes`) to keep the main repository lightweight and fast to clone.*
