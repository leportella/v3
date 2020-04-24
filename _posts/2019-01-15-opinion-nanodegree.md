---
layout: post
title: "Opinion: Machine Learning Engineer Nanodegree"
categories:
  - english
tags:
  - english
  - python
  - data science
  - courses
  - development
  - girls in tech
  - career
  - udacity
  - nanodegree
  - data
  - python
  - community 
  - pyladies
  - technology
  - tecnologia
  - programador
  - programadora
  - developer
  - mulheres na tecnologia
  - woman in tech
  - girls in tech
  - computação
  - ciência de computação
  - software development
  - software engineering
  - engenharia de software
  - desenvolvimento
  - auto-ensino
  - self-taught engineer
  - code
  - Django
  - software
  - career
  - tech career
  - open-source
  - no cs degree
  - cs
  - computer science
featured-img: auditorium
permalink: machine-learning-nanodegree.html
redirect_from: /english/2019/01/15/opinion-nanodegree.html
last_modified_at: 2019-01-15T11:48:52-05:00
---

This is a translation of [this post in portuguese.](https://medium.com/pizzadedados/opini%C3%A3o-nanodegree-de-eng-de-machine-learning-34e67cb85b33)

In April 2018 I started [Udacity's Nanodegree in Machine Learning Engineer](https://www.udacity.com/course/machine-learning-engineer-nanodegree--nd009t). The classes are not cheap and many questions asked me the same thing: does it worth it?

<center><img src="https://media.giphy.com/media/ATt7p8OO4mvvO/giphy.gif" style="height:200px;"/></center>

My answer is not the best: it depends. Yes, this is not a good answer but it is true: it depends.

When I started it (coursed changes) it was an advanced class and it was pretty clear in the course page. You must have this in mind when thinking about doing this course. If you are just starting on this area this is definetly not the course to start with. 

## Before start talking about it

If you just started to study machine learning the answer is: **no**. It doesn't worth it. There are several other courses that are more suitable for beginners (if you have the money) and several free courses around the internet.

In my case, I already had some experience with data science, a good statistical thinking and I had an idea how Machine Learning worked. Among things I studied I did the [Andrew Ng's Machine Learning classes on Coursera](https://www.coursera.org/learn/machine-learning). This was not a simple course. You have a lot of mathematics, you needed to use [Octave](https://www.gnu.org/software/octave/) (similar to [MATLAB](https://www.mathworks.com/products/matlab.html)) and you have tons of work to do. However, this clsses gave me a cool idea on how things work underneath it all. 

For instance, in Andrew's classes you have to implement the [Gradient Descent](https://en.wikipedia.org/wiki/Gradient_descent) (a technique used by some algorithms) by hand. Yeah, he teachs you the concepts, shows the formula and then you have to built one yourself. In the Nanodegree he gives you an idea how the algorithm works but it goes straight to detailed explanation on advanced models such as [SVM](https://en.wikipedia.org/wiki/Support_vector_machine) and [Random Forest](https://en.wikipedia.org/wiki/Random_forest). And this knowledge is, in a way, superficial because you don't really understand the math behind it. If you are ok with this, good! But for me, I like to know how things work and it was pretty good knowing it before starting which parameters in the SVM I should care about.

## Module 1: Supervised Learning

I really enjoyed this module. There were several classes on [Decision Trees](https://en.wikipedia.org/wiki/Decision_tree), [Naive-Bayes](https://en.wikipedia.org/wiki/Naive_Bayes_classifier), [SVM](https://en.wikipedia.org/wiki/Support_vector_machine), etc. I thought the classes were well written and the examples were really good and easy to understand. There are a LOT of exercises to do. 

The final project of this module is, in fact, a [Jupyter Notebook](https://jupyter.org/) with TODO areas. In this project you have to predict whether a person earns more than 50 thousand dolars to optimize asking for donation for a [NGO](https://en.wikipedia.org/wiki/Non-governmental_organization). This case seemed pretty realistic and interesting and several topics on the notebook asked for you to think about the problem. I really liked it.

One of the parts I really enjoyed was to do a summary on advantages and disadvantages of 8 models of supervised learning. It was not easy, but it was goof knowledge to acquire. The result of this summary I added [on this post](https://leportella.com/cheatlist/2018/05/20/models-cheat-list.html), to always have it easily when I need. 

## Module 2: Unsupervised learning

Again, the classes were excellent. I had a lot less experience on this matter, and this was a great experience for me. It gives a good idea on how [KMeans](https://en.wikipedia.org/wiki/K-means_clustering) works but it goes really further with several practical examples. On this module there is a lot on [PCA](https://en.wikipedia.org/wiki/Principal_component_analysis) but I confess that the knowledge I had from Andrew's course helped a lot here. Another cool thing I learned was how to "know" if a clustering is good. I always asked myself how to do this, and the answer was there :)

As to the project, I thought it was a bit confusing. The dataset had little connection to the real world (or I couldn't see so) and I got a bit lost. That's when I discovered that there is a place for you to set a one-o-one with a mentor. And here is an advantage of this course: the mentors are really good, patients and always available. The ones that helped me were always nice and explained me a lot of things. Even after our call they asked me by Slack if everything was ok.

Although the project was a bit confusing I really liked this module as a whole. 


## Module 3: Reinforcement learning

Well... here things weren't so good. During the first 7 classes of this module things were hard, but duable. They started with lessons on reiforcement learning in discrete spaces. Hard, but duable. In class 7 you have to set a little project to put a taxi to walk by itself. It seems huge, but you use a library to do this and it was an interesting challenge. The problem starts after it. From the 8th class on the thing goes from hard to impossible. It goes from discrete to continuous spaces, and with [deep learning](https://en.wikipedia.org/wiki/Deep_learning) and whaaaaaat? And also, there is absolutely no pratical exercises on continous spaces. 

<center><img src="https://media.giphy.com/media/UnTC9o2HMyUta/giphy.gif" style="height:200px;"/></center>

The final project uses deep learning with [TensorFlow](https://en.wikipedia.org/wiki/TensorFlow) and boy... it was hard. I had a LOT of difficulty to go through this module and project. Until the Taxi project things were pretty cool, but after this... disaster. I really think it was not necessary to go so hard so fast. It was kinf of demotivator.

## Module 4: My project

I had to choose a dataset and a problem to work here. While talking to a friend ([Pedro](https://twitter.com/pedrovilanova)), he told me about a dataset on car accidents on brazilian roads. So, an idea formed in my head: what if I could predict the type of victims in an accident based on the characteristics of accidents?

The project was success (for me at least). I used a lot of things I have learned, could go deep on my previous knowledges on supervised learning, learned how to structure a problem, deal with unbalanced classes (when you have a lot of cases of one class but not the other) and a lot more. 

This project was detailed in two posts:

* [Predicting victims on national roads in Brazil - Part I](https://leportella.com/english/2018/09/25/federal-road-accidents-I.html)
* [Predicting victims on national roads in Brazil - Part II](https://leportella.com/english/2019/01/02/federal-road-accidents-II.html)

## Resume

I really liked the program, but having a previous experience helped a lot to make it really worth it. I liked most of classes, the mentors were excellent and I learned a lot. Some parts were more difficult then I think they should be, but it definetely worth it. I also wrote a big feedback to Udacity on the end of the course. The brazilian director actually called me to talk about the feedback I gave and we talked on how they were trying to make it better. 

So, if you already have a good knowledge on statistics and an idea on how machine learning works, I think this is a good path to continue your studies. Also, the groups in Slack are a great place for networking.

Ah! And I published that this was my first degree in the tech field and I won some gifts from Udacity! Thanks!


<center><img src="https://i.imgur.com/zD1suVv.jpg" style="height:300px;"/></center>


PS: I am so sorry that Udacity is ending its operation in Brazil. I really liked all the support of my brazilian mentors and the director's plan for Brazil :/

