---
layout: post
title: "Pandas - My Cheat List"
categories:
  - cheatlist
tags:
  - en
  - python
  - pandas 
  - cheatlist
featured-img: panda
last_modified_at: 2018-05-01T14:25:52-05:00
---


Sometimes I get just really lost with all available commands and tricks one can make on pandas. 
This way, I really wanted a place to gather my tricks that I really don't want to forget.

![](https://media.giphy.com/media/14aUO0Mf7dWDXW/giphy.gif)

# Summary

* [How to list available columns on a DataFrame](#column-names)
* [How to make multiple filters](#multiple-filters)
* [How to iterate over a DataFrame](#iterate)
* [How to count the ocurrences of each unique values on a DF column](#unique-ocurrences)
* [How to save a DataFrame by chunks](#save-by-chunks)
* [A groupby example](#group-by-example)
* [How to fill values on missing months](#missing-months)
* [How to filter column elements on a list](#filter-elements-by-list)
* [How to change a Series type?](#change-series-type)
* [How to apply a function to every item of my Serie?](#apply-function)
* [How to prepare my DataFrame to apply get_dummies?](#apply-get-dummies)
* [read_csv errors of encoding](#encoding)

# My Pandas Cheat List


<h2 id='column-names'>How to list available columns on a DataFrame</h2>

```python
df.columns.values
```

<h2 id='multiple-filters'>How to make multiple filters</h2>

```python
df[(df.column > value1) & (df.column < value2)]
```

<h2 id='iterate'>How to iterate over a Dataframe</h2>

```python
for item, row in df.iterrows():
  print row()
```

<h2 id='unique-ocurrences'>How to count the ocurrences of each unique values on a DF column</h2>

```python
df[column].value_counts()

# get indexes
df[column].value_counts().index.tolist()

# get values of occurrences
df[column].value_counts().values.tolist()
```

<h2 id='save-by-chunks'>How to save a DataFrame by chunks</h2>

```python
df = pd.DataFrame([[0, 1, 2], [3, 4, 5], [6, 7, 8], [9, 10, 11]])

df1 = df.iloc[0:2,:]
df2= df.iloc[2:,:]

df1.to_csv('./teste1.csv', index=False, header=False)
df2.to_csv('./teste1.csv', index=False, header=False, mode='a')

df_final = pd.read_csv('./teste1.csv')
df_final.head()
```

<h2 id='group-by-example'>A groupby example</h2>

```python
df_grouped = df.groupby(
        by=['first_column', 'second_column']
    )['third_column'].mean().reset_index(name='mean_values_grouped')
```

<h2 id='missing-months'>How to fill values on missing months</h2>

If you have a dataframe with 2 columns: year and month. But data is not available for all months, so you need to enter missing months on 
your dataframe with empty values on them.

```python
# Original data with months not available
df1 = pd.DataFrame({
    'month': [1, 2, 4, 5, 6, 8, 9, 10, 11, 12, 1, 2, 3, 4,
              5, 8, 9, 10, 11, 12],
    'year': [2011, 2011, 2011, 2011, 2011, 2011, 2011, 2011,
             2011, 2011, 2012, 2012, 2012, 2012, 2012, 2012, 
             2012, 2012, 2012, 2012],
    'qty': [5, 7, 3, 6, 7, 8, 3, 5, 7, 10, 12,
            5, 7, 8, 1, 3, 5, 7, 8, 20]
})

# List of all months
df2 = pd.DataFrame({'month': list(range(1,13))})
```
 
Now we create an empty dataframe with all available years and months:

```python
from itertools import product

years_months = pd.DataFrame(list(product(np.unique(df2.month), np.unique(df1.year))), columns=['month', 'year'])
```

Now we can just merge both dataframes with an outer join:

```python
pd.merge(years_months, df1, how='outer')
```

<h2 id='filter-elements-by-list'>How to filter column elements on a list</h2>

```python
df[df['A'].isin([3, 6])]
```

<h2 id='change-series-type'>How to change a Series type?</h2>

```python
import pandas as pd

serie = pd.Series([1, 2, 3, 4])
series.astype(float)
```

<h2 id='apply-function'>How to apply a function to every item of my Serie?</h2>

```python
import pandas as pd

serie = pd.Series(['a', 'b', 'b', 'a'])
series.apply(lambda x: 0 if x=='a' else 1)
```

<h2 id='aplly-get-dummies'>How to prepare my DataFrame to apply get_dummies?</h2>

```python
import pandas as pd

X = pd.read_csv(..)
categorical = ['x1', 'x2', 'x4']  # columns that have categorical features in your X

for cat in categorical: 
    X[cat] = X[cat].astype(object)

X_dummy = pd.get_dummies(X)
```

<h2 id='encoding'>read_csv errors of encoding</h2>

Usually you can read a csv just by doing something like:

```python
pd.read_csv('file.csv')
```

Sometimes, an encoding error appears. The first option is to pass 'utf8' as a value of 
the parameter `encoding`.  

```python
pd.read_csv('file.csv', encoding='utf8')
```

But there are some cases where this is not enough and the following error keeps appearing:

`UnicodeDecodeError: 'utf-8' codec can't decode byte 0xc7 in position 4: invalid continuation byte`

The only thing that could resolve this was:

```python
pd.read_csv('file.csv', encoding='latin-1')
```
