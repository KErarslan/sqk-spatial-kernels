# Distance-Encoded Quantum Kernels for Spatial Estimation

**Paper:** Erarslan, K. (2026). Distance-Encoded Quantum Kernels for Spatial Estimation: Design Criteria and NISQ Hardware Characterisation. *Computers & Geosciences* (under review).

**Author:** Kaan Erarslan — Department of Mining Engineering, Kütahya Dumlupınar University, Turkey  
**ORCID:** [0000-0002-1875-4009](https://orcid.org/0000-0002-1875-4009)

---

## Overview

This repository provides the Python implementation (Google Colab notebook) for the quantum kernel kriging experiments reported in the paper. Three quantum kernel families are evaluated as replacements for variogram-based covariance matrices in ordinary kriging:

| Kernel | Description | Hardware |
|---|---|---|
| ZZFeatureMap / PauliFeatureMap | Standard feature maps | Sim only |
| SK_rx | Scalar distance kernel (Euclidean encoding) | IBM (marrakesh, fez, kingston) |
| ASK | Anisotropic scalar kernel (ellipsoidal h_e encoding) | IBM (kingston) |

Validation datasets: **Kalgoorlie Northern Zone** gold (n = 20, 3D) and **Seyitömer Aslanlı** lignite (n = 191, 2D), differing 85× in spatial scale.

---

## Repository Structure

```
sqk-spatial-kernels/
├── SpatialQuantumKernels.ipynb   # Main Colab notebook (all experiments)
├── requirements.txt              # Python dependencies
├── kalgoorlie_variogram_params.json   # Fitted variogram parameters
├── seyitomer_variogram_params.json    # Fitted variogram parameters
└── ibm_quantum_job_ids.txt            # IBM Quantum job IDs
└── README.md
```

---

## Quick Start

### Option 1 — Google Colab (recommended)

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1r17-Ha1gWVu8TSnClTC3EMgZiBBARRQ_?usp=sharing)

### Option 2 — Local installation

```bash
git clone https://github.com/KErarslan/sqk-spatial-kernels.git
cd sqk-spatial-kernels
pip install -r requirements.txt
jupyter notebook SpatialQuantumKernels.ipynb
```

---

## Dependencies

```
qiskit >= 1.0
qiskit-ibm-runtime >= 0.20
numpy >= 1.24
scipy >= 1.10
scikit-learn >= 1.3
matplotlib >= 3.7
```

---

## IBM Quantum Hardware

IBM Quantum access was provided through the **IBM Quantum Open Plan**.  
Three Eagle r3 processors were used (ibm_marrakesh, ibm_fez, ibm_kingston; 156 qubits each).

All IBM Quantum job IDs are listed in `data/ibm_quantum_job_ids.txt`.

Key hardware results:

| Experiment | Backend | Shots | α | Norm. | λ_min | R² | Status |
|---|---|---|---|---|---|---|---|
| SK_rx | ibm_marrakesh | 1,024 | — | d_max | −0.0754 | −0.662 | ✗ PSD |
| SK_rx | ibm_fez | 1,024 | — | d_max | −0.0598 | −34.23 | ✗ PSD |
| SK_rx | ibm_kingston | 8,192 | — | d_max | −0.0185 | −28.77 | ✗ PSD |
| ASK | ibm_marrakesh | 2,048 | 1.7* | d_max | −0.0044 | −20.93 | ✗ encoding error† |
| ASK | ibm_kingston | 4,096 | 1.7 | d_max | +0.0038 | 0.569 | ✓ |
| ASK | ibm_kingston | 4,096 | 2.5 | d_max | −1.624 | — | ✗ non-monotone |
| ASK | ibm_kingston | 4,096 | 0.5 | HE_MAX | +0.0048 | 0.564 | ✓ |

† ibm_marrakesh encoding error (2θ instead of θ): see Supplementary S2.

---

## Dataset Access

**Kalgoorlie Northern Zone** (n = 20 RC drillholes, Au g/t):  
Coordinates and log-transformed grades are embedded in the notebook.  
Data used with permission of Riversgold Ltd. (Riversgold, 2022).

**Seyitömer Aslanlı Lignite** (n = 191 drillholes, seam thickness and calorific value):  
Data used with permission of Kütahya Şeker Fabrikası A.Ş.  
*Note: Raw drill data cannot be shared; fitted variogram parameters are provided in `data/`.*

---

## Fitted Variogram Parameters

See `data/kalgoorlie_variogram_params.json` and `data/seyitomer_variogram_params.json`.

**Kalgoorlie (spherical model, log Au g/t):**
- C₀ = 0.015, C = 0.206, a_min = 28.1 m (0°), a_max = 66.0 m (90°), f_V = 0.4

**Seyitömer Thickness (isotropic spherical):**
- C₀ = 0.421, C = 0.774, a = 1,245 m

**Seyitömer Calorific (anisotropic spherical):**
- C₀ = 0.031, C = 0.774, a_min = 317 m (0°), a_max = 1,245 m (90°)

---

## Citation

```bibtex
@article{erarslan2026sqk,
  title   = {Distance-Encoded Quantum Kernels for Spatial Estimation:
             Design Criteria and {NISQ} Hardware Characterisation},
  author  = {Erarslan, Kaan},
  journal = {Computers \& Geosciences},
  year    = {2026},
  note    = {Under review}
}
```

---

## License

MIT License. See [LICENSE](LICENSE).

Data from Kalgoorlie and Seyitömer datasets are used with permission and are not redistributed under this license.
