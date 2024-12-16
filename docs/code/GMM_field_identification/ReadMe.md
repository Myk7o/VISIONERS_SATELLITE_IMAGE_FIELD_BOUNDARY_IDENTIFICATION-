# Field Boundary Identification and Field Acreage Calculation Using Gaussian Mixture Model (GMM) and SLIC

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
   - **GMM works perfectly with a TIF file covering the entire county,** making it highly scalable for large-scale datasets without altitude specification.

2. **SLIC Superpixel Segmentation:**
   - Refines the clusters created by GMM into compact, uniform regions with distinct boundaries.
   - Assigns unique IDs to superpixels, allowing for easy tracking of fields and monitoring of crop health over time.
   - **GMM + SLIC code works only with images captured at altitudes of 500 meters or lower,** making it ideal for drone imagery or high-resolution satellite images.

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

## GMM and SLIC Tests

The **GMM and SLIC Tests** file provides a comprehensive evaluation of the approach under different circumstances, including variations in imagery resolution, cropping patterns, and environmental conditions. It helps validate the robustness and scalability of the segmentation methodology.

---

## Data Sources and Parameters

### Sentinel-2 Data
- Data is sourced from the **AWS Sentinel-2 L2A COGS** repository.
- High-resolution imagery tiles, such as **17TLJ**, are used for this analysis.

### Shapefile and Ground Truth Data
- Ground truth data is derived from tools like **Meta AI’s Segment Anything** and shapefiles for regions like Huron County (Michigan).
  ```python
  county_boundary = gpd.read_file(county_shapefile).to_crs("EPSG:32617")
    masked_geometries = [geom for geom in county_boundary.geometry]

    with rasterio.open(tif_file_path) as src:
        crs = src.crs
        cropped_image, transform = mask(src, masked_geometries, crop=True)

        # Read bands (assuming band 1=B02, band 2=B03, band 3=B11)
        B02 = cropped_image[0].astype(np.float32)
        B03 = cropped_image[1].astype(np.float32)
        B11 = cropped_image[2].astype(np.float32)

    return B02, B03, B11
