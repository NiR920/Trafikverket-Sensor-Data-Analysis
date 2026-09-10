# Traffic data schema

The extraction component stores historical traffic measurements in PostgreSQL using the `public.traffic_data` table. The schema is designed around one measurement record identified by measurement time, location metadata, and vehicle-category measurements.

## Core fields

| Field | Purpose |
|---|---|
| `id` | Database-generated record identifier |
| `measurement_time` | Timestamp of the traffic measurement |
| `county` | Swedish county associated with the measurement point |
| `road_number` | Road number |
| `punkt_nummer` | Trafikverket measurement-point identifier |
| `created_at` | Database insertion timestamp |

## Vehicle measurements

For each supported vehicle category, the table stores a count and an average speed. Categories include:

- all vehicles
- passenger cars
- heavy vehicles
- heavy vehicles with trailer
- heavy vehicles without trailer
- three-axle tractor-trailer combinations
- two-axle tractor-trailer combinations
- three-axle tractor without trailer
- two-axle tractor without trailer
- passenger cars with trailer
- passenger cars without trailer

The naming convention is `<vehicle_category>_count` and `<vehicle_category>_avg_speed`.

## Database types

- Counts are stored as integers.
- Average speeds are stored as fixed-precision decimal values.
- Measurement timestamps use PostgreSQL `TIMESTAMP`.
- Location and road identifiers are stored as character fields because their source representation is not purely numeric in all contexts.

## Indexing

The extraction implementation creates indexes for:

- `measurement_time`
- `punkt_nummer`
- `(road_number, county)`

These support the main temporal and location-oriented retrieval patterns used by the analysis and MCP prototype.

## Data-quality considerations

The scraper normalizes Swedish decimal-comma speed values and converts vehicle counts to integers before database insertion. It also checks for an existing record using `measurement_time`, `county`, `road_number`, and `punkt_nummer` before inserting a new record.

This duplicate check is an application-level safeguard. For future production use, the same logical key should also be considered for a database-level uniqueness constraint so that concurrent extraction processes cannot create duplicates.

## Source of truth

This document describes the schema implemented by the current extraction code. It does not represent a separate analytical data warehouse schema. Analytical transformations such as Speed Ratio, Delay Index, speed variability, traffic regimes, and CO₂ estimates are derived from the stored measurements rather than stored as raw extraction fields.
