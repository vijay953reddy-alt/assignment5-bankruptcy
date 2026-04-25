# Assignment 5 Results Summary

## Problem Statement
- Goal: predict whether a company is likely to go bankrupt.
- Target: `Bankrupt?`; positive class: `1`.
- Business rule: missing a bankrupt company is worse than incorrectly flagging a healthy company.

## Dataset Summary
- Rows: 6819
- Columns: 96
- Positive count: 220
- Positive rate: 3.23%
- Missing values: 0
- Duplicate rows: 0

## Metric Choice
- The positive class is rare, so accuracy is misleading.
- PR-AUC is useful because it focuses on precision and recall for the rare class.
- Brier score measures predicted probability quality; lower is better.
- Threshold metrics like precision, recall, F1, and F2 describe one operating point only and do not fully describe calibration.

## Feature Selection
- Feature Set A used all usable numeric features.
- Feature Set B used the top 25 XGBoost importance features from the training set only.
- The test set was not used for feature selection.

## Five-Experiment Results Table
|   exp_id | model                        | feature_set   | main_settings                 |   train_pr_auc |   val_pr_auc |   overfit_gap |   val_roc_auc |   val_brier |   threshold |   val_precision |   val_recall |   val_f1_or_f2 | selected_finalist   | notes                         |
|---------:|:-----------------------------|:--------------|:------------------------------|---------------:|-------------:|--------------:|--------------:|------------:|------------:|----------------:|-------------:|---------------:|:--------------------|:------------------------------|
|        1 | Logistic Regression baseline | A             | class_weight=balanced, scaled |         0.4704 |       0.3162 |        0.1542 |        0.8776 |      0.0889 |         0.5 |          0.1761 |       0.7576 |         0.4562 | No                  | Simple baseline model.        |
|        2 | XGBoost baseline             | A             | n=200, depth=3, lr=0.05       |         0.9514 |       0.4553 |        0.4961 |        0.9457 |      0.0232 |         0.5 |          0.5714 |       0.1212 |         0.1439 | No                  | First XGBoost model.          |
|        3 | XGBoost imbalance handling   | A             | scale_pos_weight=30.0         |         0.9857 |       0.3894 |        0.5962 |        0.9434 |      0.0317 |         0.5 |          0.3962 |       0.6364 |         0.5676 | No                  | Handles rare class.           |
|        4 | XGBoost lightly tuned        | A             | best of 4 manual configs      |         0.9422 |       0.4456 |        0.4966 |        0.9498 |      0.0341 |         0.5 |          0.371  |       0.697  |         0.5928 | No                  | Light validation tuning only. |
|        5 | XGBoost selected features    | B             | best config + top 25 features |         0.814  |       0.4595 |        0.3545 |        0.9504 |      0.0415 |         0.5 |          0.3038 |       0.7273 |         0.5687 | Yes                 | Selected feature set B.       |

## Winning Model
- Selected experiment: 5
- Selected model: XGBoost selected features
- Selected before using the test set.
- Chosen using validation PR-AUC, validation Brier score, and overfit gap.
- Final validation-selected threshold: 0.66.

## Final Test Results
|   test_pr_auc |   test_roc_auc |   test_brier |   test_precision |   test_recall |   test_f2 |   final_threshold |
|--------------:|---------------:|-------------:|-----------------:|--------------:|----------:|------------------:|
|         0.418 |         0.9487 |       0.0509 |           0.3239 |         0.697 |    0.5665 |              0.66 |

## Confusion Matrix
| Actual   |   Predicted 0 |   Predicted 1 |
|:---------|--------------:|--------------:|
| Actual 0 |           942 |            48 |
| Actual 1 |            10 |            23 |

## Calibration Result
- Test Brier score: 0.0509
- Calibration curve saved as `artifacts/calibration_curve.png`.
- Probabilities should be interpreted carefully because bankruptcy is rare.

## Feature Importance Result
- Top feature: ` Debt ratio %`.
- It is the most influential according to built-in XGBoost importance.
- Limitation: importance is not the same as causation.

## Overfitting Discussion
- Overfit gap = train PR-AUC minus validation PR-AUC.
- Final model overfit gap: 0.3545.
- Use the assignment guide: 0.00–0.05 usually fine, 0.05–0.10 watch carefully, above 0.10 needs explanation.

## AI Usage
- Used Codex/AI assistance to outline the notebook and reusable evaluation function.
- Used AI assistance to generate plotting code.
- Verified manually that the test set was not used until final evaluation.
- Checked that accuracy was not used as the primary metric.
- Watched for AI suggestions that accidentally tune on the test set.
