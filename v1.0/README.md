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
The *Scripts.py* file contains Python scripts used to normalize and analyze RNA-seq datasets.

## 1. Load Dataset (*load_dataset*)
```python
# Load Dataset
rawcount_dataframe, sample_metadata_dataframe = load_dataset(dataset_accession, platform=None)
```
Load an RNA-seq dataset from GEO in the Python environment.

#### Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **dataset_accession** | **str** | GEO accession of the dataset to load to the Python environment.  For a complete list of the datasets available for download, please visit [http://amp.pharm.mssm.edu/archs4/](http://amp.pharm.mssm.edu/archs4/) |  |
| **platform** | **str** | GEO accession of the platform used to analyze the dataset.  If the dataset has been processed with multiple platforms, the first one will be selected by default. | [optional] |

#### Output
| Name (default) | Type | Description |
| ---- | ---- | ----------- |
| **rawcount_dataframe** | **DataFrame** | DataFrame containing raw gene expression counts.  Columns are GEO samples, rows are gene symbols, values are raw counts as mapped by ARCHS4 ([http://amp.pharm.mssm.edu/archs4/](http://amp.pharm.mssm.edu/archs4/)). |
| **sample_metadata_dataframe** | **DataFrame** | Metadata describing sample properties.  Columns are metadata categories, rows are GEO sample IDs, values represent metadata values as annotated on GEO. |


## 2. Normalize Dataset (*normalize_dataset*)
```python
# Normalize Dataset
normalized_expression_dataframe = normalize_dataset(rawcount_dataframe)
```
Normalize raw counts.

#### Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **rawcount_dataframe** | **DataFrame** | DataFrame containing raw gene expression counts, as provided by *load_dataset*. |  |
| **method** | **str** | String specifying the method to be used to normalize the dataset. Must be one of ```zscore```, ```vst```, ```voom```.|  |

#### Output
| Name (default) | Type | Description |
| ---- | ---- | ----------- |
| **normalized_expression_dataframe** | **DataFrame** | Normalized expression DataFrame, with the same dimensions as the input DataFrame. |


## 3. Library Sizes (*library_size*)
### 3.1 Analysis
```python
# Get Library Sizes
library_sizes = library_size.get(rawcount_dataframe)
```
Calculate library sizes from the raw expression dataframe.

#### Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **rawcount_dataframe** | **DataFrame** | DataFrame containing raw gene expression counts, as provided by ```load_dataset```. |  |

#### Output
| Name (default) | Type | Description |
| ---- | ---- | ----------- |
| **library_sizes** | **DataFrame** | DataFrame containing library sizes for each of the dataset's samples. |

### 3.2 Plots
```python
# Plot Library Sizes
library_size.plot(library_sizes)
```
Plot a histogram displaying library sizes for each sample in the RNA-seq dataset.

#### Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **library_sizes** | **DataFrame** | DataFrame containing library sizes for each of the dataset's samples, as provided by ```library_size.get()```. |  |

#### Output
A histogram displaying library sizes for each sample in the RNA-seq dataset.


## 4. Principal Components Analysis (*pca*)
### 4.1 Analysis
```python
# Run PCA
pca_results = pca.run(expression_dataframe)
```
Calculate library sizes from the raw expression dataframe.

#### Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **expression_dataframe** | **DataFrame** | DataFrame containing gene expression data, with genes on rows and samples on columns. For optimal results, normalize the input data using ```normalize_dataset``` before running the analysis. |  |

#### Output
| Name (default) | Type | Description |
| ---- | ---- | ----------- |
| **pca_results** | **dict** | Dictionary containing the following keys: <ul><li>asd</li><li>asd</li><li>asd</li></ul> |

### 4.2 Plots
```python
# Plot PCA
pca.plot(pca_results, dims=3, PCs=[1,2,3], color_by=None, color_type='auto', colors=['red', 'green', 'blue', 'yellow', 'orange', 'purple'], colorscale='Reds')
```
Plot a scatter plot displaying the results of the Principal Components Analysis.

#### Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **pca_results** | **dict** | Results generated by ```pca.run()```. |  |
| **dims** | **int** | Number of dimensions in the scatter plot; must be ```2``` or ```3```. Defaults to ```3```.| [optional] |
| **PCs** | **list** | List of integers indicating which Principal Components to plot on the x, y and z planes respectively. Defaults to ```[1, 2, 3]```.| [optional] |
| **color_by** | **list** | List of integers or strings to color points by. | [optional] |
| **color_type** | **str** | Method to color the points with, one of ```categorical```, ```continuous```, or ```auto```. ```categorical``` coloring will treat provided values as unique categories; ```continuous``` will treat values as continuous and will only work with numeric types; ```auto``` will automatically assign strings to categorical and numeric types to continuous. | [optional] |
| **colors** | **list** | List of colors to be used; only valid when ```color_type='categorical'```. | [optional] |
| **colorscale** | **str** | Name of the color scale to be used; only valid when ```color_type='continuous'```. | [optional] |

#### Output
A scatter plot displaying the results of the Principal Components Analysis.

