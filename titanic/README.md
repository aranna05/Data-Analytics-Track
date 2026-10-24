# 🚢 Titanic Survival: Executive EDA

An end-to-end Exploratory Data Analysis (EDA) of the Kaggle Titanic dataset, built for an executive presentation. The project goes from raw data to cleaned features, exact survival statistics, presentation-ready visuals and a one-page business summary, with the rationale for every decision documented in the notebook.

![Python](https://img.shields.io/badge/Python-3.9%2B-0B2545?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Wrangling-13A89E)
![Seaborn](https://img.shields.io/badge/Seaborn-Visualization-1F6F8B)
![Colab](https://img.shields.io/badge/Google%20Colab-Ready-0B2545?logo=googlecolab&logoColor=white)

---

## 📌 Project Overview

| | |
|:--|:--|
| **Goal** | Identify what drove survival on the Titanic and communicate it to business stakeholders |
| **Dataset** | [Kaggle Titanic](https://www.kaggle.com/c/titanic/data): `train.csv` (891 passengers, labelled) and `test.csv` (418 passengers, unlabelled) |
| **Approach** | Clean → Describe → Visualize → Validate statistically → Summarize |
| **Deliverable** | A single, well-documented Jupyter / Colab notebook plus 300-DPI charts |

---

## 🔑 Key Findings

> Survival was determined mainly by **who you were**: gender first, ticket class second, age and family context third.

| Segment | Survival rate |
|:--|--:|
| **Overall** | **38.4%** |
| Women / Men | 74.2% / 18.9% |
| 1st / 2nd / 3rd class | 63.0% / 47.3% / 24.2% |
| 1st-class women (best segment) | 96.8% |
| 3rd-class men (worst segment) | 13.5% |
| Children (0-12) | 58.0% |
| Small families (2-4) / Alone / Large (5+) | 57.9% / 30.4% / 16.1% |

- **Gender and class compound each other.** A 3rd-class woman (50.0%) had better odds than a 1st-class man (36.9%).
- **Family size is non-linear.** Its linear correlation with survival is near zero (0.02), yet small groups did far better than solo travellers or large families.
- **Port of embarkation is mostly a class effect.** 51% of Cherbourg passengers were first class versus 20% at Southampton.
- **All segmentations are statistically significant** (chi-square, p < 0.05). Gender has the largest effect size (Cramér's V ≈ 0.54).
- A simple logistic regression reaches about **80% cross-validated accuracy** (versus a 61.6% naive baseline).

---

## 🗂️ Repository Structure

```
.
├── titanic_eda_colab.ipynb   # Main analysis notebook (run this)
├── train.csv                 # Kaggle training data (you provide)
├── test.csv                  # Kaggle test data (optional, you provide)
├── figures/                  # Charts, created automatically when the notebook runs
└── README.md
```

---

## 🧭 Workflow

| # | Stage | What happens |
|:-:|:--|:--|
| 1 | **Acquire data** | Loads `train.csv` / `test.csv` directly, with an upload prompt in Colab |
| 2 | **Describe data** | Structure, data types, distributions and a missing-value audit |
| 3 | **Clean & preprocess** | Imputation, type conversion and feature engineering, with a rationale for each step |
| 4 | **Grouped statistics** | Exact survival rates by gender, class, gender × class, age bracket and family size |
| 5 | **Visualize** | Bar plots, correlation heatmap, KDE age distribution, class × age facets |
| 6 | **Validate (bonus)** | Chi-square tests, Cramér's V effect sizes, logistic-regression driver ranking |
| 7 | **Executive summary** | Key drivers, data-quality notes and actionable insights |

### Data cleaning decisions

| Column | Issue | Treatment | Why |
|:--|:--|:--|:--|
| `Age` | 19.9% missing | Median by `Pclass` × `Sex`, plus an `Age_Was_Missing` flag | Missingness is not random (27.7% in 3rd class), and the median is robust to outliers |
| `Embarked` | 2 missing | Mode (`S`) | Immaterial gap, so the lowest-risk fill |
| `Cabin` | 77.1% missing | Not imputed; `Cabin_Known` flag and `Deck` feature | Imputing would be fabrication; the missingness itself carries signal |
| Types | Strings and integers | `Sex`, `Embarked`, `Deck` as category; `Pclass` as ordered category | Memory-efficient and semantically correct |

**Engineered features:** `Family_Size`, `Family_Group`, `Age_Group` (Child / Youth / Adult / Senior), `Title`, `Cabin_Known`, `Deck`.

---

## 🚀 Getting Started

### Option A: Google Colab (recommended)

1. Upload `titanic_eda_colab.ipynb` to [Google Colab](https://colab.research.google.com/) (*File → Upload notebook*).
2. Run the **Setup** cell, then the **Acquire data** cell.
3. When prompted, click **Choose Files** and select `train.csv` (and optionally `test.csv`). No zip file is needed.
4. Run the remaining cells (*Runtime → Run all*).

### Option B: Local Jupyter

```bash
# 1. Install dependencies
pip install pandas numpy seaborn matplotlib scipy scikit-learn jupyter

# 2. Place train.csv (and optionally test.csv) next to the notebook, then launch
jupyter notebook titanic_eda_colab.ipynb
```

The notebook searches the current folder, `/content`, Google Drive and Kaggle input paths automatically.

---

## 📊 Outputs

Charts are saved at **300 DPI** to `figures/` when the notebook runs, in a consistent **Navy / Teal** theme with hidden top and right spines, percentage labels and clear axis titles.

| File | Description |
|:--|:--|
| `01_survival_bars.png` | Survival rate by gender, class and gender × class |
| `02_correlation_heatmap.png` | Correlation matrix of numeric variables |
| `03_age_kde.png` | Age density overlaid by survival status |
| `04_family_age.png` | Survival by family size and age bracket |
| `05_class_age_facets.png` | Age distribution by class and survival |
| `06_driver_ranking.png` | Standardised logistic-regression coefficients |

---

## 🛠️ Tech Stack

- **Python 3.9+**
- **Pandas / NumPy**: data wrangling and aggregation
- **Matplotlib / Seaborn**: visualization
- **SciPy**: chi-square tests and effect sizes
- **scikit-learn**: logistic-regression driver ranking and cross-validation

---

## ⚠️ Limitations & Data Quality Notes

- **Imputed ages:** Median imputation narrows the age spread slightly and creates a visible spike near age 25 in 3rd class, so exact age distributions should not be over-interpreted.
- **Cabin:** Used only as a known/unknown indicator. It reflects record-keeping and class, not cabin location.
- **Small samples:** Only 22 seniors and 62 passengers in large families, so those rates are indicative.
- **Scope:** The 891-passenger sample covers about 40% of the roughly 2,224 people aboard.
- **Correlation, not causation:** Findings describe associations in historical data.

---

## 🔮 Possible Next Steps

- Add fare-per-person and a ticket-group size feature.
- Use `Title` and `Deck` in a gradient-boosting model and compare against the ~80% logistic baseline.
- Build a one-slide-per-insight presentation deck from the exported charts.
- Add a Kaggle submission step using `test.csv`.

---

## 📚 References

- [Kaggle Titanic data page](https://www.kaggle.com/c/titanic/data)
- [Data Science Solutions: Startup Workflow](https://www.amazon.com/Data-Science-Solutions-Startup-Workflow/dp/1520545312) (workflow style reference)
- [A journey through Titanic](https://www.kaggle.com/omarelgabry/titanic/a-journey-through-titanic)
