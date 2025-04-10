# Tropical Cyclone Statistics over the Philippine Region

This project calculates and visualizes basic statistics of tropical cyclones (TCs) affecting the Philippine region using IBTrACS data and spatial analysis. It supports a research study examining TC duration, landfall characteristics, and rapid intensification across national operational domains. This repository serves as a companion to the manuscript *"Tropical Cyclone Evolution and Rapid Intensification Across Philippine Warning Domains (1979–2024),"* which is currently under review for publication.


## Objectives

- Identify TCs that entered the Philippine maritime area
- Estimate TC lifetime within the Philippine Coastal Waters (PCW) using 3-hourly track data
- Visualize storm behavior across different operational domains (TCID, TCAD, PAR, PCW)
- Analyze landfall characteristics (intensity, motion, and rapid intensification)

## Tools and Dependencies

The scripts are written in Python and require the following libraries:

- `pandas`
- `geopandas`
- `matplotlib`
- `shapely`
- `numpy`
- `scipy`
- `pyproj` (for CRS handling)

To run the notebooks, clone the repo and set up the environment using Conda:

```bash
git clone https://github.com/baracoma/tc-statistics.git
cd tc-statistics
conda env create -f environment.yml
conda activate tc-research
```

## Data Sources
- IBTrACS v04r01 Western Pacific Dataset
NOAA NCEI. (2024). International Best Track Archive for Climate Stewardship (IBTrACS) [Dataset].
  - Accessed April 10, 2025. https://www.ncei.noaa.gov/products/international-best-track-archive
  - File used: ibtracs.WP.list.v04r01.csv
- Philippine Waters Polygon
Custom shapefile or buffered boundary defining the Philippine Coastal Waters (PCW).
  - Note: Vector shapefiles are not included in this repo due to file size. However, the domain polygons are generated within the scripts.

## Methods

This analysis is based on tropical cyclone track data from the IBTrACS Western Pacific dataset (`v04r01`), covering the years 1979 to 2024. The methodology combines spatial filtering with time-centered intensity and motion analysis.

### 1. Filtering and Preprocessing
- IBTrACS data is filtered to include only tropical cyclones from 1979 onward to ensure satellite-era consistency.
- Track points are parsed by unique storm identifiers (`SID`) and sorted chronologically using the `ISO_TIME` field.
- Key variables such as wind speed (`WMO_WIND`), storm direction, and storm speed are cleaned and converted to numeric values as needed.

### 2. Geospatial Conversion and Domain Tagging
- TC track points are converted to a `GeoDataFrame` using `GeoPandas` for spatial processing.
- A polygon representing the **Philippine Coastal Waters (PCW)** is created using buffered geometries (0.5° by default) or loaded from a custom shapefile.
- Each track segment is evaluated for intersection with the PCW to determine if and when a storm made **landfall**.
- Additional bounding boxes define **PAR**, **TCAD**, and **TCID**, and are used to tag storm durations within each domain.

### 3. Landfall and Duration Analysis
- Landfall is defined as the **first point of intersection** between a TC track and the PCW polygon.
- Each storm is labeled according to whether it:
  - Entered the PCW
  - Originated outside and moved westward
  - Exhibited intensification before landfall
- Duration within each operational domain is estimated by counting 3-hourly track segments that intersect the domain polygon, then multiplying by 3 to convert to hours.

### 4. Rapid Intensification Tagging
- **Rapid Intensification (RI)** is defined as a ≥30-knot increase in wind speed within 24 hours prior to landfall.
- For each storm, the 3-hourly wind speed time series is linearly interpolated to capture finer fluctuations.
- The maximum 24-hour change in intensity is calculated using a rolling window.
- A storm is tagged as **LF RI** (landfalling RI) if this increase occurs within the 24 hours leading up to the PCW intersection.

### 5. Visualization and Output
- Visualizations include:
  - Spatial distribution of landfall points and grid-based intensification patterns (2° × 2°)
  - Time-centered evolution of intensity, storm speed, and acceleration (t = 0 centered on landfall)
  - Latitude-dependent scatterplots with linear regressions
- Summary statistics are exported as CSV files and figures saved as high-resolution PNGs.

## License

This project is licensed under the [MIT License](LICENSE).  
You are free to use, modify, and distribute this code with appropriate attribution.
