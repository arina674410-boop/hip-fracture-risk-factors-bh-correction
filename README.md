# hip-fracture-risk-factors-bh-correction

## Overview

This notebook performs a statistical analysis of risk factors associated with two clinical outcomes — **in-hospital mortality** and **prolonged hospitalization (>10 days)** — in patients with hip fractures, stratified by fracture localization:

- **Proximal femur fractures** (ICD-10: S72.0, S72.1)
- **Distal femur fractures** (ICD-10: S72.2–S72.9)

To control for multiple comparisons across a large set of clinical predictors, the analysis applies **Benjamini–Hochberg (BH) false discovery rate correction**. Factors that lose statistical significance after correction are identified and reported separately.

---

## Dataset

- Source file: `dataset_11351ppl_withallinformation.xlsx`
- n = 11,351 patients
- Data includes: demographics, ICD-10 diagnoses from discharge epicrises (parsed from free text in Russian), surgical timing, ICU admission, length of stay, and in-hospital mortality

> ⚠️ The dataset is not included in this repository due to patient data confidentiality.

---

## Methods

### Data Preprocessing
- Parsing of structured diagnosis categories (main, complication, concomitant, competing) from free-text epicrisis fields using regex
- ICD-10 code normalization (uppercase, deduplication)
- Binary flag creation for clinical predictors (age ≥80, sex, ICU stay, reoperation, diagnosis presence)
- Dataset split into proximal and distal fracture subsets

### Outcome Variables
| Variable | Definition |
|---|---|
| `death_flag` | In-hospital mortality (binary) |
| `long_stay_10` | Hospitalization >10 days (binary) |

### Statistical Analysis
- Univariate logistic regression with odds ratios (OR) and 95% confidence intervals
- Benjamini–Hochberg FDR correction (`statsmodels.stats.multitest.multipletests`, `method='fdr_bh'`, α=0.05)
- Identification of factors significant before but not after correction

### Output Files
| File | Contents |
|---|---|
| `dataset_with_diagn.xlsx` | Full dataset with parsed diagnosis columns |
| `dist_dataset.xlsx` | Distal fracture subset |
| `prox_dataset.xlsx` | Proximal fracture subset |
| `correction_results.xlsx` | BH-corrected results for all 4 outcome×localization combinations |
| `factors_lost_significance.xlsx` | Factors that lost significance after BH correction |

---

## Repository Structure

```
.
├── risk_factors_mort_and_longstay_dif_nosologies_with_benjamin-hochberg.ipynb
└── README.md
```

---

## Requirements

```
pandas
numpy
matplotlib
seaborn
scipy
statsmodels
openpyxl
```

Install with:

```bash
pip install pandas numpy matplotlib seaborn scipy statsmodels openpyxl
```

---

## How to Run

1. Place `dataset_11351ppl_withallinformation.xlsx` in the same directory as the notebook
2. Run all cells sequentially in Jupyter Notebook or JupyterLab
3. Output files will be saved to the working directory

---

## Author

Arina Prokudina, MD  
Physician | Clinical Data Analyst
