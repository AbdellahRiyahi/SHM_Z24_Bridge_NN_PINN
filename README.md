# Data-Driven and Physics-Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)
![Framework: TensorFlow](https://img.shields.io/badge/Framework-TensorFlow%202.13-orange.svg)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19701402.svg)](https://doi.org/10.5281/zenodo.19701402)

Companion repository for the manuscript: **“Data-Driven and Physics-Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge”**  
**Abdellah Riyahi**, Mohammed Mestari, Bouchra Bouihi — *Journal of the Civil Engineering Forum (JCEF), https://journal.ugm.ac.id/v3/JCEF/article/view/24173*.

---

## Abstract

This repository provides a **reproducible** comparison between (i) **three data‑driven neural classifiers** (NN V1–V3) and (ii) a **Physics‑Informed Neural Network** (PINN V1) for vibration‑based **multi‑class damage‑state classification** on the Z24 Bridge benchmark (Switzerland).  
To guarantee a like‑for‑like evaluation, all models are trained and tested on **windows extracted from the same vertical accelerometer channel** (**`acc_09`**, near mid‑span) with an **identical, frozen train/validation/test split** and a unified preprocessing pipeline.

---

## Contents

- [Repository at a glance](#repository-at-a-glance)
- [Dataset and data access](#dataset-and-data-access)
- [Methodology and experimental protocol](#methodology-and-experimental-protocol)
- [Results (as reported in the manuscript)](#results-as-reported-in-the-manuscript)
- [Reproducing the experiments](#reproducing-the-experiments)
- [Outputs and artifacts](#outputs-and-artifacts)
- [Citation](#citation)
- [License](#license)
- [Acknowledgments](#acknowledgments)

---

## Repository at a glance

**Implemented models**
- **NN V1–V3 (data‑driven)**: successive MLP variants reflecting progressive improvements in **depth**, **optimization**, and **regularization**.
- **PINN V1 (physics‑informed)**: a hybrid model that regularizes learning by embedding the governing equation of a **damped SDOF oscillator** into the loss.

**Core design choice (fairness)**
- **Single‑sensor protocol:** all models use the same channel (`acc_09`) and the same split indices to enable a controlled comparison.

---

## Dataset and data access

### Z24 EMS dataset
The experiments use the **Z24 EMS** dataset (MATLAB `.mat` files) collected under healthy, environmental, and progressive damage scenarios. The dataset is commonly organized as multiple “setups” (e.g., healthy state “01” and progressively damaged states “03–17”), which naturally supports supervised classification of structural conditions.

### Data redistribution policy
This repository **does not redistribute** the Z24 EMS dataset. You must obtain it from **KU Leuven — SIMCES / Structural Mechanics Section** under their terms. Keep the canonical directory layout (e.g., `DatasetPDT/<class>/avt/*.mat`) as used in the project.

### Frozen split manifest (reproducibility)
All results in the manuscript are tied to a frozen processed‑data index (JSON manifest) and split:
- **EMS manifest ID:** `4b8b4d63c00f3a81`

---

## Methodology and experimental protocol

### Sensor selection (single‑channel benchmark)
We retain a single vertical accelerometer (**`acc_09`**, near mid‑span), selected via a quantitative reliability screen:
1. **< 1%** missing samples across the healthy class “01”
2. Stable low‑frequency PSD peaks within **±5%** across “01” setups
3. **SNR > 20 dB** under ambient conditions

The same sensor is used for **NN V1–V3** and **PINN V1** to ensure like‑for‑like comparability.

### Segmentation and labeling
- Long recordings are segmented into fixed‑length windows of **N = 65,536 samples**, using **overlapping sliding windows** to increase the number of training examples and improve class balance.
- Labels follow the progressive test protocol, with **“01”** as the healthy reference and **“03–17”** as increasing damage levels (multi‑class task).

### Normalization and preprocessing
A unified pipeline is applied to all models:
- **z‑score standardization** (fit on the training set only)
- fixed window length and hop
- frozen label encoding
- constant split indices across all models

### Splits
The manuscript reports stratified sampling with a **70% / 15% / 15%** train/validation/test division, with reproducibility ensured through cached `.npz` exports and JSON manifests.

---

## Results (as reported in the manuscript)

The following headline results summarize the manuscript’s quantitative comparison:

- **Validation (Table 4):** Among data‑driven models, **NN V3** achieves Val. Accuracy ≈ **97.7%** with macro ROC–AUC ≈ **1.000**; all NN variants report **0 false negatives** on validation.
- **Held‑out test (Table 5):** NN **V1/V2/V3** reach **98.62% / 97.71% / 99.54%** accuracy, with macro ROC–AUC **0.991 / 0.998 / 1.000**.  
  **PINN V1** achieves ≈ **92%** test accuracy while maintaining low physics residuals (e.g., ‖h‖₂ ≈ 1.205×10⁻³; P95(|h|) ≈ 6.66×10⁻²) and coherent low‑frequency behavior (<10 Hz).
- **Trade‑off (Table 6):** NN V3 is preferred in data‑rich regimes; PINN V1 is advantageous when physical credibility and robustness are prioritized.

> In SHM, we emphasize metrics beyond accuracy (confusion matrices, macro precision/recall/F1, macro AUC) to explicitly monitor **false negatives**.

---

## Reproducing the experiments

### 1) Environment setup

**Conda (recommended):**
```bash
conda create -n z24-shm python=3.10 -y
conda activate z24-shm
pip install -U pip wheel
pip install tensorflow==2.13 numpy scipy pandas scikit-learn matplotlib jupyter ipykernel
```

### 2) Place the dataset

Ensure your local dataset follows the expected structure (example):
```
DatasetPDT/<class>/avt/*.mat
```

If your notebooks expect a specific root (e.g., `spinn_project/DatasetPDT/`), either:
- place the dataset there, **or**
- create a symlink, **or**
- adapt the dataset root variable inside the notebooks.

### 3) Run the notebooks

1. **Train/evaluate data‑driven models**
   - Run `NN_V1.ipynb` → `NN_V2.ipynb` → `NN_V3.ipynb`
2. **Train/evaluate physics‑informed model**
   - Run `PINN_V1.ipynb`
3. **Build paper artifacts (tables and figures)**
   - Run `reviewer_build_artifacts.ipynb` to regenerate consolidated tables/figures used in the manuscript.

---

## Outputs and artifacts

Expected outputs (file patterns may vary slightly by run/date):

- **NN metrics & predictions**
  - `reviewer_pack/metrics_comparison_rounded.csv`
  - `reviewer_pack/metrics_per_class_rounded.csv`
  - `reviewer_pack/false_negatives.csv`
  - `reviewer_pack/predictions_NN_*.npz`
- **PINN diagnostics**
  - `outputs/artifacts/pinn_kpis_*.json`
  - `outputs/artifacts/pinn_diag_*.npz`
- **Tables & figures**
  - `outputs/tables/table4_*.csv`, `table5_*.csv`, `table6_*.csv`
  - `outputs/figs/*.png` (training curves, confusion matrices, dashboards)

---

## Citation

If you use this repository or build on the manuscript, please cite:

```bibtex
@article{
  title   = {Data-Driven and Physics-Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge},
  author  = {Abdellah Riyahi, Mohammed Mestari, Bouchra Bouihi},
  journal = {Journal of the Civil Engineering Forum (JCEF)} https://journal.ugm.ac.id/v3/JCEF/article/view/24173,
  year    = {2026},
  doi     = {0.22146/jcef.24173}
}
```

---

## License

- **Code / notebooks / documentation:** **CC BY 4.0** (Creative Commons Attribution 4.0 International).
- **Data:** The Z24 EMS dataset remains the property of **KU Leuven — SIMCES / Structural Mechanics Section** and is **not redistributed** here.

---

## Acknowledgments

We thank KU Leuven’s **Structural Mechanics Section (SIMCES)** for access to the Z24 benchmark and **HPC‑MARWAN (CNRST Morocco)** for computational resources.
