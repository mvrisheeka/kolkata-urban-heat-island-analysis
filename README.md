# Urban Heat Island Analysis using Land Surface Temperature and Built-up Density

This project analyzes the spatial pattern of Urban Heat Island (UHI) zones in Kolkata District, West Bengal, India by deriving Land Surface Temperature (LST) from Landsat-8 satellite data and studying its correlation with built-up density.

Project Report submitted in fulfilment of the requirements for the Award of the Internship of Summer Training Program — Space Science Technology, Remote Sensing & GIS Using Python, India Space Academy, Department of Space Education.

**Author:** Mulakala Vrisheeka (SRM Institute of Science and Technology, RA2311003010023)
**Supervisor:** Miss. Alisha Sinha

## Introduction

Urban development has drastically changed the nature of the land surface, leading to environmental problems such as elevated surface temperatures and the urban heat island effect — the replacement of natural land with roads, buildings, and other concrete structures that accumulate more heat. Satellite-derived Land Surface Temperature, combined with vegetation and built-up indices, helps explain the patterns and causes of urban warming. Kolkata, one of India's major and rapidly growing metropolitan cities, was chosen as the study area to understand the relationship between surface temperature and built-up density.

## Objective

The main goal of this project is to analyze the spatial pattern of UHI zones in Kolkata District by deriving LST from Landsat-8 data and correlating it with built-up density, in order to reveal high-temperature zones and thermal hotspots.

Specific objectives:

1. Derive Land Surface Temperature (LST) from Landsat-8 thermal infrared imagery.
2. Calculate NDVI (Normalized Difference Vegetation Index) to estimate vegetation cover and green space distribution.
3. Map built-up density using NDBI (Normalized Difference Built-up Index).
4. Identify Urban Heat Island zones through areas of significantly elevated surface temperature.
5. Examine the relationship between built-up density and LST.
6. Investigate the spatial pattern of urban warming and locate major thermal hotspots.
7. Generate thematic maps of NDVI, NDBI, LST, and UHI zones.

## Study Area

- **Location:** Kolkata District, West Bengal, India
- **Climate:** Tropical wet and dry, with hot summers, monsoon season, and relatively cool winters
- Kolkata sits on the eastern bank of the Hooghly River and is one of the most densely populated cities in India, making it well suited for UHI analysis.

## Dataset Used

**Satellite Data:**
- Landsat-8 OLI/TIRS Collection 2 Level-2 imagery (Google Earth Engine)
- Only cloud-free scenes with less than 10% cloud cover were used

**Bands Used:**

| Band | Use |
|---|---|
| Band 4 (Red) | NDVI calculation |
| Band 5 (Near Infrared) | NDVI and NDBI calculation |
| Band 6 (Short Wave Infrared) | NDBI calculation |
| Band 10 (Thermal Infrared) | Land Surface Temperature (LST) estimation |

**Ancillary Data:**
- Kolkata District administrative boundary (GADM Administrative Boundary Dataset), used as the Area of Interest (AOI)

## Technologies Used

- Python
- Google Earth Engine (GEE)
- Google Colab / Jupyter Notebook
- Geemap
- GeoPandas
- Pandas
- NumPy
- Matplotlib

## Methodology

1. **Environment Setup** – Install geospatial libraries and authenticate Google Earth Engine.
2. **Study Area Extraction** – Extract the Kolkata District boundary from the GADM dataset and use it as the AOI.
3. **Landsat-8 Image Acquisition** – Filter Landsat-8 Collection 2 Level-2 imagery by AOI, time period, and cloud cover (< 10%), then create a median composite clipped to the AOI.
4. **NDVI Calculation** – `NDVI = (NIR − Red) / (NIR + Red)`
5. **NDBI Calculation** – `NDBI = (SWIR − NIR) / (SWIR + NIR)`
6. **Land Surface Temperature Estimation** – Derive LST from thermal Band 10 (ST_B10) using the Landsat Collection 2 surface temperature scaling factors.
7. **LST Classification** – Classify LST into five thermal categories:

   | Class | Temperature Range (°C) | Thermal Category | Map Colour |
   |---|---|---|---|
   | 1 | < 40 | Cool / Low Heat | Dark Blue |
   | 2 | 40 – 45 | Mild Heat | Green |
   | 3 | 45 – 50 | Moderate Heat | Yellow |
   | 4 | 50 – 53 | High Heat | Orange |
   | 5 | > 53 | Extreme Heat / UHI Core | Red |

8. **Urban Heat Island Delineation** – UHI zones identified using a 90th-percentile threshold (pixels exceeding 50.81 °C in this study were flagged as thermal hotspots).
9. **Built-up Density Classification** – NDBI classified into density levels:

   | NDBI Range | Category |
   |---|---|
   | < 0 | Low Built-up / Vegetation |
   | 0 – 0.2 | Moderate Built-up |
   | > 0.2 | High Built-up |

10. **Spatial Analysis and Correlation Assessment** – NDBI and LST values sampled across the AOI and analyzed for correlation between built-up density and surface temperature.

## Results

- Observed temperature range in the study area: **33.82 °C (min) – 56.59 °C (max)**
- Areas of high built-up density (dense urban core) consistently showed higher LST, while vegetation and water bodies acted as cooling agents.
- A positive correlation was found between built-up density (NDBI) and land surface temperature (LST).
- Thermal hotspots (UHI zones) were concentrated in the densely urbanized central and northern parts of the district.

Generated outputs include:
- NDVI map
- NDBI (built-up density) map
- Land Surface Temperature map
- Urban Heat Island zones map
- LST classification map
- Built-up density classification map
- Built-up density vs. temperature scatter plot
- Final combined Urban Heat Island map

## Conclusion

The study successfully mapped UHI zones in Kolkata District using Landsat-8 imagery and GIS tools. A strong relationship was found between vegetation cover, built-up density, and surface temperature distribution through the NDVI, NDBI, and LST analyses. Densely built-up areas correspond to higher temperatures, while vegetation and water bodies help cool their surroundings. This confirms that LST-based UHI analysis is an effective way to understand urban temperature distribution and identify heat-sensitive zones and thermal hotspots.

## Project Structure

```text
urban-heat-island-kolkata/
│
├── uhi_kolkata_analysis.ipynb
├── README.md
│
├── data/
│   └── kolkata_boundary.zip
│
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

## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/urban-heat-island-kolkata.git
cd urban-heat-island-kolkata
```

### 2. Open the Notebook

Open the notebook using one of the following:

- Google Colab
- Jupyter Notebook
- VS Code with the Jupyter extension

### 3. Install Required Libraries

Run the following command in the first notebook cell:

```python
!pip install -q earthengine-api geemap geopandas rasterio folium pandas numpy matplotlib
```

### 4. Authenticate Google Earth Engine

Run the following code and complete the authentication process using the link shown in the output.

**Note:** Every Earth Engine session must be initialized with a registered Cloud project. Replace `"your-gee-project-id"` below with your own GEE-registered Google Cloud project ID before running the notebook.

```python
import ee

ee.Authenticate()
ee.Initialize(project="your-gee-project-id")
```

> You can find or create your project ID at [Google Cloud Console](https://console.cloud.google.com/) and register it for Earth Engine access at [https://code.earthengine.google.com/register](https://code.earthengine.google.com/register).

### 5. Run the Notebook

Run all cells in sequence. The notebook will:

- Load the Kolkata District boundary from the GADM dataset.
- Acquire and filter cloud-free Landsat-8 imagery from Google Earth Engine.
- Calculate NDVI, NDBI, and Land Surface Temperature (LST).
- Classify LST into thermal categories and delineate UHI zones.
- Classify built-up density from NDBI.
- Assess the correlation between built-up density and LST.
- Generate maps, charts, and the final Urban Heat Island map.

### 6. View the Results

The generated outputs will be displayed in the notebook. If export commands are enabled, maps and charts can also be saved in the `outputs/` folder.
