# Quantum-Classical Hybrid Machine Learning for Breast Cancer Drug Response Prediction

> **Companion code for the manuscript:**  
> *"Benchmarking Quantum Encoding Strategies for Pharmacogenomic IC50 Regression: Angular, Amplitude, and Hybrid Approaches"*  
> Submitted to **The Journal of Supercomputing** (Springer) — 2026

---

## Overview

This repository contains all Jupyter notebooks and figures associated with the above manuscript. The study benchmarks three quantum encoding strategies — **Angular**, **Amplitude**, and **Hybrid** — combined with classical regressors (Random Forest, Decision Tree, MLP, Stacking) for predicting drug response (LN_IC50) in breast cancer cell lines.

Two pharmacogenomic datasets are used:
- **GDSC** (Genomics of Drug Sensitivity in Cancer) — IC50 values across multiple drug/cell-line pairs
- **Anchor Combinations Dataset** — synergy-based drug combination responses

Quantum feature maps are optimised using either **COBYLA** (derivative-free) or **Differential Evolution (DE)**, with a proxy sample strategy (200 or 5000 samples) to reduce quantum simulation cost.

---

## Repository Structure

```
.
├── Figures/                              # All manuscript figures (fig1–fig9)
│
├── GDSC (200 proxy samples)/
│   ├── Angular encoding/
│   │   ├── gdsc_angular_rf_cobyla_n200.ipynb
│   │   ├── gdsc_angular_mlp_cobyla_n200.ipynb
│   │   ├── gdsc_angular_dt_cobyla_n200_.ipynb
│   │   └── gdsc_angular_stacking_de_n200.ipynb
│   ├── Amplitude encoding/
│   │   ├── gdsc_amplitude_rf_cobyla_n200.ipynb
│   │   ├── gdsc_amplitude_mlp_cobyla_n200.ipynb
│   │   ├── gdsc_amplitude_dt_cobyla_n200.ipynb
│   │   └── gdsc_amplitude_stacking_cobyla_n200.ipynb
│   └── Hybrid encoding/
│       ├── gdsc_hybrid_rf_cobyla_n200.ipynb
│       ├── gdsc_hybrid_mlp_cobyla_n200.ipynb
│       ├── gdsc_hybrid_dt_cobyla_n200.ipynb
│       └── gdsc_hybrid_stacking_cobyla_n200.ipynb
│
├── GDSC (5000 proxy samples)/
│   └── gdsc_angular_rf_cobyla_n5000.ipynb
│
├── GDSC + DE/
│   └── gdsc_angular_rf_DE_full.ipynb
│
├── Anchor combinations dataset/
│   ├── anchor_angular_rf_cobyla_n200.ipynb
│   └── anchor_angular_rf_de_de_n5000.ipynb
│
├── gdsc_classical_ensemble_full/
│   └── gdsc_classical_ensemble_full.ipynb  ← Classical baseline
│
├── requirements.txt
├── CITATION.cff
└── LICENSE
```

---

## Figures

| Figure | Description |
|--------|-------------|
| `fig1_r2_all_models_gdsc.png` | R² scores across all quantum models on GDSC |
| `fig2_metrics_rf_encodings_gdsc.png` | RF performance across encoding strategies (GDSC) |
| `fig3_proxy_effect_gdsc.png` | Effect of proxy sample size on performance |
| `fig4_dataset_comparison.png` | GDSC vs Anchor Combinations dataset comparison |
| `fig5_ensemble_comparison.png` | Quantum vs classical ensemble comparison |
| `fig6_runtime_memory.png` | Runtime and memory profiling |
| `fig7_heatmap_r2_gdsc.png` | Heatmap of R² across all encoding × classifier combinations |
| `fig8_delta_r2_gdsc.png` | Delta R² (quantum − classical baseline) |
| `fig9_proxy_improvement.png` | R² improvement from n=200 to n=5000 proxy samples |

---

## Datasets

The datasets are **not included** in this repository due to size and licensing constraints.

| Dataset | Variable | Default path | Source |
|---------|----------|--------------|--------|
| GDSC (preprocessed) | `DATASET_PATH` | `/dataset_encoded_normalized1.csv` | [GDSC portal](https://www.cancerrxgene.org/) |
| Anchor Combinations | `DATASET_PATH` | `/dataset_encoded_combo_normalized.csv` | [DrugComb / Anchor](https://drugcomb.fimm.fi/) |

**To run the notebooks**, either:
1. Place the CSV files at the default paths above, **or**
2. Set the `DATASET_PATH` environment variable before launching Jupyter:
   ```bash
   export DATASET_PATH=/path/to/your/dataset_encoded_normalized1.csv
   jupyter notebook
   ```

Both CSVs must contain a `LN_IC50` column (target) plus the 12 pre-selected genomic/molecular features described in the manuscript (Section 3.1).

---

## Requirements

```bash
pip install -r requirements.txt
```

See [`requirements.txt`](requirements.txt) for the full dependency list.

**Key dependencies:**
- [PennyLane](https://pennylane.ai/) 0.44.1 — quantum circuit simulation
- `pennylane-lightning` / `pennylane-lightning-gpu` — accelerated simulation (GPU optional)
- `scikit-learn` ≥ 1.4 | Python 3.13.11 | NumPy 2.4.1 — classical ML baselines and regressors
- `scipy` ≥ 1.12 — COBYLA and Differential Evolution optimisers
- `umap-learn` — UMAP dimensionality reduction visualisation (optional)

GPU acceleration (`lightning.gpu`) is optional; notebooks fall back automatically to `lightning.qubit` (CPU) if no GPU is detected.

---

## Reproducibility

All notebooks set `SEED = 42` globally. The 3-way split strategy is fixed:

| Split | Fraction | Purpose |
|-------|----------|---------|
| Train | 70% | Quantum weight optimisation + final model training |
| Validation | 15% | Optimiser objective evaluation only |
| Test | 15% | Final held-out evaluation (touched once) |

Quantum circuit weights are cached to disk (`cache_quantum_*/best_weights.npy`) to allow resuming interrupted runs without re-optimisation.

---

## Running the Notebooks

```bash
# Clone the repository
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>

# Install dependencies
pip install -r requirements.txt

# Set dataset path (or use default absolute paths)
export DATASET_PATH=/path/to/dataset_encoded_normalized1.csv

# Launch Jupyter
jupyter notebook
```

Notebooks are self-contained and can be run independently. The classical baseline (`gdsc_classical_ensemble_full.ipynb`) does not require PennyLane.

---

## Citation

If you use this code or the figures from this repository, please cite:

```bibtex
@article{Derouich2026quantum,
  author    = {Derouich, Rania and Ben Yahia, Nesrine and [Third Author Last, First]},
  title     = {Benchmarking Quantum Encoding Strategies for Pharmacogenomic IC50 Regression},
  journal   = {The Journal of Supercomputing},
  publisher = {Springer},
  year      = {2026},
  doi       = {[DOI once assigned]}
}
```

See also [`CITATION.cff`](CITATION.cff) for machine-readable citation metadata.

---

## License

This repository is released under the **MIT License**. See [`LICENSE`](LICENSE) for details.

---

## Contact

For questions regarding the code or data preprocessing, please open a GitHub Issue or contact the corresponding author via the journal submission system.
