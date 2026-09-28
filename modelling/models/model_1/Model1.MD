# Part B - Model 1: R-GCN

## Code and usage

Run `03_model_1.ipynb` after `02_feature_engineering.ipynb`.

The model predicts the probability that an acquisition title will meet the defined engagement threshold.

## Why R-GCN was chosen

R-GCN was selected because the acquisition problem contains relationships between titles and shared metadata.

The graph links:

- movies to genres;
- genres back to movies;
- movies to release decades;
- release decades back to movies.

This allows the model to learn from both a movie's own features and information passed through related nodes.

The aim is to test whether this relational structure adds value beyond a standard tabular model.

## Model structure

The R-GCN uses two graph-convolution layers.

Each relation type has its own trainable weight matrix, so information from a genre relationship can be treated differently from information from a release-decade relationship.

Incoming neighbour messages are averaged and combined with the node's own transformed features.

The architecture is:

```text
Node features
    |
R-GCN
    |
ReLU + Dropout
    |
R-GCN
    |
Title logit
```

Key settings:

| Parameter | Value |
|---|---:|
| Hidden dimension | 65 |
| Dropout | 0.20 |
| Optimiser | Adam |
| Learning rate | 0.002 |
| Weight decay | 0.0001 |
| Maximum epochs | 120 |
| Early-stopping | 15 |

## Loss and model selection

The model uses `BCEWithLogitsLoss` because the target is binary.

A positive-class weight is applied because successful titles are less common than unsuccessful titles.

The model is trained on the training split and evaluated after each epoch using **Average Precision** on the tuning split. The best-performing epoch is retained.

Average Precision is used because the classes are imbalanced and the business use case is mainly concerned with ranking stronger acquisition candidates above weaker ones.

## Probability calibration

The R-GCN produces raw logits rather than directly calibrated probabilities.

A logistic-regression calibrator is fitted on the separate calibration split to convert these scores into:

```text
P(title meets engagement threshold)
```

## Inputs

| Input | Purpose |
|---|---|
| `model_features.csv` | Movie features, roles and targets. |
| `rgcn_node_features.csv` | Features for movie, genre and decade nodes. |
| `rgcn_edges.csv` | Graph relationships and relation types. |

## Outputs

| Output | Purpose |
|---|---|
| `rgcn_weights.pt` | Saved R-GCN weights. |
| `rgcn_training_history.csv` | Training loss and tuning AP by epoch. |
| `rgcn_calibrator.joblib` | Probability calibrator. |
| `rgcn_model_summary.csv` | Tuning AP and training time. |
| `rgcn_catalogue_predictions.csv` | Predicted probabilities for acquisition titles. |
| `rgcn_tuning_predictions.csv` | Tuning predictions for later evaluation. |
