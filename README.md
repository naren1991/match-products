# Overview

This project aims at matching products based on attributes scraped from various online websites. Attributes include the name of the product, product description

# Executive summary

* The MinHash + LSH approach seems to be able to find matches, but requires improvement to reach an acceptable F1-score [This was not feasible in the time frame]:
    * PRECISION - **37.4 %**
    * RECALL - **52.1 %**
    * F1-SCORE - **43.5 %**
* The Levenshtein dsitance method is a naive one, and was used simply to gain some intuition on the data
* Other variants of LSH methods, as well as methods listed in future scope can be tried to improve accuracy
* Blocking can be carried out by BRAND to improve matching. Exploratory analysis shows that products are very unevenly distributed across brands

# Usage
* The *Product_Matching_Based_On_Text_Attributes.html* file in the root folder contains the results and details of execution.
* The *submission.csv* file in the *submission* folder contains the final matched products
* The executable Jupyter notebook contained in the `code` folder can be run to reproduce the results. Note - The notebook was created on JupyterLab using Python 3.6
* In order to run the notebook, the *dataset.jsonl.gz* file in the data folder needs to be extracted in the same folder first
* Intermediate datasets generated are provided as a zip, so that execution can be done for specific sections of the notebook

# Folder structure

The root folder of the repository contains the executed version of the Jupyter notebook as a *.html* file for easy viewing. Additionally, the repository contains the following folders:
* `code` - Contains the source code of the Jupyter notebook as a *.ipynb* file
* `data` - Contains the original json dataset, as well as intermediate datasets created during the course of execution
* `submission` - Contains the submission file in the required format, for the best model identified

# Methodology

## Data preprocessing 

Given that the data is mostly textual in nature and is in multiple languages, the following steps were carried out to pre-process:
* **Translation** - Names and descriptions of products were translated to English
* **Stop word removal** - Stop words were removed so that these noisy values do not affect the similarity score
* **Stemming and lemmatization** - Stemming and lemmatization was performed in order to map different forms of the same root word to the same value, so that the similarity calculation is enriched.

## Attributes

The following attributes were used to identify similarity between product pairs. These were selected based on an expectation of the ability of the attribute to distinguish between different products, in an efficient way, given the constraint of time.

* NAME
* DESCRIPTION
* BRAND

## Methods tried

Since tagged data of matched products is not provided, an unsupervised learning approach needs to be taken match the products. Additionally, given the somewhat large data size, considerations of speed of execution need to be made.

In this project, similarity based clustering methods have been tried in order to match products efficiently. The following methods were tried:

1. **MinHash + Locally Sensitive Hashing**
    * The *MinHash* part of this method assigns a numeric fingerprint to each string such that when the similarity between the numeric fingerprints are calculated, it approximates to the Jaccard simiilarity between the strings. This is done to improve computational efficiency by dealing with numbers
    * The *Locally Sensitive Hashing* part of this method is aimed dealing with a high number of comparisons. This typically happens in real use cases where a large number of records need to be matched. LSH is a form of quantization where data points are mapped to buckets such that they have a high probability of being similar. This improves computational efficiency with regard to number of comparisons to be performed
2. **Levenshtein Similarity + Thresholding**- This is a naive method which calculates the Levenshtein similarity between various product pairs based on specified string columns. Similarity across columns are aggregated into a total similarity. This total similarity is thresholded to identify matches. This method is useful to gain intuition  about the dataset, but fails when the number of comparisons become large, as it is computationally inefficient.

Note - The submission in this project is based on **Method 1**

## Future scope

Further optimization of the Jaccard similarity thresholds, as well as of the parameters of MinHash and LSH is possible, though this could not be completed in the given time frame.

Additionally, there is also further scope to improve the model by considering other attributes such as the various *SPECIFICATION* attributes.

There is huge scope for trials of other methods. Some of the methods that can be used are:

* **Other Similarity measures for MinHash + LSH, as well as blocking** - Apart from the Jaccard index, other similarity measures can be used, which could be suitable for a particular type of problem/ dataset. Additionally, blocking can be used to create chunks for comparison, improving computational efficiency.
* **Other Vector Quantization Methods** - There exist a wide range of variants of Vector Quantization methods which can be applied to the above problem.
* **Graph-based entity resolution** - In this approach a graph is constructed, with each node as a record, with the relationships defined by various similarity metrics. Then, the graph is clustered to identify communities, which indicate matched records. The advantage of this method is that it can use semantic relational similarities from the structure of the graph itself to enrich matching.

# Supplementary notes & known issues

* The translation is done using a fork of the `googletrans` project which can be found here:
    * This project has a known issue - Sometimes the Google API requests fail, and the translation is not successful
* The parsed json file is available as a csv in the `data` folder in order to save on loading & parsing of raw data


