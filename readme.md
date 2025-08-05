# Tropical Cyclone Statistics over the Philippine Region

In this project we calculate and visualize basic statistics of Tropical Cyclones (TCs) affecting the Philippine. This repository serves as a companion to the manuscript *Characteristics and Near-Landfall Behavior of Tropical Cyclones Affecting the Philippines"* which was accepted for publication in [Tropical Cyclone Research and Review](https://www.keaipublishing.com/en/journals/tropical-cyclone-research-and-review/).


## Objectives

- Identify TCs that entered the Philippine maritime area
- Estimate TC lifetime within the Philippine Coastal Waters (PCW) using 3-hourly track data
- Visualize storm behavior across different operational domains (TCID, TCAD, PAR, PCW)
- Analyze landfall characteristics (intensity, motion, and rapid intensification)

## Tools and Libraries

This project uses Python with the following main libraries:

- `pandas`, `geopandas`, `numpy` — for data handling
- `shapely`, `pyproj` — for geospatial operations
- `matplotlib`, `scipy` — for visualization and statistics

To install all dependencies, see the provided [`environment.yml`](./environment.yml) file.


## Data Sources
- IBTrACS v04r01 Western Pacific Dataset
NOAA NCEI. (2024). International Best Track Archive for Climate Stewardship (IBTrACS) [Dataset].
  - Accessed April 10, 2025. https://www.ncei.noaa.gov/products/international-best-track-archive
  - File used: ibtracs.WP.list.v04r01.csv
- Philippine Waters Polygon custom shapefile or buffered boundary defining the  PCW adapted from the EEZ Archipelagic Waters v4 shapefile (Flanders Marine Institute, 2023)
  - Available at https://www.marineregions.org  
  - Note: Vector shapefiles are not included in this repo due to file size. However, the domain polygons are generated within the scripts.

## Methods

This analysis is based on tropical cyclone track data from the IBTrACS Western Pacific dataset (`v04r01`), covering the years 1979 to 2024. The methodology combines spatial filtering with time-centered intensity and motion analysis.

### 1. Filtering and Preprocessing
- IBTrACS data is filtered to include only tropical cyclones from 1979 onward to ensure satellite-era consistency.
- Track points are parsed by unique storm identifiers (`SID`) and sorted chronologically using the `ISO_TIME` field.
- Key variables such as wind speed (`WMO_WIND`), storm direction (`STORM_DIR`), storm speed (`STORM_SPEED`), `LAT`, and `LONG` are cleaned and converted to numeric values as needed.

### 2. Geospatial Conversion and Domain Tagging
- TC track points are converted to a `GeoDataFrame` using `GeoPandas` for spatial processing.
- A polygon representing the **PCW** is created using buffered geometries (0.5° by default) or loaded from a custom shapefile.
- Each track segment is evaluated for intersection with the PCW to determine if and when a storm made **landfall**.
- Additional bounding boxes (**PAR**, **TCAD**, and **TCID**) are used to tag storm durations within each domain.

### 3. Interpolation
- To enable higher temporal resolution in capturing short-term intensity fluctuations, we linearly interpolate the 6-hourly `WMO_WIND` to produce 3-hourly wind estimates surrounding landfall.
- Interpolation is performed only within valid observational intervals, ensuring no extrapolation occurs beyond the first or last data point with available intensity records. 


### 4. Landfall and Duration Analysis
- Landfall is defined as the **first point of intersection** between a TC track and the PCW polygon.
- Each storm is labeled according to whether it:
  - Entered the PCW
  - Originated outside and moved westward
  - Exhibited intensification before landfall
- Duration within each operational domain is estimated by counting 3-hourly track segments that intersect the domain polygon, then multiplying by 3 to convert to hours.

### 5. Rapid Intensification Tagging
- **Rapid Intensification (RI)** is defined as a ≥30-knot increase in wind speed within 24 hours prior to landfall.
- The maximum 24-hour change in intensity is calculated using a rolling window.
- A storm is tagged as **LF-RI** (landfalling RI) if this increase occurs within the 24 hours leading up to the PCW intersection.

### 6. Visualization and Output
- Visualizations include:
  - Spatial distribution of landfall points and grid-based intensification patterns (1° × 1°)
  - Time-centered evolution of intensity, storm speed, and acceleration (t = 0 centered on landfall)
  - Latitude-dependent scatterplots with linear regressions
- Summary statistics are exported as CSV files and figures saved as high-resolution PNGs.

## References
- Flanders Marine Institute (2023). Maritime Boundaries Geodatabase: Archipelagic Waters, version 4. Available online at http://www.marineregions.org/. https://doi.org/10.14284/629 

- Knapp, K. R., Kruk, M. C., Levinson, D. H., Diamond, H. J., & Neumann, C. J. (2010). The International Best Track Archive for Climate Stewardship (IBTrACS): Unifying Tropical Cyclone Data. Bulletin of the American Meteorological Society, 91(3), 363–376. https://doi.org/10.1175/2009BAMS2755.1  

## License

This project is licensed under the [MIT License](LICENSE). You are free to use, modify, and distribute this code with appropriate attribution.
