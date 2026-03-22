# SAR-Optical Fusion for Construction & Demolition Waste Mapping

Multi-sensor satellite-based classification of Construction and Demolition Waste (CDW) using fused Sentinel-1 SAR and Sentinel-2 optical data with machine learning.

## Overview

This project implements a binary classification pipeline (CDW vs Other) by combining Sentinel-1 SAR backscatter with Sentinel-2 optical bands and spectral indices. The 8-band composite image is used to train and evaluate multiple classifiers for automated CDW geolocation from satellite imagery.

## Input Data

### Multi-Band Composite (8 bands, 10 m resolution)

| Band | Source | Description |
|------|--------|-------------|
| VV | Sentinel-1 | Co-polarisation backscatter |
| VH | Sentinel-1 | Cross-polarisation backscatter |
| B2 | Sentinel-2 | Blue |
| B3 | Sentinel-2 | Green |
| B4 | Sentinel-2 | Red |
| B8 | Sentinel-2 | NIR |
| NDVI | Derived | Normalized Difference Vegetation Index |
| SAVI | Derived | Soil Adjusted Vegetation Index |

The Analysis Ready Data (ARD) was pre-processed using Python and openEO, with NDVI and SAVI indices computed and appended as additional bands.

### Ground Truth

Manually labelled polygons (shapefile) with two classes: `CDW` and `Other`. Labels were created in QGIS based on visual interpretation and field knowledge.

## Methodology

1. **Pixel extraction** — Raster values extracted per labelled polygon using rasterio masking
2. **Feature scaling** — QuantileTransformer for normalisation
3. **Classification** — Multiple classifiers evaluated via stratified cross-validation:
   - Random Forest
   - K-Nearest Neighbours (KNN)
   - Support Vector Machine (SVM)
   - Perceptron
   - Multi-Layer Perceptron (MLP)
4. **Hyperparameter tuning** — GridSearchCV with stratified k-fold
5. **Evaluation** — Confusion matrix, accuracy, per-class precision and recall

## Known Limitations

- **Class imbalance**: CDW samples are very limited (typically 3–4 piles of < 20 pixels each), leading to lower precision/recall for the CDW class
- **Limited CDW variety**: Training data lacks diversity in CDW material types
- **Resolution constraint**: Sentinel data at 10 m may not resolve small CDW deposits
- Techniques such as SMOTE or class-weighted loss could improve results

## Repository Structure

```
sar-optical-cdw-mapping/
├── README.md
├── .gitignore
├── notebooks/
│   └── 01_CDW_Classification.ipynb
└── data/                  # Not tracked
```

## Requirements

- Python 3.9+
- rasterio, geopandas, numpy, pandas, matplotlib
- scikit-learn

## Author

Nikos Kordalis — [GitHub](https://github.com/nikkordalis)
