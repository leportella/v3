---
layout: post
title: "Predicting victims on national roads in Brazil - Part II"
categories:
  - english
tags:
  - english
  - data science
  - machine learning
  - exploratory analysis
  - pandas
  - sklearn
  - python
featured-img: accidents
last_modified_at: 2019-01-02T21:55:52-05:00
---

This is the second part of my study on predicting the type of victims an accident can have based on the data from the
National Highway Police (Polícia Rodoviária Federal), in Brazil. This was my final report for my Machine Learning Engineer Nanodegree and my first technical diploma in the computer science field (yey!).

![](https://media.giphy.com/media/4TtTVTmBoXp8txRU0C/giphy.gif)

On [the first post](https://leportella.com/english/2018/09/25/federal-road-accidents-I.html) I showed some exploratory analysis of the dataset and the cleanups. On this post, I'll address the methodologies used for constructing the machine learning model.

This is a more personal view of my project, but you can check the full report I sent to [Udacity](https://www.udacity.com) [here.](https://github.com/leportella/federal-road-accidents/blob/master/Udacity_final_project_results.pdf)


## Goal and benchmark

First of all, I was able to found a study made by [Chong, Abraham & Paprzycki, 2005](ajith.softcomputing.net/isda-mam.pdf) that did a similar study, where they used neural network for trying to classify victims degree of injuries on traffic accidents. On this study, they had 5 classes of injures, including no injury, possible injury, non-incapacitating injury, incapacitating injury, and fatal injury. The features where based both on road and driver's characteristics. The road characteristics were similar to what we found in the datasets of National Road Police. The authors found a ~60% accuracy on predicting the type of victims.

On the Brazilian dataset we didn't have any information regarding the driver characteristics and we only had 3 types of victims described: injured, dead or no-victims. Thus, the main idea was to create a classification model that would predict one of the three classes of victims: no victims (class 0), injured victims (class 1) and dead victims (class 2).

## Project Organization

![](https://media.giphy.com/media/9Y6ZRYDjHbY6w8LA9C/giphy.gif)

The project contained two folders: a `data` folder and a `notebooks`folder.

The data folder contained 3 CSV files, two original files downloaded from the website (`datatran2016_atual.csv` and `datatran2017.csv`) and a cleaned dataset (`datatran_2016-2017.csv`)

The notebooks folder contained all Jupyter Notebooks used in my project:
Notebooks starting with 0 (`0.*`) included general analysis I did using the dataset that were not related with machine learning models and tests
Notebooks from 1 to 21 included each machine learning technique and model utilised
A notebook called `Clean` that does all the processing and data cleaning that results in the cleaned dataset (`datatran_2016-2017.csv`)

## Machine Learning Technique

Through all notebooks I used the same strategy. My cleaned dataset consisted in multiple feature columns (represented in the figure below as X) and a target column that I wished to predict (called Y). The first thing I did was to divide the dataset in two separate datasets using the function [*train_test_split*](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html). This resulted in two other datasets: a train dataset with 80% of the original data and a test dataset with 20% of the data. I used only the *train dataset* to train my model.

This way I used most of my dataset but not all to train my model. I still was able to test my model by using a set that it doesn't have seen previously. This is a way to make sure that my model is not overfitting to the data and it can generalize well to data that it hasn't seen before.

<img src="https://i.imgur.com/UZ3XMLU.png" style="height:300px;"/>

I then used my *test dataset* to check how well my model was doing. I used the `X_test` (only features of my *test dataset*) to make predictions and then compared the result of the model (`y_pred`) with the actual data (`y_test`). With this comparison I had information of accuracy, F1 score and precision and recall for each class, which helped decide which model was doing better.

<img src="https://i.imgur.com/YwnibIT.png" style="height:300px;"/>

## Initial steps

First, I created a "quick and dirt" model, using Logistic Regression (Model 01) and all variables that I considered to be of relevance for the problem. I used this as my *baseline*, the base on which I would compare all other models. Although this model achieved a considerably high accuracy score of 0.69, the recall on class two was ridiculously low (almost 0), so this was not even near good enough.

For this and all other models I used a technique called [GridSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html) that is a technique of model optimisation and parameter tunning. It permits to pass several values of parameters to a model class and the GridSearchCV evaluates, based on an accuracy metric you defined, which set of parameters gives better performance. This is very useful to optimize hyper parameter search.

I also used all features I could think that could be important for the model: weekday, UF (state in which the accident happened), Km, accident cause, accident type, moment of day, climate and road layout.

I used these approaches and features to the 2 models as well: RandomForest (Model 02) and Gaussian Naive Bayes (Model 03). None of them had a satisfactory result.

## The big problem: unbalanced classes

The main problem I was facing was that the classes were unbalanced. Accidents with no victims were the vast majority of the case (58.8%) while accidents with dead victims represented only 5.6% os the cases. I briefly studied this problem and I discovered three solutions.

The first, called **oversampling**, was to create samples for the least frequent classes.

The second, called **undersampling**, was to reduce the number of all classes based on the least frequent class (get only 5.6% of cases from each class).

The third was to add a parameter called *class balance* to some models, and let them know that the classes were unbalanced.

The first was a good approach but needed a deep study to make things right. Since creating could introduce errors and I didn’t have the time to study the technique, I decided to not use it.

The first alternative seemed to me a little more trickier, once it contained a possibility of messing too much with the data, and possibly making mistakes if not used carefully. Since I didn’t had the time to do this at the time, I avoided using this solution.

The second alternative, **undersampling**, was implemented on the Logistic Regression model and the result was good (Model 04). We had an overall drop on the accuracy (from 0.63 to 0.57) but all classes had similar errors, which made the model better than the first model.

The third alternative was tried but it didn’t gave a good result. Not as near as good as the second alternative. Thus, under sampling was the technique chosen to deal with unbalanced classes through all models from that moment on.

## Multiclass models vs models of binary classes

Since we had 3 classes that were pretty hard to solve by a single model, I tried a technique of multiple models.

First I created 3 models: one for each class (Models 10.0, 10.1 and 10.2). The first model tried to predict whether there would be victims or not, the second would predict if there would be injured people or not and the third if there would be dead victims or not. Each model returns a probability of that accident be of a certain class. So, I add this probabilities as features for a fourth model. Then, using the original features and the probabilities I have a model that defines which class the accident should be classified.

The general schema can be seen on the figure below:

<img src="https://i.imgur.com/tsMohnH.png" style="height:200px;"/>

Even though this was the most complex strategy I used, this didn’t result in a good improvement of the prediction. A general accuracy of 0.56, but a recall of only 0.39 for class 1.

## Maybe numbers are better than classes

In the middle of the process I realised that maybe the problem was trying to predict classes of victims instead of number of victims. That could result in a better model, so I tried a simple model to test this hypothesis. I used the variable `number of injured` with a Random Forest Regressor and GridSearchCV (Model 09). I evaluated the Mean Absolute Error of the model with the Mean Absolute Error of predicting the mean value of victims. The model was worse than predicting the mean value of victims, so I discarded this hypothesis and didn’t go further on trying to predict the number of injured.

## Model resume

I tried several models in the process of trying to get a better performance. During my Nanodegree, I had to create a list of several models and what were their advantages and disadvantages. I made a cheat list with this so I would have this easily when needed. [Check this cheat list here](https://leportella.com/cheatlist/2018/05/20/models-cheat-list.html). The models I used in this project were:

* Random Forest Classifier - An ensemble model that uses multiple decision trees made with random samples of the dataset. This model usually gives a good performance and is harder to overfit. However, it is usually a slow model that doesn’t scale well. It is a good option especially with smaller datasets, such as the ones with undersampling.
* Gaussian Naive Bayes - It is a model that need less training data and in highly scalable, thus it could be a good fit for the present study, specially with large data.
* Logistic Regression - It is a model that doesn’t worry about features being correlated, although it needs more data to train, it is a good choice.
* Support Vector Machines (SVM)
* KMeans - It is a fast model that works well with multi class datasets.
* Gradient Boosting - It is similar to random forest, but this model consider the error of the previous decision tree on the next one.
* Neural Networks




## Evaluating all the models

![](https://media.giphy.com/media/vsZF2hC9cH0Mo/giphy.gif)

After 10 models  (with accuracy score and F1 scores for each)  and 3 classes (with values of recall, precision and F1 score for each) things started to get really confusing. I could not visualize which model was better for what and I definitely couldn’t figure out where could I be better.

The final solution for me was to create a spreadsheet that contained all the values and format it with a color bar from 0 to 1. This way, I could visualize what was happening and filter the best models. This became a good friend of mine :) Totally recommend it! If you would like to use mine as a baseline, feel free to download it [here](https://docs.google.com/spreadsheets/d/1G0UfnlyHtR_aMCPGND4_KZXvYn5xs86nWLkW0Is_qqU/edit?usp=sharing).

![](https://i.imgur.com/ivQGug8.png)


## Final model

Even though the original proposition (multiclass model) had a similar accuracy to the benchmark model, a binary model had a better accuracy and better recall, that was chosen as the best model.

The best model was the Multi-layer Perceptron Classifier (Neural Network), was optimised with the following parameters (Model 20):

| Parameter          | Values Tested                  |  Value Optimal |
|--------------------|--------------------------------|----------------|
| activation         | identity, logistic, tanh, relu | Logistic       |
| solver             | lbfgs, sgd, adam               | Adam           |
| hidden layer sizes | 50, 100, 200                   | 100            |
| learning rates     | constant, invscaling, adaptive | invscaling     |


The results here presented are a combination of the best algorithm and variables, but this were small changes compared to the change that could be achieved only by under sampling.

On the figure below we can see the confusion matrix of the best binary model (notebook 20). We can see that only 182 accidents that had not victims were classified as with victims, while 1602 with victims were classified as without victims. The table below has the results of precision, recall and f1-score for each class.


<img src="https://i.imgur.com/Tne5rRO.png" style="height:300px;"/>

<img src="https://i.imgur.com/FC8QnIZ.png" style="height:100px;"/>

## Next steps

It was really fun working with Sklearn and this dataset. However, I think there is room for a better performance. I am currently studying Pytorch and want to give it a try with this dataset. Who knows?

Also, I used only 2 years dataset. I intend to go further back as well.
