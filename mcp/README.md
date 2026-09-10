# MCP server prototype

The thesis project includes a TypeScript/Node.js **Model Context Protocol (MCP)** server prototype in the related [`supal/trafficdata`](https://github.com/supal/trafficdata) repository.

The prototype connects to PostgreSQL and exposes parameterized operations for retrieving historical traffic data, statistics, vehicle counts, speeds, locations, dates, roads, counties, measurement points, and simple text-based analytical summaries.

## Role in the thesis

The MCP server is a **supporting prototype**. It demonstrates how an AI client can access the historical traffic database through structured tools, but it is not the primary evaluated research contribution. The primary research contributions are the automated historical data pipeline and the congestion, traffic-flow, and emissions analysis.

## Configuration

The MCP implementation uses environment variables for PostgreSQL configuration. A local `.env` file may be used during development, but credentials must never be committed to Git.

## Important implementation notes

The current prototype has been reviewed for architectural and security risks. In particular:

- vehicle-speed column names should be validated against an explicit allow-list before being interpolated into SQL;
- result limits should be bounded so an MCP request cannot accidentally request an excessive number of records;
- the phrase **"peak hours"** should not be interpreted as congestion by itself: the current prototype ranks hours by average speed, so its highest-speed hours are not necessarily the most congested hours;
- time-zone handling should be made explicit when converting database timestamps to JavaScript `Date` values;
- ASCII chart output is intended for lightweight MCP responses and should not replace the publication-quality figures used in the thesis.

These points are documented so that the prototype remains clearly separated from the validated analytical results reported in the thesis.

## Source implementation

The executable MCP implementation remains in:

```text
supal/trafficdata/mcp-server/
├── src/
│   ├── analytics.ts
│   ├── database.ts
│   ├── server.ts
│   └── test.ts
├── package.json
├── package-lock.json
└── .env.example
```

The related repository is the source of truth for the executable MCP implementation until the code is intentionally mirrored into this research repository.
