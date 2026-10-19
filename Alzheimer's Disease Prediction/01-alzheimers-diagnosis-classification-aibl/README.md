<h1 align="center">🧠 Alzheimer's Diagnosis Classification</h1>

<p align="center">
  <i>Healthy Control vs cognitive impairment on the AIBL study — kNN · Random Forest · SVM in R</i>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/R-276DC3?style=for-the-badge&logo=r&logoColor=white" alt="R">
  <img src="https://img.shields.io/badge/caret-2E7D32?style=for-the-badge" alt="caret">
  <img src="https://img.shields.io/badge/randomForest-00897B?style=for-the-badge" alt="randomForest">
  <img src="https://img.shields.io/badge/kernlab%20SVM-6A1B9A?style=for-the-badge" alt="kernlab">
  <img src="https://img.shields.io/badge/Weka%20SMOTE-EF6C00?style=for-the-badge" alt="Weka SMOTE">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Ulster%20University-MSc%20Data%20Science-C8102E?style=flat-square" alt="Ulster University">
  <img src="https://img.shields.io/badge/Best%20accuracy-93.96%25-brightgreen?style=flat-square" alt="Best accuracy">
  <img src="https://img.shields.io/badge/Dataset-AIBL%20(not%20redistributed)-orange?style=flat-square" alt="Dataset">
</p>

---

## 🎯 At a glance

| | |
|---|---|
| 🔬 **Question** | Can clinical, demographic and blood-biomarker data separate healthy controls from people with MCI or Alzheimer's? |
| 🧪 **Classes** | 🟢 **HC** (Healthy Control) vs 🔴 **Non-HC** (Mild Cognitive Impairment + Alzheimer's merged) |
| 📦 **Data** | AIBL study, 862 participants × 32 variables → 1,218 rows after SMOTE balancing |
| 🤖 **Models** | k-Nearest Neighbours · Random Forest · Support Vector Machine (RBF) |
| 🏆 **Best result** | **kNN at 93.96% accuracy** (SVM close behind at 93.68%) |
| 📄 **Report** | [`analysis-report.html`](./analysis-report.html) (rendered output) · [`analysis.Rmd`](./analysis.Rmd) (source) |

---

> [!WARNING]
> **Data is not included.** AIBL participant-level data is released under a data use agreement that prohibits redistribution. This repo contains only the code and the rendered report (summary statistics and model outputs, no raw patient records).

---

## 🔄 Pipeline

```mermaid
flowchart LR
    A["🗂️ AIBL clinical data<br/>862 participants × 32 variables"]:::data --> B["🧹 Preprocessing<br/>MCI + AD → Non-HC<br/>duplicate and NA checks"]:::prep
    B --> C["🛠️ Feature engineering<br/>MMSE → severity bins"]:::prep
    C --> D["⚖️ SMOTE in Weka<br/>609 HC / 609 Non-HC"]:::bal
    D --> E["🎯 RFE feature selection<br/>5-fold CV → 19 features"]:::sel
    E --> F["✂️ 70/30 split<br/>feature scaling"]:::sel
    F --> G["🔵 kNN<br/>k = 5"]:::model
    F --> H["🌲 Random Forest<br/>500 trees"]:::model
    F --> I["🟣 SVM<br/>RBF kernel"]:::model

    classDef data fill:#1565C0,stroke:#0D47A1,color:#ffffff
    classDef prep fill:#6A1B9A,stroke:#4A148C,color:#ffffff
    classDef bal fill:#EF6C00,stroke:#E65100,color:#ffffff
    classDef sel fill:#00897B,stroke:#00695C,color:#ffffff
    classDef model fill:#2E7D32,stroke:#1B5E20,color:#ffffff
```

1. **Preprocessing** — merged MCI and AD into a single *Non-HC* class; checked for missing values (none) and removed duplicates.
2. **Feature engineering** — binned MMSE cognitive scores into severity categories (severe / modest / early / normal).
3. **Class balancing** — applied SMOTE in Weka to fix the class imbalance.
4. **Exploratory analysis** — boxplots of every feature by diagnosis group.
5. **Feature selection** — Recursive Feature Elimination (random-forest functions, 5-fold CV) picked 19 of the original features. A correlation filter (cutoff 0.5) was also explored, but the final models use the RFE feature set.
6. **Modelling** — 70/30 train/test split (854 / 364 rows), features standardised, then three classifiers trained and compared.

### ⚖️ Class balance

```mermaid
%%{init: {'theme':'base','themeVariables':{'pie1':'#43A047','pie2':'#E53935'}}}%%
pie showData title Before SMOTE (862 participants)
    "Healthy Control (HC)" : 609
    "Non-HC (MCI + AD)" : 253
```

After SMOTE the minority class was oversampled so both classes have **609** rows (**1,218** in total), and the test set is perfectly balanced at 182 / 182.

### 🧬 Features selected by RFE (19)

| Group | Features |
|---|---|
| 🧠 Cognitive and clinical scores | `CDGLOBAL` · `MMSCORE` · `LDELTOTAL` · `LIMMTOTAL` |
| 🧬 Genotype | `APGEN1` |
| 🏥 Medical history | `MHPSYCH` · `MH2NEURL` · `MH8MUSCL` · `MH9ENDO` · `MH16SMOK` |
| 🩸 Blood / lab measures | `HMT7` · `HMT40` · `HMT102` · `RCT6` · `RCT20` · `RCT392` · `BAT126` |
| 👤 Demographics / ID | `PTGENDER` · `RID` ⚠️ *(see limitations)* |

---

## 📊 Exploratory analysis

<p align="center">
  <b>Feature distributions by diagnosis group</b><br>
  <img src="./boxplot.png" alt="Boxplot of features by diagnosis group" width="750">
</p>

<p align="center">
  <b>Class balance after SMOTE</b><br>
  <img src="./smote-result.png" alt="Class distribution after SMOTE" width="500">
</p>

---

## 🏁 Results

Hyperparameters used:

| Model | R package | Settings |
|---|---|---|
| 🔵 kNN | `class` | k = 5 |
| 🌲 Random Forest | `randomForest` | 500 trees |
| 🟣 SVM | `kernlab` | C-svc, RBF kernel, σ = 0.1, C = 100 |

### Test-set performance (364 participants)

| | Model | Accuracy | 95% CI | Kappa | Sensitivity (HC) | Specificity (Non-HC) |
|---|---|---|---|---|---|---|
| 🥇 | **kNN** | ![kNN](https://img.shields.io/badge/93.96%25-brightgreen?style=flat-square) | 0.910 – 0.962 | 0.879 | 0.912 | 0.967 |
| 🥈 | **SVM (RBF)** | ![SVM](https://img.shields.io/badge/93.68%25-green?style=flat-square) | 0.907 – 0.960 | 0.874 | 0.912 | 0.962 |
| 🥉 | **Random Forest** | ![RF](https://img.shields.io/badge/88.46%25-yellow?style=flat-square) | 0.847 – 0.916 | 0.769 | 0.769 | 1.000 |

### Confusion matrices

🟩 = correct prediction · 🟥 = misclassification

<table>
<tr>
<td>

**🔵 kNN**

| Actual ↓ / Predicted → | HC | Non-HC |
|---|:---:|:---:|
| **HC** | 🟩 **166** | 🟥 16 |
| **Non-HC** | 🟥 6 | 🟩 **176** |

</td>
<td>

**🟣 SVM**

| Actual ↓ / Predicted → | HC | Non-HC |
|---|:---:|:---:|
| **HC** | 🟩 **166** | 🟥 16 |
| **Non-HC** | 🟥 7 | 🟩 **175** |

</td>
<td>

**🌲 Random Forest**

| Actual ↓ / Predicted → | HC | Non-HC |
|---|:---:|:---:|
| **HC** | 🟩 **140** | 🟥 42 |
| **Non-HC** | 🟥 0 | 🟩 **182** |

</td>
</tr>
</table>

### 💡 Takeaways

- **kNN and SVM are effectively tied.** They differ by a single participant out of 364 (342 vs 341 correct), so I wouldn't read much into which one "won".
- **Random Forest trades accuracy for a different error profile.** It missed no one with MCI or Alzheimer's (0 false negatives) but flagged 42 healthy controls as impaired. In a screening setting where a missed case is costlier than a false alarm, that trade-off could be acceptable.
- **Cognitive scores dominate.** The features RFE ranked most useful include `CDGLOBAL`, `LDELTOTAL`, `LIMMTOTAL` and `MMSCORE`.

---

## 🔍 Limitations and next steps

> [!NOTE]
> Written up honestly, because they matter for how far the headline accuracy can be trusted.

- **SMOTE before the split.** Balancing was applied to the whole dataset before the 70/30 split, so synthetic rows derived from the same participant can land in both train and test sets, which can inflate test accuracy. Next time: split first, then apply SMOTE to the training set only.
- **`RID` was selected as a feature.** It's a participant identifier and shouldn't carry diagnostic signal; it should be dropped before modelling.
- **Test features were scaled independently.** The test set was standardised with its own mean and SD rather than the training set's.
- **Single hold-out split.** Models were compared on one train/test split; repeated or nested cross-validation would give more reliable comparisons.

---

## ▶️ Reproducing

Requires access to the AIBL data (see the warning above). Then install the packages and knit the notebook:

```r
install.packages(c(
  "tidyverse", "caret", "rpart", "randomForest", "kernlab",
  "e1071", "class", "neuralnet", "NeuralNetTools"
))
```

The notebook reads and writes CSVs via absolute local paths (`C:/Users/...`), so update those to point at your own copy of the data. The SMOTE step is run in Weka between the two halves of the notebook.

---

## 📁 Folder contents

```
01-alzheimers-diagnosis-classification-aibl/
├── README.md              ← you are here
├── analysis.Rmd           ← R Markdown source
├── analysis-report.html   ← rendered report with all model output
├── boxplot.png            ← feature distributions by diagnosis
└── smote-result.png       ← class balance after SMOTE
```

➡️ **Next:** [`02-data-validation-visualisation-aibl`](../02-data-validation-visualisation-aibl) rebuilds this pipeline from the raw AIBL files with a fuller validation, imputation and feature-selection process (Random Forest reaching 96.98%).

<p align="center"><sub>Individual MSc coursework submission · Ulster University</sub></p>
