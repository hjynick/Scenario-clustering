# Scenario clustering

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

The [CSV](https://github.com/hjynick/Scenario-clustering/blob/master/Data/TestRun/TestRun1/ground_truth_label0.csv) Data is a example of the input data. and the [TXT](https://github.com/hjynick/Scenario-clustering/blob/master/Data/TestRun1_1st.txt) data is the expected form of the output data under our consideration.

It should be noted that the loaded file address needs to be changed when it is running.
## Clustering
The ultimate goal of Scenario clustering is to find the similar scenario in test cases and then reduce the testing cost for automated vehicles. In general, identifying redundant test data in the complete data pool is difficult, since it depends on many test items. Which item is not relevant to the existing cases and which offers new test content, these question must be answered. But in our case, we do not consider all the items that can appear in test case, only an aspect of the test content will be focused, namely, traffic elements around the ego vehicle.

K-Means and the DBSCAN will be implemented with different combinations of hyper-parameters, which the hyper-parameter of K-Means is the number of clusters "k" and of DBSCAN and the neighborhood size "eps" and the minimal number of samples in each cluster "minsamples".

A example of Kmeans Clustering can be seen as this [1 state scenario using K-means](https://github.com/hjynick/Scenario-clustering/blob/master/Code/Clustering_kmeans_1st.ipynb) and the example of DBSCAN Clustering can be seen as this [1 state scenario using DBSCAN]([https://github.com/hjynick/Scenario-clustering/blob/master/Code/Clustering%20DBSCAN1st.ipynb](https://github.com/hjynick/Scenario-clustering/blob/master/Code/Clustering%20DBSCAN1st.ipynb)).

## Autoencoder
In order to achieve a complete test coverage for automated vehicles, test scenario with new features and special content should be added to the existing test scenario data pool. For this purpose, first the unknown scenario with regard to the existing scenarios should
be detected, at the same time, scenarios that have a certain similarity degree to the existing scenarios should be filtered and abandoned.
To assess the possibility of this approach, we conducted a pilot experiment on the
existing dataset based on the scenario concept in this thesis. In this experiment, the
unknown data that has a different feature with respect to the existing dataset need to
be detected, this is where auto-encoder comes in. The scenario data from [TestRun 1](https://github.com/hjynick/Scenario-clustering/tree/master/Data/TestRun/TestRun1)
in CarMaker is set as the training data and the data from the [TestRun 2](https://github.com/hjynick/Scenario-clustering/tree/master/Data/TestRun/TestRun2) is used to be
detected. To the best of our knowledge, this is the first time that a neural network model is
applied using auto-encoder to represent driving scenario features for anomaly prediction.
![pipeline](https://raw.githubusercontent.com/hjynick/Scenario-clustering/master/pic/pipelinea.jpg)

The trained Model([1-state](https://github.com/hjynick/Scenario-clustering/tree/master/Code/AEmodel_1st), [2-state](https://github.com/hjynick/Scenario-clustering/tree/master/Code/AEmodel_2st), [3-state](https://github.com/hjynick/Scenario-clustering/tree/master/Code/AEmodel_3st), [4-state](https://github.com/hjynick/Scenario-clustering/tree/master/Code/AEmodel_4st))

## License

Detectron is released under the [MIT license](https://github.com/hjynick/Scenario-clustering/blob/master/LICENSE.md). 



## Getting Help

If you have questions or require any help, I am gladly at your disposal using the email listed below.
jingyu.he@web.de
## References
- [LANGNER, Jacob ; BACH, Johannes ; RIES, Lennart ; OTTEN, Stefan ; HOLZ, Marc ;SAX, Eric: Estimating the Uniqueness of Test Scenarios derived from RecordedReal-World-Driving-Data using Autoencoders. In:2018 IEEE Intelligent VehiclesSymposium (IV). Changshu, Suzhou, China, 2018. – ISBN 9781538644522, S.1860–1866](https://ieeexplore.ieee.org/document/8500464/)
- [ULBRICH, Simon ; MENZEL, Till ; RESCHKA, Andreas ; SCHULDT, Fabian ; MAURER,Markus: Defining and Substantiating the Terms Scene , Situation , and Scenario forAutomated Driving. In:2015 IEEE 18th International Conference on IntelligentTransportation Systems, 2015, S. 982–988](https://www.ifr.ing.tu-bs.de/static/files/forschung/papers/download_pdf.php?id=859)
- [SCHULDT, Fabian ; MAURER, Markus ; MENZEL, Till: Eine Methode fur dieZuordnung von Testf"allen für automatisierte Fahrfunktionen auf X-in-the-LoopVerfahren im modularen virtuellen Testbaukasten. In:10. WorkshopFahrerassistenzsysteme.Walting, Deutschland, 2015, S. 171–182](https://www.ifr.ing.tu-bs.de/static/files/forschung/papers/download_pdf.php?id=860)
- [GEYER, Sebastian ; BALTZER, Marcel ; FRANZ, Benjamin ; HAKULI, Stephan ;KAUER, Michaela ; KIENLE, Martin ; MEIER, Sonja ; WEISSGERBER, Thomas ;BENGLER, Klaus ; BRUDER, Ralph ; FLEMISCH, Frank ; WINNER, Hermann:Concept and development of a unified ontology for generating test and use-casecatalogues for assisted and automated vehicle guidance. In:IET IntelligentTransport SystemsBd. 8, 2014. – ISSN 1751–956X, 183–189](https://ieeexplore.ieee.org/document/6818481)
- [MENZEL, Till ; BAGSCHIK, Gerrit ; MAURER; MARKUS: Scenarios for Development,Test and Validation of Automated Vehicles. In:IEEE Intelligent VehiclesSymposium, ProceedingsBd. 2018-June, 2018. – ISBN 9781538644522, S.1821–1827](https://arxiv.org/abs/1801.08598)

