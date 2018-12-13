---
layout: post
title: "Outreachy Report 1"
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
featured-img: write
last_modified_at: 2018-12-12T19:48:52-05:00
---

![](https://cdn-images-1.medium.com/max/1600/1*OsCmvuJ-lLeC7UtWK8CkNA.png)

Hi :)

On November I discovered that I was selected for the [Outreachy](https://www.outreachy.org/) internship program for the batch of December 2018 to March 2019. During this period, I'll be working on JupyterHub Project (OMG!), on creating a new JupyterHub Authenticator system and my mentors will be [Yuvi Panda](https://twitter.com/yuvipanda) and [Min RK](https://twitter.com/minrk). If you would like to read more about the project and the participation of Project Jupyter on this internship, you can read more on this [here](https://blog.jupyter.org/outreachy-jupyter-supporting-diversity-in-open-communities-dfa78db4b0bd).

As part of my training, during the next three months, I'll be sharing blog posts about what I am doing, my main struggles and workarounds.

Hope you like it ❤️

![](https://media.giphy.com/media/1136UBdSNn6Bu8/giphy.gif)

*This is me writing this post*

## Overview

When you apply to an Outreachy opening, you must first create a pull request to the project you aim to work with. In my case, I found [this micro task](https://github.com/jupyterhub/outreachy/issues/2) that was available for people that wanted to apply to JupyterHub. . 

The task was well described: I should create an option for users to reset their passwords on the First Use Authenticator. This authenticator is a separate project that you can install on JupyterHub and that allows **any** user to create a user profile on the first login. This was made on [this pull request](https://github.com/jupyterhub/firstuseauthenticator/pull/8), where I added the following things:

* A function that would allow the user to rewrite its password;
* An endpoint that would be redirected to a handler, when called;
* A handler with two methods:
A **GET** method that would return an html template;
A **POST** that would receive a new password and call the right function to do the job;
* A template for the handler.

The end result was this:

<img src="https://i.imgur.com/nWKzwbs.png" height=100>

This job was very close to the main Outreachy project I was applying. Now that I am in, my job is to construct a Native Authenticator that will help admins to manage their users (hopefully) seamlessly. Thus, I will use the knowledge I got previously and expand it :)

## The first guidelines

![](https://media.giphy.com/media/tHufwMDTUi20E/giphy.gif)

On the week before the program starts, I did a call with one of my mentors, Yuvi, to align some of the things we were wishing to accomplish on our first week.

We agreed that, on the first week, I should create a simple authenticator and get things going with code quality and tests before going deeper. One agreement we did was that, on the beginning, I should create pull requests and merge them by myself, to make things go a little bit faster. However, the pull request structure would allow the mentors and Jupyter's developers to give comments and make code reviews even after the pull request was merged. 

We also agreed with weekly meetings for organizing things and that my project would be monitored by a [Github Project](https://github.com/orgs/jupyterhub/projects/1) that would look like a [Kanban board](https://en.wikipedia.org/wiki/Kanban_%28development%29) with columns for **todo**, **doing** and **done**. Every task on the board should have a checklist of things that I should implement. 

All communication with the team developers are made through [Gitter chat](https://gitter.im/jupyterhub/jupyterhub) on a public channel. Private messages are only for personal things, mostly. 

## First tasks

I [started](https://github.com/jupyterhub/nativeauthenticator/pull/4) with a simple script that would inherit from the basic Jupyter Authenticator class and would do nothing at all. I created the `setup.py` file, for installing my package and that was it. I did it as simple as possible, the goal was “just to make it work”. 

Let's talk about the problems I got at first… when I looked at the Dummy Authenticator](https://github.com/jupyterhub/dummyauthenticator), I looked at the `setup.py` file and the authenticator file and that was it. Even though everything seem right, by no means my code would work. After at least 30 minutes trying to debug it, I realized that I assumed the project `__init__.py` file was be empty… and it **wasn't** . So… I learned (the hard way) you should **never** assume something from a file… Specially a `__init__.py` file :)

Also, I had a problem that I simply couldn't make my project work with JupyterHub. After some struggle a the help of my mentors, I discovered that JupyterHub works with a [default Spawner](https://github.com/jupyterhub/jupyterhub/wiki/Spawners) that is pretty complicated to work with from the development point and that I should use the Simple Spawner configuration during development mode. I still now quite sure what are Spawners and I still did not stop to study and understand it, but using the Simple Spawner worked for the moment  😅

Ok… things were finally working although I had not done anything special 😅

Once my basic scheme was working [I continued working on my Authenticator class. ](https://github.com/jupyterhub/nativeauthenticator/pull/5/files) I overwrote the `authenticate` method on my class and made it always return the username given. This way, my authenticator would always authenticate, whatever a password was given (this is based on the [Dummy Authenticator](https://github.com/jupyterhub/dummyauthenticator) but even simpler).


Now that I had a method that should do something different, I added a small test case to create my test structure. This was made using, as base, [another pull request](https://github.com/jupyterhub/firstuseauthenticator/pull/9/files) on “First Use Authenticator” made by Min, which helped **A LOT**. At this moment, I also started a small text on the `README.md` file, to guide everyone that would like to test what I was doing. 

Now I had a small structure and tests was time to [add some checks on my pull requests](https://github.com/jupyterhub/nativeauthenticator/pull/7) to assure code quality and that the tests were ok. This was made using CircleCI and, to be frank, it was a complicated work. I had to do several commits, things would fail in the installation steps... It was a mess… I started small (baby steps... always use baby steps), applying only [flake8 ](http://flake8.pycqa.org/en/latest/) to identify PEP-8 problems and code smells and then tests. You can see by the pull request, that I struggled a little. My main problems were:

* I tried to check if I had authorization to add CircleCI to a project that is not mine. Answer: No, but JupyterHub had already done this, so I could just use it by adding a conf file.
* I had indentation problems (number of spaces are not constant on the file o.O) 
* I had permission problems: I had to add **sudo** before any `pip install … `. I tried adding a `--user` flag, but with no success. Left this for later improvements and used `sudo` anyway 😅

![](https://media.giphy.com/media/3oz8xZwLzHuL2vt2yQ/giphy.gif)

My next task was to [create an official documentation](https://github.com/jupyterhub/nativeauthenticator/pull/9), for that I used the [ReadTheDocs Sphinx](https://docs.readthedocs.io/en/latest/intro/getting-started-with-sphinx.html) project to create a initial documentation structure. Added a small *Quickstart* file and [published it](https://native-authenticator.readthedocs.io/en/latest/) using Read the Docs. This was pretty cool! [I tried doing a pull request before the CircleCI phase](https://github.com/jupyterhub/nativeauthenticator/pull/6), but it got so messed up with all the commits I was doing that I decided to start over on a new pull request all fresh an clean.

And that was it for my first week :)

## My first impressions

People that work on the project Jupyter were really friendly and quite patient. I had a nice welcoming and a lot of guidance. Even though I was supposed to be merging my pull requests, I give some time to the other developers to look at it and… surprise! They usually answer and give feedback really quickly. I was really impressed by this.

I was ashamed to have pull requests with lot of commits struggling with small things, so I started doing `amend` commits, to hide this typos and small-fix mistakes. However, Github is now announcing to everybody that you are doing this. I went to my mentors, and they told me I could use as much amend as I like, as long as it doesn't happen on the `master` branch. This is pretty cool, but I still end up with a lot of commits. I think that *getting over* the shame of being someone that struggles with code should be something to work on.

Now, I have to start getting deeper on things and make the authenticator work a little bit more intelligently :)

See ya next week!

![](https://media.giphy.com/media/5bdhq6YF0szPaCEk9Y/giphy.gif)
