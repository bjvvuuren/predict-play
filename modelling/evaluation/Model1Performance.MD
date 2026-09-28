# Model 1 Performance — R-GCN

## Performance results

| Metric | R-GCN | Baseline |
|---|---:|---:|
| Average Precision | 0.4650 | 0.2320 |
| ROC-AUC | 0.7479 | 0.5000 |
| Brier Score | 0.1532 | 0.1782 |
| Log Loss | 0.4697 | 0.5417 |
| Precision at 0.50 | 0.5716 | 0.0000 |
| Recall at 0.50 | 0.2049 | 0.0000 |
| F1 at 0.50 | 0.3017 | 0.0000 |


## Ranking performance

Average Precision is the primary metric because the positive class is imbalanced and the business use case is primarily a ranking problem: stronger acquisition candidates should appear above weaker candidates.

The R-GCN achieved an **Average Precision of 0.4650**, compared with the baseline of **0.2320**. This is approximately **2.0 times the baseline Average Precision**, showing that the model ranks successful titles substantially better than a non-informative ranking.

The **ROC-AUC of 0.7479** also indicates useful discrimination between titles that meet and do not meet the simulated engagement threshold.

## Probability quality

The R-GCN achieved a **Brier Score of 0.1532**, lower than the baseline score of 0.1782. Its **Log Loss of 0.4697** was also lower than the baseline value of 0.5417.

Both measures therefore indicate that the R-GCN's predicted probabilities contain useful information beyond simply assigning every title the overall success rate.


## Interpretation

The R-GCN demonstrates meaningful predictive ability on the synthetic acquisition catalogue. Its ranking performance is roughly twice the baseline in Average Precision and its ROC-AUC is substantially above random performance.

The model's main advantage is that it can combine the shared title features with explicit movie-to-genre and movie-to-release-decade relationships. However, the current graph is deliberately limited, meaning that the relational information available to the R-GCN is relatively shallow.

The results therefore show that the R-GCN is a viable model for the benchmark, but its value should ultimately be assessed against the simpler XGBoost alternative using the same evaluation titles and outcomes.
