# Data-Driven and Physics-Informed Neural Networks for SHM of the Z24 Bridge

> Companion repository for the manuscript **“Data‑Driven and Physics‑Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge”**  
> *(Riyahi · Mestari · Bouihi — Journal of the Civil Engineering Forum, copyediting stage)*

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)]()
[![TF](https://img.shields.io/badge/TensorFlow-2.13%2B-orange.svg)]()

---

## TL;DR
We compare **purely data‑driven NNs (NN V1–V3)** and a **hybrid Physics‑Informed Neural Network (PINN V1)** for vibration‑based SHM on the **Z24 Bridge** benchmark.  
All models use the **same single accelerometer channel** (acc_09, near mid‑span) and a **frozen train/val/test split** to ensure a like‑for‑like comparison.

- **Best data‑driven model (NN V3):** Val. Acc. ≈ **97.7%**, macro ROC–AUC ≈ **1.000**.  
- **PINN V1 (hybrid):** lower test accuracy (~**92%**) but **low physics residuals** and **consistent low‑frequency PSD**, providing **interpretability & robustness**.

---

## Repository Layout

```
spinn_project/
├── NN_V1.ipynb                  # Data-driven baseline
├── NN_V2.ipynb                  # Deeper + regularized
├── NN_V3.ipynb                  # Final data-driven model (best)
├── PINN_V1.ipynb                # Physics-informed model (SDOF prior)
├── reviewer_build_artifacts.ipynb
│   └── Builds Tables 4–6, figures & manifest for the paper
├── outputs/                     # Exported metrics/figures/artifacts (generated)
├── reviewer_pack/               # CSV/JSON used to assemble tables (generated)
└── transfer_safe/               # (optional) helper folder for moving artifacts
```

> **Note**: The **dataset is NOT redistributed** here. See _Data Access_ below.

---

## Getting Started

### 1) Environment

You can use either Conda or pure `pip`.

**Conda**
```bash
conda create -n z24-shm python=3.10 -y
conda activate z24-shm
pip install -U pip wheel
pip install tensorflow==2.13 numpy scipy pandas scikit-learn matplotlib jupyter ipykernel
```

**Pip (virtualenv)**
```bash
python -m venv .venv
source .venv/bin/activate  # (Windows: .venv\Scripts\activate)
pip install -U pip wheel
pip install tensorflow==2.13 numpy scipy pandas scikit-learn matplotlib jupyter ipykernel
```

### 2) Data Access (Z24 EMS)
- Obtain the original **Z24 EMS** MATLAB files from **KU Leuven – SIMCES / Structural Mechanics Section** (usage subject to their terms).
- Keep the original directory layout (per class folders `01/`, `03/`, …) and point the notebooks to your local path.
- **EMS manifest ID (frozen split used in the paper):** `4b8b4d63c00f3a81`.

> The repo **does not** store or redistribute Z24 data.

### 3) Reproduce the Results
1. Open and run the notebooks in order:
   - `NN_V1.ipynb` → `NN_V2.ipynb` → `NN_V3.ipynb`  
   - `PINN_V1.ipynb`
2. Build all tables/figures required by the paper:
   - `reviewer_build_artifacts.ipynb` (produces **Tables 4–6** and the figure set)
3. Exports appear under `outputs/` and `reviewer_pack/` with timestamps.

---

## What’s Inside the Notebooks?

### NN V1–V3 (data‑driven)
- MLP classifiers with increasing **depth**, **regularization**, and **training refinements**.
- Common preprocessing for fairness: fixed windowing, `z-score` normalization (fit on train only), frozen label encoding.
- Exports: learning curves, confusion matrices, per‑class metrics, and predictions (`.npz`).

### PINN V1 (hybrid)
- Learns a displacement surrogate **u(t)** regularized by the SDOF **damped oscillator** equation of motion.
- Loss = data term + physics residual; reports **‖h‖₂ (mean)** and **P95(|h|)**, plus spectral checks near the **first natural frequency**.
- Emphasizes **physical consistency** and **interpretability** over marginal accuracy gains.

---

## Key Results (paper summary)

- **Validation (Table 4):** NN V3 leads among data‑driven models (Val. Acc. ≈ 97.7%, macro ROC‑AUC ≈ 1.000).  
- **Test set (Table 5):** NN V3 ≈ 99.54% accuracy; PINN V1 ≈ 92% with low residuals and coherent spectrum < 10 Hz.  
- **Comparative (Table 6):** We relate our models to **MiniRocket** and **WaveNet** as literature baselines; we avoid strict numerical claims against external setups and emphasize **fair, common preprocessing & splits** for internal comparisons.

---

## Reproducibility Notes
- **Single sensor** used throughout (vertical accelerometer **acc_09**, near mid‑span) selected via a reliability screen:  
  `<1%` missing samples (class 01), **±5%** PSD peak stability, **SNR > 20 dB** under ambient conditions.
- Fixed **train/val/test indices** across all models; figures and tables are regenerated from saved artifacts.
- HPC runs (HPC‑MARWAN) may need minor tweaks to batch size / CPU threads.

---

## How to Cite

If you use this repository, please cite the paper:

```bibtex
@article{riyahi2026shm,
  title   = {Data-Driven and Physics-Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge},
  author  = {Riyahi, Abdellah and Mestari, Mohammed and Bouihi, Bouchra},
  journal = {Journal of the Civil Engineering Forum},
  year    = {2026},
  note    = {Accepted, copyediting stage}
}
```

You may also reference this repository:

```text
Riyahi A., Mestari M., Bouihi B. (2026). Companion code for “Data‑Driven and Physics‑Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge”. GitHub repository. CC BY 4.0.
```

---

## License

- **Code, notebooks, and documentation** are released under **Creative Commons Attribution 4.0 International (CC BY 4.0)**.  
  You are free to share and adapt with attribution. See `LICENSE`.
- **Data** are owned by their respective providers (**KU Leuven – Structural Mechanics Section / SIMCES**). Do **not** redistribute the Z24 dataset in this repository.

---

## Acknowledgments
- KU Leuven – **Structural Mechanics Section** for access to the Z24 benchmark.  
- **HPC‑MARWAN** (CNRST, Morocco) for computational resources.

---

## Contact
- Abdellah Riyahi — <abdellah.riyahi@…>  
- Issues and questions: please open a GitHub Issue.
