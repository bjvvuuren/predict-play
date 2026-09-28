# Model Comparison — R-GCN vs XGBoost

## Overall performance

| Metric | R-GCN | XGBoost |
|---|---:|---:|
| Average Precision | 0.4650 | **0.4718** |
| ROC-AUC | 0.7479 | **0.7491** |
| Brier Score | 0.1532 | **0.1528** |
| Log Loss | **0.4697** | 0.4700 |
| Precision at 0.50 | **0.5716** | 0.5628 |
| Recall at 0.50 | 0.2049 | **0.2360** |
| F1 at 0.50 | 0.3017 | **0.3325** |

Both models substantially outperform the baseline Average Precision of **0.2320** and baseline ROC-AUC of **0.5000**.

R-GCN achieved approximately **2.00 times** the baseline Average Precision, while XGBoost achieved approximately **2.03 times** the baseline.

## Ranking comparison

Average Precision is the main comparison metric because the task is imbalanced and the practical objective is to rank stronger acquisition candidates above weaker candidates.

XGBoost achieved the highest Average Precision:

- R-GCN: **0.4650**
- XGBoost: **0.4718**
- difference: **0.0068**

The difference is small. A paired title-level bootstrap with 1,000 resamples produced a 95% interval for the XGBoost-minus-R-GCN Average Precision difference of approximately:

**-0.0004 to 0.0146**

Because this interval includes zero, the evaluation does not clearly separate the two models' ranking performance. The result is therefore better described as **broadly comparable performance with a small observed advantage for XGBoost**, rather than evidence that XGBoost decisively outperforms the R-GCN.

The two models' probabilities are also strongly related, with a correlation of approximately **0.91**, indicating that they tend to rank many titles similarly.

## Probability comparison

XGBoost produced the slightly lower Brier Score:

- R-GCN: **0.1532**
- XGBoost: **0.1528**

This gives XGBoost a very small advantage in overall probability accuracy.

R-GCN produced the slightly lower Log Loss:

- R-GCN: **0.4697**
- XGBoost: **0.4700**

The difference is extremely small. Taken together, the Brier Score and Log Loss results indicate that the probability quality of the two models is effectively very similar.

## Interpretation

The evaluation does not show a large predictive-performance gap between the two approaches. Both models approximately double the baseline Average Precision and achieve ROC-AUC values close to 0.75.

XGBoost records the highest Average Precision, ROC-AUC, F1 and Top-100 precision, as well as a marginally lower Brier Score. R-GCN records slightly higher precision at the 0.50 cutoff and a marginally lower Log Loss.

The paired Average Precision analysis shows that the observed difference is small enough that it should not be treated as a decisive statistical separation between the models.