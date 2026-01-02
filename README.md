# SHM_Z24_Bridge — Data‑Driven & Physics‑Informed NN (Z24)

> **Data‑Driven and Physics‑Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge**  
> *Abdellah Riyahi, Mohammed Mestari, Bouchra Bouihi* — Journal of the Civil Engineering Forum (JCEF), 2026 (submission)

<p align="left">
  <a href="https://creativecommons.org/licenses/by/4.0/"><img alt="License: CC BY 4.0" src="https://img.shields.io/badge/License-CC%20BY%204.0-blue.svg"></a>
  <img alt="Python" src="https://img.shields.io/badge/Python-3.10%2B-informational">
</p>

---

## 🔎 Highlights

- **Data‑driven baselines**: `NN_V1`, `NN_V2`, `NN_V3` (profondeur & régularisation croissantes).  
- **Physics‑Informed model**: `PINN_V1` avec résidu d’un oscillateur **SDOF** amorti sur la trajectoire apprise.  
- **Capteur unique**: *acc_09* (vertical, proche de la travée centrale) sélectionné via un criblage de fiabilité *(SNR > 20 dB, < 1 % de manquants en classe 01, pic PSD stable ±5 %)*.  
- **Tâche**: classification multi‑états du pont **Z24** à partir d’accélérations ambiantes.  
- **Métriques**: accuracy, macro‑F1, **macro ROC–AUC**, + **diagnostics physiques** (‖h‖ L2, P95(|h|), PSD, ω₀ vs. ω_est).

---

## 📁 Arborescence

```
.
├─ NN_V1.ipynb                      # Baseline V1
├─ NN_V2.ipynb                      # Baseline V2 (plus profond + régularisation)
├─ NN_V3.ipynb                      # Baseline V3 (optimisé)
├─ PINN_V1.ipynb                    # Physics‑Informed (résidu SDOF)
├─ Comparisons_NNv1_NNv2_NNv3.ipynb # Figures comparatives (courbes, matrices, ROC…)
├─ reviewer_build_artifacts.ipynb   # Construit Table 4/5/6 + zip des artefacts reviewers
├─ models/                          # poids & checkpoints              (gitignored)
├─ outputs/                         # figures/métriques NN             (gitignored)
├─ outputs_pinn_v2_3/               # artefacts PINN (diag, PSD…)      (gitignored)
├─ reviewer_pack/                   # CSV/JSON utilisés dans l’article
├─ figs/                            # figures exportées (optionnel)
└─ README.md • LICENSE • .gitignore
```
> `models/`, `outputs*/`, `*.npz`, `*.h5`, `*.keras` sont **ignorés** pour garder le dépôt léger.

---

## 🗂️ Données (non redistribuées)

Le dépôt **ne contient pas** les `.mat` du Z24 EMS. Indiquer le chemin local :

```bash
# Linux/macOS
export Z24_DATASET_ROOT="/path/to/DatasetPDT"
# Windows (PowerShell)
setx Z24_DATASET_ROOT "C:\path\to\DatasetPDT"
```

**Schéma attendu** (exemple) :

```
DatasetPDT/
  01/avt/01setup01.mat ... 01setup09.mat
  03/avt/03setup02.mat ...
  ...
  17/avt/17setupXX.mat
```

**Prétraitements (communs aux modèles)** : z‑score (fit sur *train*), fenêtres fixes **N = 65 536**, splits **70/15/15** (seeds gelés), encodage des labels figé.

---

## ▶️ Reproduire les résultats

1. **Baselines NN** : exécuter `NN_V1.ipynb`, `NN_V2.ipynb`, `NN_V3.ipynb`.  
   *Sorties* → `outputs/` : courbes train/val, matrices de confusion, ROC‑AUC.
2. **PINN** : exécuter `PINN_V1.ipynb`.  
   *Sorties* → `outputs_pinn_v2_3/` : ‖h‖ L2 & P95(|h|), PSD(u,h), fréquences ω₀/ω_est.
3. **Comparaisons & tables** : `Comparisons_NNv1_NNv2_NNv3.ipynb` (figures globales) puis  
   `reviewer_build_artifacts.ipynb` (génère **Table 4/5/6** + zip *reviewer_pack*).

---

## 📊 Résultats (papier)

| Model   | Val. Acc. | Test Acc. | Test Loss | Macro ROC–AUC | Notes |
|---------|-----------:|----------:|----------:|---------------:|-------|
| NN V1   | 97.71%     | 98.62%    | 0.35      | 0.991          | data‑driven |
| NN V2   | 97.25%     | 97.71%    | 0.28      | 0.998          | data‑driven |
| NN V3   | **97.71%** | **99.54%**| **0.22**  | **1.000**      | meilleur classement global |
| PINN V1 | —          | ~92%      | ~0.25     | ~0.95          | **‖h‖₂ ≈ 1.205×10⁻³**, **P95(|h|) ≈ 6.66×10⁻²**, **ω₀ ≈ 25.13 rad/s**, **ω_est ≈ 43.18 rad/s** |

> **Lecture** : NN V3 domine en **data‑rich** ; **PINN V1** privilégie robustesse & interprétabilité (résidu bas, signature spectrale cohérente < 10 Hz).

---

## 🧰 Environnement

- Python **3.10+**
- NumPy • SciPy • scikit‑learn • Matplotlib
- TensorFlow/Keras **2.13+**

> Installez via `pip install -r requirements.txt` *(si fourni)* ou créez un environnement conda équivalent.

---

## 📜 Licence

Ce dépôt est diffusé sous **Creative Commons Attribution 4.0 International (CC BY 4.0)**.  
Vous pouvez partager/remixer en **citant** les auteurs et la source.  
Voir `LICENSE` pour les détails.

---

## ✍️ Citer

Si vous utilisez ce dépôt, merci de citer l’article JCEF et ce dépôt :

```bibtex
@article{Riyahi2026_JCEF_Z24_NN_PINN,
  author  = {Riyahi, Abdellah and Mestari, Mohammed and Bouihi, Bouchra},
  title   = {Data-Driven and Physics-Informed Neural Networks for Structural Health Monitoring of the Z24 Bridge},
  journal = {Journal of the Civil Engineering Forum},
  year    = {2026}
}
```

---

## 🙏 Remerciements

Z24 EMS: KU Leuven – Structural Mechanics (SIMCES).  
Ressources de calcul: **HPC‑MARWAN (CNRST, Maroc)**.
