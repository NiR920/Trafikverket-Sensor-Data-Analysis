# Analysis

This directory contains the reproducible analytical artifacts for the thesis.

## Intended organization

```text
analysis/
├── notebooks/   # Jupyter notebooks used for exploratory and analytical work
└── scripts/     # reusable analysis and transformation scripts
```

The analytical workflow covers the measures documented in `docs/analysis-methods.md`, including Speed Ratio, Delay Index, speed variability, traffic-flow diagnostics, temporal/spatial analysis, and speed-dependent CO₂ assessment.

## Source of analytical data

Analysis should read from a documented extract or the PostgreSQL `traffic_data` table. Database credentials and uncontrolled local database files must never be committed.

## Reproducibility rules

- keep analytical transformations explicit and deterministic where possible;
- avoid hidden manual edits to source data;
- record important assumptions in the notebook or corresponding method document;
- generate publication figures from code where practical;
- keep exploratory/debugging outputs separate from final research figures;
- do not overwrite source data during analysis.

The original thesis analysis notebook is maintained in the related `supal/trafficdata` repository and should be migrated here only after its dependencies and execution assumptions have been verified.
