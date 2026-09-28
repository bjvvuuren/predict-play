# Model 2 Performance — XGBoost

## Performance results

| Metric | XGBoost | Baseline |
|---|---:|---:|
| Average Precision | 0.4718 | 0.2320 |
| ROC-AUC | 0.7491 | 0.5000 |
| Brier Score | 0.1528 | 0.1782 |
| Log Loss | 0.4700 | 0.5417 |
| Precision at 0.50 | 0.5628 | 0.0000 |
| Recall at 0.50 | 0.2360 | 0.0000 |
| F1 at 0.50 | 0.3325 | 0.0000 |

The baseline assigns the same training-prevalence probability to every title. Its Average Precision is therefore approximately equal to the positive rate of the evaluation dataset. Its threshold-based metrics are zero because the baseline probability remains below 0.50 for every title.

## Ranking performance

XGBoost achieved an **Average Precision of 0.4718**, compared with the baseline value of **0.2320**. This is approximately **2.03 times the baseline Average Precision**, showing that XGBoost provides a substantially more useful ranking of acquisition titles than a non-informative benchmark.

The model also achieved a **ROC-AUC of 0.7491**, indicating useful discrimination between successful and unsuccessful titles.

## Probability quality

XGBoost achieved a **Brier Score of 0.1528**, compared with 0.1782 for the baseline. Its **Log Loss of 0.4700** was also lower than the baseline value of 0.5417.

The lower Brier Score indicates that the model's probability estimates are more accurate than simply assigning every title the overall training success rate.

## Interpretation

XGBoost demonstrates meaningful predictive performance on the synthetic acquisition catalogue. It more than doubles the baseline Average Precision and provides useful discrimination between successful and unsuccessful titles.

The result is particularly relevant because XGBoost uses the engineered title features directly rather than relying on graph relationships. These features include release information, one-hot encoded genres, historical genre-success summaries and metadata-availability indicators.

The model therefore provides a strong tabular benchmark for the acquisition problem and should be compared directly with the R-GCN before selecting a preferred model.
