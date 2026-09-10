# Analysis Methods

## Overview

The analytical stage of the thesis uses the structured historical traffic dataset produced by the extraction and PostgreSQL workflow. The analysis focuses on traffic flow, congestion severity, traffic-flow stability, spatial and temporal patterns, and the relationship between congestion and CO₂ emissions.

The methods described here follow the analytical framework presented in the thesis. The MCP component is documented as a supporting prototype rather than as the primary analytical method.

## Study Dataset

The analysis is based on **5,137 observations** collected from **8 measurement points** across **4 Swedish counties**, covering **1998–2023**.

The main traffic variables include vehicle counts and average speeds for all vehicles, passenger cars, heavy vehicles, and more detailed vehicle categories.

## 1. Speed Ratio

Speed Ratio (SR) is used as a primary indicator of congestion severity. It compares observed traffic speed with an estimated free-flow speed.

The thesis uses the **95th percentile speed** as the free-flow speed reference. The Speed Ratio is therefore conceptually expressed as:

```text
SR = observed speed / free-flow speed
```

Lower Speed Ratio values indicate greater reductions from free-flow conditions and therefore more severe congestion.

The thesis reports a mean Speed Ratio of approximately **0.770**, with **64.7% of observations** classified as moderate-to-severe congestion using the study's SR threshold of 0.8.

## 2. Delay Index

Delay Index (DI) expresses the relative loss of speed compared with free-flow conditions:

```text
DI = 1 - (observed speed / free-flow speed)
```

A higher Delay Index corresponds to a greater reduction in speed and therefore greater delay relative to free-flow travel conditions.

Because SR and DI are directly related, they provide complementary ways of representing the severity of speed degradation.

## 3. Speed Variability

Speed variability is used to assess traffic-flow stability. The coefficient of variation (CV) is used to represent variability relative to the mean speed.

The thesis uses **CV < 0.3** as the stable-flow criterion. Under this classification, **96.2% of observations** were characterized as stable flow.

This measure complements the congestion indicators: a traffic state can exhibit reduced speed while still showing relatively stable speed behavior.

## 4. Traffic-Flow Regimes

The study examines the relationship between traffic volume and speed to distinguish different traffic-flow states. The analysis identifies three broad regimes:

- **Uncongested flow** — traffic operates with comparatively higher speeds and lower interaction effects.
- **Transitional flow** — increasing traffic demand is associated with declining speeds and changing operating conditions.
- **Breakdown/congested flow** — traffic operates under substantially reduced speeds and stronger congestion effects.

The empirical analysis indicates a capacity level of approximately **1,500 vehicles per hour** in the studied observations. This value is treated as an empirical finding for the study dataset rather than as a universal road-capacity value.

## 5. Vehicle-Type Analysis

Passenger cars and heavy vehicles are analyzed separately to examine differences in their traffic-flow characteristics.

The thesis finds that heavy vehicles consistently exhibit lower Speed Ratios and contribute disproportionately to congestion severity. Vehicle-type analysis is therefore important for interpreting congestion patterns beyond total traffic volume alone.

The more detailed vehicle categories available in the database can also support further investigation of trailer and non-trailer groups.

## 6. Temporal Analysis

Traffic conditions are examined across time to identify variations in traffic demand, speed, and congestion severity.

The analysis considers peak-hour dynamics and historical temporal patterns. This allows periods of elevated traffic demand and degraded operating conditions to be identified rather than relying only on overall averages.

## 7. Spatial Analysis and Hotspots

The measurement-point and location metadata allow congestion conditions to be compared spatially.

Spatial analysis is used to identify locations where congestion severity and related emissions are comparatively high. The thesis combines congestion and emissions information to identify **congestion-emission hotspots**.

These hotspots provide an analytical basis for discussing targeted traffic-management and mitigation measures.

## 8. CO₂ Emissions Analysis

The environmental analysis uses a **COPERT-based speed-dependent CO₂ modeling approach**.

The analysis links traffic operating conditions to estimated emission intensity, with vehicle speed serving as an important factor. Because the relationship between speed and emissions is nonlinear, the thesis examines how changes in congestion severity affect estimated emissions rather than assuming a simple linear relationship.

The results indicate that severe congestion can produce disproportionately higher per-vehicle emission impacts.

## 9. Congestion–Emission Relationship

The congestion and environmental analyses are combined to investigate how traffic-state deterioration relates to CO₂ emissions.

The analytical sequence is:

```text
Traffic volume + speed
        |
        v
Congestion indicators
(SR, DI, variability)
        |
        v
Traffic-flow state
        |
        v
COPERT-based CO₂ estimation
        |
        v
Spatial/temporal hotspot analysis
```

This integration allows the thesis to move from describing traffic conditions to assessing their environmental implications.

## 10. MCP Querying Prototype

The project includes an MCP server prototype connected to the PostgreSQL database. It supports targeted, parameterized queries by dimensions such as location, vehicle type, and time interval.

An AI client can use these query capabilities to retrieve relevant traffic records and generate natural-language summaries or reports.

The MCP component is a **supporting prototype**. The primary research contribution remains the reproducible historical data pipeline and the empirical congestion and emissions analysis.

## Interpretation and Scope

The analytical results describe the selected historical observations and measurement points used in the thesis. Findings such as the empirical capacity level, congestion proportions, and hotspot patterns should therefore be interpreted within the study scope rather than generalized automatically to all Swedish roads or traffic conditions.

The analysis methods are linked to the data-engineering documentation in [`data-source.md`](data-source.md) and [`data-pipeline.md`](data-pipeline.md), which describe how the underlying historical observations are acquired and structured.
