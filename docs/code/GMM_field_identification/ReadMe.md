# Field Boundary Identification and Field Acreage Calculation

This repository applies Gaussian Mixture Models (GMM) and SLIC Superpixel Segmentation to analyze Sentinel-2 satellite imagery. The method identifies agricultural fields by crop type, determines their boundaries, and calculates the number and acreage of these fields. The approach is effective for both large-scale datasets, including TIF files covering entire counties, and single-image analysis.

---

## Method

### Data Loading and Preprocessing
- **Data Loading:** The algorithm processes Sentinel-2 GeoTIFF files, focusing on RGB bands (2, 3, and 11) for clustering and segmentation.
- **Preprocessing:** The elbow method is applied to determine the optimal number of clusters for GMM, with **6 clusters** identified as the most effective solution for balancing accuracy and efficiency.

### Clustering and Segmentation
1. **GMM Clustering:**
   - Groups pixels based on spectral reflectance values to separate fields by color similarities.
   - The elbow method determines the optimal number of clusters, ensuring minimal computational overhead without compromising accuracy.

2. **SLIC Superpixel Segmentation:**
   - Refines the clusters created by GMM into compact, uniform regions with distinct boundaries.
   - Assigns unique IDs to superpixels, allowing for easy tracking of fields and monitoring of crop health over time.

### Boundary and Acreage Calculation
- **Polygon Creation:**
   - Converts segmentation labels into spatial polygons representing individual fields.
   - Cleans polygons to remove noise, smooth boundaries, and ensure accuracy.
   - Valid polygons are stored in a GeoDataFrame, with a minimum area threshold applied to exclude insignificant polygons.

- **Acreage Calculation:**
   - Calculates the acreage for each identified field polygon.
   - GMM estimated **461,427 acres** out of the actual **536,219 acres**, achieving **86% accuracy**.
   - Discrepancies are primarily due to overlapping or closely situated fields with similar crop types.

### Visualization
- Field boundary polygons are visualized overlaid on True Color Imagery for confirmation.
- Comparative visualizations highlight identified boundaries against ground truth data, such as Meta AI’s **Segment Anything** model.

---

## Data Sources and Parameters

### Sentinel-2 Data
- Data is sourced from the **AWS Sentinel-2 L2A COGS** repository.
- High-resolution imagery tiles, such as **17TLJ**, are used for this analysis.

### Shapefile and Ground Truth Data
- Ground truth data is derived from tools like **Meta AI’s Segment Anything** and shapefiles for regions like Huron County (Michigan).
- Example paths to required data files:
  ```python
  data_folder = "/data/2022"
  county_shapefile = "/data/shp_gmu/26063.shp"
  ground_truth_path = os.path.join(data_folder, "cdl_2022.tif")
