# Documentation

The documentation records the research design, data provenance, analytical definitions, schema, and reproducibility requirements for the thesis project.

## Research design

- [`research-questions.md`](research-questions.md) — research questions addressed by the thesis.
- [`methodology.md`](methodology.md) — overall methodological approach.
- [`data-source.md`](data-source.md) — Trafikverket source and acquisition context.
- [`data-pipeline.md`](data-pipeline.md) — extraction, normalization, duplicate handling, and PostgreSQL storage.

## Analysis

- [`analysis-methods.md`](analysis-methods.md) — congestion, traffic-flow, temporal/spatial, and emission-analysis concepts.
- [`traffic-data-schema.md`](traffic-data-schema.md) — PostgreSQL table structure implemented by the extraction component.

## Reproducibility and QA

- [`reproducibility.md`](reproducibility.md) — environment, data, and execution considerations.
- [`quality-assurance.md`](quality-assurance.md) — repository, code, MCP, dependency, and figure review findings.

The thesis PDF remains the authoritative source for the final academic wording, reported numerical results, and submitted figures. Repository documentation is intended to make the technical workflow easier to inspect and reproduce without silently changing the thesis methodology.
