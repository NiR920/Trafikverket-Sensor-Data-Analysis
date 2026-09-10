# Congestion and Traffic Flow Analysis Using Trafikverket Sensor Data

A reproducible data engineering and analytics framework for extracting, structuring, and analyzing historical traffic sensor data from the Swedish Transport Administration (Trafikverket).

This repository accompanies a **Master's thesis in Business Intelligence (Microdata Analysis) at Dalarna University**.

## Overview

Historical traffic sensor data can provide valuable insight into congestion, traffic flow, vehicle behavior, and environmental impacts. However, historical records available through Trafikverket's web interface are distributed in semi-structured formats and require preprocessing before they can be used effectively for large-scale analysis.

This thesis develops an end-to-end workflow that transforms web-based traffic records into structured, analysis-ready data and applies that data to congestion and environmental analysis.

The project combines:

- automated traffic-data extraction using Python and Selenium
- structured storage in PostgreSQL
- congestion and traffic-flow analysis in Python
- CO₂ emission assessment using speed-dependent COPERT-based modeling
- visualization and dashboarding with Power BI
- an MCP (Model Context Protocol) server prototype for AI-assisted querying of the historical traffic database

## Research Questions

The thesis addresses two main research questions:

1. **How can unstructured web-based traffic sensor data provided by Trafikverket be automatically extracted, structured, and stored to support reliable historical traffic analysis?**
2. **How can extracted historical traffic data be analyzed to identify spatial and temporal congestion hotspots, and how can these traffic states be linked to traffic-related emissions?**

## System Architecture

The overall workflow can be summarized as:

```text
                         Trafikverket
                              │
                              ▼
                 Python Data Extraction
                    Selenium / Pandas
                              │
                              ▼
                        PostgreSQL
                       traffic_data
                              │
             ┌────────────────┼────────────────┐
             │                │                │
             ▼                ▼                ▼
        Python Analysis    Power BI          MCP Server
             │                │                │
             └────────────────┼────────────────┘
                              ▼
                  Congestion & CO₂ Analysis
                              │
                              ▼
                    Research Findings
```

The extraction pipeline accepts user-provided Trafikverket URLs, retrieves traffic measurement data, parses and validates the extracted records, and stores them in PostgreSQL. The database then serves as the central source for the analytical components. The MCP prototype provides an additional interface for targeted, parameterized access to the stored data.

## Data Pipeline

The data workflow consists of the following stages:

### 1. Data input

One or more URLs from the Trafikverket traffic information system are provided as input to define the scope of data collection.

### 2. Automated extraction

A Python-based scraper uses Selenium and a headless Chrome/Chromium browser to navigate the Trafikverket interface and retrieve historical traffic measurement data.

### 3. Parsing and normalization

The extracted information is transformed from the Swedish web interface and semi-structured presentation into structured records suitable for analysis. Data types, completeness, and consistency are validated during processing.

### 4. Duplicate handling

The extraction process is designed to be rerunnable. Duplicate records are checked before insertion so that repeated extraction runs do not unnecessarily introduce redundant records.

### 5. Database storage

Processed records are stored in PostgreSQL in the `traffic_data` database/table structure. The stored data include measurement timestamps, location information, vehicle counts, average speeds, vehicle categories, and audit metadata.

## Analytical Framework

The empirical analysis is based on **5,137 observations** from **eight measurement points** across **four Swedish counties**, covering the period **1998–2023**.

Three congestion indicators are constructed and analyzed:

- **Speed Ratio (SR)**
- **Delay Index (DI)**
- **Speed Variability**

Free-flow speed is estimated using a **95th-percentile methodology**, providing a statistical reference for evaluating observed traffic speeds.

The analysis examines:

- spatial congestion patterns and hotspots
- temporal variation in traffic conditions
- peak-hour behavior
- differences between passenger cars and heavy vehicles
- flow-speed relationships and traffic regimes
- relationships between congestion severity and emissions

## Environmental Analysis

The thesis extends the traffic analysis to environmental impacts by integrating congestion indicators with **speed-dependent CO₂ emission modeling based on the COPERT methodology**.

The analysis identifies a non-linear relationship between congestion severity and emission intensity, with severe congestion associated with disproportionately higher per-vehicle emissions. Spatial analysis is also used to identify locations where congestion and emissions form notable hotspots.

## MCP Server Prototype

The project includes a prototype **Model Context Protocol (MCP) server** that exposes structured historical traffic data through parameterized queries.

The prototype enables an AI client to retrieve traffic information based on attributes such as:

- location
- vehicle type
- time interval
- vehicle counts
- vehicle speeds

The MCP component is presented in the thesis as a practical prototype that complements the main research contributions rather than as the primary evaluated contribution.

## Main Findings

The thesis reports several important findings, including:

- persistent moderate congestion in the analyzed observations
- systematic differences between passenger cars and heavy vehicles
- meaningful temporal variation in traffic conditions
- clear transitions between uncongested, transitional, and breakdown traffic regimes
- disproportionately higher emission intensity under severe congestion
- spatial concentration of congestion-emission hotspots

The diagnostic analysis reports an empirical capacity of approximately **1,500 vehicles/hour**, a mean Speed Ratio of **0.770**, and **64.7%** of observations classified as moderate-to-severe congestion using the study's SR threshold. The analysis also reports that **96.2%** of observations exhibit stable flow according to the study's coefficient-of-variation criterion.

## Repository Structure

The main repository is intended to become the central research and documentation hub for the thesis.

```text
Trafikverket-Sensor-Data-Analysis/
│
├── README.md
├── LICENSE
├── CITATION.cff
├── .gitignore
│
├── docs/
│   ├── research-questions.md
│   ├── methodology.md
│   ├── data-source.md
│   ├── data-pipeline.md
│   ├── analysis-methods.md
│   └── reproducibility.md
│
├── analysis/
│   ├── notebooks/
│   └── scripts/
│
├── results/
│   ├── figures/
│   ├── tables/
│   └── README.md
│
├── data/
│   └── README.md
│
├── mcp/
│   └── README.md
│
├── thesis/
│   └── thesis.pdf
│
└── requirements.txt
```

> **Note:** The structure above is the planned organization of the research repository. Files and directories will be added progressively as the project is reorganized.

## Related Data Extraction Repository

The automated Trafikverket extraction component is maintained in the related repository:

**[supal/trafficdata](https://github.com/supal/trafficdata)**

That repository contains the Python scraper, command-line helpers, setup scripts, dependency definitions, input URL configuration, and PostgreSQL integration used for data extraction.

This separation keeps the extraction software focused on **data acquisition**, while this repository serves as the broader **thesis research, analysis, results, and documentation hub**.

## Reproducibility

Reproducibility is a central goal of the project. The workflow is designed so that traffic data can be extracted repeatedly, transformed into a structured format, stored centrally, and subsequently analyzed using documented methods.

The extraction component supports rerunnable processing and duplicate detection. The analytical workflow is based on explicitly defined congestion indicators and a documented emission-modeling approach.

As the repository is reorganized, environment specifications, analysis notebooks/scripts, methodological documentation, and reproducibility instructions will be added here.

## Data Availability and Responsible Use

The thesis analyzes historical traffic information obtained from Trafikverket's public traffic information system. The repository will document the data source and analytical workflow without unnecessarily committing large generated datasets or environment-specific database contents to version control.

Database credentials, local configuration, and other secrets should **never** be committed to the repository.

## Thesis

**Title:** *Congestion and Traffic Flow Analysis Using Trafikverket Sensor Data*

**Authors:** Md Nazmul Islam Razib and Md Ariful Ahsan  
**Degree:** Master of Science (MSc) in Business Intelligence  
**Subject:** Microdata Analysis (Business Intelligence)  
**University:** Dalarna University  
**Course code:** MI4002  
**Credits:** 15

The thesis PDF is currently included in this repository and will be organized under `thesis/thesis.pdf` as part of the repository restructuring.

## Technology Stack

| Area | Technology |
|---|---|
| Data extraction | Python, Selenium, Pandas |
| Database | PostgreSQL |
| Analysis | Python |
| Visualization | Power BI |
| AI data access | MCP, TypeScript, Node.js |
| Version control | Git / GitHub |

## Project Status

The research and thesis work are completed. The GitHub repository is currently being reorganized to provide a clearer, professional, and reproducible presentation of the work.

Planned repository improvements include:

- structured documentation
- analysis notebooks and scripts
- methodology documentation
- reproducibility instructions
- result organization
- MCP documentation
- citation and licensing information

## Academic Context

This repository accompanies the thesis submitted to **Dalarna University** as part of the MSc in Business Intelligence program. The project integrates data engineering, traffic-flow analysis, environmental assessment, visualization, and an AI-assisted data-access prototype into a single analytical workflow.
