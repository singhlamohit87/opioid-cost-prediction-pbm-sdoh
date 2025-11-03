# Data Overview

This repository uses publicly available datasets to model opioid cost per capita at the U.S. county level (2013–2023).

## Data Sources
1. CMS Medicare Part D Prescriber Public Use Files (2013–2023)  
   https://data.cms.gov/provider-summary-by-type-of-service/medicare-part-d-prescribers  
2. County Health Rankings & Roadmaps (CHR 2024)  
   https://www.countyhealthrankings.org/explore-health-rankings/rankings-data-documentation  
3. U.S. Census County Population Estimates  
   https://www.census.gov/programs-surveys/popest/data.html  

## Merge Keys
- **FIPS (Federal Information Processing Standard)**: uniquely identifies counties.  
- **Year (2013–2023)**: used to join datasets chronologically.  

## Notes
- No restricted or patient-level data were used.
- Only publicly available, de-identified county-level data are referenced.
- The included `sample_data.csv` file illustrates variable structure and format for reproducibility.
