# Data Pipeline

## Overview

The thesis uses an end-to-end pipeline that converts historical Trafikverket web data into structured records for database storage and subsequent congestion and emissions analysis.

```text
Trafikverket historical web pages
            |
            v
   URL-based data selection
            |
            v
 Python + Selenium scraper
            |
            v
 Page metadata + measurement tables
            |
            v
 Parsing and normalization
            |
            v
 Duplicate detection / validation
            |
            v
       PostgreSQL
            |
            +-------------------+
            |                   |
            v                   v
     Python analysis       Power BI / MCP
            |
            v
 Congestion + CO₂ analysis
```

The extraction implementation is maintained in the related thesis repository, [`supal/trafficdata`](https://github.com/supal/trafficdata).

## 1. Input: Trafikverket URLs

The scraper is initialized with a Trafikverket URL. The implementation parses URL query parameters to identify measurement-point IDs (`punktnrlista`) when they are supplied.

The related scraper project also supports processing multiple URLs from a configuration/input file. This allows several measurement pages to be collected in a repeatable workflow rather than being retrieved manually one by one.

## 2. Browser-Based Extraction

The scraper uses Selenium with Chrome WebDriver to navigate the Trafikverket website and interact with rendered page elements.

The implementation supports both normal browser execution and headless execution. WebDriver Manager is used to obtain a compatible ChromeDriver, with a system ChromeDriver fallback.

The scraper uses Selenium waits and expected conditions so that extraction can proceed when required page elements are available rather than relying only on fixed delays.

## 3. Page Metadata Extraction

Before processing the measurement records, the scraper extracts contextual metadata from the rendered page. The implementation reads Trafikverket page elements for:

- county (`Län`)
- road number (`Vägnr`)
- measurement-point number (`Punktnummer`)
- direction (`Riktning`)

These values provide location and context for the measurements collected from the page.

## 4. Measurement Data Extraction

The scraper collects the available measurement occasions and traffic values from the Trafikverket page. The extracted structure includes measurement time and vehicle-specific counts and average speeds.

The database model implemented by the scraper contains fields for:

- all vehicles: count and average speed
- passenger cars: count and average speed
- heavy vehicles: count and average speed
- heavy vehicles with trailer: count and average speed
- heavy vehicles without trailer: count and average speed
- three-axle tractor-trailers: count and average speed
- two-axle tractor-trailers: count and average speed
- three-axle tractor units without trailer: count and average speed
- two-axle tractor units without trailer: count and average speed
- passenger cars with trailer: count and average speed
- passenger cars without trailer: count and average speed

This detailed vehicle structure supports the thesis analysis of passenger-car and heavy-vehicle traffic differences.

## 5. Coordinate Handling

The scraper includes coordinate handling for measurement points. It first checks a local coordinate cache using the measurement-point ID. If coordinates are not already cached, it attempts to locate latitude and longitude information in page scripts and stores successfully retrieved coordinates in the cache.

The cache is persisted in `coordinate_cache.txt`, allowing coordinates already obtained during earlier extraction runs to be reused.

## 6. Parsing and Normalization

Values displayed by the source website are converted into database-compatible representations before insertion.

The scraper explicitly handles Swedish decimal notation for speed values, converting decimal commas to decimal points. Speed values are stored as numeric values, while traffic counts are converted to integers.

Missing or empty count values are handled as zero by the scraper's count parser, while unavailable speed values can be represented as null.

## 7. PostgreSQL Storage

PostgreSQL is the central persistence layer of the extraction workflow. The scraper connects to a database named `traffic_data` and can create the required `public.traffic_data` table when it does not already exist.

The table contains a generated primary key, measurement timestamp, location metadata, vehicle measurements, and a record creation timestamp.

Indexes are created for:

- `measurement_time`
- `punkt_nummer`
- `(road_number, county)`

These indexes support common time-, measurement-point-, road-, and county-oriented queries used by downstream analysis.

## 8. Duplicate Detection

Before inserting a record, the scraper checks whether a record already exists with the same combination of:

- measurement time
- county
- road number
- measurement-point number

If such a record exists, the row is skipped. This composite-key check is an important part of making repeated extraction runs less likely to create duplicate observations.

## 9. Study Dataset

The thesis analysis uses a dataset containing **5,137 observations** from **8 measurement points** across **4 Swedish counties**, covering **1998–2023**.

The pipeline therefore provides the data-engineering foundation for the empirical analysis rather than being separate from it: the historical records extracted and structured through this process are the inputs to the congestion, traffic-flow, and CO₂ analyses.

## 10. Downstream Analysis

After storage, the structured traffic data can be accessed by the analytical components of the thesis. The analysis includes:

1. traffic-flow and speed analysis;
2. congestion indicators such as Speed Ratio and Delay Index;
3. speed variability and traffic-flow regime analysis;
4. spatial and temporal hotspot analysis;
5. speed-dependent CO₂ estimation using the COPERT-based approach; and
6. analysis of the relationship between congestion severity and emissions.

The project also includes an MCP server prototype that provides parameterized access to the PostgreSQL data for natural-language querying. It is treated as a supporting component of the thesis rather than the primary analytical contribution.

## 11. Reproducibility

The pipeline is designed so that another researcher can understand the sequence from source URLs to database records and downstream analysis. The related scraper repository documents installation, dependencies, PostgreSQL setup, URL input, and execution options.

Environment-specific credentials should be configured locally and should not be committed to version control.

## Related Implementation

The implementation described here is maintained in:

**[`supal/trafficdata`](https://github.com/supal/trafficdata)**

This repository should be considered a core technical component of the overall thesis project and not an unrelated auxiliary project.
