Cloudless Raster Mosaic — Satellite Tile Merging Pipeline
============================================================

This repository provides a complete workflow for generating a **cloudless, georeferenced satellite mosaic** from multiple raster tiles.The pipeline includes CRS standardization, cloud masking, mosaicing using GDAL VRT streaming, preview generation, and result validation.

The entire workflow is designed for **Google Colab / Kaggle** and works efficiently even with large datasets.

Features
-----------

*   Automatic dataset download & extraction
    
*   Metadata inspection for all tiles
    
*   Reprojection + resampling to uniform CRS
    
*   Brightness–saturation cloud masking
    
*   RAM-safe mosaic creation using GDAL VRT
    
*   Preview generation (PNG + 8-bit GeoTIFF)
    
*   Metadata CSV and mosaic validation report
    

Libraries Used
=================

### ✔ System / GDAL  
- `gdal-bin`  
- `libgdal-dev`

### ✔ Python Libraries  
- `rasterio`  
- `rioxarray`  
- `geopandas`  
- `numpy`  
- `matplotlib`  
- `PIL (Pillow)`  
- `rasterstats`  
- `tqdm`  
- `scipy` (for morphological cloud-mask operations)  
- `subprocess` (for GDAL VRT mosaic)

---

    

Steps to Run
===============

1️⃣ Install Dependencies
------------------------

```bash
!apt-get update -y && apt-get install -y gdal-bin libgdal-dev
!pip install rasterio rioxarray geopandas matplotlib pillow numpy rasterstats tqdm
```

2️⃣ Download & Extract Dataset
------------------------------

Dataset URL: https://objectstore.e2enetworks.net/btechtasksampledata/data.zip

Extracted to:

`   /content/raster_mosaic_task/   `

3️⃣ List and Validate Tiles
---------------------------

*   Recursively finds all .tif/.tiff files
    
*   Prints CRS, pixel resolution, bounds, and metadata
    
*   Previews the first image automatically (RGB or grayscale)
    

4️⃣ Standardize Tiles (Reproject + Resample)
--------------------------------------------

All tiles are converted to a unified grid:

PropertyValue -- **CRS** EPSG:3857,**Resolution**1.21 meters, **Data Type** uint8

Outputs stored in:

`   standardized_tiles/   `

5️⃣ Cloud Masking
-----------------

Clouds are detected using:

*   Mean brightness
    
*   Saturation threshold
    
*   Very high reflectance detection
    
*   Morphological cleaning using:
    
    *   binary\_opening (3×3)
        
    *   binary\_closing (5×5)
        

Masked pixels → set to **NoData (0)**.

Outputs stored in:

`   cloudmasked_tiles/   `

6️⃣ RAM-Safe Mosaic Generation (GDAL)
-------------------------------------

Uses GDAL VRT streaming:

1.  gdalbuildvrt → creates virtual mosaic
    
2.  gdal\_translate → writes final compressed GeoTIFF
    

Output:

`   cloudless_mosaic.tif   `

Properties:

*   LZW compression
    
*   Tiled
    
*   BigTIFF-safe
    
*   Unified CRS & resolution
    

7️⃣ Generate Preview Outputs
----------------------------

###  PNG Preview

*   Max 1200×1200
    
*   Percentile stretch (2–98%)
    

### 8-bit GeoTIFF

Optional lightweight visualization.

Files:

`   cloudless_mosaic_preview.png  cloudless_mosaic_8bit.tif   `

8️⃣ Metadata & Validation Reports
---------------------------------

### ✔ Tile Metadata CSV

tiles\_metadata.csv includes:

*   Tile name
    
*   CRS
    
*   Width, height
    
*   Pixel size
    
*   Bands
    
*   Data type
    
*   NoData value
    

### ✔ Mosaic Validation Report

mosaic\_report.txt includes:

*   Final mosaic dims
    
*   CRS
    
*   Bounds
    
*   Total NoData pixels
    
*   Band info
    
*   Pixel resolution
    

Summary of Results
=====================

### ✔ All input tiles successfully processed

### ✔ Reprojected to a unified CRS (EPSG:3857 @ 1.21m)

### ✔ Cloud-removal successful using improved RGB mask

### ✔ Final cloudless mosaic generated via GDAL

### ✔ PNG + 8-bit TIFF previews created

### ✔ Metadata CSV and validation reports exported

Final output files:

`   cloudless_mosaic.tif  cloudless_mosaic_preview.png  tiles_metadata.csv  mosaic_report.txt   `
