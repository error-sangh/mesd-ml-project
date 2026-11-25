# MESD: MOSFET Electrical Modeling and Machine Learning

Reproduction and extension of the research presented in **"Descriptor: MOSFET Electrical Simulation Dataset (MESD)"** (Zhang et al., IEEE Data Descriptions, 2024).

This project investigates feature importance and machine-learning-based prediction of MOSFET drain current using the publicly available MESD simulation dataset.

## 1. Project Objectives

The project has two main objectives:

**Paper Reproduction**

- Reproduce the feature-importance analysis presented in Figure 7 of the original MESD paper.
- Evaluate the contribution of transistor geometry, voltage biases, temperature, and process corner to drain current.
- Compare the reported feature importance with results obtained using the available 55 nm NMOS devices.

**Research Extension**

- Investigate alternative feature importance techniques, including permutation importance and feature ablation.
- Develop regression models for predicting MOSFET drain current.
- Compare linear, polynomial, ensemble, and neural-network approaches.
- Evaluate generalization across previously unseen transistor geometries.
- Study how feature transformations and regularization affect prediction accuracy.

## 2. Reference

Y. Zhang et al., "Descriptor: MOSFET Electrical Simulation Dataset (MESD)," *IEEE Data Descriptions*, vol. 1, pp. 27–32, 2024.

- Paper: [https://doi.org/10.1109/IEEEDATA.2024.3481356](https://doi.org/10.1109/IEEEDATA.2024.3481356)
- Dataset: [https://github.com/SJTU-YONGFU-RESEARCH-GRP/MESD-MOSFET-Electrical-Simulation-Dataset](https://github.com/SJTU-YONGFU-RESEARCH-GRP/MESD-MOSFET-Electrical-Simulation-Dataset)
- Dataset DOI: [https://doi.org/10.21227/wa5a-r991](https://doi.org/10.21227/wa5a-r991)

## 3. Dataset

The main reproduction experiments use three 55 nm NMOS devices from the N55A process technology.

| Device     | Simulation records | Operating points |
| ---------- | -----------------: | ---------------: |
| N55A-nmos1 |              7,500 |           97,500 |
| N55A-nmos2 |              7,500 |           97,500 |
| N55A-nmos3 |              7,500 |           97,500 |

Each device contains MOSFET characteristics generated using the BSIM4.5 compact model and Cadence Spectre simulation.

**Input features:** Vgs, Vds, W, L, Temp, Corner.

**Prediction target:** Ids (drain current).

The dataset includes 10 widths, 10 lengths, 5 temperatures, 3 process corners, 5 drain-voltage values, and 13 gate-voltage values per device.

The data represent simulated transistor behavior, not direct physical measurements.

## 4. Methodology

### Data Exploration

- Inspect the simulation metadata and JSON structure.
- Convert voltage-sweep records into tabular operating points.
- Check missing values, duplicate inputs, and negative currents.
- Visualize Ids–Vgs and Ids–Vds characteristics.
- Compare electrical behavior across all three 55 nm NMOS devices.

### Paper Reproduction

Reconstruct the feature-importance analysis from Figure 7.

Published reference values:

| Feature | Importance |
| ------- | ---------: |
| Vgs     |       0.41 |
| W       |       0.19 |
| Vds     |       0.18 |
| L       |       0.17 |
| Corner  |       0.03 |
| Temp    |       0.02 |

The paper does not fully specify the original device selection, preprocessing, or estimator configuration. Therefore, reconstructed results are identified as reproduction attempts rather than exact replications.

### Proposed Improvements

- Compare importance across different NMOS devices.
- Evaluate permutation importance and feature ablation.
- Investigate feature engineering and model complexity.
- Compare regression algorithms using consistent evaluation protocols.
- Analyze generalization to unseen transistor dimensions.

## 5. Repository Structure

```text
data/
  raw/              Original MESD JSON files
  processed/        Derived datasets

notebooks/
  MESD_project.ipynb

pyproject.toml
uv.lock
README.md
```

## 6. Environment and Execution

The project uses Python, Jupyter Notebook, pandas, NumPy, Matplotlib, and scikit-learn, with dependencies managed through `uv`.

Install dependencies:

```bash
uv sync
```

Launch Jupyter:

```bash
uv run jupyter lab
```

Open `notebooks/MESD_project.ipynb`.

## 7. Limitations

The original paper primarily describes dataset construction, validation, and illustrative feature importance analysis. It does not provide a fully specified predictive-regression benchmark for direct accuracy comparison.

Here, the predictive modeling experiments in this repository are extensions of the original work and performance claims will be restricted to evaluation protocols executed within this project.

## 8. License and Attribution

The MESD dataset and its original publication belong to their respective authors. Please consult the official dataset repository for applicable usage terms.

This repository is an independent academic reproduction and extension.