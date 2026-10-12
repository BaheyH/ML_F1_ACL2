# Mandatory Data-Engineering Questions

Each question needs executable multi-hop joins, a stated grain, a missingness
policy, a visualization, and a short evidence-based conclusion. Raw percentages
without the required joins or controls do not satisfy the milestone.

## Q1. Front-row conversion by circuit and era

**Question:** Which circuits convert a front-row start into a podium most often,
and does the ranking change after the 2014 hybrid era?

### Data and grain

- Base: deduplicated driver-race rows from `results`.
- Join `races` for year and `circuits` for circuit identity/name/country.
- Use `grid` from `results`; front row means `grid` in `{1, 2}`.
- Target: `positionOrder <= 3`.
- Compare `year < 2014` with `year >= 2014` (or document an alternative
  boundary treatment).

### Method

For each circuit and era, calculate:

`front-row podium conversion = front-row drivers finishing on podium / all front-row drivers`

Report sample size alongside conversion rate. Apply a sensible minimum-support
rule or uncertainty interval so circuits with very few races do not dominate the
ranking. Plot ranked rates for each era or a before/after slope chart.

### Required caution

Grid and qualifying position are not interchangeable. Discuss qualifying only
as supporting context or a separate comparison; use actual grid for the stated
front-row question.

## Q2. Home-race podium effect after controlling for grid

**Question:** Do drivers podium more often at a circuit in their home country
than elsewhere after controlling for starting position?

### Data and grain

- Base: deduplicated driver-race rows from `results`.
- Join `races -> circuits` for circuit country.
- Join `drivers` for driver nationality.
- Retain grid and podium target from `results`.

### Method

1. Create a documented nationality-to-country mapping. Direct string matching
   is inadequate (for example, `British` and `UK`).
2. Define `is_home_race` from that mapping.
3. Compare podium rates within comparable grid strata, e.g. front row, P3-P5,
   P6-P10, and P11+, reporting counts.
4. Preferably supplement the stratified comparison with a logistic model using
   podium as target and grid controls plus `is_home_race`; interpret it as an
   association, not a causal effect.
5. Plot home versus away results by grid stratum, with uncertainty where
   practical.

### Required caution

The Indianapolis 500 can distort nationality/home-race analysis. State whether
it is excluded, retained, or separately analysed. Missing/ambiguous nationality
mappings must be counted and reported rather than guessed.

## Q3. Mechanical-retirement rates by constructor and era

**Question:** Which constructors have the highest mechanical-retirement rate in
2014-2021 versus 2022-2024?

### Data and grain

- Base: race-start-level rows from deduplicated `results`.
- Join `constructors` for constructor name.
- Join `status` for the reason a result ended.
- Join `races` for year and era.

### Method

1. Define and publish a status mapping with at least three categories:
   `mechanical`, `non_mechanical`, and `finished/classified`.
2. Mechanical examples can include engine, gearbox, hydraulics, brakes, power
   unit, suspension, transmission, and tyre/wheel failures only if the team
   explicitly justifies the classification.
3. Exclude collisions, accidents, spins, driver errors, disqualifications,
   withdrawals, and regulatory outcomes from the mechanical category.
4. Decide and state the denominator: normally race starts, not all historical
   result rows. Explain how DNQs/DNPQs and withdrawals are treated.
5. For each constructor and era calculate:

`mechanical retirement rate = mechanical retirements / eligible race starts`

6. Show numerator and denominator alongside the rate. Use a minimum-starts
threshold or uncertainty interval before ranking constructors.

### Required visualization

Use a grouped bar chart, paired dot plot, or heatmap showing constructor rates
across both eras, with sample sizes visible.

## Common reporting template

For each answer, record:

1. Question and analytical definition.
2. Tables, join keys, and final grain.
3. Data exclusions, missingness, and historical edge-case choices.
4. Calculation and visualization.
5. Result and limitation.
