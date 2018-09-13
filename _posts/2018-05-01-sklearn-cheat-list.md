---
layout: post
title: "Sklearn - My Cheat List"
categories:
  - cheatlist
tags:
  - en
  - python
  - sklearn 
  - cheatlist
featured-img: calculator
last_modified_at: 2018-05-01T14:25:52-05:00
---


Sometimes I get just really lost with all available commands and tricks one can make on sklearn. 
This way, I really wanted a place to gather my tricks that I really don't want to forget.


# Summary

* [How to check results of accuracy for each of the classes available on a classification problem?](classification-results-by-class)
* [How to normalize features?](#normalize-features)
* [How to create a bag of words dataframe matrix](#bag-of-words)
* [How to export a trained model](#export-model)

# My Sklearn Cheat List

<h2 id='classification-results-by-class'>How to check results of accuracy for each of the classes available on a classification problem?</h2>

```python
from sklearn.metrics import classification_report

classification_report(y_test, y_pred, target_names=['A', 'B', 'C']

# Results:

             precision    recall    f1-score 

A              0.9         ...         ...
      
B              0.9         ...         ...
 
C              0.9         ...         ...
 
avg/total      0.9         ...         ...
```

<h2 id='normalize-features'>How to normalize features?</h2>

```python
from sklearn import preprocessing

normalized_X = preprocessing.normalize(X)
```

<h2 id='bag-of-words'>How to create a bag of words dataframe matrix</h2>

```python
import pandas as pd
from sklearn.feature_extraction.text imprt CountVectorizer

documents = ['Hello, how are you!',
             'Win money, win from home.',
             'Call me now.',
             'Hello, Call hello you tomorrow?']

count_vector = CountVectorizer()
count_vector.fit(documents)

doc_array = count_vector.transform(documents).toarray()

freq_matrix = pd.DataFrame(doc_array, columns=count_vector.get_feature_name())
```

<h2 id='export-model'>How to export a trained model</h2>

```python
from sklearn.externals import joblib

joblib.dump(model, 'name.pkl')

# to read
model = joblib.load('name.pkl')
```
