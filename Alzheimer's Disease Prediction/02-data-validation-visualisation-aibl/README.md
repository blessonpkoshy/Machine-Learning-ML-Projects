# Data Validation and Visualisation (AIBL Dataset)

University coursework (MSc Data Science, Ulster University — module **COM736: Data
Validation and Visualisation**) extending the classification work in
[`01-alzheimers-diagnosis-classification-aibl`](../01-alzheimers-diagnosis-classification-aibl)
with a full data validation, imputation and feature selection pipeline built directly from the
raw AIBL source files, and a comparison of model accuracy before vs after preprocessing.

## Overview

- **Language / tools:** R — `tidyverse`, `dplyr`, `caret`, `randomForest`, `missForest`, `RSBID`
  (SMOTE for mixed data), `corrplot`, `Boruta`, `e1071`
- **Task:** Same target as project 1 (HC vs Non-HC diagnosis), but built from scratch by
  merging multiple raw AIBL source tables and validating/cleaning the data end-to-end
- **Report:** [`report.pdf`](./report.pdf) — full write-up (IEEE format) with methodology,
  figures and results
- **Source:** [`analysis.Rmd`](./analysis.Rmd) — the R Markdown notebook

## Data

Built from eight raw AIBL source files (clinical dementia rating, lab data, MMSE,
neuropsychological battery, diagnosis conversion, medical history, APOE genotype, and patient
demographics), merged on participant ID. As with project 1, the AIBL dataset is released under
a data use agreement that prohibits redistribution, so **no raw or merged data is included in
this repository**.

> **Note:** the rendered HTML output for this notebook is intentionally not included here —
> it contains `head()`/`arrange()` previews of the merged dataset (a handful of raw patient
> rows), which isn't appropriate to publish. The PDF report only contains aggregate figures
> and results, so that's included instead.

## Pipeline

1. **Data integration** — merged 8 raw AIBL CSVs (demographics, clinical, lab, cognitive,
   genotype) into a single dataset (862 baseline observations, 36 variables)
2. **Baseline model** — trained Random Forest directly on the raw merged data (before any
   cleaning) as a comparison point
3. **Preprocessing** — dropped irrelevant identifiers (RID, VISCODE, EXAMDATE etc.), factorised
   categorical variables, encoded invalid/placeholder values as `NA`
4. **Feature engineering** — binned MMSE scores into severity groups
5. **Missing value imputation** — `missForest` (random-forest-based multiple imputation)
6. **Class balancing** — SMOTE for mixed-type data (`RSBID::SMOTE_NC`)
7. **Statistical checks** — Shapiro-Wilk normality tests on all features
8. **Feature selection** — PCA (variance/loadings check), Kendall correlation analysis, and
   the **Boruta** algorithm to confirm the most relevant features
9. **Final model** — Random Forest retrained on the cleaned, balanced, feature-selected data

## Results

| Model | Accuracy |
|---|---|
| Random Forest — raw/unprocessed data | 95.33% |
| Random Forest — after full preprocessing & feature selection | **96.98%** |

Full confusion matrices, sensitivity/specificity, boxplots (pre/post imputation), correlation
plots and the Boruta feature selection results are in [`report.pdf`](./report.pdf).

## Notes

- Individual coursework submission; code and write-up are the author's own work.
- Reproducing the analysis requires access to the raw AIBL dataset files under their own data
  use agreement — not provided here.
