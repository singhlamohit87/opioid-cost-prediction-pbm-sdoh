# Opioid Cost Prediction Using PBM and SDOH: County-Level Machine Learning Framework

## Overview

This repository contains all data, code, and model artifacts supporting the study:

**"Does Social Context Shape the Fiscal Mechanics of Opioid Spending? A County-Level Machine-Learning Analysis of Medicare Part D Utilization and Social Determinants of Health, United States, 2013–2023"**

**Author:** Mohit Singhal, MS  
**Affiliation:** Independent Researcher, Virginia Beach, VA, USA  
**ORCID:** 0009-0009-1518-4598  
**Contact:** singhlamohit@gmail.com  
**GitHub:** https://github.com/singhlamohit87/opioid-cost-prediction-pbm-sdoh  
**Zenodo DOI:** 10.5281/zenodo.19842331  
**Manuscript submitted to:** Frontiers in Public Health (under review)

---

## Study Summary

This study develops and validates a county-level machine-learning framework that integrates Pharmacy Benefit Management (PBM) utilization metrics with Social Determinants of Health (SDOH) indicators to explain geographic variation in Medicare Part D opioid spending across approximately 3,100 U.S. counties from 2013 to 2023.

### Key Findings

- **Temporal validation:** Random Forest model trained on 2013–2020 data achieves R² = 0.926 and RMSE = $499 on a completely held-out 2021–2023 temporal test set spanning the COVID-19 disruption period
- **H1 confirmed:** PBM utilization metrics explain 77.7% of temporal holdout variance vs. 11.1% for SDOH indicators alone, establishing utilization as the proximate fiscal determinant
- **H2 partially confirmed:** Severe housing instability (pct_severe_housing_problems) is the strongest SDOH predictor (mean |SHAP| = 161), outranking poverty, unemployment, and provider density — a novel finding
- **Policy simulation:** Integrated PBM + SDOH scenario produces 8.2% directional reduction in mean predicted opioid cost per capita
- **Access paradox:** SDOH expansion produces predicted cost increases in access-constrained states (Alaska, Arizona, DC), consistent with preventive-care health economics

---

## Data Sources

All data used in this study are publicly available from federal repositories. No proprietary or restricted-access data were used.

| Dataset | Source | Years | URL |
|---------|--------|-------|-----|
| Medicare Part D Opioid Prescribing Data | Centers for Medicare and Medicaid Services (CMS) | 2013–2023 | https://data.cms.gov/ |
| County Health Rankings | University of Wisconsin Population Health Institute | 2014–2024 | https://www.countyhealthrankings.org/ |
| County Population Estimates | U.S. Census Bureau | 2013–2023 | https://www.census.gov/ |

---

## Repository Structure

```
├── data/
│   └── cleaned/
│       └── feature_engineered_panel.csv     # Merged, cleaned county-year panel (3,100 counties, 2013–2023)
├── models/
│   ├── final_model.pkl                       # Trained Random Forest pipeline (scikit-learn)
│   └── final_feature_list.json              # Final 15 features used in model
├── results/
│   ├── model_comparison_summary.csv         # Sub-model comparison (integrated vs SDOH-only vs utilization-only)
│   ├── shap_feature_importance_auto.csv     # Global SHAP mean absolute importance values
│   ├── policy_simulation_results.csv        # National simulation results for 9 scenarios
│   ├── county_level_predictions.csv         # County-level baseline predictions and scenario outputs
│   ├── state_savings_SDOH_and_PBM.csv       # State-level aggregated simulation savings
│   └── top5_counties_opioid_cost.csv        # Top 5 highest-burden counties with scenario results
├── notebooks/
│   ├── 05_data_predictive_modeling.ipynb    # Feature engineering and initial modeling
│   ├── 06_data_model_experiments_updtd.ipynb # Final model training with temporal split
│   ├── 07_ModelEvaluation_updtd.ipynb       # Model evaluation, SHAP, sub-model comparison
│   └── 08_modelsimulation_updtd.ipynb       # Counterfactual policy simulations and visualization
└── README.md                                # This file
```

---

## Feature List

The final model uses 15 features selected through automated correlation-based pruning (threshold r > 0.90):

### PBM Utilization Features (3)
| Feature | Description |
|---------|-------------|
| log1p_claims_per_1k | Log-transformed opioid claims per 1,000 population |
| opioid_prscrbng_rate | Opioid prescribing rate per 100 Medicare beneficiaries |
| tot_clms | Total Medicare claims |

### SDOH Features (12)
| Feature | Description |
|---------|-------------|
| pct_severe_housing_problems | Percentage of adults with severe housing problems |
| pct_unemployed | Unemployment rate |
| pct_uninsured | Percentage uninsured |
| pct_children_in_poverty | Child poverty rate |
| income_ratio | Income inequality measure (80th/20th percentile ratio) |
| mental_health_provider_rate | Mental health providers per 100,000 population |
| primary_care_physicians_rate | Primary care physicians per 100,000 population |
| pct_adults_reporting_currently_smoking | Adult smoking prevalence |
| pct_adults_with_obesity | Adult obesity prevalence |
| pct_excessive_drinking | Excessive drinking prevalence |
| injury_death_rate | Injury death rate per 100,000 population |
| population | County population |

**Note:** log1p_cost_per_claim was excluded due to collinearity with log1p_claims_per_1k (Pearson r = 0.908 > threshold 0.90). This prevents the PBM cost-per-claim reduction scenario from producing simulation output and is acknowledged as a model limitation.

---

## Model Architecture and Training

| Parameter | Value |
|-----------|-------|
| Algorithm | Random Forest Regressor (scikit-learn 1.4) |
| n_estimators | 200 |
| max_depth | 20 |
| min_samples_split | 5 |
| random_state | 42 |
| Imputation | Median imputation |
| Scaling | Z-score standardization |
| Hyperparameter tuning | RandomizedSearchCV, 5-fold CV on training set |

### Temporal Validation Split

| Split | Years | Observations |
|-------|-------|-------------|
| Training | 2013–2020 | 81,975 county-year |
| Temporal holdout | 2021–2023 | 30,880 county-year |

The holdout set spans the COVID-19 disruption period and was withheld entirely from training, hyperparameter tuning, and all model selection decisions.

---

## Model Performance

| Model | Holdout R² (2021–2023) | Holdout RMSE ($) |
|-------|------------------------|------------------|
| Integrated PBM-SDOH (final) | 0.926 | 499 |
| Utilization-Only | 0.777 | 1,499 |
| SDOH-Only | 0.111 | 5,981 |
| Linear Regression Baseline | 0.897 | 525 |

Training cross-validation: mean R² = 0.948 (SD = 0.023)

---

## Policy Simulation Scenarios

All simulation results are directional sensitivity analyses based on counterfactual modification of predictor values. They are associational and should not be interpreted as causal estimates of real-world intervention effects.

| Scenario | Δ% vs. Baseline | Δ Per Capita ($) |
|----------|-----------------|------------------|
| Integrated PBM + SDOH | −8.2% | −$1.70 |
| PBM Triple Play | −6.0% | −$1.23 |
| PBM Utilization Guidance (−5%) | −4.4% | −$0.91 |
| SDOH Integrated | −2.6% | −$0.53 |
| PBM Formulary Tightening (−10%) | −1.6% | −$0.32 |
| SDOH Smoking Reduction (−5%) | −1.1% | −$0.23 |
| SDOH Mental-Health Expansion (+20%) | −0.6% | −$0.12 |
| SDOH Primary-Care Expansion (+10%) | −0.4% | −$0.09 |
| SDOH Obesity Reduction (−5%) | −0.3% | −$0.07 |
| PBM Cost-per-Claim Reduction (−3%) | 0.0% | $0.00 (feature excluded) |

---

## Replication Instructions

### Requirements

```
Python 3.13
scikit-learn >= 1.4
shap >= 0.44
pandas >= 2.0
numpy >= 1.26
matplotlib >= 3.8
joblib
plotly (for choropleth maps)
```

### Steps to Reproduce

1. Download all files from this repository
2. Place the feature_engineered_panel.csv in `data/cleaned/`
3. Run notebooks in order:
   - `05_data_predictive_modeling.ipynb` — feature engineering
   - `06_data_model_experiments_updtd.ipynb` — model training and feature selection
   - `07_ModelEvaluation_updtd.ipynb` — evaluation, SHAP, sub-model comparison
   - `08_modelsimulation_updtd.ipynb` — policy simulations and figures
4. All outputs will be saved to `results/` and `models/`

### Key Variable: Temporal Split

The critical code for the temporal validation split is in notebook 06, Step 7:

```python
# Temporal validation split - train 2013-2020, validate 2021-2023
train_mask = panel['year'] <= 2020
test_mask = panel['year'] >= 2021

X_train = X_selected[train_mask]
X_test = X_selected[test_mask]
y_train = y[train_mask]
y_test = y[test_mask]
```



---

## License

This repository is licensed under Creative Commons Attribution 4.0 International (CC BY 4.0). You are free to share and adapt the material for any purpose, provided appropriate credit is given.

---

## Ethics Statement

This study uses exclusively publicly available, de-identified federal administrative and survey data. No human subjects were directly involved. The research was exempt from Institutional Review Board review in accordance with 45 CFR 46.101(b).

---

## Funding and Conflicts of Interest

The author received no specific funding for this work. No conflicts of interest are declared.
