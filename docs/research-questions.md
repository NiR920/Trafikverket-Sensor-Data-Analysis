# Research Questions

## Thesis Context

The thesis, *Congestion and Traffic Flow Analysis Using Trafikverket Sensor Data*, investigates how historical traffic sensor information can be transformed into a reliable analytical resource and subsequently used to study congestion and traffic-related emissions.

## Research Questions

### RQ1 — Data Extraction and Structuring

**How can unstructured web-based traffic sensor data provided by Trafikverket be automatically extracted, structured, and stored to support reliable historical traffic analysis?**

This question focuses on the data-engineering challenge of collecting historical traffic measurements from the Trafikverket web interface and transforming them into structured, analysis-ready records. The work addresses automated extraction, parsing, validation, duplicate handling, and centralized PostgreSQL storage.

### RQ2 — Congestion and Emissions Analysis

**How can extracted historical traffic data be analyzed to identify spatial and temporal congestion hotspots, and how can these traffic states be linked to traffic-related emissions?**

This question focuses on the analytical use of the extracted dataset. The thesis evaluates congestion through Speed Ratio (SR), Delay Index (DI), and Speed Variability, and connects traffic conditions with speed-dependent CO₂ emission modeling based on the COPERT methodology.

## Scope of the Study

The empirical analysis uses **5,137 observations** from **eight measurement points** across **four Swedish counties**, covering **1998–2023**.

The study examines:

- spatial and temporal congestion patterns
- peak-hour traffic behavior
- differences between passenger cars and heavy vehicles
- flow-speed relationships and traffic regimes
- congestion severity and emission intensity
- spatial congestion-emission hotspots

## Role of the MCP Prototype

The project also includes an MCP server prototype for AI-assisted, parameterized access to the historical traffic database. This component supports the broader workflow but is treated in the thesis as a practical prototype rather than the primary research contribution.
