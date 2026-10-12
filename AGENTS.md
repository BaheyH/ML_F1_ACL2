# Contributor and Agent Guide

## Project purpose

This repository contains a Formula 1 podium-prediction project for Milestone 1.
The task is binary classification: predict whether a driver finishes on the
podium (`positionOrder <= 3`) using only information known after qualifying and
before the race starts.

The authoritative project requirements are the milestone brief supplied to the
team. If this guide conflicts with that brief, follow the brief.

## Non-negotiable data rules

- The modeling grain is exactly one row per `(raceId, driverId)`.
- Raw source CSV files must remain unchanged. Perform transformations on copies
  and make the transformations reproducible in the notebook/code.
- Treat literal `\N` values as missing values; do not treat them as ordinary
  strings.
- Use `positionOrder <= 3` for the podium target. Do not use `position` as the
  target because it is missing for non-classified drivers.
- The prediction cutoff is after qualifying and before the race start.
- Never use current-race results, finishing position, current-race points,
  status, laps, pit stops, fastest-lap fields, race time, or other post-race
  fields as model features.
- Standings and form features must use only earlier completed races. A
  same-race teammate result must never become a feature.
- Qualifying position and actual grid position are different concepts. Grid is
  valid pre-race information; document any use of qualifying data and its
  historical missingness.

## Required milestone work

- Audit missing values, semantic types, primary keys, foreign keys, and the
  effect of cleaning with reproducible code and visualizations.
- Detect and document shared-drive duplicates. Apply and justify a deterministic
  rule that leaves one row per `(raceId, driverId)`.
- Answer all three required data-engineering questions with appropriate joins,
  a visualization, and a concise written interpretation.
- Use a chronological train/validation/test split. Fit imputers, scalers,
  encoders, thresholds, and hyperparameters on training/validation data only;
  keep the test set untouched until final evaluation.
- Deliver two statistical ML models, one shallow neural network, feature-group
  ablation, at least two preprocessing-order experiments, global and local XAI,
  and inference examples for complete and partially missing records.

## F1 and historical edge cases

- Early results contain shared drives; do not silently drop duplicates without
  documenting the rule.
- DNQ/DNPQ, pit-lane starts, withdrawals, and the 1950-1960 Indianapolis 500
  require explicit inclusion/exclusion decisions. Record the rationale and
  assess possible era bias.
- Qualifying coverage is incomplete in earlier eras. Do not fabricate qualifying
  times or silently discard historical rows solely because qualifying data is
  missing.
- Constructor and driver identities change over time. Preserve identifiers and
  document any mapping or grouping assumptions.
- A mechanical retirement must be defined from an explicit, reviewable status
  mapping. Do not classify collisions, driver errors, or regulatory decisions
  as mechanical failures.

## Collaboration and quality standards

- Inspect existing work before editing it. Preserve working functionality and
  avoid unnecessary rewrites.
- Do not claim a result, plot, metric, or successful run unless it was actually
  produced and checked.
- Keep notebooks run-all ready: no hidden manual steps, machine-specific paths,
  or dependency on prior cell state outside notebook order.
- Explain assumptions, joins, missingness treatment, and feature validity near
  the code that implements them.
- Prefer clear, small, reviewable changes. Do not overwrite teammates' work
  without coordinating first.
- Treat model explanations as associations, not evidence of causation.
