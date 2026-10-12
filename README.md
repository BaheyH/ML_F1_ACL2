# Formula 1 Podium Prediction

## Objective

Predict whether a driver finishes on the podium in a Formula 1 race. The target
is binary: `podium = 1` when `positionOrder <= 3`, otherwise `0`.

Every prediction must use only information available **after qualifying and
before the race starts**.

## Dataset

The project uses the Formula 1 World Championship / Ergast relational dataset
for 1950-2024. It contains 14 related CSV tables, including races, results,
drivers, constructors, qualifying, standings, pit stops, lap times, sprint
results, and status records.

Dataset source: [Kaggle - Formula 1 World Championship (1950-2024)](https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020)

> The URL slug retains an older `1950-2020` name. The downloaded archive used by
> this project was verified to contain 75 seasons and 1,125 races spanning
> 1950-2024.

## Local data setup for the team

The notebook's original loading cell expects these exact 14 CSV files:

```text
circuits.csv
constructor_results.csv
constructor_standings.csv
constructors.csv
driver_standings.csv
drivers.csv
lap_times.csv
pit_stops.csv
qualifying.csv
races.csv
results.csv
seasons.csv
sprint_results.csv
status.csv
```

Each teammate should download the same **Formula 1 World Championship
(1950-2024)** dataset version and extract those files into this location:

```text
data/
  raw/
    circuits.csv
    ...
    status.csv
```

Use a project-relative `DATA_DIR` in the notebook rather than a personal
computer path:

```python
from pathlib import Path

DATA_DIR = Path("../data/raw")  # notebook is stored in Notebooks/
df_circuits = pd.read_csv(DATA_DIR / "circuits.csv")
```

The `data/raw/` folder should be ignored by Git unless the team explicitly
chooses a large-file storage solution. The repository should contain code and
documentation, while each team member keeps an identical local copy of the raw
dataset. Before running, verify that `races.csv` and `seasons.csv` reach 2024;
the older 2020-only release is insufficient for the milestone's final test era.

## Repository guide

- `AGENTS.md` — shared rules for contributors and coding agents.
- `Notebooks/` — the run-all analysis and modeling notebook.
- `docs/requirements.md` — milestone acceptance criteria.
- `docs/data-plan.md` — data contract, cleaning, joins, and leakage policy.
- `docs/data-questions.md` — plans for the three required analyses.
- `docs/modeling-plan.md` — feature, evaluation, experiment, XAI, and inference plan.

## Required final deliverables

1. A run-all Jupyter/Kaggle notebook.
2. EDA, data cleaning, and answers to all three data-engineering questions.
3. A validated, leakage-safe driver-race modeling table.
4. A comparison of two statistical models and one shallow neural network.
5. Ablation and preprocessing-order experiments.
6. Global and local model explanations.
7. An inference function demonstrated on complete and partially missing records.
8. A PDF analytical report and accessible Kaggle/GitHub links.

## Project constraints

- Unit of analysis: one driver entering one race, unique by `(raceId, driverId)`.
- Do not edit raw input CSV files.
- Do not use current-race outcomes as features.
- Use time-aware splits; do not randomly split historical races.

See `AGENTS.md` and the documents under `docs/` for the full working rules.
