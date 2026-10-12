# Milestone 1 Requirements

This is a working checklist distilled from `ACL2_MS1_DESC.pdf`. The original
brief remains authoritative.

## 1. Data cleaning and auditing

- [ ] Load and describe the 14 relational CSV tables.
- [ ] Perform initial EDA before cleaning, with appropriate visualizations.
- [ ] Replace literal `\N` missing-value sentinels in working copies.
- [ ] Apply semantic types to dates and duration/time fields used in analysis.
- [ ] Perform post-cleaning EDA that demonstrates the cleaning impact.
- [ ] Validate primary keys as unique and non-null, including composite keys
  where a table has no single identifier.
- [ ] Reconcile relevant foreign keys against their parent tables.
- [ ] Produce exactly one row per `(raceId, driverId)` for modeling.
- [ ] Detect shared-drive duplicates, apply a deterministic rule, and justify it.

## 2. Mandatory data-engineering questions

- [ ] Q1: Identify circuits with the strongest front-row-to-podium conversion,
  comparing periods before and after the 2014 hybrid era.
- [ ] Q2: Test whether drivers podium more at home after controlling for grid
  position.
- [ ] Q3: Compare constructor mechanical-retirement rates in 2014-2021 and
  2022-2024.
- [ ] For every question: use the required multi-hop joins, state analysis
  grain and missingness policy, include a plot, and give a written conclusion.

## 3. Features, preprocessing, and splitting

- [ ] Select pre-race features and support each choice with EDA, a hypothesis,
  or a documented experiment.
- [ ] Implement leakage-safe historical driver/constructor form.
- [ ] Keep qualifying position distinct from actual starting grid.
- [ ] Handle historical qualifying-coverage drift explicitly.
- [ ] Show the effect of preprocessing on the data distribution, range, or values.
- [ ] Compare at least two preprocessing-order variants.
- [ ] Use and justify a chronological train/validation/test split.
- [ ] Learn preprocessing only on training data; reserve the test set for final
  evaluation.

## 4. Modeling and evaluation

- [ ] Train at least two statistical ML models on the same table and feature set.
- [ ] Train one shallow neural network (roughly one or two hidden layers).
- [ ] Establish a baseline model.
- [ ] Report ROC-AUC, PR-AUC, and F1 for every model.
- [ ] Report training/validation results and final unseen-test results.
- [ ] Select thresholds and hyperparameters without looking at the test set.
- [ ] Run feature-group ablation experiments.

## 5. Explainability, inference, and submission

- [ ] Produce global permutation importance.
- [ ] Produce global SHAP explanations.
- [ ] Produce at least one local explanation using LIME and/or local SHAP.
- [ ] Plot and interpret all explanations as associations, not causal effects.
- [ ] Implement and demonstrate inference on one complete and one
  partially-missing record.
- [ ] Create a structured report containing figures, answers, feature validity,
  and limitations such as era sensitivity and qualifying coverage.
- [ ] Ensure the notebook runs from top to bottom without manual interventions.
