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
### 1.1 Analysis
```python
# Load Dataset
rawcount_dataframe, sample_metadata_dataframe = load_dataset(dataset_accession, platform=None)
```
Load an RNA-seq dataset from GEO in the Python environment.

#### 1.1.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **dataset_accession** | **str** | GEO accession of the dataset to load to the Python environment.  For a complete list of the datasets available for download, please visit [http://amp.pharm.mssm.edu/archs4/](http://amp.pharm.mssm.edu/archs4/) |  |
| **platform** | **str** | GEO accession of the platform used to analyze the dataset.  If the dataset has been processed with multiple platforms, the first one will be selected by default. | [optional] |

#### 1.1.2 Output
| Name (default) | Type | Description |
| ---- | ---- | ----------- |
| **rawcount_dataframe** | **DataFrame** | DataFrame containing raw gene expression counts.  Columns are GEO samples, rows are gene symbols, values are raw counts as mapped by ARCHS4 ([http://amp.pharm.mssm.edu/archs4/](http://amp.pharm.mssm.edu/archs4/)). |
| **sample_metadata_dataframe** | **DataFrame** | Metadata describing sample properties.  Columns are metadata categories, rows are GEO sample IDs, values represent metadata values as annotated on GEO. |

<br>

## 2. Normalize Dataset (*normalize_dataset*)
### 2.1 Analysis
```python
# Normalize Dataset
normalized_expression_dataframe = normalize_dataset(rawcount_dataframe)
```
Normalize raw counts.

#### 2.1.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **rawcount_dataframe** | **DataFrame** | DataFrame containing raw gene expression counts, as provided by *load_dataset*. |  |
| **method** | **str** | String specifying the method to be used to normalize the dataset. Must be one of ```zscore```, ```vst```, ```voom```.|  |

#### 2.1.2 Output
| Name (default) | Type | Description |
| ---- | ---- | ----------- |
| **normalized_expression_dataframe** | **DataFrame** | Normalized expression DataFrame, with the same dimensions as the input DataFrame. |

<br>

## 3. Library Sizes (*library_size*)
### 3.1 Analysis
```python
# Get Library Sizes
library_sizes = library_size.get(rawcount_dataframe)
```
Calculate library sizes from the raw expression dataframe.

#### 3.1.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **rawcount_dataframe** | **DataFrame** | DataFrame containing raw gene expression counts, as provided by ```load_dataset```. |  |

#### 3.1.2 Output
| Name (default) | Type | Description |
| ---- | ---- | ----------- |
| **library_sizes** | **DataFrame** | DataFrame containing library sizes for each of the dataset's samples. |

### 3.2 Plots
```python
# Plot Library Sizes
library_size.plot(library_sizes)
```
Plot a histogram displaying library sizes for each sample in the RNA-seq dataset.

#### 3.2.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **library_sizes** | **DataFrame** | DataFrame containing library sizes for each of the dataset's samples, as provided by ```library_size.get()```. |  |

#### 3.2.2 Output
A histogram displaying library sizes for each sample in the RNA-seq dataset.

<br>

## 4. Principal Components Analysis (*pca*)
### 4.1 Analysis
```python
# Run PCA
pca_results = pca.run(expression_dataframe, axis='cols')
```
Perform a Principal Components Analysis (PCA) on an expression dataframe.

#### 4.1.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **expression_dataframe** | **DataFrame** | DataFrame containing gene expression data, with genes on rows and samples on columns. For optimal results, normalize the input data using ```normalize_dataset``` before running the analysis. |  |
| **axis** | **string** | Axis along which to perform the dimensionality reduction; must be one of ```rows``` or ```cols```.  Defaults to ```cols```. | [optional] |

#### 4.1.2 Output
| Name (default) | Type | Description |
| ---- | ---- | ----------- |
| **pca_results** | **dict** | Dictionary containing the following keys: <ul><li>asd</li><li>asd</li><li>asd</li></ul> |

### 4.2 Plots
```python
# Plot PCA
pca.plot(pca_results, dims=3, PCs=[1,2,3], color_by=None, color_type='auto', colors=['red', 'green', 'blue', 'yellow', 'orange', 'purple'], colorscale='Reds')
```
Plot a scatter plot displaying the results of the Principal Components Analysis.

#### 4.2.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **pca_results** | **dict** | Results generated by ```pca.run()```. |  |
| **dims** | **int** | Number of dimensions in the scatter plot; must be ```2``` or ```3```. Defaults to ```3```.| [optional] |
| **PCs** | **list** | List of integers indicating which Principal Components to plot on the x, y and z planes respectively. Defaults to ```[1, 2, 3]```.| [optional] |
| **color_by** | **list** | List of integers or strings to color points by. | [optional] |
| **color_type** | **str** | Method to color the points with, one of ```categorical```, ```continuous```, or ```auto```. ```categorical``` coloring will treat provided values as unique categories; ```continuous``` will treat values as continuous and will only work with numeric types; ```auto``` will automatically assign strings to categorical and numeric types to continuous. | [optional] |
| **colors** | **list** | List of colors to be used; only valid when ```color_type='categorical'```. | [optional] |
| **colorscale** | **str** | Name of the color scale to be used; only valid when ```color_type='continuous'```. | [optional] |

#### 4.2.2 Output
A scatter plot displaying the results of the Principal Components Analysis.

<br>

## 5. t-SNE Analysis (*tsne*)
### 5.1 Analysis
```python
# Run t-SNE
tsne_results = tsne.run(expression_dataframe, axis='cols')
```
Calculate library sizes from the raw expression dataframe.

#### 5.1.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **expression_dataframe** | **DataFrame** | DataFrame containing gene expression data, with genes on rows and samples on columns. For optimal results, normalize the input data using ```normalize_dataset``` before running the analysis. |  |
| **dims** | **int** | Number of dimensions to reduce the dataset to; must be ```2``` or ```3```. | [optional] |
| **axis** | **string** | Axis along which to perform the dimensionality reduction; must be one of ```rows``` or ```cols```.  Defaults to ```cols```. | [optional] |

#### 5.1.2 Output
| Name (default) | Type | Description |
| ---- | ---- | ----------- |
| **tsne_results** | **DataFrame** | DataFrame containing the embedding of the samples in the lower dimensionality space. |

### 5.2 Plots
```python
# Plot t-SNE
tsne.plot(tsne_results, dims=3, PCs=[1,2,3], color_by=None, color_type='auto', colors=['red', 'green', 'blue', 'yellow', 'orange', 'purple'], colorscale='Reds')
```
Plot a scatter plot displaying the results of the Principal Components Analysis.

#### 5.2.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **pca_results** | **dict** | Results generated by ```pca.run()```. |  |
| **dims** | **int** | Number of dimensions in the scatter plot; must be ```2``` or ```3```. Defaults to ```3```.| [optional] |
| **PCs** | **list** | List of integers indicating which Principal Components to plot on the x, y and z planes respectively. Defaults to ```[1, 2, 3]```.| [optional] |
| **color_by** | **list** | List of integers or strings to color points by. | [optional] |
| **color_type** | **str** | Method to color the points with, one of ```categorical```, ```continuous```, or ```auto```. ```categorical``` coloring will treat provided values as unique categories; ```continuous``` will treat values as continuous and will only work with numeric types; ```auto``` will automatically assign strings to categorical and numeric types to continuous. | [optional] |
| **colors** | **list** | List of colors to be used; only valid when ```color_type='categorical'```. | [optional] |
| **colorscale** | **str** | Name of the color scale to be used; only valid when ```color_type='continuous'```. | [optional] |

#### 5.2.2 Output
A scatter plot displaying the results of the Principal Components Analysis.

<br>

## 5. Clustergrammer Analysis (*clustergrammer*)
### 5.1 Display
```python
# Plot Clustergram
clustergrammer.display(expression_dataframe, )
```
Calculate library sizes from the raw expression dataframe.

#### 5.1.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **expression_dataframe** | **DataFrame** | DataFrame. |  |

#### 5.1.2 Output
| Name (default) | Type | Description |
| ---- | ---- | ----------- |
| **tsne_results** | **DataFrame** | DataFrame containing the embedding of the samples in the lower dimensionality space. |

<br>

## 5. Clustergrammer (*clustergrammer*)
### 5.1 Plots
```python
# Plot Clustergram
clustergrammer.plot(expression_dataframe, normalize=True, norm_axis='row', nr_genes=500)
```
Plot an interactive clustergram of the most highly variable genes in the dataset.

#### 5.1.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **expression_dataframe** | **DataFrame** | DataFrame containing gene expression data, with genes on rows and samples on columns. For optimal results, normalize the input data using ```normalize_dataset``` before running the analysis. |  |
| **normalize** | **bool** | Boolean value specifying whether to perform a Z-score normalization on the dataset. Defaults to ```True```. | [optional] |
| **norm_axis** | **string** | Axis along which to perform the Z-score normalization; must be one of ```row``` or ```col```.  Defaults to ```row```.| [optional] |
| **nr_genes** | **int** | Integer value specifying the number of genes, ranked by variance, to sample from the original dataset.  Fewer genes will make the visualization faster, but might impact the clustering negatively.  Defaults to ```500```.| [optional] |

#### 5.1.2 Output
An interactive clustergram displaying the expression levels of the most variably expressed genes within the dataset.

<br>

## 6. Differential Gene Expression (*signature*)
### 6.1 Analysis
```python
# Run Analysis
signature_dataframe = signature.get(rawcount_dataframe, control_samples, experimental_samples, method='limma')
```
Compute a differential gene expression signature given a raw count dataframe, control samples, and treated samples.

#### 6.1.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **rawcount_dataframe** | **DataFrame** | DataFrame containing raw gene expression counts, as provided by ```load_dataset```. |  |
| **control_samples** | **list** | List of sample IDs to be treated as controls in the differential expression analysis. |  |
| **experimental_samples** | **list** | List of sample IDs to be treated as experimental condition in the differential expression analysis. |  |
| **method** | **str** | Method used to perform the differential expression analysis.  Must be one of ```limma```, ```DESeq2``` or ```cd```.  Defaults to ```limma```. | [optional] |

#### 6.1.2 Output
| Name (default) | Type | Description |
| ---- | ---- | ----------- |
| **signature_dataframe** | **DataFrame** | DataFrame containing genes on rows, and differential expression statistics on the columns.  Columns vary according to the differential expression method selected. |

### 6.2 Plots
```python
# Plot Analysis
signature.display(signature_dataframe, display_type='volcano')
```
Display the results of the differential expression analysis.

#### 6.2.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **signature_dataframe** | **DataFrame** | DataFrame containing gene expression signature, generated from ```signature.get()```. |  |
| **display_type** | **string** | String specifying how to display the results; must be one of ```volcano```, ```ma``` or ```table```.  Defaults to ```volcano```. |  |

#### 6.2.2 Output
Display the results of the differential gene expression analysis with a selection of different methods.

<br>

## 7. Enrichr (*enrichr*)
### 7.1 Analysis
```python
# Run Enrichr
enrichr_results = enrichr.run(signature_dataframe, geneset_selection_method='nr_genes', nr_genes=500, pvalue_threshold=0.05, libraries=[])
```
Perform an enrichment analysis on a differential gene expression signature.

#### 7.1.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **signature_dataframe** | **DataFrame** | A differential gene expression signature DataFrame generated by ```signature.run()```. |  |
| **geneset_selection_method** | **str** | String specifying the method to use to extract the genesets to send to Enrichr; must be one of ```pvalue_threshold``` or ```nr_genes```.  Defaults to ```nr_genes```. | [optional] |
| **nr_genes** | **int** | An integer specifying the size of the geneset to be taken from each end of the gene expression signature.  Defaults to ```500```. | [optional] |
| **pvalue_threshold** | **double** | Numeric value specifying the minimum differential expression pvalue to extract the genesets to send to Enrichr.  Defaults to ```0.05```. | [optional] |
| **libraries** | **list** | List of Enrichr libraries to perform enrichment analysis on.  For a full list of available libraries, visit [http://amp.pharm.mssm.edu/Enrichr/#stats](http://amp.pharm.mssm.edu/Enrichr/#stats).  Defaults to ```['GO_Biological_Process_2017b', 'GO_Molecular_Function_2017b', 'ChEA_2016', 'KEA_2015', 'KEGG_2016']```. | [optional] |

#### 7.1.2 Output
| Name (default) | Type | Description |
| ---- | ---- | ----------- |
| **enrichr_results** | **dict** | Dictionary containins two keys, *upregulated* and *downregulated*.  Values are lists of gene symbols corresponding to the upregulated and downregulated genesets respectively. |

### 7.2 Plots
```python
# Display Enrichment Results
enrichr.display(enrichr_results)
```
Display the results of the enrichment analysis.

#### 7.2.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **display_type** | **str** | String specifying how to display the results; must be one of ```table``` or ```scatter```.  Defaults to ```table```. |  |

#### 7.2.2 Output
Display the results of the enrichment analysis with a selection of different methods.

<br>

## 8. L1000CDS<sup>2</sup> (*l1000cds2*)
### 8.1 Analysis
```python
# Run L1000CDS2
l1000cds2_results = l1000cds2.run(signature_dataframe, nr_genes=1000)
```
Perform a signature search using L1000CDS<sup>2</sup>.

#### 8.1.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **signature_dataframe** | **DataFrame** | A differential gene expression signature DataFrame generated by ```signature.run()```. |  |
| **nr_genes** | **int** | Number of genes to submit to the API.  Defaults to ```1000```. | [optional] |

#### 8.1.2 Output
| Name (default) | Type | Description |
| ---- | ---- | ----------- |
| **l1000cds2_results** | **DataFrame** | DataFrame containing the most similar or opposite gene expression signatures. |

### 8.2 Plots
```python
# Display L1000CDS2 Results
l1000cds2.display(l1000cds2_results)
```
Plot the results of the L1000CDS<sup>2</sup> analysis.

#### 8.2.1 Parameters
| Name | Type | Description | Notes |
| ---- | ---- | ----------- | ----- |
| **display_type** | **str** | String specifying how to display the results; must be one of ```table``` or ```barchart```.  Defaults to ```table```. |  |

#### 8.2.2 Output
Display the results of the L1000CDS<sup>2</sup> analysis with a selection of different methods.
