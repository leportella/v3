---
layout: post
title: "Machine Learning Models - My Cheat List"
categories:
  - cheatlist
tags:
  - en
  - data science
  - machine learning
  - nanodegree
  - supervised models 
  - cheatlist
featured-img: analytics
last_modified_at: 2018-05-01T14:25:52-05:00
---


# Summary

* [Supervised Models](#supervised)
    - [Logistic Regression](#logistic-regression)
    - [Decision Tree](#decision-tree)
    - [Ensemble Methods](#ensemble-methods)
    - [K-nearest Neighbors](#knearesneighbors)
    - [Gaussin Naive Bayes](#gaussiannb)
    - [SVM](#svm)
    - [Stocastic Gradient Descent](#stochastic-gradient-descente)
* [Unupervised Models](#unsupervised)
    - [Kmean](#kmeans)
    - [Hierarchical Clustering](#hierarchical)
    - [DBSCAN](#dbscan)

# Machine Learning Models Cheat List

<h2 id='supervised'>Supervised Models</h2>

This is a small revision on advantages and disadvantages of each model, based on 
suggested models of Udacity's Nanodegree in Machine Learning Engineer.

<h3 id='logistic-regression'>Logistic Regression</h3>

#### Advantages

* Don't have to worry about features being correlated
* You can easily update your model to take in new data (unlike Decision Trees or SVM)

#### Disadvantages

* Deals bad with outliers
* Must have lots of incomes for each class
* Presence of multicollinearity

<h3 id='decision-tree'>Decision Tree</h3>

#### Advantages

* Easy to understand and interpret (for some people)
* Easy to use - Doesn’t need data normalisation, dummy variables, etc 
* Can handle multi-output models
* Easily handle feature interactions
* Don't have to worry about outliers

#### Disadvantages

* It can be easily overfitted
* Stability —> small changes in data can lead to completely different trees
* If a class dominates, it can easily be biased
* Don't support online learning --> you should rebuilt the tree when new data comes


<h3 id='ensemble-methods'>Ensemble Methods</h3>

#### Advantages

* Harder to overfit
* Usually better perfomance than a single model

#### Disadvantages

* Scaling —> usually it trains several models, which can have a bad performance with larger datasets
* Hard to implement in real time platform
* Complexity increases
* Boosting delivers poor probability estimates (https://arxiv.org/ftp/arxiv/papers/1207/1207.1403.pdf)

<h3 id='knearesneighbors'>K-nearest Neighbors</h3>

#### Advantages

* Little training time
* Works well with multiclass datasets 
* Good for highly unusual data

#### Disadvantages

* Need to determine value of k (distance)
* Neighbors-based methods are known as non-generalizing machine learning methods, since they simply “remember” all of its training data
* The accuracy of KNN can be severely degraded with high-dimension data because there is little difference between the nearest and farthest neighbor.

<h3 id='gaussiannb'>Gaussian Naive Bayes</h3>

#### Advantages

* Need less training data tran models like logistic regression
* Highly scalable
* Not sensitive to irrelevant features
* Returns the degree of certanty of the answer
* Good when you need something fast and that perfoms well

#### Disavantages

* Can't learn interactions between features e.g., it can’t learn that although you love movies with Brad Pitt and Tom Cruise, you hate movies where they’re together).

<h3 id='svm'>SVM</h3>

#### Advantages

* High accuracy
* Nice theoretical guarantees regarding overfitting
* Especially popular in text classification problems

#### Disavantages

* Memory-intensive
* Hard to interpret
* Complicated to run and tune

<h3 id='stochastic-gradient-descente'>Stochastic Gradient Descent</h3>

#### Advantages

* Efficiency
* Ease implementation

#### Disavantages

* A lot of hyperparameters to tune
* Sensitive to feature scaling


<h2 id='unsupervised'>Unupervised Models</h2>

<h3 id='kmeans'>KMeans</h3>

#### Advantages

* Good when you have an idea of an ideal number of clusters
* Can scale well with lots of samples, scale medium with number of clusters

#### Disadvantages

* Doesn't handle missing values very well
* Can't find clusters that aren't circular or spherical

#### Choosing the value of K

For choosing the value of k cluster we can use the elbow method:

```python
from sklearn.clusters import Kmeans
from sklearn.metrics import silhouette_score

X = pd.DataFrame(...)

possible_k_values = range(2, len(X)+1, 5)

scores = []
for k in possible_k_values:
    model = Kmeans(n_clusters=k).fit(X)
    prediction = model.predict(X)
    score = silhouette_score(X, predictions)
    scores.append((k, score))
```

Then find the best numbers of clusters by choosing a k that has a lower 
score of errors but can still be good enough for your problem.

<h3 id='hierarchical-clustering'>Hierarchical Clustering</h3>

#### Advantages

* Resulting hierarchical representation can be very informative
* Provides an additional ability to visualize 
* Especially potent when the dataset contains real hierarchical relationship (e.g. Evolutionary biology)

#### Disadvantages

* Sensitive to noise and outliers
* Computationally intensive O(N^2)

#### Implementation on Sklearn

```python
from sklearn import cluster

X = pd.DataFrame(...)

cls = cluster.AgglomerativeClustering(n_clusters=3, linkage='ward')
labels = cls.predict(X)
``` 

#### Get a dendrogram from a hierarchical clustering

```python
from scipy.cluster.hierarchy import dendogram, ward
import matplotlib.pyplot as plt

X = pd.DataFrame(...)
linkage_matrix = ward(X)

dendogram(linkage_matrix)
plt.show()

```

<h3 id='dbscan'>DBSCAN</h3>

#### Advantages:

* We don't need to specify the number of clusters
* Flexibility in shapes and sizes of clusters
* Able to deal with noise and outliers

#### Disadvantages

* Border points that are reachable from two clusters is assigned to the cluster that finds it first
* Faces difficulty finding clusters of varying densities

#### Tips:

* Small min samples and small episilon results in many small clusters
* Small min samples and large episilon results in most points being on the same cluster
* Large min samples results in most of points being classified as noise, except on desen regions when episilon is high
* Do not use silhouetter coefficient to test this model! [Recomendado](http://citeseerx.ist.psu.edu/viewdoc/download;jsessionid=83C3BD5E078B1444CB26E243975507E1?doi=10.1.1.707.9034&rep=rep1&type=pdf)

<h3 id='gmm'>Gaussian Mixture Model</h3>

#### Advantages

* Soft-clustering (you can see percentages of cluster participation on each sample)
* Cluster shape flexibility

#### Disadvantages

* Sensitive to initialization values
* Possible to converge to a local optimum
* Slow convergence rate

## General References

* [Choosing a machine learning classifier](http://blog.echen.me/2011/04/27/choosing-a-machine-learning-classifier/)
* [1](https://kevinzakka.github.io/2016/07/13/k-nearest-neighbor/#pros-and-cons-of-knn)
* [Sklearn documentation on Neighbors](http://scikit-learn.org/stable/modules/neighbors.html#neighbors)
* [3](http://people.revoledu.com/kardi/tutorial/KNN/Strength%20and%20Weakness.htm)
* [Sklearn documentation on Stochatic Gradient Descent](http://scikit-learn.org/stable/modules/sgd.html)
* [Sklearn documentation on Ensemble Methods](http://scikit-learn.org/stable/modules/ensemble.html)
* [Logistic Regression Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression)
* [Logistic Regression for machine learning](https://machinelearningmastery.com/logistic-regression-for-machine-learning/)
* [What are the advantages of logistic regression](https://www.quora.com/What-are-the-advantages-of-logistic-regression)
* [The disadvantages of Logistic Regression](https://classroom.synonym.com/disadvantages-logistic-regression-8574447.html)
