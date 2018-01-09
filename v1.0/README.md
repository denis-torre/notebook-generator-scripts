# Notebook Generator Scripts | Version 1.0
This repository contains the 1.0 release of the Notebook Generator Scripts.

## init.ipy
The *init.ipy* file contains commands used to initialize the Jupyter Notebook environment by loading useful Python and R libraries.


### Overview
This release includes the following features:
* Load *Python libraries* os, sys, pandas.
* Initialize *matplotlib and plotly* for use in the Jupyter environment.
* Load *R kernel* using rpy2 magic.
* Import *Scripts.py*, which contains Python scripts for data analysis.
* Load *Scripts.R*, which contains R scripts for data analysis.


## Scripts.py
The *Scripts.ipy* file contains Python scripts used to normalize and analyze RNA-seq datasets.

## 1. Load Dataset (*load_dataset*)
```python
# Load Dataset
rawcount_dataframe, sample_metadata_dataframe = load_dataset(dataset_accession, platform=None)
```
Load an RNA-seq dataset from GEO in the Python environment.

### Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **dataset_accession** | **str** | GEO accession of the dataset to load to the Python environment.  For a complete list of the datasets available for download, please visit [http://amp.pharm.mssm.edu/archs4/](http://amp.pharm.mssm.edu/archs4/) | [required] |
| **platform** | **str** | GEO accession of the platform used to analyze the dataset.  If the dataset has been processed with multiple platforms, the first one will be selected by default. | [optional] |

### Output
| Name (default) | Type | Description |
| ---- | ---- | ----------- |
| **rawcount_dataframe** | **DataFrame** | DataFrame containing raw gene expression counts.  Columns are GEO samples, rows are gene symbols, values are raw counts as mapped by ARCHS4 ([http://amp.pharm.mssm.edu/archs4/](http://amp.pharm.mssm.edu/archs4/)). |
| **sample_metadata_dataframe** | **DataFrame** | Metadata describing sample properties.  Columns are metadata categories, rows are GEO sample IDs, values represent metadata values as annotated on GEO. |