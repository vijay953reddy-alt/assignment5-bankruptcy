# Assignment 5: Comparative Binary Classification with XGBoost

## Project Overview

This project builds a binary classification model to predict whether a company is likely to go bankrupt. The target variable is `Bankrupt?`, where `1` means the company went bankrupt and `0` means it did not.

The main goal is not to get a perfect score, but to follow a proper machine learning workflow using train, validation, and test splits, model comparison, imbalance-aware metrics, calibration checking, and final model evaluation.

## Dataset

Dataset used:

- Source: `https://github.com/bipin-a/ml-workshop/tree/main/training`
- File used: `training/data.csv`
- Target column: `Bankrupt?`
- Positive class: `1`
- Approximate rows: 6,819
- Approximate columns: 96

Only `training/data.csv` is used for modeling.

## Models Compared

The notebook runs exactly 5 experiments:

1. Logistic Regression baseline
2. XGBoost baseline
3. XGBoost with imbalance handling
4. Lightly tuned XGBoost
5. XGBoost with selected features

## Metrics Used

Because the dataset is highly imbalanced, accuracy is not used as the main metric.

Main metrics:

- PR-AUC: used to compare model discrimination for the rare bankrupt class
- Brier Score: used to check probability calibration

Secondary metrics:

- ROC-AUC
- Precision
- Recall
- F2-score
- Confusion matrix

## Project Structure

```text
.
├── vijay assignment5.ipynb
├── README.md
├── results_summary.md
├── training/
│   └── data.csv
└── artifacts/
    ├── experiment_results.csv
    ├── final_test_metrics.csv
    ├── confusion_matrix.csv
    ├── precision_recall_curve.png
    ├── calibration_curve.png
    ├── roc_curve.png
    ├── feature_importance.png
    ├── feature_importance.csv
    └── final_model.joblib
