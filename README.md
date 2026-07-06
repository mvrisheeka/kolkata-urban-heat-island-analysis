# Urban Heat Island Analysis using Land Surface Temperature and Built-up Density 

Maps Urban Heat Island (UHI) zones in Kolkata District, West Bengal, India using Landsat-8 satellite data. Derives Land Surface Temperature (LST), vegetation cover (NDVI), and built-up density (NDBI), then analyzes how urbanization drives surface heating.

## Features

- Loads Kolkata District boundary from GADM
- Fetches cloud-free Landsat-8 Collection 2 Level-2 imagery from Google Earth Engine
- Calculates NDVI (vegetation), NDBI (built-up density), and LST (surface temperature)
- Classifies LST into 5 thermal categories
- Delineates UHI zones using a 90th-percentile threshold
- Classifies built-up density levels
- Correlates built-up density with surface temperature
- Generates maps and charts for all of the above

## Study Area

Kolkata District, West Bengal, India — tropical wet/dry climate, dense urban core, ideal for studying UHI effects.

## Dataset

- **Source:** Landsat-8 OLI/TIRS, Collection 2 Level-2 (Google Earth Engine), < 10% cloud cover
- **Bands:** Red (B4), NIR (B5), SWIR (B6), Thermal (B10)
- **Boundary:** GADM Kolkata District administrative boundary

## Formulas

```
NDVI = (NIR - Red) / (NIR + Red)
NDBI = (SWIR - NIR) / (SWIR + NIR)
LST  = ST_B10 * 0.00341802 + 149.0 - 273.15   (°C)
```

## Classification Reference

**LST Categories**

| Range (°C) | Category | Colour |
|---|---|---|
| < 40 | Cool / Low Heat | Dark Blue |
| 40–45 | Mild Heat | Green |
| 45–50 | Moderate Heat | Yellow |
| 50–53 | High Heat | Orange |
| > 53 | Extreme Heat / UHI Core | Red |

**Built-up Density (NDBI)**

| Range | Category |
|---|---|
| < 0 | Low Built-up / Vegetation |
| 0–0.2 | Moderate Built-up |
| > 0.2 | High Built-up |

## Key Results

- Temperature range observed: **33.82 °C – 56.59 °C**
- Dense built-up areas run hotter; vegetation and water bodies cool their surroundings
- Positive correlation between NDBI and LST — more urbanization, more heat
- Thermal hotspots cluster in the central/northern, densely built parts of the district

## Tech Stack

Python · Google Earth Engine · Geemap · GeoPandas · Pandas · NumPy · Matplotlib

## Project Structure

```text
urban-heat-island-kolkata/
├── uhi_kolkata_analysis.ipynb
├── README.md
├── data/
│   └── kolkata_boundary.zip
└── outputs/
    ├── ndvi_map.png
    ├── ndbi_map.png
    ├── lst_map.png
    ├── uhi_zones_map.png
    ├── lst_classification_map.png
    ├── builtup_density_map.png
    ├── builtup_vs_temperature.png
    └── final_uhi_map.png
```

## Setup

```bash
git clone https://github.com/<your-username>/urban-heat-island-kolkata.git
cd urban-heat-island-kolkata
```

Open `uhi_kolkata_analysis.ipynb` in Google Colab, Jupyter, or VS Code, then run:

```python
!pip install -q earthengine-api geemap geopandas rasterio folium pandas numpy matplotlib
```

```python
import ee

ee.Authenticate()
ee.Initialize(project="your-gee-project-id")  # replace with your own GEE-registered Cloud project
```

> Create/find your project ID at [Google Cloud Console](https://console.cloud.google.com/), and register it for Earth Engine at [https://code.earthengine.google.com/register](https://code.earthengine.google.com/register).

Then run all cells in sequence. Outputs (maps, charts) will display inline and save to `outputs/` if export is enabled.
