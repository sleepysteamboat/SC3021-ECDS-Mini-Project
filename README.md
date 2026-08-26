# SC3021-ECDS-Mini-Project
# Default Risk Prediction Across Lending Contexts

SC3021 ECDS2 Group 5 project comparing borrower default risk across secured
and unsecured consumer lending.

## Authors
* **Heidi** - [@sleepysteamboat](https://github.com/sleepysteamboat)
* **Yee Han** - [@yeexhxn](https://github.com/yeexhxn)

## Research Questions

**Primary:** Do borrower financial characteristics that predict default risk
differ between secured lending (HMEQ) and unsecured P2P lending (LendingClub,
Prosper)?

**Secondary:** Are these predictive patterns consistent across P2P platforms
(LendingClub vs. Prosper), or do they vary by platform?

## Overview

We investigate how borrower financial characteristics influence default
likelihood across three US lending contexts: secured home equity lending
(HMEQ), unsecured peer-to-peer lending (LendingClub), and marketplace
consumer lending (Prosper). Default prediction is framed as a binary
classification problem using five cross-comparable borrower attributes:
loan amount, debt-to-income ratio (DTI), delinquencies, derogatory marks,
and credit inquiries. Each dataset is modelled separately with a Random
Forest classifier, and feature importances are compared across datasets to
identify universal vs. context-specific risk factors.

## Datasets

Raw data is not included in this repo (large files, Kaggle-hosted). Download
each and place under `data/` before running:

| Dataset | Source | Size (raw) |
|---|---|---|
| HMEQ | [Kaggle](https://www.kaggle.com/datasets/ajay1735/hmeq-data) | 5,960 rows × 13 cols |
| LendingClub 2015 | [Kaggle](https://www.kaggle.com/datasets/somyaagarwal69/loan-data-2015) | 421,094 rows × 74 cols |
| Prosper | [Kaggle](https://www.kaggle.com/datasets/nurudeenabdulsalaam/prosper-loan-dataset/data) | 113,937 rows × 81 cols |

Expected folder structure:
```
data/
├── hmeq.csv
├── loan_data_2015.csv
└── prosperLoanData.csv
```

## Methodology

1. **Data cleaning** — per-dataset missingness handling (mode imputation for
   categoricals, mean/median imputation for numerics based on skewness),
   dropping columns above a missingness threshold, filtering to resolved
   loan outcomes only.
2. **Feature engineering** — ratio features (e.g. loan-to-value,
   loan-to-income), composite credit risk scores, and binned tiers
   (job stability, credit history, debt burden) for cross-dataset
   comparability.
3. **Modelling** — a Random Forest classifier trained independently per
   dataset on the five core features.
4. **Cross-dataset comparison** — feature importance variance across HMEQ,
   LendingClub, and Prosper (primary RQ), and pairwise comparison between
   LendingClub and Prosper (secondary RQ).

## Key Findings

- **DTI and loan amount are universal risk signals**, ranking 1st/2nd across
  all three datasets (DTI importance: 0.463 HMEQ, 0.569 LendingClub, 0.346
  Prosper).
- **Delinquency history is context-sensitive** — a strong signal in HMEQ
  (default rate rises from 6.7% with no delinquencies to 100% at 6–10 prior
  delinquencies) but weak in LendingClub (11.9%→17.6%), with Prosper in
  between.
- **LendingClub and Prosper show moderate platform divergence** (avg.
  absolute importance difference: 0.089), sharing DTI and loan amount as
  top predictors but disagreeing on the third-ranked feature (inquiries vs.
  delinquency rate).
- Model recall on the default class is weak for both P2P platforms
  (LendingClub: 0.06, Prosper: 0.33) vs. HMEQ (0.34), suggesting the five
  features alone are insufficient for reliable default detection in P2P
  contexts.

Full interpretation, limitations, and recommended actions for credit risk
officers are in the notebook's "Share" and "Act" sections.

## Setup & Running

This notebook was developed in Google Colab and mounts Google Drive for data
I/O. To run it:

1. Install dependencies: `pip install -r requirements.txt`
2. Place the three raw CSVs under `data/` (see structure above)
3. If running outside Colab, replace the `drive.mount(...)` cell and
   `/content/drive/MyDrive/...` paths with local paths (e.g. `data/` and
   `cleaned_datasets/`)
4. Run cells top to bottom — each dataset section (HMEQ, LendingClub,
   Prosper) is self-contained and saves a cleaned CSV before the modelling
   and comparison sections

## Ethics Notes

All datasets are publicly available and anonymised (no PII). The project's
final reflection discusses fair-lending considerations — features like DTI
and delinquency history may correlate with protected characteristics, and
the Random Forest model's lack of interpretability could pose regulatory
risk (e.g. under the US Equal Credit Opportunity Act) if deployed without
explainability tooling. See the notebook's "Final Reflection" section for
the full discussion.
