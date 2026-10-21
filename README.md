Supplementary Software 1 - analysis code
=========================================
Python 3.13 with numpy 2.5, pandas 3.0, scipy 1.18, rasterio 1.5, shapely 2.2,
matplotlib 3.11 and adjustText.

Place in this folder:
  rasters/                                    classified suitability rasters
                                              (<scenario>_continuous_<crop>_classified.tif;
                                              scenario = current, best (SSP1-2.6),
                                              moderate (SSP2-4.5), worst (SSP5-8.5);
                                              classes 0-3, 255 = no data)
  ne50.geojson                                Natural Earth 1:50m admin-0 countries
  Supplementary_Data_1_suitability_areas.xlsx Supplementary Data 1

Run in this order:
  1. python areas.py   class areas (Africa and global) and class transitions -> areas.pkl
  2. python swap.py    loss zones, swap coverage, candidate contribution, depth -> swap.pkl
  3. python stats.py   group comparison, sensitivity, ranking tests, quadrants -> stats.pkl,
                       crop_metrics_37.csv
  4. python figs.py    Figures 1-5 -> figs/

Shared settings (file names, scenarios, Africa mask, cell areas) are in common.py.
Random seeds: permutation test seed = 1 (20,000 permutations); figure jitter seed = 7.


# Supplementary Software 2: Analysis Code

## Overview

This folder contains Jupyter notebooks supporting the extraction and preparation of climate data, exploratory data analysis (EDA), fuzzy climatic-suitability modelling, output plotting, raster masking, and occurrence-based model validation for crop suitability analysis.

The notebooks are provided as analysis code. They are not a self-contained application: the input occurrence tables, climate rasters, crop-mask raster, and output folders must be available and the file paths must be configured for the computer on which the notebooks are run.

## Contents

### 1. `Data_extraction_EDA_Model_implementation.ipynb`
Main workflow for data extraction and analysis. It includes library setup, raster/data import and stacking, EDA of bioclimatic variables, and crop-specific fuzzy suitability modelling. The notebook contains crop-specific sections, including descriptive statistics, correlations and hierarchical clustering, and raster-based suitability calculations.

### 2. `fuzzy_output_plots.ipynb`
Calculates and plots fuzzy suitability scores for selected crop occurrence/EDA records. The notebook includes crop-specific sections (for example, taro, sorghum, African mango, baobab, spider plant, teff, African yam, and jute mallow) and can export ranked suitability plots and result tables. Check each section for its selected BIO variables and aggregation operator; these settings may differ between crops.

### 3. `Crop_Masking.ipynb`
Applies a spatial mask to classified suitability rasters. The notebook uses raster processing/reprojection and window-based processing to produce masked raster outputs. The crop-mask raster and input/output directories must be configured before running.

### 4. `Model_validation.ipynb`
Overlays occurrence records with classified suitability rasters and calculates occurrence-recovery results. It reads occurrence records from an Excel file, searches the raster directory for relevant classified rasters, and writes a results workbook. Review the crop/species matching, coordinate columns, raster naming conventions, NoData handling, and the definition of a recovered record before interpreting the output.

## Requirements

The notebooks use Python and common scientific/geospatial libraries. Depending on the cells executed, the main dependencies include:

- `numpy`
- `pandas`
- `matplotlib`
- `seaborn`
- `scipy`
- `scikit-learn`
- `scikit-fuzzy`
- `rasterio`
- `geopandas`
- `numba`
- `tqdm`
- `openpyxl`
- `networkx`

Install only the packages needed for the notebook sections you intend to run. For example, in a terminal associated with the selected Python environment:

```bash
python -m pip install numpy pandas matplotlib seaborn scipy scikit-learn scikit-fuzzy rasterio geopandas numba tqdm openpyxl networkx
```

Geospatial packages such as `rasterio` and `geopandas` can depend on platform-specific binary libraries. If installation fails, use a compatible Python environment and follow the package installation instructions for your operating system.

## Inputs and path configuration

The notebooks currently contain machine-specific paths, including Windows paths such as `D:\\underutilized_crops\\...` and macOS paths under `/Volumes/...`. These paths will not work on another computer unless the same folders exist.

Before running a notebook:

1. Extract the ZIP archive to a working folder.
2. Open the notebook in JupyterLab, Jupyter Notebook, or another compatible notebook environment.
3. Locate the cells labelled `PATHS`, `FILE PATHS`, `INPUT_FOLDER`, `OUTPUT_FOLDER`, or similar.
4. Replace the example/current paths with the actual locations of your input files and output folders.
5. Confirm that the input files exist and that the output directory is writable.
6. Run the notebook cells in order, checking intermediate outputs before proceeding.

The required inputs vary by notebook and may include climate/bioclimatic rasters, occurrence-record Excel workbooks, classified suitability rasters, and a crop-mask raster. These data files are not included in this code archive unless supplied separately.

## General workflow

A typical analysis sequence is:

1. **Prepare/extract data:** use `Data_extraction_EDA_Model_implementation.ipynb` to load the required climate data and occurrence/EDA tables and conduct exploratory analysis.
2. **Generate suitability outputs:** run the relevant crop-specific fuzzy modelling sections to produce continuous or classified suitability rasters.
3. **Apply the crop mask if required:** use `Crop_Masking.ipynb` on the relevant rasters.
4. **Plot occurrence-based fuzzy scores:** use `fuzzy_output_plots.ipynb` for the crop-specific EDA suitability plots and result tables.
5. **Evaluate occurrence recovery:** use `Model_validation.ipynb` to compare occurrence points with the relevant classified rasters.

The exact order may vary depending on which outputs already exist and which analysis is being reproduced.

## Method and interpretation notes

- Fuzzy suitability scores represent modelled suitability under the membership functions, selected bioclimatic variables, and aggregation rules implemented in the relevant notebook. They should not automatically be interpreted as calibrated probabilities of crop presence.
- Occurrence recovery measures whether a test occurrence meets the raster suitability/recovery criterion. A high recovery rate alone does not establish specificity, discrimination against background environments, or causal climatic relationships.
- Occurrence records may reflect uneven sampling and may not consistently distinguish cultivated, wild, or irrigated occurrences. Do not treat locations without occurrence records as confirmed climatic absences unless independent evidence supports that interpretation.
- Check the suitability threshold and NoData treatment used in the validation code. Record counts, cleaning filters, train/test separation, and any spatial thinning or bias correction should be documented based on the procedures actually applied.
- For reproducibility, record the Python version, package versions, input-data sources and dates, parameter choices, and output filenames.

## Reproducibility and troubleshooting

- Run cells in sequence, especially where later cells rely on variables created earlier.
- If a `FileNotFoundError` occurs, check the path and filename, including capitalization and file extensions.
- If a crop selection returns no rows, inspect the available values in the crop/species column; common names and scientific names may differ.
- If a raster overlay gives unexpected recovery results, verify that the occurrence coordinates are longitude/latitude in the expected order, that the raster CRS is handled correctly, and that NoData values are excluded.
- Large rasters may require substantial memory and processing time. Where tile/window-based processing is implemented, retain that approach for large datasets.
- The notebooks have not been guaranteed to run unchanged on every system. Review paths, input schemas, package compatibility, and notebook outputs before using the results in a publication.




