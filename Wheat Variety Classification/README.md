
<h1 align="center">🌾 Statistical Modelling of Wheat Variety Classification</h1>

<p align="center">
  <i>Geometric Feature Analysis & Multi-Model Classification on the UCI Seeds Dataset — SVM · Decision Tree · Naive Bayes · Random Forest · kNN in R</i>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/R-276DC3?style=for-the-badge&logo=r&logoColor=white" alt="R">
  <img src="https://img.shields.io/badge/kNN-2E7D32?style=for-the-badge" alt="caret">
  <img src="https://img.shields.io/badge/randomForest-00897B?style=for-the-badge" alt="randomForest">
  <img src="https://img.shields.io/badge/SVM-6A1B9A?style=for-the-badge" alt="kernlab">
  <img src="https://img.shields.io/badge/Decision%20Tree-EF6C00?style=for-the-badge" alt="rpart">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Ulster%20University-MSc%20Data%20Science-C8102E?style=flat-square" alt="Ulster University">
  <img src="https://img.shields.io/badge/Best%20Accuracy-95.24%25-brightgreen?style=flat-square" alt="Best accuracy">
  <img src="https://img.shields.io/badge/Dataset-UCI%20Seeds%20(210%20instances)-orange?style=flat-square" alt="Dataset">
</p>

---

## 🎯 At a glance

| | |
|---|---|
| 🔬 **Question** | Can geometric kernel measurements accurately classify wheat varieties using statistical machine learning models? |
| 🧪 **Classes** | 🌾 **Canadian** · 🌾 **Rosa** · 🌾 **Kama** (70 instances each, perfectly balanced) |
| 📦 **Data** | UCI Seeds Dataset — 210 instances × 8 attributes (Area, Perimeter, Compactness, Length, Width, Asymmetry Coefficient, Groove Length, Variety) |
| 🤖 **Models** | Support Vector Machine (SVM) · Decision Tree (rpart) · Naive Bayes · Random Forest · k-Nearest Neighbours (kNN) |
| 🏆 **Best result** | **SVM (RBF Kernel) at 95.24% accuracy** |
| 📄 **Report** | [`analysis-report.pdf`](./analysis-report.pdf) (rendered report) · [`analysis.Rmd`](./analysis.Rmd) (R Markdown source) |

---

## 🔄 Pipeline

```mermaid
flowchart LR
    A["🗂️ UCI Seeds Dataset<br/>210 instances × 8 geometric attributes"]:::data --> B["🧹 Preprocessing<br/>Null & duplicate checks<br/>Clean data verification"]:::prep
    B --> C["📊 Exploratory Analysis<br/>Histograms, boxplots<br/>& Corrplot matrix"]:::prep
    C --> D["🎯 RFE Feature Selection<br/>5-fold CV (Random Forest)<br/>Selected 4 optimal features"]:::sel
    D --> E["✂️ 80/20 Train/Test Split<br/>Feature scaling & factor encoding"]:::sel
    E --> F["🟣 SVM<br/>RBF kernel"]:::model
    E --> G["🟠 Decision Tree<br/>rpart"]:::model
    E --> H["🔵 Naive Bayes<br/>e1071"]:::model
    E --> I["🌲 Random Forest<br/>500 trees"]:::model
    E --> J["🟢 kNN<br/>k = 7"]:::model

    classDef data fill:#1565C0,stroke:#0D47A1,color:#ffffff
    classDef prep fill:#6A1B9A,stroke:#4A148C,color:#ffffff
    classDef sel fill:#00897B,stroke:#00695C,color:#ffffff
    classDef model fill:#2E7D32,stroke:#1B5E20,color:#ffffff

```

1. **Preprocessing** — Inspected structure, verified zero missing values, checked and removed duplicate entries.
2. **Exploratory Data Analysis** — Generated distribution histograms, outlier boxplots, and a correlation matrix (`corrplot`) showing strong collinearity among geometric properties (e.g., width & length, area & perimeter).
3. **Feature Engineering** — Applied Recursive Feature Elimination (RFE) via 5-fold cross-validation (`rfeControl` with `rfFuncs`) to distill the attribute set down to the 4 most discriminative features: `Length_Kernel_Groove`, `Area`, `Perimeter`, and `Asymmetry_Coefficient`.
4. **Modelling & Evaluation** — Performed an 80/20 train/test split, scaled features, and benchmarked five classification algorithms using confusion matrices.

### 🧬 Features selected by RFE (4)

| Feature | Description |
| --- | --- |
| `Length_Kernel_Groove` | Length of the kernel groove (mm) |
| `Area` | Area of the wheat kernel ($\text{mm}^2$) |
| `Perimeter` | Perimeter of the kernel (mm) |
| `Asymmetry_Coefficient` | Characterizes kernel asymmetry |

---

## 🏁 Results

### Test-set performance comparison

| Rank | Model | R Package | Accuracy | Key Performance Notes |
| --- | --- | --- | --- | --- |
| 🥇 | **SVM (RBF Kernel)** | `kernlab` |  | Outperformed all models; exceptional handling of nonlinear class boundaries. |
| 🥈 | **Decision Tree** | `rpart` |  | High interpretability with clear decision rules and tree splits. |
| 🥉 | **Naive Bayes** | `e1071` |  | Robust probabilistic classification assuming conditional independence. |
| 4 | **Random Forest** | `randomForest` |  | Strong ensemble performance with 500 trees. |
| 5 | **kNN ($k=7$)** | `class` |  | Proximity-based classification; solid baseline performance. |

---

## 💡 Key Takeaways

* **SVM Emerges as Top Performer:** Achieving **95.24% accuracy**, Support Vector Machines with an RBF kernel proved exceptionally adept at handling complex, non-linear relationships among kernel geometries.
* **Feature Selection Efficiency:** RFE successfully reduced dimensionality from 7 input features down to 4 core geometric metrics without sacrificing predictive power, confirming that groove length, area, perimeter, and asymmetry are primary drivers of variety differentiation.
* **Robustness Across Methods:** Tree-based and probabilistic models (`rpart`, Naive Bayes) also performed strongly above 92%, validating the distinct separability of the Canadian, Rosa, and Kama wheat varieties.

---

## ▶️ Reproducing

To run the analysis locally, ensure you have R installed along with the required dependencies:

```r
install.packages(c(
  "rpart", "rpart.plot", "class", "kernlab", 
  "corrplot", "caret", "randomForest", "e1071", "ggplot2"
))

```

*Note: Update the local file path in `analysis.Rmd` pointing to your copy of `seeds_dataset.txt` before running the notebook.*

---

## 📁 Folder Contents

```
Statistical Modelling of Wheat Variety Classification/
├── README.md               ← You are here
├── analysis.Rmd            ← R Markdown source code
├── analysis-report.pdf     ← Comprehensive rendered report
└── seeds_dataset.txt       ← UCI Seeds dataset (geometric attributes)

```





<p align="center"><sub>Individual MSc coursework submission · Ulster University</sub></p>