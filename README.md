# IPTRBM Baseline Models (Phase 1)

This repository contains baseline data preparation and provisional outcome scaffolding for the **Integrated Pain Therapy Risk–Benefit Model (IPTRBM)**, using a synthetic chronic pain cohort (`synthetic_patients_v1.csv`). It focuses on transparent, interpretable preprocessing and outcome construction to support Phase 1 baseline risk models (efficacy and harms).

## Contents

- `synthetic_patients_v1.csv`  
  Synthetic chronic pain patient sample with demographics, pain scores, comorbidities, labs, medications, and utilization features aligned to the Phase 1 data schema.

- `IPTRBM_Data_Schema_v1.json`  
  Schema describing patient inputs and outcomes (demographics, pain, comorbidities, labs/vitals, meds, utilization, and outcome slots).

- `Outcome_Label_Definitions.md`  
  Canonical reference for event label logic (UGIB, AKI, overdose, pain response, and others). **All production labels must follow this document and achieve PPV ≥85% before modeling.**

- `01_modelbaselines.ipynb`  
  Notebook that:
  - Loads `synthetic_patients_v1.csv`.
  - Selects and cleans features consistent with `IPTRBM_Data_Schema_v1.json`.
  - Creates *provisional* outcome columns:
    - `outcome_pain30` (≥30% pain reduction proxy, simulated from baseline pain).
    - `outcome_UGIB` (risk-enriched proxy using NSAID use, age, antiplatelet/anticoagulant, prior PUD/UGIB).
    - `outcome_AKI` (proxy using eGFR and nephrotoxic meds: NSAID, diuretic, ACEi/ARB).
    - `outcome_Overdose` (proxy using opioid_current, MEDD, benzo, SUD, naloxone_rx).
  - One-hot encodes categorical variables.
  - Standardizes numeric features using `sklearn.preprocessing.StandardScaler`.
  - Saves prepared feature and outcome matrices as parquet files plus a fitted scaler.

## Important caveats

- All outcomes in this repo are **provisional proxies** built for scaffolding and model experimentation only.
- They **do not** implement the full OMOP-based label logic and **do not** meet the PPV ≥85% requirement.
- These labels **must not** be used for clinical decision-making or external evaluation.
- Before moving beyond Phase 1:
  - Replace provisional labels with validated implementations per `Outcome_Label_Definitions.md`.
  - Document label versions and PPV in model cards and changelog.

## How to run

1. Clone the repository:
