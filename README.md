# Estimating Ground-Level CO Concentrations with ML: Milan Case Study

This repository contains the data processing and training pipelines for the paper **"Estimating Ground-Level Carbon Monoxide Concentrations Using Machine Learning Techniques: The Metropolitan City of Milan Case Study"**.

The study presents a machine learning framework to estimate daily average ground-level Carbon Monoxide (CO) concentrations by fusing satellite data (Sentinel-5P), atmospheric reanalysis data (CAMS, ERA5), and ground measurements from the ARPA Lombardia network.

## 📖 Overview

Accurate monitoring of ground-level air pollutants like Carbon Monoxide (CO) is crucial for public health and environmental policy. This work addresses the global disparity in air quality monitoring stations by developing a scalable, data-driven model that estimates surface CO concentrations in data-scarce environments.

The core methodology integrates multi-source data, employs advanced feature engineering, and leverages machine learning models, including a custom **Dense Attention Network (DAN)**, to achieve high-fidelity estimates. The framework identifies optimal temporal windows for data aggregation and uses SHAP values for model interpretability.

## ✨ Key Features

*   **Multi-Source Data Fusion**: Integrates Sentinel-5P CO column data, CAMS reanalysis, and ERA5 meteorological variables.
*   **Advanced Feature Engineering**: Includes boundary layer height-adjusted CO (`CO_per_blh`), solar thermal contrast, cyclically-encoded wind direction, and lagged meteorological features.
*   **Temporal Window Optimization**: Systematically evaluates different aggregation windows (4h to 24h before satellite overpass) to capture key atmospheric dynamics.
*   **Machine Learning Pipeline**: Implements and compares eight models (SVR, RF, XGB, MLP, etc.), including a custom **Dense Attention Network (DAN)** and an ensemble model.
*   **Bayesian Hyperparameter Tuning**: Uses `BayesSearchCV` for efficient model optimization.
*   **Model Interpretability**: Employs SHAP (SHapley Additive exPlanations) for global and local feature importance analysis.
*   **Robust Validation**: Includes shuffle-split and chronological data splitting strategies, 20 independent validation runs, and statistical significance testing.

## 🚀 Getting Started

### Prerequisites

Before running the code, ensure you have Conda installed on your system. The environment configuration is managed through the provided `environment.yml` file.

### Installation

1.  Clone this repository:
    ```bash
    git clone [https://github.com/zyl009/Estimating-Ground-Level-Carbon-Monoxide-Concentrations-Using-Machine-Learning-Techniques.git]
    ```

2.  Create and activate the Conda environment using the provided YAML file:
    ```bash
    conda env create -f environment.yml
    conda activate env
    ```

This will install all necessary dependencies including:
- Core data handling libraries (NumPy, Pandas, SciPy)
- Geospatial processing tools (GeoPandas, PyProj)
- Machine learning frameworks (scikit-learn, XGBoost, TensorFlow)
- Visualization libraries (Matplotlib, Seaborn, SHAP)

## 📊 Data

The study uses the following data sources for the Metropolitan City of Milan (MCM) from January 2019 to November 2024:
*   **Sentinel-5P (L3)**: Offline Carbon Monoxide (CO) from Google Earth Engine.
*   **CAMS Reanalysis (EAC4)**: Surface CO and relative humidity.
*   **ERA5**: Boundary layer height, wind components, temperature, radiation, pressure, precipitation, and leaf area indices.
*   **ARPA Lombardia**: In-situ ground-level CO measurements (used for training and validation).

## 🧠 Methodology Summary

1.  **Temporal Harmonization**: Data is aggregated into daily values for a specific time window aligned with the Sentinel-5P overpass (e.g., 21:00-15:00 GMT+1 was found optimal).
2.  **Spatial Harmonization**: All datasets are reprojected to UTM 32N and interpolated to a common ~3 km grid.
3.  **Modeling**: Multiple ML models are trained to predict ARPA ground measurements using the fused satellite and reanalysis features.
4.  **Interpretation**: SHAP analysis identifies the most important features (e.g., `cams_CO`, `blh_pre`, `CO_per_blh`).
5.  **Validation**: Model performance is robustly assessed through multiple data splits and statistical tests.

## 📈 Results

The **Dense Attention Network (DAN)** achieved the best performance on the test set:
*   **Normalized Root Mean Squared Error (NRMSE)**: `0.4879 ± 0.0252`
*   **R²**: `0.7763`
*   **RMSE**: `0.1440 mg/m³`

The optimal temporal aggregation window was found to be **21:00 (previous day) to 15:00 (current day) GMT+1**, effectively capturing nighttime accumulation, morning emissions, and daytime dilution.

## 👥 Contributing

This project was developed as part of a research collaboration. Key contributions:
*   **Z. Liang**: Lead developer; contributed to experimental design and methodology conceptualization, and implemented the core ML pipeline, data preprocessing, feature engineering, model training, hyperparameter tuning, analysis, and manuscript writing.
*   **J. R. Cedeno Jimenez**: Provided co-supervision, experimental design, methodology conceptualization (based on prior NO₂ work), and manuscript review.
*   **V. Yordanov**: Assisted in manuscript review.
*   **M. A. Brovelli**: Provided supervision, project administration, funding acquisition, and critical review of the methodology and manuscript.

We welcome questions and discussions regarding the code. Please open an Issue for any bugs or clarifications.

## 📜 License

This project is licensed under the **MIT License**. This means you are free to use, modify, and distribute the code, provided appropriate credit is given. See the `LICENSE` file for full details.

## 🙏 Acknowledgments
We acknowledge the data providers:
*   **ESA/Copernicus** for Sentinel-5P data
*   **Copernicus Atmosphere Monitoring Service (CAMS)** and **Copernicus Climate Change Service (C3S)** for reanalysis data
*   **ARPA Lombardia** for ground measurement data

## 📚 Citation

If you use this code or findings from our work in your research, please cite our paper:

```bibtex
@article{liang2025estimating,
  title={Estimating Ground-Level Carbon Monoxide Concentrations Using Machine Learning Techniques: The Metropolitan City of Milan Case Study},
  author={Liang, Z. and Cedeno Jimenez, J.R. and Yordanov, V. and Brovelli, M.A.},
  year={2025}
}
```

---
**Repository Maintainer**: [Z. Liang](mailto:zhongyou.liang@mail.polimi.it)
