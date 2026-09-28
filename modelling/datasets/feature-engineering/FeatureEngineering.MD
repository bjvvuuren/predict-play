# Part B - Feature Engineering

## Code and usage

Run `02_feature_engineering.ipynb` after `01_preprocessing.ipynb`.

The notebook converts the preprocessing outputs into a shared set of model-ready title features for **XGBoost** and the **R-GCN**. The feature engineering is designed around the real prediction problem: scoring acquisition titles that do not yet have their own StadioChoice viewing history.

## Feature engineering approach

The design choice was to use only information that would realistically be available when evaluating a new acquisition title.

This means the models can use title metadata and historical evidence from other titles, but they cannot use the candidate title's own engagement outcome.

The feature set therefore focuses on:

- release year and title age;
- MovieLens genres;
- number of genres;
- TMDB metadata availability;
- historical genre performance calculated from a separate reference library.

The same core title features are used by both models so that the comparison is based mainly on the modelling technique rather than completely different input data.

## Why a reference library is used

A new acquisition title has no StadioChoice viewing history.  To recreate a cold-start acquisition decision, 25% of platform titles are reserved as a **reference library**.

The platform titles are split into:

- **25% reference** - used to create historical genre summaries;
- **50% training** - used to fit the models;
- **12.5% tuning** - used for model selection and early stopping;
- **12.5% calibration** - used to calibrate predicted probabilities.

The acquisition catalogue remains separate and has no development target.

## Historical genre features

The reference library is used to estimate how genres have performed historically.

For each genre the notebook calculates:

- number of reference titles;
- number of successful reference titles;
- a smoothed success rate.

Smoothing is used because some genres have fewer reference examples. Rather than allowing a small number of titles to create an extreme success rate, the genre estimate is pulled toward the overall reference success rate.

These genre statistics are then summarised for each movie as:

- `reference_genre_success_mean`
- `reference_genre_success_max`
- `reference_genre_support`
- `reference_genre_coverage`

This provides a historical analogue signal without using the candidate title's own outcome.

## Shared tabular features

The main model-ready feature table contains:

### Numeric features

- filled release year;
- title age relative to 2026;
- release-year missing flag;
- genre count;
- reference genre success statistics;
- reference genre support and coverage;
- TMDB availability flag.

Missing release years are filled using the **training-set median only**.

Numeric variables are standardised using a `StandardScaler` fitted only on training titles. This avoids allowing tuning, calibration or acquisition titles to influence the fitted preprocessing values.

### Genre features

MovieLens genres are one-hot encoded.

For example:

```text
genre_action
genre_comedy
genre_drama
genre_thriller
```

This gives XGBoost a direct tabular representation of genre membership and also forms part of the movie-node features used by the R-GCN.

## Why the R-GCN graph uses genre and release decade

The R-GCN requires explicit relationships between entities rather than only a flat feature table.

The graph contains:

- movie nodes;
- genre nodes;
- release-decade nodes.

Four directed relationships are created:

- `movie_has_genre`
- `genre_has_movie`
- `movie_released_in_decade`
- `decade_has_movie`

Genre and release decade were chosen because they are available consistently for both platform and acquisition titles.

The current TMDB daily export does not provide the richer actor, director or studio information that would support a larger knowledge graph. The graph is therefore deliberately limited to relationships that can be constructed consistently without additional API calls.

The purpose of the graph is to allow the R-GCN to learn a movie representation from both its own metadata and the metadata relationships it shares with other titles.

## Why both models use the same movie features

XGBoost receives the engineered title features directly.

The R-GCN movie nodes start with the same feature block, but the R-GCN can additionally pass information through the genre and decade relationships.

This gives a useful comparison:

```text
XGBoost
title features
    -> prediction

R-GCN
title features + graph relationships
    -> prediction
```

If the R-GCN performs better, the graph structure is adding useful information. If performance is similar, the additional graph complexity may not be justified with the limited relationships currently available.

## Features deliberately excluded

The following variables are not used as predictors:

- `tmdb_popularity`
- `popularity_percentile`
- `qualifying_probability`
- `observed_households`
- `qualifying_households`
- watch duration
- MovieLens rating values
- synthetic search demand
- synthetic licence information

TMDB popularity was used to construct the synthetic target, while household and viewing fields directly reveal that outcome. Including them would create target leakage.

Synthetic search data is also excluded because it was generated independently and should not be treated as evidence of real title demand.

## Inputs

| Input | Purpose |
|---|---|
| `platform_catalogue.csv` | Metadata for simulated existing platform titles. |
| `acquisition_catalogue.csv` | Metadata for titles to be scored as acquisition candidates. |
| `platform_title_outcomes.csv` | Provides the binary target for platform titles. |

## Outputs

| Output | Purpose |
|---|---|
| `model_features.csv` | Shared title-level feature matrix used by XGBoost and R-GCN movie nodes. |
| `genre_reference_stats.csv` | Historical genre statistics created from the reference library. |
| `rgcn_edges.csv` | Directed movie-genre and movie-decade graph relationships. |
| `rgcn_node_features.csv` | Feature matrix for all movie, genre and decade graph nodes. |
| `feature_scaler.joblib` | Training-fitted scaler used for the numeric features. |
