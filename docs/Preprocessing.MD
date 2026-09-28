# Part B - Data Preprocessing

## Code and usage

Run `01_preprocessing.ipynb` from top to bottom before `02_feature_engineering.ipynb`.

The notebook downloads the public MovieLens 20M dataset, enriches the movie catalogue with the TMDB daily ID export, creates a simulated StadioChoice platform and acquisition catalogue, generates synthetic viewing outcomes, and exports the tables used by the later modelling notebooks.

A fixed random seed of `42` is used for reproducibility.

## Why synthetic data is used

The public MovieLens dataset provides movie metadata and user-title interactions, but it does not contain the StadioChoice business outcome required for this project.

The modelling target is:

> A title is successful when at least **150 distinct households** each watch at least **50% of the title runtime** during a 12-month period.

MovieLens does not contain streaming watch duration, StadioChoice catalogue membership, acquisition outcomes, or StadioChoice search logs. These fields therefore have to be simulated so that the public dataset can represent the business problem.

The synthetic data is a modelling exercise rather than observed StadioChoice behaviour. Results must therefore be validated against real business data before operational use.

## Inputs

| Input | Use |
|---|---|
| `movies.csv` | MovieLens movie ID, title and genres. |
| `links.csv` | Links MovieLens titles to TMDB IDs. |
| `ratings.csv` | Identifies household-title interactions. `userId` is treated as a simulated household. The rating value itself is **not** used to generate the target. |
| TMDB daily movie ID export | Supplies TMDB identifiers and popularity used only to simulate the synthetic viewing outcome. |

The TMDB daily export is downloaded for the previous day. It is used instead of making individual TMDB API requests.

## How the synthetic data is constructed

### 1. Build the title catalogue

MovieLens `movies.csv` and `links.csv` are joined using `movieId`.

The notebook then creates:

- `release_year`
- `canonical_title`
- `tmdb_id`
- an internal `title_id` in the form `STAD-MOV-000001`

### 2. Split titles into platform and acquisition pools

Distinct MovieLens titles are randomly divided into:

- **40% platform titles** - treated as titles already available on StadioChoice.
- **60% acquisition catalogue titles** - treated as titles that could potentially be acquired.

The split is performed at title level using seed `42`.

### 3. Add TMDB metadata

The catalogue is joined to the TMDB daily movie export using `tmdb_id`.

TMDB popularity is retained because it is used to create the synthetic viewing outcome. It is **not** used as a model predictor because that would leak the mechanism used to generate the target.

### 4. Generate synthetic viewing

Only MovieLens household-title pairs belonging to the simulated platform catalogue are used.

For each pair:

1. TMDB popularity is converted to a percentile across platform titles.
2. The percentile becomes the probability that the household qualifies as an engaged viewer.
3. A qualifying household is assigned a watch fraction between **50% and 100%**.
4. A non-qualifying household is assigned a watch fraction between **5% and 49%**.
5. A fixed synthetic runtime of **120 minutes** is used because the TMDB daily export does not provide runtime.
6. A viewing timestamp is randomly assigned within the synthetic period **2025-09-01 to 2026-09-01**.

The simulated viewing records are then aggregated to household-title level and finally to title level.

The binary target is:

```text
target = 1 if qualifying_households >= 150
target = 0 otherwise
```

A qualifying household must have watched at least 50% of the synthetic title runtime.

### 5. Generate synthetic search data

The notebook also creates 50,000 illustrative search events.

The main assumptions are:

- 70% of searches are for platform titles.
- 30% are for acquisition-catalogue titles.
- acquisition-title searches return zero results.
- 75% of successful searches result in a click.
- title search demand is generated independently using random lognormal weights.

The search simulation is included to reproduce the requested data structure. It is not used as evidence of real demand and is excluded from the predictive models.

## Outputs

| Output | Description |
|---|---|
| `data/content_catalogue.csv` | Full title catalogue and platform/acquisition assignment. |
| `data/platform_catalogue.csv` | Titles assigned to the simulated existing platform catalogue. |
| `data/acquisition_catalogue.csv` | Titles held out as acquisition candidates. |
| `data/platform_sessions.csv` | Synthetic household-title viewing sessions. |
| `data/platform_title_outcomes.csv` | Title-level household counts and the binary modelling target. |
| `data/search_queries.csv` | Synthetic StadioChoice-style search events. |

`household_title_totals` and `daily_viewing` are created during preprocessing as intermediate validation tables and do not need to be exported for model training.

## Leakage boundary

The following fields are excluded from model predictors because they either generate or directly reveal the synthetic target:

- `tmdb_popularity`
- `popularity_percentile`
- `qualifying_probability`
- `qualifying_households`
- `observed_households`
- watch duration and watch fraction
- MovieLens rating values

The modelling notebooks therefore have to predict title success from metadata and historical analogue features rather than being given the information used to create the answer.
