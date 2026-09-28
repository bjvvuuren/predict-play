# Part B - Model 2: XGBoost

## Code and usage

Run `03_model_2.ipynb` after `02_feature_engineering.ipynb`.

The model predicts:

> **P(title meets the engagement threshold)**

XGBoost uses the shared engineered title features created in the feature-engineering notebook. 

## Why XGBoost was chosen

XGBoost provides a strong tabular baseline for the acquisition problem.

It is well suited to:

- nonlinear relationships;
- interactions between title features;
- mixed-strength predictors;
- imbalanced binary classification.

It also provides a useful comparison with the R-GCN because both models use the same core movie features, while only the R-GCN uses graph relationships.

## Model configuration

The model uses relatively shallow, regularised trees and a low learning rate to reduce overfitting.

| Parameter | Value |
|---|---:|
| Objective | `binary:logistic` |
| Tree method | `hist` |
| Maximum trees | 800 |
| Maximum depth | 4 |
| Learning rate | 0.04 |
| Minimum child weight | 5 |
| Row sampling | 0.85 |
| Feature sampling | 0.85 |
| L1 regularisation | 0.1 |
| L2 regularisation | 5.0 |
| Early stopping | 40 rounds |
| Random seed | 42 |

`scale_pos_weight` is calculated from the training data as:

```text
negative titles / positive titles
```

This gives more weight to the less common positive class.

## Training and model selection

The model is trained on the training split.

The tuning split is supplied as the evaluation set and controls early stopping using **log loss**. Training stops if log loss does not improve for 40 rounds.

After training, **Average Precision** is calculated on the tuning split.

Average Precision is reported because the target is imbalanced and the business problem is concerned with ranking stronger acquisition candidates above weaker ones.

## Probability calibration

The selected XGBoost model produces a raw prediction margin for each title.

A logistic-regression calibrator is then fitted using only the separate calibration split.

This converts the model score into:

```text
P(title meets engagement threshold)
```

The calibration step is probability post-processing rather than a separate competing model.

## Inputs

| Input | Purpose |
|---|---|
| `model_features.csv` | Shared engineered title features, development roles and platform targets. |

## Outputs

| Output | Purpose |
|---|---|
| `xgboost.ubj` | Saved XGBoost model. |
| `xgboost_calibrator.joblib` | Probability calibrator. |
| `xgboost_model_summary.csv` | Tuning Average Precision and training time. |
| `xgboost_catalogue_predictions.csv` | Predicted probabilities for acquisition titles. |
| `xgboost_tuning_predictions.csv` | Tuning predictions for later evaluation. |