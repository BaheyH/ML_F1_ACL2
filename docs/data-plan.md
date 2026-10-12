# Data Plan and Leakage Contract

## Analytical contract

| Item | Decision |
|---|---|
| Unit of analysis | One driver entering one race |
| Unique key | `(raceId, driverId)` |
| Target | `podium = (positionOrder <= 3)` |
| Prediction point | After qualifying, before race start |
| Raw data policy | CSV files are immutable; create cleaned copies |

`positionOrder` is the target source because it contains a ranking for every
entrant. `position` is not suitable because non-classified drivers can have
missing values.

## Core tables and roles

| Table | Project role |
|---|---|
| `results` | Target, driver-race base, actual grid, constructor and status links |
| `races` | Season, round, date, race name, and circuit link |
| `drivers` | Driver identity and nationality |
| `constructors` | Constructor identity and nationality/name |
| `circuits` | Circuit identity, country, and location |
| `qualifying` | Qualifying position and session times where available |
| `driver_standings` | Earlier-season driver standings only |
| `constructor_standings` | Earlier-season constructor standings only |
| `status` | Retirement-status classification for analysis Q3 only |
| `sprint_results` | Optional pre-race information; use only after an explicit format/era review |

`lap_times`, `pit_stops`, and current-race result details are post-race and are
not model features. They may be audited but must not leak into predictions.

## Cleaning and integrity plan

1. Audit literal `\N` counts before replacing them with missing values in copied
   dataframes.
2. Parse relevant dates and durations into appropriate numeric/datetime forms.
3. Validate all primary keys. Use documented composite keys for event tables
   that have no standalone ID.
4. Validate foreign-key relationships before joining.
5. Use merge validation (`one_to_one`, `many_to_one`, etc.) and row-count
   assertions for every modeling-table join.
6. Preserve IDs during construction so joins and exclusions remain auditable.

## Duplicate and historical-data decisions

### Shared drives

Early F1 can contain multiple result rows for the same `(raceId, driverId)`.
The team must retain one deterministic row and document the rule. A defensible
choice is to prioritize the car the driver actually started (`grid > 0`), then
use a documented result-based tie-breaker if needed. This handles target-row
definition; it must never be confused with a pre-race feature.

### DNQ, DNPQ, withdrawals, and pit-lane starts

These cases have different meanings and must not be conflated merely because
some use `grid == 0`. Before modeling, document whether the population is all
result rows or race starters only. If non-starters are retained, report their
share and evaluate era bias because they cannot realistically podium.

### Indianapolis 500

The 1950-1960 Indianapolis 500 is part of the championship data but differs
substantially from other races. Decide whether to retain it, exclude it, or
present it separately; document the rationale and apply it consistently.

### Qualifying coverage

Detailed qualifying records are sparse in early eras. Do not drop old races or
invent qualifying times. Keep a qualifying-availability flag and use actual
grid position as the broadly available pre-race starting-position feature.

## Leakage policy

Allowed at prediction time:

- Actual starting grid.
- Qualifying result/time, where already known and available.
- Race/circuit/driver/constructor identity information available before the race.
- Historical features calculated strictly from earlier completed races.
- Standings recorded after an earlier round, not the current round.

Forbidden as features:

- Current-race finishing position or `positionOrder`.
- Current-race points, status, laps, elapsed time, fastest lap, pit stops, or
  any current-race outcome.
- Same-race teammate results or same-race standings.
- Any aggregation that includes the target race itself.

## F1 terminology relevant to the data

- **Qualifying position** is the order set by qualifying performance.
- **Grid position** is where the car actually starts and may differ after
  penalties or pit-lane starts.
- **Podium** means final positions 1-3.
- **DNQ/DNPQ** means the driver did not qualify/pre-qualify for a race start.
- **Mechanical retirement** is a race-ending component failure, not a collision,
  driver error, disqualification, or regulatory outcome.
