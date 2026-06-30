# Prenatal Antidepressant Exposure and Child Special Educational Needs: Analysis Code

## Overview

This repository contains Stata analysis code for a population-based retrospective cohort study examining the association between maternal depression and antidepressant exposure during pregnancy and special educational needs (SEN) outcomes in children. The study uses linked administrative data from the SAIL Databank (Secure Anonymised Information Linkage), Wales.

## Study Summary

**Research question:** Is prenatal antidepressant exposure associated with increased risk of special educational needs in children, and does this association differ by maternal depression status?

**Data source:** SAIL Databank, Wales — linked primary care, hospital, educational and birth records

**Exposures:**
- Maternal depression during pregnancy and up to 15 months prior (binary)
- Antidepressant exposure during pregnancy and up to 1 month prior (binary)
- Individual antidepressants, classes and combinations (SSRI, TCA, TeCA, SNRI)

**Outcomes:**
- All-cause special educational needs
- Autism spectrum disorder
- ADHD
- Communication difficulties
- Learning difficulties
- Physical and medical difficulties
- Behavioural, emotional and social difficulties (BESD)

**Statistical methods:**
- Generalised estimating equations (GEE) with binomial family and logit link
- Cox proportional hazards regression
- Flexible parametric survival models (Royston-Parmar, stpm3)
- Regression standardisation via standsurv

## Repository Contents

This repository contains:

**Stata analysis code:**
- **GEE main analysis** — GEE models for all-cause SEN and individual outcomes including interaction term, predicted probabilities and marginal effects
- **GEE individual antidepressant analysis** — GEE models for each individual antidepressant class including odds ratios, predicted probabilities and marginal effects
- **Cox proportional hazards analysis** — survival analysis for ADHD diagnosis including interaction term and proportional hazards assumption checks
- **Flexible parametric survival analysis** — stpm3 models for ADHD diagnosis including cumulative incidence and risk differences via regression standardisation


**Read code lists:**
- CSV files containing Read codes used to identify exposures, outcomes and covariates in SAIL datasets — see Read Code Lists section below for details

## Read Code Lists

Read code lists used to identify exposures, outcomes and covariates in SAIL are provided as CSV files. These were developed for use with SAIL Databank primary care data and represent the codes as implemented in Stata on SAIL.

| File | Description |
|---|---|
| `antidepressants_full.csv` | Read codes for antidepressant prescriptions — includes all classes (SSRI, TCA, TeCA, SARI, SNRI, MAOI, Other) |
| `depression_dx_short.csv` | Read codes for depression diagnosis |
| `anxiety_dx_short.csv` | Read codes for anxiety disorder diagnosis |
| `adhd_dx_full.csv` | Read codes for ADHD diagnosis |
| `adhd_rx_full.csv` | Read codes for ADHD prescription records |
| `epilepsy_dx.csv` | Read codes for epilepsy diagnosis |
| `antipsychotics.csv` | Read codes for antipsychotic prescriptions |
| `anxiolytics.csv` | Read codes for anxiolytic prescriptions |
| `smi_dx_short.csv` | Read codes for severe mental illness diagnosis |

**Notes on Read code lists:**
- Short code lists contain exact Read codes as coded in Stata on SAIL
- Full code lists include additional descendent codes
- For `smi_dx_short`, the codes `E1...` and `E11..` are coded with periods rather than umbrella codes as their descendant codes include depression codes which would otherwise be incorrectly captured
- Read codes are specific to primary care records in SAIL and may require adaptation for use in other electronic health record systems

## Requirements

### Stata
- Stata SE 19.0 or later
- The following user-written packages (install via SSC):
  - `stpm3` — flexible parametric survival models
  - `estout` / `esttab` — table export
  - `ggh4x` (optional) — per-panel axis scales in faceted plots


## Data Access

The data used in this study are not publicly available. Access to SAIL Databank data is available to researchers through an application process. Further information is available at [https://saildatabank.com](https://saildatabank.com).

**Note:** This repository contains analysis code only. No data, participant-level information or outputs containing small cell counts are included in this repository in accordance with SAIL Databank data governance requirements.

## How to Run the Analysis

1. Apply for and obtain access to the relevant SAIL Databank datasets
2. Clone this repository to your secure research environment
3. Update the file paths at the top of each do file to point to your working directory
4. Ensure all required Stata packages are installed
5. Run each do file in sequence — GEE analysis first, followed by Cox and stpm3 survival analyses
6. Outputs will be saved to your specified working directory

## Analysis Overview

### GEE Analysis
Generalised estimating equations with binomial family and logit link were used to examine associations between prenatal antidepressant exposure and SEN outcomes. All models include an interaction term between maternal depression and antidepressant exposure. Three model specifications are fitted for each outcome:
- **Unadjusted** — depression and antidepressant exposure only
- **Partially adjusted** — additionally includes sex, deprivation, ethnicity and age
- **Fully adjusted** — additionally includes maternal age, maternal smoking, parity, multiple birth and epilepsy

Predicted probabilities and marginal effects are estimated using the Stata `margins` command with covariates set to their reference categories. Separate models are fitted for each individual antidepressant class, with women exposed to more than one antidepressant type excluded from class-specific analyses.

### Cox Proportional Hazards Analysis
Cox proportional hazards regression is used as the primary survival analysis for time to ADHD diagnosis. The proportional hazards assumption is assessed using log-log plots and Schoenfeld residuals tests. Children enter the risk set at age 5 years and are followed until ADHD diagnosis or the study end date, whichever occurs first.

### Flexible Parametric Survival Analysis
Flexible parametric survival models (Royston-Parmar models) implemented using the `stpm3` command are additionally fitted to estimate absolute risk via regression standardisation using the `standsurv` command. Marginal cumulative incidence and risk differences are estimated for each exposure group, with confidence intervals derived using the delta method.



## Citation

[To be updated following publication]

## Ethics and Data Governance

This study was approved by the SAIL Databank Information Governance Review Panel. All data were accessed within the SAIL secure research environment in accordance with data governance requirements.

