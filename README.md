# Assessing Climate-Driven Flood Risk and Community Resilience in Broad Channel

> **Project status:** Phase 1 (Plan & Design) is complete. Data collection, analysis, and results are in progress and will be added to this repository as the project moves forward.

---

## Overview

This project builds a data pipeline that combines **climate data, flood-model data, and community data** to measure flood risk in Broad Channel and to inform resilience planning.

## Background: Why Broad Channel?

Broad Channel is Jamaica Bay's only inhabited island. It is a low-lying community built on filled marshland, with frequent high-tide flooding and limited drainage.

| Fact | Detail |
|---|---|
| 2,370 | Residents on the island (2020 Census) |
| 1% | Annual-chance FEMA flood zone covers nearly the entire island |
| 97% | Share of households damaged by Hurricane Sandy (2012) |

**The problem:** Sea levels are rising and storms may be getting more intense, but there is no clear, data-driven picture of what that means for this community's flood risk.

## Research Questions

1. **Q1: Is rainfall changing?** Analyze decades of daily rainfall to test for trends in annual totals and heavy-rain days.
2. **Q2: How does sea-level rise change flooding?** Compare NYC DEP stormwater flood scenarios under current and future sea levels.
3. **Q3: Who is exposed, and are resilience plans enough?** Profile social vulnerability and review grey, nature-based, and policy plans.

## Data Sources

| Category | Source |
|---|---|
| Rainfall | gridMET daily precipitation for Broad Channel, 1979–2026 |
| Water levels | NOAA CO-OPS tide gauge at The Battery (Station 8518750) |
| Flood scenarios | NYC DEP Stormwater Flood Maps (2024) |
| Climate projections | CMRA Climate Resilience Toolkit, Queens County |
| Exposure & community | NOAA Coastal County Snapshots, FEMA flood zones, Census data |

## Tools & Technologies

Python · pandas · SciPy · Matplotlib · Climate Engine · NYC Open Data

## System Design

### Data flow

```
Data sources  →  Ingest & clean  →  Store  →  Analyze  →  Outputs
```

1. **Data sources:** gridMET rainfall, NOAA water levels, NYC DEP flood maps, CMRA projections, Census / FEMA / NOAA exposure data.
2. **Ingest & clean (Python + pandas):** download CSV / GIS files, check for gaps and bad values, standardize dates and units, aggregate daily data to annual.
3. **Store:** one project database of cleaned tables.
4. **Analyze:** three modules.
   - Rainfall trend module → rainfall charts
   - Flood scenario module → scenario maps
   - Exposure & vulnerability module → risk summary and recommendations

### Rainfall trend module (logic)

1. Load daily rainfall CSV
2. Check for missing or bad values; flag and clean gaps
3. Aggregate daily data to annual totals
4. Set the heavy-rain threshold (99th percentile)
5. Count heavy-rain days per year
6. Run a linear trend test (SciPy)
7. If `p < 0.05`, mark the trend as significant; otherwise mark it as no significant trend
8. Plot charts and export results

The flood-scenario module will follow the same pattern: load → clean → compare scenarios → export maps.

### Database schema

Every dataset links to a shared `locations` table, so rainfall, water levels, flood extent, and community data can be compared for the same place.

| Table | Key columns |
|---|---|
| `locations` | `location_id` (PK), name, county, latitude, longitude |
| `rainfall_daily` | `record_id` (PK), `location_id` (FK), date, precip_in, is_heavy_rain |
| `water_levels` | `reading_id` (PK), `location_id` (FK), station_id, timestamp, water_level_ft |
| `flood_scenarios` | `scenario_id` (PK), name, rain_in_per_hr, sea_level_rise_in, target_year |
| `flood_extent` | `extent_id` (PK), `scenario_id` (FK), `location_id` (FK), flood_type, area_sq_ft |
| `social_indicators` | `indicator_id` (PK), `location_id` (FK), indicator_name, value, source_year |

## Project Plan & Milestones

| Phase | Focus | Status |
|---|---|---|
| 1. Plan & Design | Define goals and research questions, pick datasets, design data flow and database | ✅ Current / complete |
| 2. Collect & Clean | Download all datasets, clean them in Python, load into the database | ⏳ Upcoming |
| 3. Analyze | Build the rainfall, flood-scenario, and exposure modules and run the analysis | ⏳ Upcoming |
| 4. Results & Final Presentation | Create charts and maps, write findings, present recommendations | ⏳ Upcoming |

## Expected Outcomes

- **Rainfall trend charts:** show whether annual rainfall and heavy-rain days are changing over time
- **Flood scenario maps:** compare flooding under current and future sea levels
- **Vulnerability profile:** identify which residents are most exposed to flooding
- **Resilience recommendations:** evaluate grey, nature-based, and policy options for the community

## Expected Challenges

- Missing days and gaps in long climate records
- Datasets use different formats, units, and map scales
- Flood models rely on assumptions about future rain and sea level
- Community data is only available at larger areas (county or tract level)

## Repository Structure (planned)

```
CISC4900-Research_Project/
├── data/
│   ├── raw/          # downloaded public datasets
│   └── processed/    # cleaned tables
├── notebooks/        # exploration and analysis
├── src/              # cleaning and analysis modules
├── outputs/          # charts, maps, exported results
├── docs/             # project presentation and diagrams
└── README.md
```

*This layout will be updated as the project develops.*

## Getting Started

*Setup and run instructions will be added once the code is in place.* The project will use Python 3 with pandas, SciPy, and Matplotlib.

## Author

**Tasnim Islam**, CISC 4900 Project Presentation

## Acknowledgments

Data provided by gridMET, NOAA CO-OPS, NYC Department of Environmental Protection, FEMA, the U.S. Census Bureau, and the Climate Mapping for Resilience and Adaptation (CMRA) tool.
