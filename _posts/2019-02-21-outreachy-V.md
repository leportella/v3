---
layout: post
title: "Outreachy Report V"
categories:
  - english
tags:
  - english
  - python
  - jupyter
  - outreachy
  - open-source
  - development
  - girls in tech
  - pyladies
  - project jupyter
  - internship
  - newbie
  - SQLAlchemy
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
featured-img: write
permalink: outreachy-V.html
redirect_from: /english/2019/02/21/outreachy-V.html
last_modified_at: 2019-02-21T11:48:52-05:00
---

<center><img src="https://cdn-images-1.medium.com/max/1600/1*OsCmvuJ-lLeC7UtWK8CkNA.png" style="height:300px;"/></center>


This is the fifth post on my internship on the [Outreachy Program](https://www.outreachy.org/) with [Project Jupyter](https://jupyter.org/). The previous posts are available and should be read in order if you want to understand the big picture:

* [Outreachy I](https://leportella.com/english/2018/12/12/outreachy-I.html)
* [Outreachy II](https://leportella.com/english/2019/01/11/outreachy-II.html) 
* [Outreachy III](https://leportella.com/english/2019/01/23/outreachy-III.html) 
* [Outreachy IV](https://leportella.com/english/2019/02/05/outreachy-IV.html)

## Increasing documentation

<center><img src="https://media.giphy.com/media/XIqCQx02E1U9W/giphy.gif" style="height:200px;"/></center>


Native Authenticator was pretty advanced, but we still needed more information available on the documentation. And this got me thinking: what is relevant to make a good documentation? I know some good examples of a good documentation and an big amount of awful ones. I read [an article in portuguese](https://medium.com/magnetis-backstage/como-escrever-boas-documenta%C3%A7%C3%B5es-b36131d78f7c) on how to do a good documentation but it still didn't click to me.

This is something particularly weird for me: I love writing technical blog posts but I don't think I am good writing documentation. 

For doing a good documentation I did three things:

### 1. Added "big pictures" sections

I just let [the first page of the documentation](https://native-authenticator.readthedocs.io/en/latest/) have the description of what Native Auth has of features (both optional and default). After that, I created a session called [Default Workflow](https://native-authenticator.readthedocs.io/en/latest/quickstart.html#default-workflow) that gives an idea how the authenticator works in the default manner. I used the fluxograms I created for my blog posts to make it easier to have an idea how it works. 

### 2. Added documentation on basic things

I documented then somethings that are, in a way, trivial, but people would need to go to the code to see it. This included [how to create new users](https://native-authenticator.readthedocs.io/en/latest/quickstart.html#adding-new-users), [restrictions that the authenticator imposes](https://native-authenticator.readthedocs.io/en/latest/quickstart.html#usernames-restrictions) and links to other features available ([authorization area](https://native-authenticator.readthedocs.io/en/latest/quickstart.html#authorize-or-unauthorize-users) and [change password endpoint](https://native-authenticator.readthedocs.io/en/latest/quickstart.html#change-password)).

### 3. Added optional features

I created [a single document with all optional features we have](https://native-authenticator.readthedocs.io/en/latest/options.html), since it is not a great amount of them and the description for each are kind of short. I added more fluxograms everywhere I thought was relevant, most of them derived from the ones I used in previous blog posts.

...

I thought that the result was good enough, but please feel free to either contribute or open an issue if you see any possibility of improvement! 🤓


## People are using it

Something weird started to happen this month... people were **using** the project.

<center><img src="https://media.giphy.com/media/l4Ho0At2UD2d7WyD6/giphy.gif" style="height:200px;"/></center>

A person [reported problems with installation](https://github.com/jupyterhub/nativeauthenticator/issues/45) and another opened a [pull request that tried to fix that bug](https://github.com/jupyterhub/nativeauthenticator/pull/51). 

Then I had [another pull request](https://github.com/jupyterhub/nativeauthenticator/pull/52) to review and I asked the person to break it into two new pull requests, to make things easier to manage.

This was pretty exciting! I haven't even launched the library yet and there are people using something I did! Awesome!


## Launched Native Authenticator on Pypi!

Documentation in order, we could launch the library on [Pypi](https://pypi.org/)!

I followed [this instructions to deploy it](https://setuptools.readthedocs.io/en/latest/setuptools.html#distributing-a-setuptools-based-project). However, I didn't understand (at that time) why would we should put the packages on [TestPypi](https://test.pypi.org/) instead of the Pypi. 

<center><img src="https://media.giphy.com/media/D2kFkQwMzFcVq/giphy.gif" style="height:300px;"/></center>

I launched the version 0.0.1 and the problems started: it couldn't find the templates. I tried the distributions locally and everything worked, I launched it on Pypi... nothing. I thought "that's ok! I delete the packaged from the Pypi and add it again with the same version number". Guess what? You can't. That's *why* you have TestPypi. Dãã. Seriously: don't launch on Pypi unless you are absolutely sure it is stable. And use TestPypi to do this 😅. 

Finally, the setup for adding files was this: I added the line `include_package_data=True` on the `setup.py` and a [Manifesto.in](https://github.com/jupyterhub/nativeauthenticator/blob/master/MANIFEST.in) to list the filed I want to be with the package. By the way, the last **stable** version is actually number 0.0.4.

[This is the documentation on adding packages on a library](https://setuptools.readthedocs.io/en/latest/setuptools.html#including-data-files), because *theoretically* the way I did is not the only way.

## Writing about the project

I also spent a couple days working to write an official blog post on the [Jupyter Blog](https://blog.jupyter.org/) to share the new authenticator to the world. 

The article is still under revision, but it will be out soon (maybe this week). You simply **can not** imagine how happy I was when I saw this on [my Medium account](https://medium.com/@leportella):

<center><img src="https://i.imgur.com/hJIRENn.png
" style="height:100px;"/></center>

 This is something amazing for me and truly an honor. 

<center><img src="https://media.giphy.com/media/DYH297XiCS2Ck/giphy.gif
" style="height:200px;"/></center>


## Starting to add the project to The Littlest JupyterHub

I started to get in touch with [The littlest JupyterHub](https://github.com/jupyterhub/the-littlest-jupyterhub) (TLJH) which is distribution of JupyterHub for 1 to 50 users in a single server.

[We will add Native Authenticator as the default authenticator for TLJH](https://github.com/jupyterhub/the-littlest-jupyterhub/issues/264), so I needed to understand better how this distribution works. 

I started by [adding the Native Auth as an option for people that uses TLJH](https://github.com/jupyterhub/the-littlest-jupyterhub/pull/284), which included default instalation and some docs. However, one thing that is missing to change authenticators, as pointed out by Yuvi's issue, is a way to import users from the default authenticator to Native Authenticator. We don't want people to loose access when changing the authenticator, right?

I'm still working on this, so you'll know more about it in the next report!

## Going to Jupyter team meet in Washington DC

An **amazing** thing that happened to me this month was knowing I'll be able to attend the Jupyter in-person team meeting that will happen in Washington DC! It will be a full week of working with people that admire so much! I am counting the days for the next 3 weeks 🙃

<center><img src="https://media.giphy.com/media/WuGSL4LFUMQU/giphy.gif
" style="height:300px;"/></center>

My internship will end in mid-March, so it will most certainly end in huge style! Not only that, but I get the chance to see [Carol](https://twitter.com/WillingCarol), [3 years after she helped me have a wonderful time at Pycon](https://leportella.com/english/2016/06/07/a-tale-of-a-kingdom-far-far-away.html).

All communication and definitions of the team happens on [this repository](https://github.com/jupyterhub/team-compass) and you will see all the information about the in-person meeting [in this issue](https://github.com/jupyterhub/team-compass/issues/110).  

So, if you like my texts, you probably get one about this week in April or so! I intend to do something similar to [what I did when I went to Pycon 2016 with the conference financial aid](https://leportella.com/english/2016/06/07/a-tale-of-a-kingdom-far-far-away.html).

