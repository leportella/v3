---
layout: post
title: "What to do when data is missing?"
categories:
  - english
tags:
  - english
  - python
  - jupyter
  - open-source
  - development
  - girls in tech
  - pyladies
  - interpolation
  - missing data
  - data
  - datasets
featured-img: interp
last_modified_at: 2019-01-14T11:48:52-05:00
---

## AKA magics to plug holes in your dataset

<center><img src="https://media.giphy.com/media/MSd5euCFgqULK/giphy.gif" style="height:300px;"/></center>

<center><i>and it is not with little foxes ;)</i></center> 

This is the translation of [this post in portuguese :)](https://medium.com/databootcamp/o-que-fazer-quando-faltam-dados-255ef5508a4f)

We can roughly divide any kind of data into two categories: temporal and timeless. Timeless data are quite common in the most commonly used datasets in data science tutorials: such as features of Titanic survivors, flower petal sizes, or tumor characteristics. Data may have temporal data, such as the moment when the user has made a determined attitude, but time is a secondary feature of the data, and not its essence.

When dealing with temporal data, on the other hand, the moment (time) when those data were created are extremely important. Classic examples are dólar value through the day or the number of users per hour in a website.

When a timeless data has missing data, we usually are taught that the simplest way is to remove that data row from our dataset. It is best to remove the data because many algorithms can't make analysis with missing data and, most cases, the data are abundant enough to have little to no impact on removing a few lines.

When we are talking about temporal data, the removal of a data can be very troubling. These type of data need pattern, specially if we must do a frequency analysis. So, which alternatives we have then? This text is precisely to talk about them :)

All graphics and code are available on [this notebook](https://github.com/leportella/interpolation-example/blob/master/interp_port.ipynb).

## Our temporal data

We will use the following data to analyze. We have a pretty simple data that has a senoidal shape. We know, before anything, that this data doesn't have abrupt variations and that it should be received in regular time intervals. We can see, then, that the nineth element was lost and this is the data we will focus to fullfil. 

<center><img src="https://i.imgur.com/9Iwfi2x.png" style="height:300px;"/></center>

The original data is in red:

<center><img src="https://i.imgur.com/mcA3Ov9.png" style="height:300px;"/></center>

## Fill with mean

One possibility is to fill the missing data with the mean value of the whole series. The result is the presented below:

<center><img src="https://i.imgur.com/G53c1JE.png" style="height:300px;"/></center>

Seems weird, rigth? And it is! Since we know the data general pattern, this alternative is really unsuited. However, this method can be quite usefull depeding on what you have, since the mean of the whole data remains the same.

However, don't be fooled by it! The mean will not change but the other statistical characteristics may (and probably will) change. Because we are adding another data more close to the mean, the standard deviation tend to be smaller. See:

<center><img src="https://i.imgur.com/BDJcVY9.png" style="height:300px;"/></center>

## Interpolation

Interpolation is a mathematical method that adjusts a function to your data and uses this function to extrapolate the missing data. The most simple type of interpolation is the linear interpolation, that makes a mean between the values before the missing data and the value after.  

Since our data is a curve and not a line, we have a difference between the real original point and the interpolated point. However, since we have a good idea on how our data should behave, this is a pretty good approximation that solves our problem.

<center><img src="https://i.imgur.com/Bwj5Rw5.png" style="height:300px;"/></center>

Of course, we could have a pretty complex pattern here and linear interpolation could not be enough. Although I am just showing one type, we have several different types of interpolation, one for each reality you may bump into. Just in Pandas we have the following options: ‘linear’, ‘time’, ‘index’, ‘values’, ‘nearest’, ‘zero’, ‘slinear’, ‘quadratic’, ‘cubic’, ‘barycentric’, ‘krogh’, ‘polynomial’, ‘spline’, ‘piecewise_polynomial’, ‘from_derivatives’, ‘pchip’, ‘akima’.

<center><img src="https://media.giphy.com/media/j9djzcMmzg8ow/giphy.gif" style="height:200px;"/></center>

## Interpolation when data pattern is unknown

It is important to say that not all temporal signals have a clear pattern such as that we just saw. So, let's take a look to a second temporal series. 

<center><img src="https://i.imgur.com/dQclr46.png" style="height:300px;"/></center>

In this case, we don't know the pattern and we can't predict for sure the data that was lost and we can't predict the low value it actually had. Thus, linear interpolation may not be the best choice:  

<center><img src="https://i.imgur.com/5dosVQY.png" style="height:300px;"/></center>

In this case filling the missing value with the mean value of the series makes more sense! Since no data will actually predict the missing value, the value of the mean will keep the data align to its overall behavior and you get your continuity back. Depending on which kind of analysis you want to do, this could be exactly what you need.

<center><img src="https://i.imgur.com/Pkd5SX4.png" style="height:300px;"/></center>

In our case the missing data and the mean have a really close value, but this is not always the case. Imagine if the missing point was the pick in the beginning of the data?

## Filling with other data

Another option is to replace the missing data with data that surrounds it. We can, for instance, replace for the previous data. This technique is called Forward Fill because it propagates existent data forward. 

<center><img src="https://i.imgur.com/8C7rA6P.png" style="height:300px;"/></center>

We can do the opposite as well, it is called Backward Fill:

<center><img src="https://i.imgur.com/SqEqMlg.png" style="height:300px;"/></center>

Table with statistics of all series:

<center><img src="https://i.imgur.com/Uqq6cjP.png" style="height:300px;"/></center>

## Which to use?

Hard to say. If your data has a known pattern, probably some kind of interpolation will fit better. However, if the pattern is unknown, I recommend you to evaluate the analysis you want to make and choose the best method for it or the one that gives you the best result :)

<center><img src="
https://media.giphy.com/media/OmAdpbVnAAWJO/giphy.gif" style="height:300px;"/></center>

## Cool references

- [Missing value given the mean — Khan Academy](https://www.khanacademy.org/math/ap-statistics/summarizing-quantitative-data-ap/mean-median-more/v/using-mean-to-find-missing-value?fbclid=IwAR3nCjBx5QzGcpuc85U0gtAL-xo4eVjnNdX0TEeWYGTz5JOBVxhr-M40OxQ)
- [Comparison of Linear Interpolation Method and Mean Method to Replace the Missing Values in Environmental Data Set](https://www.researchgate.net/publication/271978892_Comparison_of_Linear_Interpolation_Method_and_Mean_Method_to_Replace_the_Missing_Values_in_Environmental_Data_Set?fbclid=IwAR2vF8E4iMlz4dXxi97CUXY5GS_SNMsPU9CjKB8V-e4ma7gMjJq1NUDTY88)
- [Multivariate Time-series Similarity Assessment via Unsupervised Representation Learning and Stratified Locality Sensitive Hashing: Application to Early Acute Hypotensive Episode Detection](https://ieeexplore.ieee.org/abstract/document/8506445?fbclid=IwAR1gFtE-QcsEVXCj1vCi5bW7k07Z_t-87FbU1Bj28rrIXKpLuXf8kncGZKM)
- [Principled missing data methods for researchers](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3701793/?fbclid=IwAR2vSfeQOJ2aCm-SckRCMMgjrhsT0PYpMPopVxxN8vMhMa9Bm1FwUH2P8oc)
- [Missing Data & How to Deal: An overview of missing data](https://liberalarts.utexas.edu/prc/_files/cs/Missing-Data.pdf?fbclid=IwAR2yhT-hls2I3CHgD4Pg91ZognPKGnHrJN1FlOiawH9JI0r0HZgOj5qAWMY)
- [Missing-data imputation](http://www.stat.columbia.edu/~gelman/arm/missing.pdf?fbclid=IwAR0LABcI7HXU8UL_KSD-LppIfL-I6SmasrIBCcmPpQnTTjTbNcCr1A9UAeQ)
- [A review of missing values handling methods on time-series data](https://www.researchgate.net/publication/313867740_A_review_of_missing_values_handling_methods_on_time-series_data?fbclid=IwAR1Sxb8-munSBEPUSZGIOe4Lo1tfQpZZmon4K2Xlw_KbajLzrS02dW2yIxI)
