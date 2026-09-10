# Reproducibility Guide

## Purpose

This document describes how the thesis project is organized so that the data-engineering and analytical workflow can be understood, reproduced, and extended.

The project consists of two closely related repositories:

1. **Main research repository:** `NiR920/Trafikverket-Sensor-Data-Analysis`
2. **Data-extraction repository:** `supal/trafficdata`

The second repository is a core technical component of the thesis. It contains the Trafikverket extraction implementation used to acquire and structure the historical traffic data.

## End-to-End Workflow

The complete workflow is:

```text
Trafikverket
     |
     v
Historical traffic web pages
     |
     v
supal/trafficdata
Python + Selenium extraction
     |
     v
Parsing / normalization
     |
     v
PostgreSQL traffic_data database
     |
     +------------------------+
     |                        |
     v                        v
Python analysis          Power BI / MCP
     |                        |
     +-----------+------------+
                 v
       Congestion + CO₂ results
                 |
                 v
              Thesis
```

## 1. Source and Data Acquisition

The source data are historical traffic measurements made available through Trafikverket's web interface. The extraction process uses URL-based selection and browser automation to retrieve the rendered information.

The related scraper repository documents the required Python, Chrome, PostgreSQL, and pip environment and provides setup scripts for supported operating systems.

## 2. Extraction Environment

The extraction component is based on Python and uses Selenium with Chrome WebDriver. The documented environment requires Python 3.9.6 or newer and Google Chrome. PostgreSQL 12 or newer is documented for database storage.

The scraper supports headless execution and uses WebDriver Manager with a system-driver fallback. Its waiting logic is designed to proceed when required page elements become available.

## 3. PostgreSQL Environment

The extraction workflow stores records in a PostgreSQL database named `traffic_data`. The scraper can create the required `public.traffic_data` table and indexes when needed.

A reproducible local setup should provide:

- a running PostgreSQL server;
- a `traffic_data` database;
- a database account with the required permissions;
- connection settings configured locally for the scraper.

**Credentials must be supplied through the local environment/configuration and must not be committed to the public repository.**

## 4. URL Inputs

The scraper accepts Trafikverket URLs and can process multiple URLs using the project's input/configuration mechanism.

For reproducibility, the URLs or measurement-point selections used for an analysis should be documented as part of the experiment or research record. This makes it possible to identify which Trafikverket pages were used to construct a particular dataset.

## 5. Extraction and Data Processing

A typical reproduction sequence is:

1. prepare the Python environment;
2. install the dependencies from `requirements.txt`;
3. prepare PostgreSQL;
4. configure local database credentials;
5. provide the Trafikverket input URLs;
6. run the scraper;
7. allow the scraper to extract and parse the rendered measurement data;
8. store the structured records in PostgreSQL; and
9. use the resulting database records for analysis.

The scraper performs normalization of source values, including conversion of Swedish decimal-comma speed values and traffic counts into database-compatible numeric values.

## 6. Duplicate Protection and Repeated Runs

The extraction implementation checks for existing records using measurement time, county, road number, and measurement-point number before insertion. Re-running an extraction therefore does not automatically create a duplicate row when the same composite key is already present.

This is important when rebuilding or extending the historical dataset in multiple extraction runs.

## 7. Coordinate Reuse

The scraper maintains a coordinate cache keyed by measurement-point ID. Existing coordinates can be reused from the cache, while missing coordinates can be obtained from available page information when possible.

For reproducibility, the coordinate-cache file should be treated as generated/supporting extraction state rather than as a substitute for documenting the measurement points themselves.

## 8. Study Dataset

The thesis analysis uses **5,137 observations** from **8 measurement points** across **4 Swedish counties**, covering **1998–2023**.

Reproduction of the reported analytical findings therefore requires matching the study's selected measurement points, historical observations, preprocessing, and analytical definitions rather than simply collecting an arbitrary Trafikverket dataset.

## 9. Analytical Reproduction

The main analytical workflow uses:

- Speed Ratio (SR), based on observed speed relative to the 95th-percentile free-flow speed;
- Delay Index (DI), expressed as `1 - observed speed / free-flow speed`;
- speed variability using the coefficient of variation;
- traffic-flow regime analysis;
- spatial and temporal analysis;
- COPERT-based speed-dependent CO₂ estimation; and
- combined congestion-emission hotspot analysis.

The thesis reports a mean Speed Ratio of approximately 0.770, 64.7% of observations with SR below 0.8, 96.2% stable-flow observations under the study's CV criterion, and an empirical capacity level of approximately 1,500 vehicles per hour.

These values should be treated as study-specific results and reproduced using the same underlying observations and definitions.

## 10. MCP Component

The project includes an MCP server prototype connected to the PostgreSQL database. It supports parameterized querying by dimensions such as location, vehicle type, and time interval and can provide data to an AI client for natural-language querying and reporting.

The MCP prototype is a supporting component. It should not be treated as a replacement for the underlying extraction, database, and analytical workflow when reproducing the primary thesis results.

## 11. Repository Roles

### Main repository

`NiR920/Trafikverket-Sensor-Data-Analysis` is the central research repository. It contains the thesis documentation, methodological descriptions, planned analysis assets, results organization, and supporting project documentation.

### Extraction repository

`supal/trafficdata` contains the implementation responsible for retrieving historical Trafikverket traffic data and loading structured records into PostgreSQL.

Keeping these repositories linked allows the research repository to remain focused while preserving the complete technical provenance of the data acquisition process.

## 12. Reproducibility Limitations

Exact reproduction can depend on the availability and behavior of the Trafikverket web interface, the historical records exposed by the source, local browser/WebDriver compatibility, PostgreSQL configuration, and the exact input URLs used during data acquisition.

For this reason, reproducibility should include both the documented software workflow and a clear record of the source selections and analytical inputs used for a reported result.

## Related Documentation

- [`research-questions.md`](research-questions.md)
- [`methodology.md`](methodology.md)
- [`data-source.md`](data-source.md)
- [`data-pipeline.md`](data-pipeline.md)
- [`analysis-methods.md`](analysis-methods.md)
