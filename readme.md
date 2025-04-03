# Tropical Cyclone Statistics over the Philippine Region

This project calculates basic statistics of tropical cyclones (TCs) over the Philippine region using IBTrACS data and spatial analysis.

## Objectives
- Identify TCs that entered the Philippine maritime area
- Calculate the number of TC track points within a 0.5° buffer around Philippine waters
- Estimate TC lifetime within Philippine bounds based on 3-hourly data
- Visualize TC tracks and interactions with the Philippine region

## Data Sources
- **IBTrACS Western Pacific CSV** (`ibtracs.WP.list.v04r01.csv`)
- **Philippine Waters Boundary Shapefile** (with holes filled):  
  `/home/cs-iesm-geostorm/Data/gis/vector/boundaries/waters/philippine_waters_filled.shp`

## Methods
- Filter IBTrACS data from 1979 onward
- Loop through each unique TC (`SID`) to:
  - Convert TC points to GeoDataFrame
  - Check which points fall within a 0.5° buffered polygon of Philippine waters
  - Count points and estimate total hours spent within the area
- Plot all TC points, highlighting those inside the buffer

## Sample Result
- Out of 1507 tropical cyclones since 1979, **460** entered the Philippine region.
- The average time spent within the Philippine waters (including 0.5° buffer): **~28 hours**

## Visualization
- TC points inside PH buffer are shown in red; others in gray
- Philippine waters and the buffer zone are outlined for reference

## Tools
- Python (Pandas, GeoPandas, Shapely, Matplotlib)
- QGIS (for pre-processing shapefiles)

---

