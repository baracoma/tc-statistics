# Tropical Cyclone Statistics over the Philippine Region

This project calculates and visualizes basic statistics of tropical cyclones (TCs) affecting the Philippine region using IBTrACS data and spatial analysis. It supports research on TC behavior by computing storm duration within national operational domains and mapping landfall-relevant characteristics.

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
- `descartes` (for older matplotlib-geopandas compatibility, if needed)

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
- Filter IBTrACS data from 1979 onward
- Process each unique TC (SID) to:
    - Convert track points to a GeoDataFrame
    - Apply a 0.5° buffer around Philippine waters
    - Identify intersection points and calculate the time spent within the buffer
    - Tag landfalling TCs and evaluate rapid intensification (RI) within 24 hours of coastal entry