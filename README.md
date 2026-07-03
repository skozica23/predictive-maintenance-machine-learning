# Predictive Maintenance Using Machine Learning

## 👥 Project Team
* **Szymon Kozica**

## 📝 Project Concept & Business Context
Industrial downtime caused by unexpected machine failure costs manufacturing facilities billions of dollars annually. This project implements a **Predictive Maintenance** system designed to proactively detect imminent machine failures before they lead to catastrophic breakdowns. 

By analyzing real-time sensor measurements, the predictive pipeline shifts factory operations from a *reactive* (repairing after breakage) to a *proactive* model. The primary objective is to optimize the trade-off between missing critical failures (maximizing **Recall**) and avoiding expensive false alarms (maximizing **Precision**).

## 📊 Data Description
The dataset contains 10,000 operational data points from manufacturing machines, consisting of the following key engineering features:
* `Type`: Product quality variant (L/M/H for Low/Medium/High quality).
* `Air temperature [K]`: Ambient temperature of the manufacturing environment.
* `Process temperature [K]`: Temperature generated during the operational process.
* `Rotational speed [rpm]`: The angular speed of the machine tool spindle.
* `Torque [Nm]`: The rotational force applied by the machine (strongly negatively correlated with rotational speed due to laws of electric motors).
* `Tool wear [min]`: The duration the current cutting tool has been active.
* `Machine failure`: The target label (binary: 0 for normal operation, 1 for imminent failure). *Note: The data exhibits a severe class imbalance, with failures accounting for only ~3.39% of the dataset.*

## 💻 Environment & Setup
The project is built using a structured framework and a localized dependency management system to ensure 100% reproducibility across different machines.

### Prerequisites & Installation
* **Python Version:** 3.10+
* **Core Libraries:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `plotly`

To replicate the exact development environment and install all necessary dependencies, clone this repository, open your terminal in the project root, and execute:

```bash
pip install -r requirements.txt
```

### Project Structure
```text
├── data/
│   └── ai4i2020.csv                         # Raw industrial telemetry data
├── plots/
│   ├── 01_feature_importance.png            # Feature weightings from Random Forest
│   ├── 04_model_comparison.png              # Static benchmark chart for GitHub preview
│   └── 04_model_performance_comparison.html # Interactive presentation dashboard
├── .gitignore                               # Standard rules ignoring .venv and cache
├── requirements.txt                         # Exact python package dependencies file
└── predictive-maintenance.ipynb             # Core end-to-end analytical notebook
```

## 🧠 Model Evolution & Comparative Analysis
To tackle the extreme class imbalance, models were evaluated strictly using **Precision**, **Recall**, and **F1-Score** (for the failure class), using stratified splitting and balanced class weighting.

| Model Architecture | Precision | Recall | F1-Score | Status |
| :--- | :---: | :---: | :---: | :--- |
| **Logistic Regression** *(Linear Baseline)* | 0.14 | 0.82 | 0.24 | Baseline |
| **Decision Tree** *(Non-linear individual)* | 0.31 | 0.87 | 0.46 | Intermediate |
| **Optimized Random Forest** *(GridSearchCV)* | **0.50** | **0.79** | **0.62** | **Production Ready** |

### Key Findings:
* **The Baseline Failure:** Logistic Regression struggled significantly with high false-positive rates (Precision of 14%), which in a real factory would trigger endless unnecessary maintenance checks.
* **The Ensemble Triumph:** Transitioning to an ensemble architecture (**Random Forest**) combined with hyperparameter tuning via `GridSearchCV` established a highly robust model. It successfully captures **79% of all machine failures** while cutting false-alarm deployment costs in half (boosting Precision to **50%**), resulting in a dominant **F1-score of 0.62**.

## 🎯 Key Conclusions
* **Physical Realism:** Feature importance evaluation confirmed that the model's decisions are highly rooted in mechanical laws, relying primarily on `Torque`, `Rotational speed`, and `Tool wear` to spot failures.
* **Operational Impact:** Deploying the Optimized Random Forest model allows a manufacturing plant to prevent 4 out of 5 catastrophic machine breakdowns while ensuring maintenance teams are only deployed on valid, high-probability alerts.
