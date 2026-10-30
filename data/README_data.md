# Data Overview

This repository uses publicly available datasets to model OPII at the U.S. county level (2013–2023).

## Data Sources

| Dataset | Source | Period or release |
|---|---|---|
| Medicare Part D Prescribers by Geography and Drug | [CMS Data](https://data.cms.gov/) | 2013–2023 |
| County population estimates | [U.S. Census Bureau](https://www.census.gov/) | 2013–2023 |
| County Health Rankings | [County Health Rankings & Roadmaps](https://www.countyhealthrankings.org/) | 2024 release (underlying source years vary) |
| Rural–Urban Continuum Codes | [USDA Economic Research Service](https://www.ers.usda.gov/data-products/rural-urban-continuum-codes/) | 2023 |

## Merge Keys
- **FIPS (Federal Information Processing Standard)**: uniquely identifies counties.  
- **Year (2013–2023)**: used to join datasets chronologically.  

## Notes
- No restricted or patient-level data were used.
- Only publicly available, de-identified county-level data are referenced.
- The included `sample_data.csv` file illustrates variable structure and format for reproducibility.
