# Methodology

## 1. Research Approach

The thesis follows an end-to-end data engineering and analytical workflow for historical traffic sensor data. The methodology connects automated data acquisition, structured database storage, congestion analysis, and environmental assessment.

The workflow is designed to reduce manual handling of historical traffic records and provide a reproducible basis for subsequent analysis.

## 2. Data Acquisition

Historical traffic information is collected from the Trafikverket web-based traffic information system. Because the historical records are presented through a web interface and are available in semi-structured forms, the project uses automated browser-based extraction rather than relying on manual collection.

The extraction component is implemented in Python and uses Selenium to navigate the relevant Trafikverket pages. Pandas is used during processing of extracted tabular information.

The extraction workflow accepts one or more Trafikverket URLs and is designed to support repeated data collection.

The extraction software is maintained separately in the related [`supal/trafficdata`](https://github.com/supal/trafficdata) repository.

## 3. Data Processing and Validation

The extracted records are transformed into structured observations suitable for database storage and statistical analysis.

The processing stage includes:

- parsing values from the Trafikverket presentation format
- converting numeric values into analysis-ready types
- handling Swedish decimal notation where required
- retaining measurement timestamps and location metadata
- organizing observations by vehicle category and measurement type
- checking records before database insertion
- detecting duplicates during repeated extraction runs

The objective is to create consistent records while preserving the traffic measurements required for the thesis analysis.

## 4. Database Storage

PostgreSQL is used as the central storage layer for the extracted traffic data.

The database structure stores measurement information including:

- measurement time
- county
- road number
- measurement-point number
- total vehicle counts and speeds
- passenger-car counts and speeds
- heavy-vehicle counts and speeds
- trailer and non-trailer categories
- record creation metadata

Indexes are used for important query dimensions such as measurement time, measurement point, and road/county information.

This centralized database provides a common data source for the Python analysis, visualization workflow, and MCP prototype.

## 5. Study Dataset

The empirical analysis reported in the thesis contains **5,137 observations** collected from **eight measurement points** across **four Swedish counties**.

The observations cover the period **1998–2023**.

The study considers traffic measurements by vehicle type and examines both spatial and temporal variation in traffic conditions.

## 6. Congestion Measurement

Three congestion indicators are constructed and evaluated:

### Speed Ratio (SR)

Speed Ratio compares observed traffic speed with a reference free-flow speed. The thesis uses a **95th-percentile methodology** to estimate free-flow speed.

A lower Speed Ratio indicates a larger reduction in observed speed relative to the free-flow reference and therefore greater congestion severity.

### Delay Index (DI)

The thesis evaluates delay using speed-based formulations. In the selected definition, Delay Index is expressed as:

```text
DI = 1 - v_obs / v_ff
```

where:

- `v_obs` = observed traffic speed
- `v_ff` = estimated free-flow speed

### Speed Variability

Speed variability is used as an additional indicator of traffic-flow stability. The analysis evaluates the coefficient of variation (CV) of speed to distinguish stable and more variable traffic conditions.

## 7. Traffic-Flow Analysis

The traffic-flow analysis examines the relationship between traffic volume and speed. The thesis identifies distinct traffic regimes, including:

1. uncongested flow
2. transitional flow
3. breakdown/congested flow

The empirical analysis reports an approximate capacity of **1,500 vehicles/hour** for the studied observations.

Temporal analysis is used to examine variation across periods of the day, including peak-hour behavior. Vehicle-type differences are also evaluated, particularly the contrast between passenger cars and heavy vehicles.

## 8. Emission Analysis

The environmental component links traffic conditions to speed-dependent CO₂ emissions.

The thesis uses a **COPERT-based speed-dependent emission modeling approach** to evaluate how changes in traffic speed and congestion severity relate to emission intensity.

The analysis examines:

- emission differences associated with traffic conditions
- passenger-car and heavy-vehicle behavior
- the relationship between congestion severity and per-vehicle emissions
- spatial concentration of congestion and emission hotspots

The results indicate a non-linear relationship between congestion severity and emission intensity, with severe congestion associated with disproportionately higher per-vehicle emissions.

## 9. Spatial and Temporal Analysis

The analytical framework evaluates traffic conditions across both location and time.

Spatial analysis is used to identify measurement points and areas associated with higher congestion and emission intensity. Temporal analysis is used to examine changes in traffic conditions and peak-hour dynamics.

Combining these dimensions allows the study to identify congestion-emission hotspots rather than evaluating traffic conditions only as an overall aggregate.

## 10. AI-Assisted Data Access

An MCP server prototype is included as a supporting component of the project. The server provides parameterized access to the PostgreSQL traffic database so that an AI client can request information using dimensions such as location, vehicle type, and time interval.

The MCP prototype demonstrates how structured traffic data can be made accessible through a natural-language-oriented interface. It is treated as a practical prototype and supporting component rather than the primary evaluated research contribution.

## 11. Reproducibility Principles

The methodology emphasizes a separation between data acquisition, storage, analysis, and presentation:

```text
Data Source
    ↓
Automated Extraction
    ↓
Parsing & Validation
    ↓
PostgreSQL Storage
    ↓
Analytical Processing
    ↓
Congestion & Emission Analysis
    ↓
Visualization / AI-Assisted Access
```

This separation makes it possible to rerun extraction independently from analysis and to use the same centralized dataset across different analytical interfaces.

## 12. Methodological Scope

The methodology reflects the scope and methods evaluated in the thesis. The MCP server is a prototype, while the primary research contributions are the reproducible historical traffic-data pipeline and the empirical congestion-emissions analysis.
