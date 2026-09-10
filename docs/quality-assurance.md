# Quality assurance and technical review

This document records the repository-level review performed before treating the project as a professional public research repository.

## Scope reviewed

- thesis PDF and reported analytical figures
- main thesis repository structure and documentation
- related Python extraction repository
- PostgreSQL schema and insertion logic
- analysis notebook content and execution assumptions
- TypeScript MCP database layer
- MCP analytics layer
- dependency and ignore configuration

## Python extraction review

The scraper implements the core acquisition workflow: Selenium-based browsing, Trafikverket page metadata extraction, Swedish decimal-comma parsing, PostgreSQL insertion, coordinate caching, and application-level duplicate checks.

### Findings requiring attention

1. **Database credentials are hard-coded in the current scraper.** The source defines a PostgreSQL password directly in `scraper.py`. This must be replaced with environment/local configuration before the scraper is presented as production-ready.
2. **Python dependencies were incomplete in the extraction repository.** The scraper imports `psycopg2`, although its original `requirements.txt` did not list the PostgreSQL driver. The main research repository now provides a consolidated baseline `requirements.txt` covering the observed extraction and notebook dependencies.
3. **Duplicate prevention is application-level.** The scraper checks for an existing `(measurement_time, county, road_number, punkt_nummer)` combination before inserting. A database-level uniqueness constraint should be considered for concurrent or parallel extraction.
4. **Coordinate cache is a generated local artifact.** It should remain ignored unless a deliberately curated cache is required as a research input.
5. **Broad exception handling exists in parts of the scraper.** Network/browser parsing failures should be logged with enough context to diagnose the affected URL, measurement point, and operation.

## Analysis notebook review

The original `Traffic data analysis.ipynb` is a substantial thesis artifact and should be preserved rather than recreated from memory. Inspection of its stored notebook content confirms that it:

- loads PostgreSQL traffic data into Pandas;
- uses `psycopg2-binary` and SQLAlchemy;
- uses NumPy and Matplotlib for analytical work and visualization;
- produces the analytical outputs reflected in the submitted thesis;
- contains notebook cells that install packages during execution;
- contains a PostgreSQL connection string with database credentials directly in notebook code.

The hard-coded notebook connection string is a reproducibility/security issue and should be replaced with environment variables or a local configuration mechanism before the notebook is mirrored into the main repository. Package installation commands should also be removed from the analytical workflow and replaced by the repository-level dependency specification.

Because the notebook is large and contains saved outputs, it should be migrated only as the original notebook artifact, followed by a separate cleanup pass that preserves analytical logic and reported results.

## MCP review

The TypeScript MCP implementation uses parameterized SQL values for normal filters, but its average-speed method interpolates a caller-provided column name directly into SQL. Column identifiers cannot be parameterized like values, so an explicit allow-list is required before interpolation.

The MCP analytics implementation also labels the highest-average-speed hours as **"Peak Hours"**. That label is potentially misleading for congestion analysis: the highest speeds are not necessarily the highest congestion. The thesis should remain the authoritative source for congestion definitions and reported peak-hour findings.

The prototype also converts database timestamps through JavaScript `Date` objects. Time-zone assumptions should be explicit when the tool is used across environments.

## Thesis figure review

The submitted thesis PDF contains the reported analytical figures, including:

- system architecture
- Speed Ratio analysis
- Delay Index heatmap
- spatial congestion/emission hotspot analysis
- integrated congestion-emission analysis
- hourly temporal patterns
- integrated emission analysis
- spatiotemporal heatmaps
- speed-flow regimes
- validation diagnostics
- hourly congestion/volume/emission patterns
- location-by-hour heatmaps
- distribution and speed-flow diagnostics
- hourly emissions heatmap
- MCP/Claude Desktop screenshots

The PDF was inspected page-by-page for figure presence and rendering. The figures are embedded in the submitted document and were not found to be missing or structurally broken. The MCP screenshots are appropriately treated as prototype evidence rather than as primary analytical figures.

For future repository exports, publication-quality analytical figures should be generated from code where the source is available. Screenshots should be reserved for interface demonstrations.

## Reproducibility review

The repository should not commit:

- database credentials
- `.env` files
- local PostgreSQL database files or uncontrolled database dumps
- virtual environments
- `__pycache__` or `.pyc` files
- OS metadata such as `.DS_Store`
- generated coordinate caches unless explicitly versioned as research inputs
- large generated output directories

The main repository `.gitignore` now covers these classes of artifacts.

## Repository organization review

Development-history documents such as setup summaries, troubleshooting notes, batch installers, and environment-specific fixes belong in the technical extraction repository only when they remain useful to someone maintaining that software. They should not be copied wholesale into the thesis research repository.

The main repository instead emphasizes:

1. research questions and methodology;
2. source and data-pipeline documentation;
3. analytical methods;
4. schema and reproducibility documentation;
5. analysis code/notebooks;
6. results and figures;
7. thesis document;
8. supporting MCP documentation.

## Final status

The main repository is now professionally organized and the research documentation, licensing metadata, citation metadata, dependency baseline, data governance, schema documentation, and QA record are in place.

The remaining executable-code hardening must be performed in `supal/trafficdata` because the connected GitHub integration currently does not permit write access to that repository. Those changes should be made before calling the scraper/MCP implementation production-hardened: remove hard-coded database credentials, remove notebook credential strings, replace notebook package-install cells with dependency management, strengthen duplicate protection, validate MCP SQL identifiers, clarify peak-hour semantics, and make time-zone behavior explicit.
