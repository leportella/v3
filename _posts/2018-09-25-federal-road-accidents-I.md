---
layout: post
title: "Predicting victims on national roads in Brazil - Part I"
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
permalink: predicting-victims-national-roads-I.html
redirect_from: /english/2018/09/25/federal-road-accidents-I.html
last_modified_at: 2018-09-25T22:05:52-05:00
---

This week, I finished my Nanodegree in Machine Learning Engineer by Udacity. To finish the course, I had to create a final 
study. Talking with a dear friend of mine, [Pedro](https://twitter.com/pedrovilanova), he said that the National Highway 
Police of Brazil had a dataset on car accidents in federal roads. I decided to study this dataset, and try to predict which 
types of victims an accident would have based on the local, hour and accident characateristics. 

I decided to publish two blog posts telling about this project, what I have achieved and the main techniques I used to do this 
project. On this first post, I will explain a little on the context of the project and show the initial exploratory analysis.

## Context

Roads are considered one of the most common ways to travel and transport material in Brazil. 
Nevertheless, Brazilian roads are considered extremely dangerous and unsafe. 
[According to OMS](https://www.metrojornal.com.br/foco/2017/05/01/brasil-e-o-quinto-pais-mundo-em-mortes-no-transito-segundo-oms.html), 
Brazil is the fifth country with deaths in traffic accidents, with more than 
[47 thousand deaths per year](https://www1.folha.uol.com.br/seminariosfolha/2017/05/1888812-transito-no-brasil-mata-47-mil-por-ano-e-deixa-400-mil-com-alguma-sequela.shtml).  

There are three main types of road in Brazil: local, regional (by state) and national roads. 
National roads are monitored by the Federal Highway Police (Polícia Rodoviária Federal, aka, PRF), 
which keeps records on time, place and characteristics of the ccidents that happened in these highways per year. 
The dataset are [publicly available](https://www.prf.gov.br/portal/dados-abertos) on the PRF website.

The project used the data from the PRF in an attempt to predict the type of victims an 
accident can have based on the characteristics of the highway, the moment of the accident and main characteristics 
of the accident. The main goal was to define if that accident will likely have no victims, 
injuries victims or death victims. We considered information on the time of the accident 
(moment of the day, day of week, etc), on the place the accident happened (road number, kilometer, state, etc) 
and the accident characteristics (such as cause of the accident, and type of accident). 

If we can predict, with a certain amount of confidence, the type of victims based solely on the local and 
time of the accident, one can assume that there is a problem on Brazilian Roads, and one can predict most dangerous areas. 
Even if this is not possible, the predictions could help identifying most problematic areas and help determine priorization of 
resources in areas where resources are scarce.

## Benchmark

[Chong, Abraham & Paprzycki, 2005](ajith.softcomputing.net/isda-mam.pdf) did a similar study, where they used neural network for trying to classify victims degree of injuries on traffic accidents. 
On this study, they had 5 classes of injures, including no injury, possible injury, non-incapacitating injury, incapacitating injury, and fatal injury. 
The features where based both on road and driver's characteristics. 
The road characteristics were similar to what we found in the datasets of National Road Police. The authors found a ~60% accuracy on predicting the type of victims.


## Exploratory Analysis

From all the 180991 records, 58.8% had no victims, 35.4% had injured victims and only 5.6% had dead victims. 
So we have a clear case of unbalanced classes:

![](https://i.imgur.com/pmPHxEE.png)

Most of the accidents happens during the weekends, being Saturday the day with higher number of accidents. 
Monday is the weekday with higher number of accidents, while Tuesday is the day with less number of accidents:

![](https://i.imgur.com/J5aAjgQ.png)

Rear collision are the most common type of accident, with almost 35000 accidents registered. 
Lateral and transversal collision are the second and third more common cause of accidents, with more than 20000 records. 

![](https://i.imgur.com/X3TIEW2.png)

Most accidents happened during the day (56.3%), while a third happened at night (32.8%) and 
~11% happened in the transition between day and night. 

![](https://i.imgur.com/YjA1RaK.png)

While most of the accidents with injured victims occurred during day time (61 thousand), 
accidents during the night with dead victims were higher than during the day, probably due to 
worse visibility causing accidents with more fatal victims. 

![](https://i.imgur.com/XczFuBv.png)

It is possible to see that some causes of accidents are highly correlated to the type of accident caused. 
For instance, rear collision are highly associated with unsafely distance and incompatible speed can be 
associated with the vehicle being out of the road. 

![](https://i.imgur.com/zHCHuOs.png)

We can see that the state of MG, PR and SC are the states that have more accidents, followed by SP, RS and RJ. 
Although having a small number of accidents (2961), MA is the state with the highest number of accidents with 
dead victims in proportion (12.2%). SC, for instance has 10 times the number of accidents than MA 
but only 3.3% of them had dead victims.  

![](https://i.imgur.com/WbEW17d.png)

Most accidents occur in straight highways followed by accidents in curves. In general, one every 
10 accidents with victims have dead victims.

![](https://i.imgur.com/5Q98hP4.png)

Accidents in bridges have a higher chance to have dead victims (8,5%), followed by accidents in temporary detours (7,5%) 
while accidents in roundabout have the lowest chance of dead victims (1.6%). Be the other hand, accidents in tunnel 
have the higher percentage of accidents with no victims (43%) followed by accidents in curves (41%).

![](https://i.imgur.com/qJilGmw.png)

It is possible to observe in the next figure, that accidents involving fire and cargo spill has a high chance of 
having no victims (> 90%). Accidents with vehicle fall, vehicle occupant fall and bike collision have a high chance 
of having injured victims (> 80%). Frontal collision and pedestrian trampling have a huge chance of having dead victims (28%) 
while bike collision has a 15% chance of having dead victims. Thus, we can say that pedestrian trampling, 
frontal collision and bike collision are the most dangerous type of accidents.  

![](https://i.imgur.com/bZ07G9r.png)
