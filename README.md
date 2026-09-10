# Congestion and Traffic Flow Analysis Using Trafikverket Sensor Data

A reproducible research project for extracting, structuring, and analyzing historical traffic sensor data from the Swedish Transport Administration (Trafikverket), with a focus on congestion, traffic flow, and speed-dependent CO₂ emissions.

This repository accompanies a **Master's thesis in Business Intelligence (Microdata Analysis) at Dalarna University**.

## Research questions

1. **How can unstructured web-based traffic sensor data provided by Trafikverket be automatically extracted, structured, and stored to support reliable historical traffic analysis?**
2. **How can extracted historical traffic data be analyzed to identify spatial and temporal congestion hotspots, and how can these traffic states be linked to traffic-related emissions?**

## Project architecture

```text
Trafikverket historical web data
              │
              ▼
     Python / Selenium scraper
              │
              ▼
        PostgreSQL database
         traffic_data table
              │
       ┌──────┼─────────┐
       ▼      ▼         ▼
   Analysis  Power BI   MCP prototype
       │      │         │
       └──────┼─────────┘
              ▼
   Congestion + traffic-flow
       + CO₂ assessment
              │
              ▼
        Thesis findings
```

The extraction implementation is maintained in the related [`supal/trafficdata`](https://github.com/supal/trafficdata) repository. This repository is the central research hub for the thesis: methods, reproducibility, analysis organization, results, data documentation, and the thesis document are maintained here.

## Study dataset

The thesis analyzes **5,137 observations** from **eight measurement points** across **four Swedish counties**, covering **1998–2023**.

The analytical framework includes:

- Speed Ratio (SR)
- Delay Index (DI)
- speed variability
- free-flow speed based on the 95th percentile
- traffic-flow regimes and empirical capacity
- passenger-car versus heavy-vehicle comparisons
- temporal and peak-hour analysis
- spatial congestion/emission hotspot analysis
- speed-dependent CO₂ modeling based on COPERT

The thesis reports a mean Speed Ratio of **0.770**, **64.7%** of observations classified as moderate-to-severe congestion under the study's SR criterion, **96.2%** stable-flow observations under the study's coefficient-of-variation criterion, and an empirical capacity of approximately **1,500 vehicles/hour**.

## Repository structure

```text
Trafikverket-Sensor-Data-Analysis/
├── README.md
├── LICENSE
├── CITATION.cff
├── .gitignore
├── requirements.txt
├── docs/
│   ├── README.md
│   ├── research-questions.md
│   ├── methodology.md
│   ├── data-source.md
│   ├── data-pipeline.md
│   ├── analysis-methods.md
│   ├── traffic-data-schema.md
│   ├── reproducibility.md
│   └── quality-assurance.md
├── analysis/
│   ├── README.md
│   ├── notebooks/
│   └── scripts/
├── results/
│   ├── README.md
│   ├── figures/
│   └── tables/
├── data/
│   └── README.md
├── mcp/
│   └── README.md
└── thesis/
    └── thesis.pdf
```

The directories are organized by research function rather than by development history. Generated databases, credentials, virtual environments, caches, compiled files, and other machine-specific artifacts are intentionally excluded from version control.

## Data pipeline

The extraction workflow uses user-provided Trafikverket URLs to select historical measurement records. Selenium renders the web interface, the scraper extracts measurement data and page metadata, values are normalized, duplicate records are checked, and the resulting records are stored in PostgreSQL.

The stored schema includes measurement time, county, road number, measurement-point number, vehicle counts, average speeds, vehicle categories, and record creation metadata.

See [`docs/data-pipeline.md`](docs/data-pipeline.md), [`docs/data-source.md`](docs/data-source.md), and [`docs/traffic-data-schema.md`](docs/traffic-data-schema.md).

## Analysis and results

The analysis connects the extracted traffic data to congestion indicators, traffic-flow diagnostics, vehicle-type comparisons, temporal patterns, spatial hotspots, and speed-dependent CO₂ estimates.

The thesis contains the complete set of reported figures. The repository's `results/` area is reserved for reproducible, clearly labeled figure and table exports; figures should be generated from documented analysis code rather than manually edited screenshots whenever source code is available.

## MCP prototype

A TypeScript/Node.js MCP server is included in the related technical repository. It provides parameterized access to the PostgreSQL traffic database and prototype analytical tools for querying traffic statistics, locations, dates, vehicle categories, speeds, and counts.

The MCP component is a **supporting prototype**, not the primary evaluated research contribution. The primary contributions are the historical data pipeline and the congestion/traffic-flow/emissions analysis.

## Reproducibility

The project documents the complete conceptual workflow from Trafikverket extraction through PostgreSQL storage and downstream analysis. Reproduction requires access to the Trafikverket source interface, a compatible browser/Selenium environment, PostgreSQL, and the analysis environment.

Database credentials must be supplied through local configuration or environment variables and must never be committed to Git.

See [`docs/reproducibility.md`](docs/reproducibility.md).

## Related repositories

- **Data extraction and MCP implementation:** https://github.com/supal/trafficdata
- **Main thesis research repository:** https://github.com/NiR920/Trafikverket-Sensor-Data-Analysis

The two repositories should be treated as one thesis project with different responsibilities: `supal/trafficdata` contains the executable acquisition/MCP implementation, while this repository provides the research record and public-facing organization.

## Thesis

**Title:** *Congestion and Traffic Flow Analysis Using Trafikverket Sensor Data*  
**Authors:** Md Nazmul Islam Razib and Md Ariful Ahsan  
**Degree:** Master of Science (MSc) in Business Intelligence  
**Subject:** Microdata Analysis (Business Intelligence)  
**University:** Dalarna University  
**Course:** MI4002  
**Credits:** 15

The submitted thesis PDF is retained in the repository under its original uploaded filename. It is not renamed automatically because the GitHub file API available for this project cannot safely perform a binary-file move without re-uploading the complete PDF.

## Technology stack

| Area | Technology |
|---|---|
| Data extraction | Python, Selenium, Pandas |
| Database | PostgreSQL |
| Analysis | Python, Jupyter |
| Visualization | Power BI and Python-based analysis |
| AI data access | MCP, TypeScript, Node.js |
| Version control | Git / GitHub |

## Project status

The research and thesis work are completed. The repositories are being maintained as a professional, reproducible record of the completed work. Organization focuses on preserving the original implementation and reported results while separating research artifacts from local/development artifacts.

## License and citation

The repository includes an MIT license for the software/repository materials and a `CITATION.cff` file for citation metadata. The thesis document remains the authors' academic work; repository licensing should not be interpreted as changing the university's or authors' rights to the thesis text.
