# Data-Driven and Physics-Informed Neural Networks for SHM of the Z24 Bridge

**Official code and companion material** for:

> *Data-Driven and Physics-Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge*  
> Abdellah Riyahi, Mohammed Mestari, Bouchra Bouihi — Journal of the Civil Engineering Forum (JCEF), 2026 *(accepted, in copyediting)*

---

## Overview

This repository compares two learning paradigms for vibration-based SHM on the **Z24 Bridge** benchmark:

- **NN V1–V3 (data-driven):** three progressively improved MLP classifiers (depth/regularization/optimization).  
- **PINN V1 (hybrid):** a physics-informed model embedding the damped SDOF oscillator as a constraint on the learned displacement surrogate.

All experiments use the **same sensor channel** *(vertical accelerometer `acc_09`, near mid-span)* and an identical preprocessing/split pipeline for fair, like-for-like comparison.

**Key outcomes (paper figures & tables reproduced here):**
- **NN V3** delivers the strongest predictive performance *(val. accuracy ≈ 97.7%, macro ROC–AUC ≈ 1.00)*.  
- **PINN V1** yields **low physics residuals** and a **coherent low-frequency spectral signature**, trading a bit of accuracy for robustness and interpretability.

---

## Repository layout

```
spinn_project/
├── NN_V1.ipynb           # Data-driven baseline (MLP)
├── NN_V2.ipynb           # Deeper + regularized variant
├── NN_V3.ipynb           # Best data-driven variant
├── PINN_V1.ipynb         # Physics-informed model (SDOF constraint)
├── reviewer_build_artifacts.ipynb   # Builds Tables 4–6 + figures
├── outputs/              # Exported figures, metrics, tables
├── models/               # (optional) saved weights/checkpoints
└── reviewer_pack/        # CSV/JSON used to assemble tables
```

> **License:** CC BY 4.0 (see *LICENSE*).  
> **Dataset:** **not redistributed** here. See “Data access” below.

---

## Environment

- **Python** ≥ 3.10  
- **Core stack:** TensorFlow 2.13, NumPy, SciPy, scikit-learn, Pandas, Matplotlib

Example (conda + pip):

```bash
conda create -n z24-shm python=3.10 -y
conda activate z24-shm
pip install tensorflow==2.13.* numpy scipy scikit-learn pandas matplotlib
```

---

## Data access (Z24 EMS)

The Z24 EMS dataset is available through the **SIMCES / KU Leuven** distribution. You must obtain the original `.mat` files independently.

- **EMS manifest ID used in this study (frozen split):** `4b8b4d63c00f3a81`
- Set an environment variable (or edit notebooks) to your local path, e.g.:
  ```bash
  export Z24_EMS_ROOT=/path/to/Z24/EMS
  ```

We do **not** redistribute any raw data in this repository.

---

## Reproducing paper results

1. **Train/evaluate data-driven models:**  
   Open `NN_V1.ipynb`, `NN_V2.ipynb`, and `NN_V3.ipynb`, run all cells.  
   Exports: training curves, confusion matrices, and metrics into `outputs/` and `reviewer_pack/`.

2. **Run the physics-informed model:**  
   Open `PINN_V1.ipynb` and run all cells.  
   Exports: physics residual diagnostics (‖h‖₂, P95(|h|)), spectral checks, and KPI JSON/NPZ.

3. **Assemble Tables 4–6 (paper):**  
   Run `reviewer_build_artifacts.ipynb`.  
   Exports: consolidated CSV/MD/DOCX in `outputs/tables/` and figures in `outputs/figs/`.

---

## Notes on fairness & comparability

- **Single channel** (`acc_09`) across **all models**.  
- **Unified preprocessing:** fixed window/hop, train-set z-score only, frozen label encoding, identical train/val/test indices.  
- External baselines (MiniRocket, WaveNet) are discussed qualitatively; numbers from literature are **not** claimed as head-to-head due to preprocessing/split sensitivity.

---

## Citation

If you use this code or build on our findings, please cite:

```bibtex
@article{riyahi2026shm,
  title   = {Data-Driven and Physics-Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge},
  author  = {Riyahi, Abdellah and Mestari, Mohammed and Bouihi, Bouchra},
  journal = {Journal of the Civil Engineering Forum (JCEF)},
  year    = {2026},
  note    = {accepted, in copyediting}
}
```

---

## Acknowledgments

We thank **KU Leuven – Structural Mechanics Section** for access to the Z24 dataset and **HPC-MARWAN (CNRST Morocco)** for compute resources that supported the experiments.

---

### Suggested GitHub “Description” (one-liner)

> Code and artifacts for *Data-Driven & Physics-Informed Neural Networks for SHM of the Z24 Bridge* (JCEF 2026): NN V1–V3 vs PINN V1, unified preprocessing, reproducible Tables 4–6.

### Suggested GitHub “Topics”

`structural-health-monitoring` `physics-informed-neural-networks` `pinn` `deep-learning` `time-series` `civil-engineering` `z24-bridge` `tensorflow` `mlp`
