# Data-Driven and Physics-Informed Neural Networks for SHM of the Z24 Bridge — Companion Code

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)
![Topic: SHM](https://img.shields.io/badge/Topic-Structural%20Health%20Monitoring-forestgreen.svg)

> Companion repository for the paper  
> **Data-Driven and Physics-Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge (JCEF)**.

This repository provides the notebooks and helper assets to reproduce the key figures and tables—training curves, confusion matrices, consolidated metrics, and physics diagnostics—for the comparison between **three data‑driven neural networks (NN V1–V3)** and a **Physics‑Informed Neural Network (PINN V1)** on the **Z24 Bridge** benchmark.

> **Data notice** — The Z24 dataset is **not distributed** here. You must supply your own local copy and configure paths/manifest IDs in the notebooks. See **Data (not provided)** below.

---

## Table of Contents

- [Repository Layout](#repository-layout)
- [Requirements](#requirements)
- [Quick Start](#quick-start)
- [Data (not provided)](#data-not-provided)
- [Reproducing the Paper’s Artifacts](#reproducing-the-papers-artifacts)
- [Notes on Fairness & Reproducibility](#notes-on-fairness--reproducibility)
- [License](#license)
- [How to Cite](#how-to-cite)
- [Acknowledgments](#acknowledgments)
- [Contact](#contact)

---

## Repository Layout

```
spinn_project/
├─ NN_V1.ipynb
├─ NN_V2.ipynb
├─ NN_V3.ipynb
├─ PINN_V1.ipynb
├─ Comparisons_NNv1_NNv2_NNv3.ipynb
├─ reviewer_build_artifacts.ipynb
├─ outputs/                 # figures/tables/metrics exported by notebooks
├─ reviewer_pack/           # consolidated CSV/JSON for tables & review artifacts
├─ figs/                    # (optional) additional figures
├─ models/                  # (optional) saved model weights
└─ outputs_pinn_v2_3/       # PINN diagnostics (npz/json) if produced
```

**Notebooks overview**
- **`NN_V1.ipynb`, `NN_V2.ipynb`, `NN_V3.ipynb`** — Train & evaluate each NN variant; export metrics, confusion matrices, and training histories.  
- **`PINN_V1.ipynb`** — Train & evaluate the physics‑informed model; export physics diagnostics (‖h‖₂, P95(|h|), ω₀, ω_est).  
- **`Comparisons_NNv1_NNv2_NNv3.ipynb`** — Generate side‑by‑side plots (train/val curves) and confusion matrices.  
- **`reviewer_build_artifacts.ipynb`** — Consolidate per‑model outputs into camera‑ready **Table 4/5/6** (CSV + Markdown) and pack a ZIP for submission.

---

## Requirements

- Python **3.10+**
- Recommended stack:
  - `tensorflow` / `keras`
  - `numpy`, `scipy`, `pandas`, `scikit-learn`
  - `matplotlib`
- GPU is optional but recommended for faster training

> If you do not use a `requirements.txt`, install the packages above with `pip`.

---

## Quick Start

1. **Clone** this repository and create a clean Python environment.  
2. **Configure your Z24 data** (dataset root or EMS manifest) in the first cell(s) of each notebook.  
3. **Run the NNs**: open `NN_V1.ipynb`, `NN_V2.ipynb`, `NN_V3.ipynb` and run all cells.  
   - Each notebook writes metrics & figures to `outputs/` and `reviewer_pack/`.  
4. **Run the PINN**: open `PINN_V1.ipynb` and run all cells.  
   - Physics diagnostics (‖h‖₂, P95(|h|), ω₀, ω_est) are exported to `outputs/` and (optionally) `outputs_pinn_v2_3/`.  
5. **Build camera‑ready tables/figures**: open `reviewer_build_artifacts.ipynb` and run all cells.  
   - It consolidates **Table 4/5/6** (CSV + Markdown) and can produce a ZIP in `outputs/`.

---

## Data (not provided)

- This repository **does not** include the Z24 dataset.  
- Use your local copy and configure:
  - **Dataset root** (e.g., `/path/to/Z24/`) **or** your **EMS manifest ID/path**.
  - A **common train/val/test split** shared across NN V1–V3 and PINN V1 (fair comparison).
- The build notebook includes helper cells to **echo the active EMS manifest ID** for traceability.

> **Sensor note** — Experiments use a single vertical accelerometer (**acc_09**, near mid‑span) chosen via a quantitative reliability screen (<1% missing samples on class 01, PSD peak stability ±5% on class 01, SNR > 20 dB). The same channel is used across all models.

---

## Reproducing the Paper’s Artifacts

- **Figures 3–5** — Training vs. validation curves  
  Run the NN notebooks (to export histories), then run `Comparisons_NNv1_NNv2_NNv3.ipynb` to generate overlaid plots with two‑column readability (larger fonts/line widths; distinct train/val colors).

- **Table 4** — Validation metrics (NN) & physics diagnostics (PINN)  
  After executing the NN and PINN notebooks, run `reviewer_build_artifacts.ipynb` to export a consolidated CSV/MD table.

- **Table 5** — Test‑set summary (loss, accuracy, macro AUC)  
  Produced by the same build notebook from the saved predictions/metrics in `outputs/`.

- **Table 6** — Comparative summary vs. literature baselines  
  Built from the consolidated metrics and curated baseline references (no external code claims of superiority).

All generated files are written under `outputs/` and `reviewer_pack/`.

---

## Notes on Fairness & Reproducibility

- **Unified preprocessing**: fixed window length & hop; `z`‑score normalization **fitted on training only**; frozen label encoding.  
- **Constant splits**: identical train/val/test indices across NN V1–V3 and PINN V1.  
- **Seeds**: set where relevant (subject to typical DL nondeterminism).  
- **No dataset shipping**: provide your own Z24 copy and keep a record of your **EMS manifest ID** / dataset version.

---

## License

This work is released under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license.  
You are free to share and adapt the material for any purpose, even commercially, as long as appropriate credit is given.

- Human‑readable summary: https://creativecommons.org/licenses/by/4.0/  
- Legal code: https://creativecommons.org/licenses/by/4.0/legalcode

---

## How to Cite

If you use this code, please cite the paper and optionally this repository.

> Riyahi, A., Mestari, M., & Bouihi, B. (2026).  
> *Data‑Driven and Physics‑Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge.*  
> Journal of the Civil Engineering Forum (JCEF).

---

## Acknowledgments

We thank the Z24 community for maintaining this benchmark and the JCEF reviewers/editors for feedback that improved clarity and robustness.

---

## Contact

**Abdellah Riyahi** — Project maintainer  
For questions or collaborations, please open an issue or reach out directly.
