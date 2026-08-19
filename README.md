# Maji Ndogo Water Crisis Analysis

A data analysis project exploring water access, gender parity, and sustainable development indicators for **Maji Ndogo** ("small water" in Swahili) — a fictional country used as the setting for the well-known ALX/Data Science data analysis curriculum. The project combines a farm/water survey database, UN Sustainable Development Goal (SDG) data, and gender parity indicators to explore the water crisis from several angles, visualized in Power BI.

## What this project covers

This repo is data-first: it holds the datasets, database, and Power BI reports used for the analysis, rather than analysis scripts. It's built around three connected themes:

1. **Water access on the ground** — survey data on water sources, queue times, water quality, and who is affected (`Md_summary.csv`, `Maji_Ndogo_farm_survey_small.db`)
2. **Gender parity and development context** — how water access connects to labor force participation, literacy, and urban/rural population splits by gender (`Gender_parity_2022.csv`, `Gender_Egypt.csv`, `Indicator*.csv`)
3. **Global development benchmarking** — how Maji Ndogo (and comparable countries) measure up against the UN's Sustainable Development Goals (`UN_SDG_dashboard_data.csv`)

## The database: `Maji_Ndogo_farm_survey_small.db`

A SQLite database with four related tables, each keyed on `Field_ID` (5,654 records each):

| Table | Contents |
|---|---|
| `geographic_features` | Elevation, latitude/longitude, location, slope |
| `weather_features` | Rainfall, min/max/average temperatures |
| `soil_and_crop_features` | Soil fertility, soil type, pH |
| `farm_management_features` | Pollution level, plot size, crop type, annual yield vs. standard yield |

This is designed to be explored with SQL joins across tables on `Field_ID` — e.g. relating soil quality and rainfall to actual crop yield outcomes.

## Key datasets

| File | What it contains |
|---|---|
| `Md_summary.csv` | Field-level water source survey data: location, source type, pollutant levels (ppm), biological contamination results, queue time, visit counts, number of people served, and demographic breakdown (% male/female/child) of who uses each source |
| `Country.csv` | Country reference table — name, major region, region, development classification |
| `Indicator.csv` / `Indicator_combined.csv` | Reference tables describing the development/gender indicators used elsewhere in the dataset |
| `Gender_parity_2022.csv` | Country-level gender indicators: labor force participation, unemployment, literacy, and urban/rural population, split by gender, plus GDP per capita |
| `Gender_Egypt.csv` | Egypt-specific gender/development indicator time series |
| `UN_SDG_dashboard_data.csv` | Country-level scores against all 17 UN Sustainable Development Goals, plus overall SDG Index rank and score |
| `MD_Full_map.json` / `MD_Map.png` | Geographic map data/image used for spatial visualizations |

There are a few duplicate/versioned files (e.g. `Country (1).csv`, `Country (2).csv`, `Gender_parity_data_transformations (1/2/3).csv`) — these look like iterative saves during data cleaning. Worth consolidating to the final version and removing the older copies to avoid confusion about which is authoritative.

## Dashboards

- **`MD_PROJECT.pbix`** and **`Magi Ndogo.pbix`** — Power BI report files. Open these in Power BI Desktop to explore the interactive dashboards built from the datasets above (these two look like they may be different iterations of the same dashboard — worth checking which is the current/final version).

## How to explore this project

**With Power BI:** Open `MD_PROJECT.pbix` (or `Magi Ndogo.pbix`) in Power BI Desktop — this is the fastest way to see the finished analysis and visualizations.

**With SQL / Python, directly on the data:**
```python
import sqlite3
import pandas as pd

conn = sqlite3.connect("Maji_Ndogo_farm_survey_small.db")
df = pd.read_sql("""
    SELECT g.Field_ID, g.Elevation, w.Rainfall, s.Soil_fertility, f.Annual_yield
    FROM geographic_features g
    JOIN weather_features w ON g.Field_ID = w.Field_ID
    JOIN soil_and_crop_features s ON g.Field_ID = s.Field_ID
    JOIN farm_management_features f ON g.Field_ID = f.Field_ID
""", conn)
```

## Requirements

```
pandas
openpyxl      # for reading the .xlsx files
```
Power BI Desktop (Windows) is required to open the `.pbix` dashboard files.

## Suggested next steps for this repo

- Add a short write-up (even a few paragraphs) of the actual findings/insights from the analysis — right now the story lives inside the Power BI file, which isn't viewable on GitHub itself
- Clean up the duplicate/versioned CSVs into single final files
- Consider exporting key dashboard visuals as static images so the results are visible directly on the GitHub repo page, since `.pbix` files can't be previewed in-browser
