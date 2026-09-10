# Data Source

## Source

The traffic measurements analyzed in this thesis originate from the **Swedish Transport Administration (Trafikverket)** traffic information system.

The project focuses on historical traffic sensor records that are accessible through the Trafikverket web interface. The historical information is presented through web-based views and semi-structured tables, which makes automated extraction and normalization necessary for systematic analysis.

## Why Automated Extraction Is Used

The thesis identifies the historical data-access process as a practical data-engineering challenge. The records are distributed across web pages and require manual interaction when collected individually. This creates unnecessary effort and can increase the risk of duplication and inconsistency.

The project therefore uses an automated browser-based extraction workflow to retrieve the historical records and transform them into structured data suitable for database storage and analysis.

## Data Extraction Component

The extraction software is maintained separately in the related repository:

**[supal/trafficdata](https://github.com/supal/trafficdata)**

The extraction component uses:

- Python
- Selenium
- Pandas
- PostgreSQL
- a headless Chrome/Chromium browser for automated navigation

The scraper accepts Trafikverket URLs as input and processes the corresponding historical traffic information before storing structured records in PostgreSQL.

## Data Characteristics

The study dataset used in the thesis contains:

| Characteristic | Study scope |
|---|---|
| Observations | 5,137 |
| Measurement points | 8 |
| Counties | 4 Swedish counties |
| Time period | 1998–2023 |
| Main analytical focus | Traffic flow, congestion, and emissions |

The extracted records contain traffic measurements and metadata needed for the analytical workflow, including measurement time, location information, vehicle categories, vehicle counts, and speed measurements.

## Vehicle Categories

The database structure distinguishes traffic measurements by vehicle type. The thesis particularly examines differences between:

- passenger cars
- heavy vehicles

The extraction/database workflow also contains more detailed vehicle categories, including trailer and non-trailer classifications.

## Data Storage

After extraction and processing, the records are stored in PostgreSQL. The database acts as the central storage layer used by the analytical components of the project.

The stored structure includes fields for measurement timestamps, county, road number, measurement-point identifiers, vehicle counts, vehicle speeds, vehicle categories, and record metadata.

Indexes are used for important query dimensions such as measurement time, measurement point, and road/county information.

## Data Quality and Repeatability

The extraction workflow is designed to support repeated runs. Before records are inserted into the database, the process checks for existing records to reduce duplicate observations caused by repeated extraction.

The processing workflow also converts extracted values into consistent numeric representations and handles the decimal formatting used by the source presentation where required.

## Data Availability in This Repository

This repository documents the source and analytical workflow rather than committing the complete database contents or environment-specific data to version control.

The related extraction repository contains the software used to acquire the source records. Database credentials and other environment-specific configuration must remain outside version control.

## Intended Use

The extracted dataset is used for academic analysis of:

- congestion severity
- traffic-flow regimes
- spatial congestion patterns
- temporal traffic variation
- passenger-car and heavy-vehicle differences
- speed-dependent CO₂ emissions
- congestion-emission hotspots

The data-source documentation should be read together with [`methodology.md`](methodology.md) and [`research-questions.md`](research-questions.md) to understand how the source data are transformed and used in the thesis.
