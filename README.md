﻿# Scenario clustering

Scenario Clustering is a graduation design completed at the The Institute for Information Processing Technologies in Karlsruhe institute of Technology. It is written in Python and the Autoencoder part is powered by the [Tensorflow](https://github.com/tensorflow/tensorflow) deep learning framework.

## Background

Today a large number of automated driving functions is available for most vehicles. While enjoying the convenience of these functions, reducing the individual risk for driver and guarantee the overall safety on the public road stay a big challenge. For this purpose, witnessed tremendous efforts dedicated to develop tools for the testing of automated driving functions. However, the identification of test cases creates difficulties. On the one hand, the required test coverage for safeguarding automated driving should be guaranteed, on
the other hand, the similar or redundant test cases are expected to be reduced so as to achieve test efficiency and minimize the expense.
In this connection, machine learning as a revolutionary technique is to applied to discover useful knowledge from test data. Clustering is the one of the techniques for the purpose of finding hidden patterns or similar groups in data, and another significant machine learning technology is autoencoder which is a special kind of neural network, and can be used to learn the feature of data and reconstruct it.
This study penetrates different approaches in the state-of-the-art accomplishments in the scenario definition and meta description of scenarios. Based on a suitable concept for meta description of the scenario, several clustering algorithms will be implemented to achieve an exploratory insight of the scenario data generated in this work. Besides, auto-encoder should be applied to a scenario database to figure out the uniqueness of the external scenarios concerning the database. Ultimately, as a selecting tool, the autoencoder will be evaluated if it can take over the tasks like extending scenario dataset under a certain degree of scenario similarity.
 
## Introduction
The complete work include three parts: 
- The data precessing pipeline.
- The Clustering implementation.
- The Autoencoder implementation.
- 
### The Data
Because of the lack of data from the vehicles on the real road. We use the data from the simulation software CarMaker.
The Raw data form the Carmaker look like this:
![raw](https://raw.githubusercontent.com/hjynick/Scenario-clustering/master/pic/raw.JPG)
The first three columns indicate the time steps and speed information of ego car. For ’ego.long’, ’-1’, ’0’, ’1’ means ’decelerate’, ’constant tempo’ and ’accelerate’ respectively. For ’ego.lat’, ’-1’, ’0’, ’1’ means ’left lane change’, ’no lane change’ and ’right lane change’ respectively. The following columns are speed and state information for all the object vehicles. For example, ’0.long’ and ’0.lat’ provide the speed information like ego car, and ’0st.long’ and ’0st.lat’ indicate the position relative to the ego in our concept. Here should be noted that state ’-99’ indicates ’not detected’, which means that this vehicle has not appeared within the 205m range of the ego car.

### The Concept
Each of the nine grids surrounding the ego-vehicle is described with a
2 digits tuple. The first digit with -1, 0, 1 indicates respectively the front, the same level and the behind position, and the second digit with -1, 0, 1 indicates respectively the left lane, the same lane and the right lane positions.
![Concept](https://raw.githubusercontent.com/hjynick/Scenario-clustering/master/pic/concept.JPG)


Additional Scenario Concept are more complex and include more items, they can be difficult implemented. There are not a clear solution sofar. For more details about these models, please see [References](#references) below.
# Implementation

## Environment Setting
Our code requires:
-   Python 3

We recommend to use Anaconda to integrate all the needed package, the following package are needed:
- Jupyter notebook
- Keras
- Matplotlib
- Numpy
- Pandas
- Qtpy
- Scipy
- scikit-learn
- Tensorflow

## Data processing
The [Pipeline]([https://github.com/hjynick/Scenario-clustering/blob/master/Code/Python_dataprocessing%20pipeline.ipynb](https://github.com/hjynick/Scenario-clustering/blob/master/Code/Python_dataprocessing%20pipeline.ipynb)) describes the whole process of the data preprocessing.

In the context of out meta-description concept, the format of the scenario should be a sequence what ever how lang it is. So in this consideration, one scenario should be one row of a matrix. The goal of the subsection is to extract the scenario data from the Raw data simulated with CarMaker in last section and return the output in form of matrix. For example, 10 1-state scenario should be a 10*7 matrix.

The [CSV]([https://github.com/hjynick/Scenario-clustering/blob/master/Data/TestRun/TestRun1/ground_truth_label0.csv](https://github.com/hjynick/Scenario-clustering/blob/master/Data/TestRun/TestRun1/ground_truth_label0.csv)) Data is a example of the input data. and the [TXT](https://github.com/hjynick/Scenario-clustering/blob/master/Data/TestRun1_1st.txt) data is the expected form of the output data under our consideration.

It should be noted that the loaded file address needs to be changed when it is running.
## Clustering
The ultimate goal of Scenario clustering is to find the similar scenario in test cases and then reduce the testing cost for automated vehicles. In general, identifying redundant test data in the complete data pool is difficult, since it depends on many test items. Which item is not relevant to the existing cases and which offers new test content, these question must be answered. But in our case, we do not consider all the items that can appear in test case, only an aspect of the test content will be focused, namely, traffic elements around the ego vehicle.

K-Means and the DBSCAN will be implemented with different combinations of hyper-parameters, which the hyper-parameter of K-Means is the number of clusters "k" and of DBSCAN and the neighborhood size "eps" and the minimal number of samples in each cluster "minsamples".

A example of Kmeans Clustering can be seen as this [1 state scenario using K-means](https://github.com/hjynick/Scenario-clustering/blob/master/Code/Clustering_kmeans_1st.ipynb) and the example of DBSCAN Clustering can be seen as this [1 state scenario using DBSCAN]([https://github.com/hjynick/Scenario-clustering/blob/master/Code/Clustering%20DBSCAN1st.ipynb](https://github.com/hjynick/Scenario-clustering/blob/master/Code/Clustering%20DBSCAN1st.ipynb)).

## Autoencoder
In order to achieve a complete test coverage for automated vehicles, test scenario with new features and special content should be added to the existing test scenario data pool. For this purpose, first the unknown scenario with regard to the existing scenarios should
be detected, at the same time, scenarios that have a certain similarity degree to the existing scenarios should be filtered and abandoned.

## Update

- 4/2018: Support Group Normalization - see [`GN/README.md`](./projects/GN/README.md)

## License

Detectron is released under the [Apache 2.0 license](https://github.com/facebookresearch/detectron/blob/master/LICENSE). See the [NOTICE](https://github.com/facebookresearch/detectron/blob/master/NOTICE) file for additional details.



## Model Zoo and Baselines

We provide a large set of baseline results and trained models available for download in the [Detectron Model Zoo](MODEL_ZOO.md).

## Installation

Please find installation instructions for Caffe2 and Detectron in [`INSTALL.md`](INSTALL.md).

## Quick Start: Using Detectron

After installation, please see [`GETTING_STARTED.md`](GETTING_STARTED.md) for brief tutorials covering inference and training with Detectron.

## Getting Help

To start, please check the [troubleshooting](INSTALL.md#troubleshooting) section of our installation instructions as well as our [FAQ](FAQ.md). If you couldn't find help there, try searching our GitHub issues. We intend the issues page to be a forum in which the community collectively troubleshoots problems.

If bugs are found, **we appreciate pull requests** (including adding Q&A's to `FAQ.md` and improving our installation instructions and troubleshooting documents). Please see [CONTRIBUTING.md](CONTRIBUTING.md) for more information about contributing to Detectron.

## References

- [Data Distillation: Towards Omni-Supervised Learning](https://arxiv.org/abs/1712.04440).
  Ilija Radosavovic, Piotr Dollár, Ross Girshick, Georgia Gkioxari, and Kaiming He.
  Tech report, arXiv, Dec. 2017.
