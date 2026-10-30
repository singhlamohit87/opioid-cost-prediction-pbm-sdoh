# Community-Level Opioid Prescribing Burden and Surveillance Gaps in Rural America

## Overview

This repository documents independent research on geographic variation in U.S. opioid prescribing and the limitations of relying solely on claims-per-prescriber rankings to characterize community-level prescribing burden.

**Study:** *Community-Level Opioid Prescribing Burden in Rural America and the Surveillance Gap That Sustains It*

**Author:** Mohit Singhal  
**Affiliation:** Independent Researcher, USA  
**ORCID:** [0009-0009-1518-4598](https://orcid.org/0009-0009-1518-4598)  
**Contact:** [mohit.rsda@gmail.com](mailto:mohit.rsda@gmail.com)  
**Repository:** [singhlamohit87/opioid-cost-prediction-pbm-sdoh](https://github.com/singhlamohit87/opioid-cost-prediction-pbm-sdoh)  


> **Repository status:** The earlier README documented a different analysis of Medicare Part D opioid spending. The current manuscript analyzes an Opioid Prescribing Intensity Index (OPII), not opioid spending. Historical notebooks and artifacts should be treated as legacy materials until the OPII implementation and results are uploaded and verified.

## Research question

Do county-level rankings based on opioid claims per prescriber overlook communities with high prescribing burden when claims are also considered relative to the population?

The analysis evaluates this question using Medicare Part D geographic prescribing data, county population estimates, rural–urban classifications, and social determinants of health (SDOH) indicators.

## Opioid Prescribing Intensity Index (OPII)

For county \(c\) in year \(t\):

\[
\mathrm{OPII}_{c,t}
=\left(\frac{\mathrm{OpioidClaims}_{c,t}}{\mathrm{Population}_{c,t}}\right)
\times\left(\frac{\mathrm{OpioidClaims}_{c,t}}{\mathrm{OpioidPrescribers}_{c,t}}\right)
=\frac{\mathrm{OpioidClaims}_{c,t}^{2}}{\mathrm{Population}_{c,t}\,\mathrm{OpioidPrescribers}_{c,t}}.
\]

The first factor captures opioid claims relative to county population; the second captures claims relative to the number of opioid prescribers. OPII is a **prescribing-intensity construct**, not a direct measure of opioid use disorder, overdose mortality, individual exposure, or causal harm. Medicare Part D claims do not represent all prescriptions dispensed in a county, and the population denominator includes residents outside Part D.

## Study design and data

| Component | Current study |
|---|---|
| Geographic units | 2,557 U.S. counties |
| Study period | 2013–2023 |
| Analytical observations | 28,065 county-years |
| Prescribing data | CMS Medicare Part D Prescribers by Geography and Drug |
| Population | U.S. Census Bureau county population estimates |
| Rural–urban classification | USDA 2023 Rural–Urban Continuum Codes (RUCC) |
| SDOH covariates | 2024 County Health Rankings release; treated as time-invariant in the analysis |
| Prediction model | Random Forest regression |
| Temporal evaluation | Training years 2013–2020; holdout years 2021–2023 |
| Geographic evaluation | Leave-one-state-out assessment |
| Model interpretation | SHAP, including rural–metropolitan comparisons |
| Scenario analysis | Model-based perturbations of predictor values |

**Temporal interpretation:** Because the SDOH indicators are drawn from a later, largely time-invariant release, the chronological outcome holdout should not be described as fully prospective or free from all future-information concerns.

## Main findings

- **Rural–metropolitan disparity:** Mean OPII in 2023 was approximately **100,674 in rural counties versus 50,220 in metropolitan counties**.
- **Ranking discordance:** **165 counties** were in the highest OPII decile but not in the highest claims-per-prescriber decile. These counties collectively contained approximately **13.2 million residents**; this is their geographic population, not a count of opioid recipients or people with opioid use disorder.
- **Rural concentration:** **53.8%** of the 165 discordant counties were rural, compared with **23.4%** of counties in the full study sample.
- **Temporal model performance:** Random Forest holdout **R² = 0.913** for 2021–2023. This is variance explained, not 91.3% classification accuracy.
- **Geographic robustness:** Mean leave-one-state-out **R² = 0.891**.
- **Model interpretation:** Mean absolute SHAP attribution for opioid prescribing rate was reported as **0.72 for rural** and **0.54 for metropolitan** counties. SHAP explains model predictions and should not be interpreted as identifying causal drivers.
- **Illustrative scenario:** A 20% perturbation of the prescribing-rate feature was associated with a **34% reduction in predicted OPII nationally** and **40% in rural counties**. These are model-based sensitivity results, **not estimated effects of implemented policies**.

### What the 165-county comparison demonstrates

The analysis documents disagreement between **two specific county-ranking approaches**: OPII and opioid claims per prescriber. It does **not** establish that these counties are absent from every federal surveillance system or excluded from actual funding decisions.

## Data sources

| Dataset | Source | Period or release |
|---|---|---|
| Medicare Part D Prescribers by Geography and Drug | [CMS Data](https://data.cms.gov/) | 2013–2023 |
| County population estimates | [U.S. Census Bureau](https://www.census.gov/) | 2013–2023 |
| County Health Rankings | [County Health Rankings & Roadmaps](https://www.countyhealthrankings.org/) | 2024 release (underlying source years vary) |
| Rural–Urban Continuum Codes | [USDA Economic Research Service](https://www.ers.usda.gov/data-products/rural-urban-continuum-codes/) | 2023 |

## Repository organization and reproducibility

The **previous repository layout** contained the following artifacts associated with the earlier opioid **cost** analysis:

```text
data/cleaned/feature_engineered_panel.csv
models/final_model.pkl
models/final_feature_list.json
results/model_comparison_summary.csv
results/shap_feature_importance_auto.csv
results/policy_simulation_results.csv
results/county_level_predictions.csv
results/state_savings_SDOH_and_PBM.csv
results/top5_counties_opioid_cost.csv
notebooks/05_data_predictive_modeling.ipynb
notebooks/06_data_model_experiments_updtd.ipynb
notebooks/07_ModelEvaluation_updtd.ipynb
notebooks/08_modelsimulation_updtd.ipynb
```

**Important:** The file names above are documented from the previous README and are **not verified as reproducing the OPII analysis**. In particular, opioid cost predictions, cost-per-capita simulations, and the former 15-feature model must not be conflated with the OPII results reported here.

To make this repository fully reproducible for the current manuscript, upload or identify the exact OPII analysis scripts/notebooks, input-data processing steps, final feature list, trained model or training procedure, OPII county-year output, validation results, SHAP outputs, and scenario definitions. Then replace this legacy inventory with verified paths and execution instructions. Until then, this README is a study description rather than a verified end-to-end reproduction guide.

## Interpretation and limitations

- The study is ecological and cannot identify individual-level prescribing behavior, medication use, addiction, or overdose outcomes.
- CMS Part D data capture a subset of prescribing activity; using total county population as a denominator can be affected by Medicare coverage and demographic composition.
- OPII is sensitive to the number of prescribers and to the mathematical scaling of its two factors; it should be interpreted alongside its components.
- County-level rankings can vary by year, denominator definitions, and data completeness.
- SDOH covariates from the 2024 release are treated as time-invariant, limiting historical and prospective interpretation.
- SHAP results are model explanations, not causal effects.
- Counterfactual feature perturbations do not demonstrate that a real-world intervention would produce the same change.
- The observed ranking discrepancy concerns OPII versus claims per prescriber; it is not a comprehensive evaluation of all opioid surveillance programs.

## Research ethics, funding, and competing interests

**Ethics:** The analysis uses publicly available aggregate county-level data and does not involve recruitment or direct interaction with human participants. Institutional review requirements depend on the applicable institution and regulations; no independent IRB determination is asserted here.

**Funding:** No specific funding was reported for this independent research.

**Competing interests:** None declared by the author.

## Citation and versioning

Please cite the eventual journal publication when available. Until then, identify the work by its manuscript title and author, and use a repository release or Zenodo version DOI **only after confirming that the archived files match this OPII study**.

## License

The previous README specified **Creative Commons Attribution 4.0 International (CC BY 4.0)**. Retain this statement only if the repository's actual license and permissions for code and third-party data are consistent with that designation. Code licensing and source-data reuse rights may differ.

