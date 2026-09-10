# Data

This directory documents the data used by the thesis project. It is intentionally **not** a location for uncontrolled raw database dumps or credentials.

## Data flow

```text
Trafikverket web interface
        ↓
Python / Selenium extraction
        ↓
PostgreSQL traffic_data
        ↓
Documented analytical dataset
        ↓
Congestion / traffic-flow / CO₂ analysis
```

The extraction implementation and its runtime configuration are maintained in the related [`supal/trafficdata`](https://github.com/supal/trafficdata) repository.

## Version-control policy

Do not commit:

- database passwords or `.env` files;
- local PostgreSQL database files;
- large uncontrolled raw extracts;
- generated caches;
- temporary CSV exports that cannot be traced to a documented research step.

If a data extract is required for reproducibility, it should be accompanied by provenance information, date/range, source URL or query scope, schema description, and an explanation of any transformations.

See [`docs/data-source.md`](../docs/data-source.md), [`docs/data-pipeline.md`](../docs/data-pipeline.md), and [`docs/traffic-data-schema.md`](../docs/traffic-data-schema.md).
