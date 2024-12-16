# Random Forest Approach for Field Boundary Identification and Acreage Calculation
This repository implements a Random Forest-based Classification approach using Sentinel-2 satellite imagery to identify land cover types, detect agricultural field boundaries, and calculate the number and acreage of these fields. The methodology efficiently utilizes spectral indices and Scene Classification Layer (SCL) data to produce robust results.

## Method

1.**Data Loading and Resampling**:

Sentinel-2 bands are loaded from GeoTIFF files.
Bands are spatially transformed and resampled to a consistent resolution and extent using a reference band.

2.**Feature Engineering**:

- The algorithm calculates several spectral indices for each pixel:
  - NDVI (Normalized Difference Vegetation Index): Indicates vegetation health.
  - NDWI (Normalized Difference Water Index): Identifies water bodies.
  - SAVI (Soil Adjusted Vegetation Index): Reduces soil noise in vegetation detection.
  - NBR (Normalized Burn Ratio): Highlights burned areas or soil.

3.**Label Creation and Masking**:

- Labels are derived from the Scene Classification Layer (SCL), assigning predefined classes:
- Vegetation, Water, Urban Area, Bare Soil, Forest, Wetlands, Snow/Ice, and Cloud.
- Unclassified pixels are masked out and excluded from the analysis.

4.**Random Forest Classification**:

- Features (spectral bands and indices) are stacked into a multi-dimensional array.
- A Random Forest model is trained using a stratified subset of the data, ensuring balanced class representation.
- The model predicts land cover types for the entire image.

5.**Boundary Detection and Enhancement**:

- Detected field boundaries are refined using OpenCV's contour detection.
- Contours are visualized on a normalized classified map for enhanced field visualization.

6.**Field Statistics and Metrics**:

- The total number of fields detected and their acreage is calculated.
- Metrics like IoU (Intersection over Union) are computed by comparing the predicted map with ground truth data.
- System resource usage, including CPU usage, memory usage, and processing time, is recorded.

## Sentinel-2 Data
Sentinel-2 data is available through AWS (no AWS account required):  
[Sentinel-2 L2A COGS](https://registry.opendata.aws/sentinel-2-l2a-cogs/)

For this analysis, tile **17TLJ** was used, but other tiles can also be utilized.

The following bands were used:

- B02 (Blue)
- B03 (Green)
- B04 (Red)
- B08 (NIR)
- B11 (SWIR1)
- B12 (SWIR2)

Additionally, the Scene Classification Layer (SCL) was used to derive class labels.

## Data Folder Structure

The tile data folder structure mimics the Sentinel-2 folder structure and naming conventions (Year/month_number/code_for_day_of_shooting). 

Example: 2022/6/S2A_17TLJ_20220628_0_L2A

## Shapefile and Ground Truth Data

The shapefile and ground truth data are sourced from the **CropScape - Cropland Data Layer** project by the **George Mason University Center for Spatial Information Science and Systems**:  

[CropScape - Cropland Data Layer](https://nassgeodata.gmu.edu/CropScape/)

The paths to the data folder, shapefile, and ground truth data are defined in the "Data Paths and Parameters" block. The default paths are as follows:

```python
data_folder = "/data/2022"
county_shapefile = "/data/shp_gmu/26063.shp"
ground_truth_path = os.path.join(data_folder, "cdl_2022.tif")
```

Adjust these paths as needed based on your local setup.