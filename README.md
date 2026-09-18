# Mineral Classification from SEM Data

## Hackathon: "Intelligent Data Analysis in the Oil and Gas Industry" (2026)

### Project Overview

This project was completed as part of the hackathon **"Intelligent Data Analysis in the Oil and Gas Industry" (2026)**. The task was to **classify various minerals based on raster electron microscopy (SEM) data**. The dataset contained spectral measurements of chemical element concentrations for each mineral sample, and the goal was to build a model capable of predicting the mineral class from these measurements.

---

### Dataset & Problem Statement

- **Input:** Raster electron microscopy data — quantitative measurements of chemical elements (in wt.%) present in each sample.
- **Target:** Mineral class (a multiclass classification problem with a significant number of categories).
- **Challenges:**
  - Noisy measurements and outliers in elemental concentrations.
  - Inconsistent mineral names in the target variable.
  - Class imbalance across mineral types.
  - Feature engineering requiring domain knowledge in mineralogy.

---

### Data Preprocessing

The following preprocessing steps were applied before modeling:

1. **Outlier Removal**
   - Removed samples where the concentration of any element exceeded **100%**, since such values are physically impossible and indicate measurement errors.

2. **Target Variable Cleaning**
   - Corrected and unified inconsistent mineral names in the target column (typos, alternative spellings, and duplicate categories).

3. **Domain-Driven Feature Engineering**
   - After studying the fundamentals of mineralogy, I generated a set of **new informative features**, mostly based on **ratios between the fractions of different chemical elements** in minerals.
   - These ratio-based features are particularly meaningful in mineralogy, since the stoichiometric relationships between elements are often what distinguishes one mineral from another (e.g., Si/Al, Fe/Mg, Ca/Mg ratios).

---

### Modeling

Several models were trained and compared:

| Model | Notes |
|---|---|
| **Random Forest** | Best performance — selected as the final model |
| **SVM** | Competitive but lower accuracy |
| **CatBoost** | Good baseline, slightly below Random Forest |

**Hyperparameter tuning** was performed using:
- `GridSearchCV` — for exhaustive search over the hyperparameter grid.
- `StratifiedKFold` — to preserve class distribution across folds, which is crucial given the class imbalance.

---

### Results

| Metric | Value |
|---|---|
| **Accuracy (CV)** | **0.81** |
| **F1-score (CV)** | **0.63** |

The gap between accuracy and F1-score reflects the inherent class imbalance in the dataset — the model performs well on majority classes but struggles more with rare minerals, which is a known challenge in mineralogical classification tasks.

---

### Key Takeaways

- **Domain knowledge matters:** Understanding mineralogy allowed the creation of ratio-based features that significantly boosted model performance.
- **Data quality is critical:** Removing physically impossible values (>100%) and cleaning target labels were essential preprocessing steps.
- **Ensemble methods win:** Random Forest outperformed both SVM and CatBoost on this tabular, imbalanced dataset.
- **Proper validation:** `StratifiedKFold` ensured that evaluation metrics were reliable given the imbalanced class distribution.

---

### Tech Stack

- **Python**
- **pandas**, **NumPy** — data manipulation
- **scikit-learn** — Random Forest, SVM, `GridSearchCV`, `StratifiedKFold`
- **CatBoost** — gradient boosting baseline
- **matplotlib** / **seaborn** — exploratory data analysis

---

### Possible Future Improvements

- Apply class-balancing techniques (SMOTE, class weights) to improve F1-score on minority classes.
- Try more advanced gradient boosting (LightGBM, XGBoost) with tuned objectives for imbalanced multiclass problems.
- Explore feature selection to reduce noise from the large number of ratio features.
- Consider dimensionality reduction (PCA, UMAP) combined with clustering to detect mineral subgroups.