# MOSFET Drain Current Prediction Using Machine Learning

**Dataset Submission**

## 1. Dataset Description

**Dataset Name:** MOSFET Electrical Simulation Dataset (MESD)

**Original Source:** SJTU Yongfu Research Group, GitHub

**Original Contributors:** Yuhang Zhang, Kain Lu Low, Longfan Li, Leilai Shao, Yanan Sun, Jian Zhao, Yabin Sun, Yanling Shi, and Yongfu Li

**Dataset Descriptor Published:** 2024

**Official Dataset URL:** https://github.com/SJTU-YONGFU-RESEARCH-GRP/MESD-MOSFET-Electrical-Simulation-Dataset

**Dataset Descriptor DOI:** https://doi.org/10.1109/IEEEDATA.2024.3481356

This project uses the N45B-nmos1 subset of MESD. It contains electrical characteristics for one 45 nm NMOS device simulated in Cadence Spectre using the BSIM4.3 compact model. Each JSON record contains a gate-voltage sweep at a fixed drain voltage, width, channel length, temperature, and process corner. The exploration notebook pairs each gate voltage with its corresponding drain current to produce one row per operating point.

### Download and Load Instructions

1. Use the bundled `data/raw/N45B-nmos1.json` file in this project. If you need a fresh copy, download [the JSON at the pinned upstream revision](https://raw.githubusercontent.com/SJTU-YONGFU-RESEARCH-GRP/MESD-MOSFET-Electrical-Simulation-Dataset/ab685f075e800c1d2e8312b710c5e840f91ae2e7/MESD2.0/N45B-nmos1.json) and save it to `data/raw/N45B-nmos1.json`.
2. Run `uv sync` from the project root to install the Python environment.
3. Open [`notebooks/01_dataset_exploration.ipynb`](notebooks/01_dataset_exploration.ipynb) in VS Code, select the project's `.venv/bin/python` kernel, and run all cells.

### Dataset Characteristics

| Property | Description |
| --- | --- |
| Technology and device | 45 nm NMOS, N45B PDK, `nmos1` |
| Simulator and compact model | Cadence Spectre, BSIM4.3 |
| Gate voltage sweeps | 9,000 |
| Points per sweep | 11 |
| Total operating points | 99,000 |
| Input features | 6 (5 numerical and 1 categorical) |
| Learning task | Supervised regression |
| Missing values across the inputs and target | 0 |
| Operating points at positive drain bias | 82,500 |

The notebook found no exact duplicate I–V curves among the 7,500 positive-drain-bias sweeps. At the highest gate voltage, drain current followed (ff > tt > ss) in all 2,500 matched positive-bias conditions.

### Dataset Files

`data/raw/N45B-nmos1.json` contains device metadata and a `Records` array of voltage sweeps. It also contains simulated capacitance values, which are excluded from the drain-current predictors.

### Input Features

| Field | Description | Values |
| --- | --- | --- |
| `Vgs` | Gate-to-source voltage | 11 values from 0 to 1.0 V in 0.1 V steps |
| `Vds` | Drain-to-source voltage | 6 values from 0 to 1.0 V in 0.2 V steps |
| `W` | Transistor width | 10 values from 120 to 1,000 nm |
| `L` | Channel length | 10 values from 60 to 1,000 nm |
| `Temp` | Simulation temperature | −40, 0, 25, 85, and 125 °C |
| `Corner` | Process corner | `ff` (fast-fast), `tt` (typical-typical), and `ss` (slow-slow) |

### Regression Target

The target is `Ids`, the simulated drain current in amperes. It ranges from −8.9331 × 10⁻¹⁰ A to 5.6515 × 10⁻⁴ A. Current values are strictly positive at positive drain bias, so that subset supports a base-10 log current experiment. The original signed currents at zero drain bias are retained in the dataset.

## 2. Engineering Problem

The goal is to predict the drain current of a 45 nm NMOS transistor from voltage bias, geometry, temperature and process corner. Drain current curves help circuit designers evaluate how strongly a transistor conducts under different operating conditions. This project uses simulation data to evaluate whether a regression model can estimate those curves for transistor widths absent from training.

1. **Compare regression models.** Use linear regression as a baseline, then compare k-nearest neighbors, polynomial ridge regression, random forest, and gradient boosting.
2. **Evaluate unseen widths.** Hold out entire transistor widths and keep every voltage sweep at those widths in the test set. Compare predictions using mean absolute error, root mean square error, and plots of simulated and predicted I–V curves.
3. **Test the value of additional inputs.** Compare voltage-only models with models that also use device geometry, temperature, and process corner.
4. **Evaluate low current prediction.** On the same positive-drain-bias points, compare fitting directly in amperes with fitting a log-current target. Convert predictions back to amperes for a common error comparison.

## 3. Reference Research Paper

**Quantitative Evaluation of Model Complexity for MOSFET Drain Current Prediction Using Gradient Boosting and Baseline Regressors.**

P. Sirish Kumar et al., *2025 10th International Conference on Research in Intelligent Computing in Engineering (RICE)*, pp. 1–6.

**Paper URL:** https://doi.org/10.1109/RICE67503.2025.11439997


## 4. Dataset Citation

Zhang, Y., Low, K.-L., Li, L., Shao, L., Sun, Y., Zhao, J., Sun, Y., Shi, Y., & Li, Y. (2024). Descriptor: MOSFET Electrical Simulation Dataset (MESD). *IEEE Data Descriptions, 1*, 27–32. [10.1109/IEEEDATA.2024.3481356](https://doi.org/10.1109/IEEEDATA.2024.3481356)

**Dataset License:** [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/), as specified in the [upstream repository license](https://github.com/SJTU-YONGFU-RESEARCH-GRP/MESD-MOSFET-Electrical-Simulation-Dataset/blob/ab685f075e800c1d2e8312b710c5e840f91ae2e7/LICENSE).
