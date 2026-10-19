# Alzheimer's Diagnosis Classification: AIBL Coursework

Two related MSc Data Science courseworks (Ulster University) that apply machine learning in R to
the **AIBL study** (Australian Imaging, Biomarker & Lifestyle Study of Ageing). Both tackle the
same problem: classifying participants as **Healthy Control (HC)** or **Non-HC** (Mild Cognitive
Impairment or Alzheimer's Disease combined) from clinical, demographic and blood biomarker data.

The second project is a follow-up to the first. It rebuilds the pipeline from the raw source
files with a more rigorous data validation and feature selection process.

## Projects

| # | Project | Focus | Key techniques | Best result |
|---|---|---|---|---|
| 1 | [Alzheimer's Diagnosis Classification](./01-alzheimers-diagnosis-classification-aibl) | Comparing classifiers on a prepared dataset | kNN, Random Forest, SVM (RBF), SMOTE, RFE feature selection | 93.96% accuracy (kNN) |
| 2 | [Data Validation and Visualisation](./02-data-validation-visualisation-aibl) | End-to-end data validation, from raw files to model | Multi-file merge, missForest imputation, SMOTE-NC, Boruta, PCA, correlation analysis | 96.98% accuracy (Random Forest) |

## How the two fit together

1. **Project 1** shows the modelling side: several classifiers evaluated side by side after
   class balancing and feature selection.
2. **Project 2** shows the data side: merging eight raw AIBL tables, imputing missing values,
   checking distributions, selecting features with Boruta, and measuring how much preprocessing
   improves a Random Forest (95.33% on raw data, 96.98% after preprocessing).

Reading them in order (1, then 2) gives the clearest picture.

## Tech stack

- **Language:** R, written as R Markdown notebooks
- **Packages:** `tidyverse`, `caret`, `randomForest`, `e1071`, `kernlab`, `class`, `neuralnet`,
  `missForest`, `RSBID`, `Boruta`, `corrplot`

## Data availability

The AIBL dataset is released under a data use agreement that does not permit redistribution, so
**no raw or processed patient data is included** in this repository. Each project contains the
code and a report with aggregate results only. Reproducing the analysis requires obtaining the
data directly from AIBL under its own terms.

## Repository layout

```
.
├── 01-alzheimers-diagnosis-classification-aibl/
│   ├── README.md
│   ├── analysis.Rmd
│   ├── analysis-report.html
│   ├── boxplot.png
│   └── smote-result.png
└── 02-data-validation-visualisation-aibl/
    ├── README.md
    ├── analysis.Rmd
    └── report.pdf
```
