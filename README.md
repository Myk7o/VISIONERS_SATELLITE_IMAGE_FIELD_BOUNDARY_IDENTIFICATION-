# VISIONERS_SATELLITE_IMAGE_FIELD_BOUNDARY_IDENTIFICATION-

## Project Overview

This project addresses challenges in identifying field boundaries using satellite data. These methods are applied in the agricultural sector to improve commodity price forecasting, particularly for crops such as wheat, soybeans, and corn. Our approach leverages Sentinel-2 satellite images and computer vision algorithms to process the data and meet several critical requirements.

## Objective

1. **Field Boundary Identification (FBI):** Identify boundaries of agricultural fields in Sentinel-2 satellite images.
2. **Field Size Calculation (FAC):** Calculate field acreage based on identified boundaries.


## Problem Definition

Identifying field boundaries and calculating acreage using remote sensing images is critical for forecasting crop yields. However, this task is complex due to:

The algorithm must work in all conditions, using the limited available images (1-5 usable images per month due to cloud cover).

## Approach

Sentinel-2 satellite data, stored in AWS S3, will be used to classify cropsand analyze crop growth progression. We will explore multiple algorithms and document the advantages and disadvantages of each to meet the project's goals.

## Dataset

- **Satellite Data:** Sentinel-2 Cloud-Optimized GeoTIFFs from the European Space Agency (ESA), available through AWS S3.

- **Sponsor:** Center for Air Transportation Systems Research @ George Mason University  
