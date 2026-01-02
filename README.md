# Data-Driven and Physics-Informed Neural Networks for SHM: The Z24 Bridge Case Study

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)
![Framework: TensorFlow](https://img.shields.io/badge/Framework-TensorFlow%202.13-orange.svg)
![Topic: SHM](https://img.shields.io/badge/Topic-Structural%20Health%20Monitoring-forestgreen.svg)

This repository provides the official implementation and companion code for the manuscript:
> [cite_start]**"Data-Driven and Physics-Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge"**[cite: 2].
> [cite_start]*Authors: Abdellah Riyahi, Mohammed Mestari, and Bouchra Bouihi*[cite: 3].
> [cite_start]*Published in the Journal of Civil Engineering Forum (JCEF), 2026*[cite: 1, 2].

---

## 📖 Project Overview
[cite_start]This research presents a systematic comparison between three generations of data-driven Multi-Layer Perceptrons (MLP) and a Physics-Informed Neural Network (PINN)[cite: 33, 41]. [cite_start]The study utilizes the **Z24 Bridge** benchmark in Switzerland to evaluate vibration-based damage detection[cite: 12, 13].

### Implemented Models
* [cite_start]**NN V1–V3 (Data-Driven)**: These models represent progressive improvements in architecture depth, optimization, and regularization[cite: 81, 107].
* [cite_start]**NN V3 (Optimized)**: Features 6 layers with progressively decreasing neuron counts and advanced training strategies like batch normalization[cite: 121, 120].
* [cite_start]**PINN V1 (Hybrid)**: Integrates the differential equation of a damped single-degree-of-freedom (SDOF) oscillator directly into the loss function to ensure physical consistency[cite: 133, 147].

---

## 📊 Performance Summary
[cite_start]All experiments were conducted using a single vertical accelerometer channel (**acc_09**, near mid-span) selected via a quantitative reliability screen[cite: 89, 91].

### [cite_start]Key Metrics (Validation Set) [cite: 197]
| Metric | NN V1 | NN V2 | NN V3 |
| :--- | :---: | :---: | :---: |
| **Accuracy (%)** | 97.71% | 97.25% | **97.71%** |
| **Macro ROC–AUC** | 0.991 | 0.998 | **1.000** |
| **False Negatives** | 0 | 0 | **0** |

### [cite_start]Physics Diagnostics (PINN V1) [cite: 203]
* **Mean Residual ($||h||_2$):** $\approx 1.205 \times 10^{-3}$
* **95th Percentile ($P_{95}(|h|)$):** $0.06660$
* **Estimated Frequency ($\omega_{est}$):** $43.18$ rad/s (consistent with low-frequency vibration modes).

---

## 📁 Repository Layout
* `NN_V1.ipynb` to `NN_V3.ipynb`: Data-driven model training and evaluation[cite: 107].
* [cite_start]`PINN_V1.ipynb`: Hybrid model implementation with physics-based regularization[cite: 124, 125].
* `reviewer_build_artifacts.ipynb`: Consolidates outputs to generate Tables 4–6 and figures for the paper[cite: 35, 42].
* `outputs/`: Generated figures, metrics, and training histories.

---

## ⚙️ Setup and Data Access
### 1. Requirements
* Python 3.10+ [cite: 167]
* [cite_start]TensorFlow 2.13, SciPy, NumPy, Pandas, Scikit-learn, Matplotlib [cite: 167]

### 2. Data Access (Z24 EMS)
The Z24 dataset is **not redistributed** in this repository.
* [cite_start]Obtain the MATLAB `.mat` files from **KU Leuven – SIMCES Section**[cite: 309, 358].
* **EMS manifest ID**: `4b8b4d63c00f3a81` (used for the frozen split in the paper)[cite: 52, 105].

### 3. Usage
1.  Run the **NN** notebooks (`V1` through `V3`) to generate baseline metrics.
2.  Run `PINN_V1.ipynb` to evaluate physics-guided learning.
3.  Execute `reviewer_build_artifacts.ipynb` to build the final comparative tables.

---

## 📜 Citation
If you find this research or code useful, please cite the original article:

```bibtex
@article{riyahi2026shm,
  title   = {Data-Driven and Physics-Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge},
  author  = {Riyahi, Abdellah and Mestari, Mohammed and Bouihi, Bouchra},
  journal = {Journal of the Civil Engineering Forum (JCEF)},
  year    = {2026},
  note    = {Accepted, camera-ready stage}
}
