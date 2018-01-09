# Notebook Generator Scripts - Version 1.0
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

This release includes the following scripts:

### Load ARCHS4 Dataset (load_dataset)
```python
rawcount_dataframe, sample_metadata_dataframe = load_dataset(dataset_accession, platform=None)
```