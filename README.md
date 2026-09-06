# Blood Donation Prediction

Predicts whether a blood donor will donate again, using historical donation
data from a mobile blood donation vehicle in Taiwan.

## Problem Statement

Blood transfusion services depend on reliable, repeat donors. This project
builds a classification model to predict whether a donor will give blood
again during a future donation drive, based on their donation history.

- **Task 1:** Complete data analysis report on the donor dataset
- **Task 2:** Build a predictive model to identify likely repeat donors

## Dataset

576 donor records with the following features:

| Feature | Description |
|---|---|
| Months since Last Donation | Months since the donor's most recent donation |
| Number of Donations | Total donations made by the donor |
| Total Volume Donated (c.c.) | Total blood volume donated (removed — see below) |
| Months since First Donation | Months since the donor's first donation |
| Made Donation in March 2007 | Target: 1 = donated, 0 = did not |

## Approach

1. **Data Cleaning** — dropped an unused index column and renamed columns
   for readability.
2. **Exploratory Data Analysis**
   - Target class distribution (~76% did not donate, ~24% did — imbalanced)
   - Feature distributions (right-skewed across the board)
   - Correlation analysis — found *Number of Donations* and *Total Volume
     Donated* were perfectly correlated (1.0), since volume is simply a
     fixed multiple of donation count. Dropped the redundant column.
   - Outlier check via boxplots — kept legitimate high-frequency donors.
3. **Modeling**
   - Stratified train/test split to preserve class balance
   - Feature scaling for Logistic Regression
   - Trained three models: Logistic Regression, Decision Tree, Random Forest
4. **Evaluation**
   - Compared models on Accuracy, Precision, Recall, F1 Score, and ROC-AUC
   - Accuracy alone is misleading here due to class imbalance, so F1 and
     ROC-AUC were prioritized

## Results

| Model | Accuracy | Precision | Recall | F1 Score | ROC-AUC |
|---|---|---|---|---|---|
| Logistic Regression | 0.776 | 0.625 | 0.179 | 0.278 | 0.572 |
| Decision Tree | 0.655 | 0.286 | 0.286 | 0.286 | 0.529 |
| **Random Forest** | 0.733 | 0.429 | 0.321 | **0.367** | **0.593** |

**Random Forest** is recommended for production — it has the best balance of
precision and recall (highest F1 Score) and the strongest ability to
separate the two classes (highest ROC-AUC).

## Challenges Faced

- **Multicollinearity** between Number of Donations and Total Volume
  Donated — resolved by dropping the redundant feature.
- **Class imbalance** in the target variable — addressed with stratified
  sampling and by weighting model comparison toward F1/ROC-AUC rather than
  raw accuracy.
- **Outliers** in donation frequency — kept as valid data rather than
  removed, since they reflect real donor behavior.

## Tools Used

Python, Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn

## Repository Contents

- `PRCP-1011-BloodDonaPred.ipynb` — full notebook (EDA, modeling, reports)
- `Warm_Up_Predict_Blood_Donations_-_Training_Data.csv` — dataset
- `README.md` — this file
