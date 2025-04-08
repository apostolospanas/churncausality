# churncausality

ChurnCausality is a research-grade pipeline for causal and predictive analysis of player churn in freemium game environments. It combines synthetic behaviour simulation, DoWhy-based causal inference, counterfactual reasoning, and leakage-aware ML modelling to explore how in-game engagement and monetisation behaviours affect retention outcomes.

Key components include:

Engineered player telemetry across 100,000 users over 12 months.

Realistic churn probabilities based on engagement scores.

Causal graph discovery and adjustment for confounding (booster usage → churn).

Chronological data splits to avoid data leakage.

ML benchmarking with Logistic Regression, Random Forest, XGBoost.

Interpretability via feature importance, DAGs, and counterfactual churn predictions.

Built for experimentation, transparency, and scalable extension into production-grade churn modelling.



---

## Overview

This repository simulates a high-fidelity player dataset to explore the causal and predictive drivers of churn. The approach integrates causal modelling, traditional ML classification, and leakage-aware feature engineering. Emphasis is placed on temporal validation, DAG-based causal structure learning, and model interpretability.

---

## Key Features

- **Synthetic Dataset Generator**
  - Simulates behaviour for 100,000 unique players over 365 days.
  - Includes gameplay metrics, player skill, monetisation behaviour, and churn outcomes.

- **Causal Inference with DoWhy**
  - Constructs a Directed Acyclic Graph (DAG) to assess the causal impact of **booster usage** on **month-1 churn (M1_churn)**.
  - Adjusts for confounders such as number of attempts, session length, skill, purchases, and ad clicks.

- **Leakage-Aware Machine Learning**
  - Detects and removes leaky features that would be unavailable at prediction time.
  - Applies chronological train-test splitting to simulate real-world deployment.
  - Trains and compares Logistic Regression, Random Forest, and XGBoost using appropriate scaling techniques.

- **Evaluation and Visualisation**
  - Compares feature importances across models.
  - Evaluates models using ROC curves and AUC on future-based test data.
  - Highlights predictive limitations and confounding effects.

---


## Best Practices Implemented

### 1. Data Leakage Mitigation

- Removed features unavailable at inference time:
  - `churned` (label leakage)
  - `days_since_first_play` (temporal leakage)
  - `total_levels_played` (aggregated future stat)
- Ensured train-test integrity using quantile-based chronological split (80% training, 20% testing).

### 2. Regularisation and Scaling

- Applied **L2 regularisation** in Logistic Regression (`C=0.1`) to improve generalisation.
- Scaled features appropriately:
  - `StandardScaler` for linear models (Logistic Regression)
  - `MinMaxScaler` for tree-based models (Random Forest, XGBoost)

### 3. Causal Inference

- Used DoWhy to define treatment, outcome, and confounders within a formal causal model.
- Visualised and interpreted the DAG to understand engagement-related pathways impacting churn.

### 4. Model Evaluation Integrity

- Evaluated models on temporally distinct test data.
- Used `StratifiedKFold` cross-validation to maintain class balance.
- Reported ROC curves and AUC to assess discriminatory power.

### 5. Explainability and Diagnostics

- Visualised:
  - Feature importances across models
  - DAG structure from DoWhy
  - Counterfactual impacts of treatment (booster usage)

---
