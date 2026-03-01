# Backorder Risk Prediction (Kaggle)

## Overview
This project builds a binary classification model to predict whether a product will go on backorder (`went_on_backorder = Yes/No`). Because backorders are rare, the project emphasizes **ranking and prioritization** (Top-K review) using **Precision–Recall metrics** rather than accuracy.

**Primary goal:** Identify the highest-risk items (e.g., Top 5%) so an operations team could prioritize mitigation actions (replenishment, expediting, substitutions, inventory rebalancing, etc.).

---

## Dataset
Source: Kaggle — *Predict Product Backorders*  
Files used:
- `Kaggle_Training_Dataset_v2.csv`
- `Kaggle_Test_Dataset_v2.csv`

> Note: Raw data files are not included in this repo. Download them from Kaggle and place them in a local `data/` folder.

---

## Methods
### Preprocessing
- Dropped identifier column `sku` from model features (kept for reporting/submission).
- Converted Yes/No flag features to 0/1.
- Handled missing values with **median imputation**, fitting medians on the training split and applying to validation/test to avoid leakage.
- Ensured train/validation/test feature columns match before prediction.

### Modeling
- Baseline: Logistic Regression (class imbalance aware)
- Final model: `HistGradientBoostingClassifier` (scikit-learn)

### Evaluation
Because the dataset is highly imbalanced, evaluation focuses on:
- **PR-AUC (Average Precision)**
- **Top-K Prioritization:** Precision@K and Recall@K for Top 0.5%, 1%, 2%, 5% risk-ranked items
- Precision–Recall curve

Artifacts saved:
- `pr_curve.png`
- `top15_feature_importance.png`
- `topk_metrics.csv`
- `results.json`

---

## Results
Held-out validation performance:
- **PR-AUC:** **0.228**
- **Top 5% prioritization:** **Precision@K = 0.100**, **Recall@K = 0.745**

Interpretation:
- Using the model as a risk-ranking tool, reviewing the **top 5% highest-risk items** captures about **74.5% of true backorders** while maintaining about **10% precision** in the flagged list.

---

## Repository Structure


---

## How to Run
1) Clone the repo:
```bash
git clone <github.com/spensersmith99/Backorder_Project>
cd <Backorder_Project>
```
2) Install the requirements
```bash
pip install -r requirements.txt
```
3) Download the Kaggle dataset and place files here:<br>
Dataset Link: https://www.kaggle.com/datasets/ztrimus/backorder-dataset <br>
data/Kaggle_Training_Dataset_v2.csv<br>
data/Kaggle_Test_Dataset_v2.csv
<br>
4) Open and run the notebook:
```bash
jupyter notebook BO_Project.ipynb
```
