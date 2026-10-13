# Modeling, Evaluation, and Reporting Plan

## Feature contract

Candidate features must be known by the pre-race cutoff and supported by EDA or
a documented experiment.

Potential valid groups:

- **Grid:** actual grid position and derived bands/indicators.
- **Qualifying:** qualifying position/time plus an availability indicator.
- **Driver history:** earlier race count and historical/rolling podium rate.
- **Constructor history:** prior constructor results aggregated before the
  current race, never same-race teammate outcomes.
- **Prior standings:** driver/constructor standings from the latest earlier
  round in the same season.
- **Context:** circuit and constructor identifiers encoded as categorical data,
  subject to validation evidence and unseen-category handling.

### Initial compact baseline

The first model uses: actual grid; qualifying position and its availability
indicator; driver/constructor race counts, long-term podium rates, and last-5
podium rates; and prior driver/constructor rank. This keeps features
interpretable and comparable across historical scoring eras. Derived grid flags,
raw standing points, and circuit/constructor identifiers remain available only
for validation-only ablations.

Never include current-race results, status, points, laps, pit stops, fastest
lap, or other post-race information.

## Recommended temporal evaluation

Use a chronological split, with an initial proposal of:

| Split | Seasons | Purpose |
|---|---|---|
| Train | 1950-2017 | fit models and preprocessing |
| Validation | 2018-2021 | choose features, hyperparameters, and threshold |
| Test | 2022-2024 | final untouched evaluation |

This expands validation beyond the unusual 2020 season while retaining the most
recent regulation era as the final future-style test. Do not use random
row-level splitting: that would mix future information with historical training
data.

Fit all imputers, encoders, scalers, clipping bounds, and transformations on
training data only. Validation can guide decisions; test data cannot.

## Models and baseline

Train all main models on the same prepared driver-race rows and core feature
set. At minimum include:

1. A simple statistical baseline, such as regularized logistic regression.
2. A second statistical ML model, such as random forest or gradient boosting.
3. A shallow feed-forward neural network with one or two hidden layers.

Document the selected hyperparameters, class-imbalance treatment, and the
reason a final model is preferred. Complexity is not an objective by itself.

## Metrics and thresholding

For each model, report on training, validation, and final test data:

- ROC-AUC
- PR-AUC
- F1 score at a selected threshold

Choose the threshold and hyperparameters using training/validation performance
only. Because podiums are rare, PR-AUC should normally be considered the
primary ranking metric; state and justify the final primary/secondary choice.

Include a model-comparison table and suitable plots, such as ROC and
precision-recall curves.

## Required experiments

### Feature-group ablation

Remove one feature group at a time, retrain, and compare validation performance.
Suggested groups: grid, qualifying, driver form, constructor form, standings,
and categorical context. Keep the model, split, and evaluation process fixed.

### Preprocessing-order experiments

Run at least two experiments in which the order of preprocessing operations
changes, for example:

- clipping outliers then imputing versus imputing then clipping;
- log-transforming non-negative points before imputation versus after imputation.

Use validation only for comparison. Report tied or negative outcomes honestly.

## Explainability

- Calculate and plot global permutation importance.
- Calculate and plot global SHAP importance.
- Produce one local explanation using LIME or local SHAP for a single
  driver-race prediction.
- Explain that importance describes model reliance/association, not causation.

## Inference demonstration

Provide a function that accepts a feature record, applies the saved
preprocessing and selected model, and returns a podium probability/classification.
Demonstrate it on:

1. one complete valid record; and
2. one partially missing record handled by the documented preprocessing policy.

## Final report structure

1. Objective, data, and prediction cutoff.
2. Cleaning, integrity checks, and historical edge-case decisions.
3. EDA and answers to the three data questions.
4. Feature validity and leakage prevention.
5. Split, preprocessing, models, and metrics.
6. Ablation and preprocessing-order experiments.
7. Global and local explanations.
8. Inference demonstration, limitations, and conclusion.
