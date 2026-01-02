# Data-Driven and Physics-Informed Neural Networks for Structural Health Monitoring  
## The Z24 Bridge Case Study (Switzerland)

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.13-orange.svg)](https://www.tensorflow.org/)

Official code and research artifacts for the paper:

**“Data-Driven and Physics-Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge”**  
**Authors:** Abdellah Riyahi, Mohammed Mestari, Bouchra Bouihi  
**Journal:** *Journal of Civil Engineering Forum (JCEF)* — camera-ready (submitted/accepted Aug 2025; DOI/pages pending)

> **Important** — The Z24 dataset is **not redistributed** in this repository. Please obtain it from the official provider (KU Leuven / SIMCES) and point the notebooks to your local copy.

---

## Table of contents
- [Highlights](#highlights)
- [Repository layout](#repository-layout)
- [Quick start](#quick-start)
- [Dataset (Z24 EMS)](#dataset-z24-ems)
- [Reproducing the paper results](#reproducing-the-paper-results)
- [Results summary](#results-summary)
- [Citation](#citation)
- [License](#license)
- [Contact](#contact)
- [Acknowledgements](#acknowledgements)

---

## Highlights
- **Like-for-like benchmark:** all models share the same preprocessing and the same fixed split, and use one vertical accelerometer (**acc_09**, near mid-span) to ensure fair comparison.
- **Data-driven baselines (NN V1–V3):** three progressively improved MLP classifiers.
- **Physics-informed model (PINN V1):** adds an SDOF damped-oscillator residual to the loss for physical consistency and interpretable diagnostics.
- **Reproducible artifacts:** one notebook rebuilds the tables/figures used in the manuscript.

---

## Repository layout
> Filenames match the manuscript notation.

- `NN_V1.ipynb` — baseline data-driven MLP (training + evaluation)
- `NN_V2.ipynb` — deeper MLP + improved training strategy
- `NN_V3.ipynb` — optimized MLP (best performance in the paper)
- `PINN_V1.ipynb` — Physics-Informed NN (SDOF residual diagnostics)
- `reviewer_build_artifacts.ipynb` — regenerates paper-ready tables/figures from saved outputs
- `outputs/` — figures, metrics, logs, exported tables (generated)

---

## Quick start

### 1) Create a clean environment
```bash
python -m venv .venv
# Linux/macOS
source .venv/bin/activate
# Windows
# .venv\Scripts\activate
```

### 2) Install dependencies
If you have a `requirements.txt`:
```bash
pip install -r requirements.txt
```

Otherwise, the minimal stack used in the paper:
```bash
pip install "tensorflow==2.13.*" numpy scipy pandas scikit-learn matplotlib jupyter
```

### 3) Launch Jupyter
```bash
jupyter notebook
```

---

## Dataset (Z24 EMS)

### Dataset access
This repository **does not include** Z24 `.mat` files. Obtain the dataset from the official KU Leuven SIMCES page:

- https://bwk.kuleuven.be/bwm/z24/obtain

To reproduce the *frozen split* used throughout NN V1–V3 and PINN V1, the experiments were run with:

- **EMS manifest ID:** `4b8b4d63c00f3a81`

### Preprocessing used in the paper (summary)
- **Segmentation:** fixed windows of **N = 65,536 samples** (with overlaps / sliding windows)
- **Normalization:** per-window z-score standardization (fit on train only)
- **Split:** stratified train/val/test (70% / 15% / 15%), fixed seed and frozen indices

### Suggested local folder structure
```text
data/
  z24_ems/
    <setup_id>/
      avt/
        <setup_file>.mat
```

If your local structure differs, update the **data path cell** at the beginning of each notebook.

---

## Reproducing the paper results

### Recommended run order
1. **Train/evaluate the data-driven models**
   - Run: `NN_V1.ipynb`, `NN_V2.ipynb`, `NN_V3.ipynb`
2. **Run physics-informed experiments**
   - Run: `PINN_V1.ipynb`
3. **Rebuild manuscript artifacts**
   - Run: `reviewer_build_artifacts.ipynb`  
   This notebook consolidates outputs and regenerates the tables/figures referenced in the manuscript.

### Tips for reproducibility
- Keep the same random seeds used in the notebooks.
- Do not mix files between splits if you want to match the paper tables exactly.
- If you run on a cluster (e.g., HPC), save `outputs/` to preserve plots/metrics for the artifact builder notebook.

---

## Results summary

### Validation (same split for NN V1–V3)
| Model | Val. Accuracy (%) | Macro ROC–AUC | Notes |
|---|---:|---:|---|
| NN V1 | 97.71 | 0.991 | baseline |
| NN V2 | 97.25 | 0.998 | deeper & more stable |
| **NN V3** | **97.71** | **1.000** | best overall |

### Held-out test set (paper)
| Model | Test Accuracy (%) | Macro ROC–AUC |
|---|---:|---:|
| NN V1 | 98.62 | 0.991 |
| NN V2 | 97.71 | 0.998 |
| **NN V3** | **99.54** | **1.000** |
| PINN V1 | ~92 | ~0.95 |

> PINN V1 provides **physics diagnostics** (e.g., residual norms and frequency consistency) that complement pure classification metrics.

---

## Citation
If you use this repository in academic work, please cite the paper.

```bibtex
@article{riyahi_z24_nn_pinn,
  title   = {Data-Driven and Physics-Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge},
  author  = {Riyahi, Abdellah and Mestari, Mohammed and Bouihi, Bouchra},
  journal = {Journal of Civil Engineering Forum (JCEF)},
  year    = {2025},
  doi     = {10.22146/jcef.XXXXX},
  note    = {Camera-ready version; replace DOI/pages once final metadata is available}
}
```

> **Recommendation:** add a `CITATION.cff` file (GitHub-friendly) once the final DOI/pages are known.

---

## License
- **Paper / text:** the manuscript is distributed under **CC BY-SA 4.0**.
- **Code:** please add a software license file (e.g., MIT / BSD-3 / Apache-2.0).  
  Until a `LICENSE` file is included, default copyright rules apply.

---

## Contact
**Abdellah Riyahi** (corresponding author)  
Email: `a.riyahi@enset-media.ac.ma`

---

## Acknowledgements
- KU Leuven / SIMCES for providing access to the Z24 Bridge benchmark dataset.
- HPC resources used for training and evaluation (as reported in the manuscript).
