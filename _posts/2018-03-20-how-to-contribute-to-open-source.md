---
layout: post
title: "How to contribute to an open-source project without writing any code"
categories:
  - english
tags:
  - en
  - open-source
  - begginers
  - study
  - code
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
featured-img: open
permalink: open-source-without-code.html
translation: open-source-sem-codigo.html
redirect_from: /english/2018/03/20/how-to-contribute-to-open-source.html
last_modified_at: 2017-03-09T14:25:52-05:00
---

**Disclaimer: In an old post, I talked about how frustrating it was writing documentation for 3 whole months. 
However, I should make some clear: there are times to write code and times to write documentation. 
I was frustrated in just doing for 3 long months while I was hoping to become a programmer. 
This made me unconfortable on writing documentation for while and I didn't want to do it at all. 
Now, I realized that eventually a good documentation can save you time, work and frustrations on developing a new project.**

Starting to contribute to an open-source project is always recomended to someone who is studing to be a programmer. 
Many say that it is an excelent way to show your potential as a developer and 
some companies ask about these contribution as a way of evaluation. 
Besides that, when you contribute to an open-source, you are helping and adding value a community that is 
committed in creating solutions that broad and open (thus, it is beautiful ❤).

However, everytime I asked people how could I start contributing (and everytime people asked how they could start) I always 
got/said something like "look for a project on Github". Big news! What project? Who? Why? What the hell should I be doing?
I am in a process to contribute more to open-source projects and so I did this small guide on how to help a project based on 
a contribution I wanted to do in a project.

A thing I wanted you to have in mind is that contributing not always means coding. There are several pull requests¹ that need revision, issues (new problems/tasks) that need to be created so that developers know whats wrong with the code and there is 
documentation that needs help. Today, we will focus on a documentation case.

**¹Pull requests are visualizations on what was changed on a code when someone is trying to add new code to an existing repository. From GitHub documentation: Pull requests let you tell others about changes you've pushed to a repository on GitHub. Once a pull request is opened, you can discuss and review the potential changes with collaborators and add follow-up commits before the changes are merged into the repository.**

I know that for someone that is eager to program, this can be awkward and even discouraging. But writing documentation is extremely helpful. Many cool projects have documentation flaws or even inconsistent. And there is more: to write a good 
documentation it is necessary, many times, to spend hours reading the project code to understand what it does and to 
explain to users in a didatic way. So, let's make a contribution together? Come with me :)

## Create your own project

Simple tip: it is *extremely* difficult to contribute to a project that you have no idea how it works. So, try make a side project. Big or small, with big purposes or not, it doesn't matter. Start creating something that gets your attention. Big chances that you will need new modules and libraries to develop your project. Add value to a project you use (and see its value) 
is something much more motivating then chosing a random project. In my case, I started a small project using Django and I wanted 
to add some authentication through Github. 

## Having a hard time

I found out that the most used module for this case was the [social-app-django](https://github.com/python-social-auth/social-app-django)
from [python-social-auth](https://github.com/python-social-auth/social-app-django). I read the documentation 
and was very confused. I didn't understand what I was supposed to do nor how to do it. I struggled a lot and when through the 
code to try to figure it out. Finally, I found [an article](https://github.com/python-social-auth/social-app-django) from Vitor Freitas that explained me everything step-by-step. 
Beautiful, it worked! I saw an opportunity to improve the documentation. I was handling other features that weren't in 
the article nor in the documentation. Hey! I can contribute to this!

## Getting more information

Big projects usually have a file to indicate how the expect your contribution to occur. In the social-app-django, I found the 
contribution guide:

<center>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/img/posts/contribute.png" style="height:300px;"/>
</center>
<br/>

From that document, I knew that the first step was having a Github account (check!), add an issue explaining the problem and, 
if possible, make a project fork. Let's go!

## Creating a new issue

I followed the instructions and added a new issue explaining my problem, showing the article and saying that I was willing 
to make this contribution to the project. If it was a code problem, it would be recommended to say which steps you did to 
bump into this problem. Add screenshots from your screen showing the error you got. 

It is always important to remember that, even though you are willing to help, managing a project like this is usually 
complex. In most cases, people do this kind of open-source projects on their free time. They are not always with time (and 
will) to read a bad written issue or a unexplained issue. Be polite, helpful and don't get angry if you doesn't get an answer
anytime soon!

In my case, I explained my problem, showed where I got help and said that I could help with this.

<center>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/img/posts/contribute2.png" style="height:300px;"/>
</center>

<center>
<i><a href="https://github.com/python-social-auth/social-app-django/issues/44">My issue</a> showed where I found the solution and shows I am willing to contribute</i>
</center>
<br/>


I waited for 8 days but got no answer. Decided to go forward. 

## Creating a fork

I realized that the documentation was not on the same repository as the code, but on a special repository called **social-docs**.
So, I made a fork from this repository. A fork means that I will have a copy of the repository in [my Github account](https://github.com/leportella/social-docs). This way, I can create new branches, make changes and send pull-requests.

Then I found two parts of the documentation that I would like to improve. I would like to contribute to the general documentation 
for the [Django module](https://github.com/python-social-auth/social-docs/blob/master/docs/configuration/django.rst) and the chapter on [Github authentication](https://github.com/python-social-auth/social-docs/blob/master/docs/backends/github.rst). 

## Changing the files

We should create a separate branch from the master branch, so I can make the changes. This branch will be used to create 
a pull-request on the original repository.

I read the documentation, understood the code and finally, I was able to make two pull-requests, one to each document.

My open-source contribution was done!

<center>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/img/posts/contribute3.png" style="height:300px;"/>
</center>
<br/>

This may seem simple to some people, but it took me some hours of code reading trying to figure this out. Luckly, this 
small contribution will help other developers and they will be wuicker on make things done. So, if you have no ideia how 
to contribute, how about helping documentation? Did you got stuck somewhere because of wrong docs? Why don't you help them?

<center>
<h3>If you want to go fast, go alone. If want to go far away, go together (African Proverb)
</h3>
</center>

<center>
  <img src="https://cdn-images-1.medium.com/max/800/1*b4np_iaYln1XAJVu_ttd9w.gif" style="height:400px;"/>
</center>
<br/>