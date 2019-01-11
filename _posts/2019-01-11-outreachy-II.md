---
layout: post
title: "Outreachy Report II"
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
featured-img: write
last_modified_at: 2019-01-11T11:48:52-05:00
---

<center><img src="https://cdn-images-1.medium.com/max/1600/1*OsCmvuJ-lLeC7UtWK8CkNA.png" style="height:300px;"/></center>

Hello there! This is the seconde report on my work on [Outreachy](https://www.outreachy.org/) internship program. On the [first report](https://leportella.com/english/2018/12/12/outreachy-I.html) I told a little bit on how I got into the program, which problem I am focusing and my first tasks :).

Now, almost one month after my first text, here I am to tell you more about what have I done and what were my struggles this time!


## Vacation

<center><img src="https://media.giphy.com/media/k7NvI2SBTgA1NuO5Tu/giphy.gif" style="height:300px;"/></center>


Just after I finished my first weeks of work here I was also finishing my moving from Brazil to Ireland. So things were absolutely crazy and I couldn't focus at all. I was pretty nervous to move to another city, country and continent and this was making me unable to focus. We decided that was best for me to take vacations during this stressing period. 

This was fundamental for me. I could do my moving (and my goodbyes) with calm and so I just started working again this year. My deadlines are all pushed a couple weeks forward, but it totally work it. Thank you, Yuvi!

## Finishing my initial setup

I was already with [CircleCi](https://circleci.com/) with [flake8 ](http://flake8.pycqa.org/en/latest/) verification and tests, but there was still things missing until I finished the initial setup I was suppose to.

I should include [Codecov](https://codecov.io/) to my analysis, to check if my new pull requests would always increase the test coverage. Since Codecov was already with authorization to the [JupyterHub](https://github.com/jupyterhub) organization, [I just added the codecov on the circleci file](https://github.com/jupyterhub/nativeauthenticator/pull/12/):

```
  - run: 
      name: Running code coverage 
      command: codecov
```

After that I added badges on the project `README` file. To do this, you should go to [https://shields.io](https://shields.io) and find the badge you would like to add to your project. Then, you should just add your project link for the github like this:

`![![Circle Ci Badge](https://img.shields.io/circleci/project/github/<your-organization-name>/<you-repo-name>.svg)](https://circleci.com/gh/<your-organization-name>/<you-repo-name>)`

I got confused with a lot of  `!` and `[]` but finally I got it to work (and you have a model here, if you need :). Just remember that you must already have CircleCi connected with your project, otherwise it won't work.

I also tried to add more tests to my Handler, but this is way more complicated than I thought, and I was told to let it go for now.

<center><img src="https://media.giphy.com/media/nisRCk7fmAxeLt8S72/giphy.gif" style="height:200px;"/></center>

## Organizing things

After getting too much into code, is pretty easy to loose yourself and your main goal. I took a day to organize what I wanted to do. I looked at the plan I did before entering the internship and broke the activities in issues that had topics to fill. I already put questions where I needed answers and add things I thought it would be nice to have. This simple day made my life **extremely** easy on the next days.


## Add a signup process

I wanted to have a SingUp page, where new users could add information to request access to the system.

Whenever a user entender a path on the browser, the path (or endpoint) will lead to a `Handler` class and for a specific method in that class. If the user makes a `GET` to the page, it will look for the `get` method inside the `Handler` class. This method, will then return a `html` template back to the user. 


<center><img src="https://i.imgur.com/F2AEvCq.png" style="height:300px;"/></center>

[I added](https://github.com/jupyterhub/nativeauthenticator/pull/11/) an endpoint called `/signup` and created a simple Handler that would have only a `get` method that would return a simple `html` page written "signup page". 

Now that I knew that my handler was working, [I added](https://github.com/jupyterhub/nativeauthenticator/pull/14/) a form on my signup template so the user could send me information and a `post` method on my Handler class that would receive the information.

Now I had a full cicle that my user would fill the signup form and I would receive this information. But so far, I haven't done anything with the information. I actually just throw it away ¯\_(ツ)_/¯ .

## Problems with imports

After finishing the process of the new endpoint I realized that my `Handler` class and my `Authenticator` class in a unique file was not working. Things would quiclky be too confusing to handle. Thus, I divided the classes in separate files. 

I had some problems with imports and while studying it a bit, I found [this article](https://chrisyeh96.github.io/2017/08/08/definitive-guide-python-imports.html) which was pretty nice to find! 


## Dealing with users on JupyterHub

[JupyterHub](https://github.com/jupyterhub/jupyterhub) already creates a database to store information from the users and notebooks they are dealing with. Although I saw that [another authenticator](https://github.com/jupyterhub/firstuseauthenticator) used a separated database for keeping information on their users, I decided that was best if I used the same database created by JupyterHub.

This was pretty tricky. I studied the internals of Jupyter trying to figure out where was the database and how the hack could I add some table to it. 

<center><img src="https://media.giphy.com/media/gKsJUddjnpPG0/giphy.gif" style="height:200px;"/></center>

<center><i>Me trying to understand where the database was</i></center>

Finally I gave up and asked Yuvi and... it was on my authenticator class as `self.db` 😅. 

The project uses [SQLAlchemy](https://www.sqlalchemy.org/) as a way to work with a SQL database. Although similar to Django ORM (that I already knew) I had some trouble understanding how [SQLAlchemy](https://www.sqlalchemy.org/) deals with stuff and bumped into [a huge problem that took me days of struggling](https://github.com/jupyterhub/nativeauthenticator/issues/26). I studied SQLAlchemy as much as I could to undersatnd what was wrong and how to do this. This resulted in two blog posts: [one is a tutorial on SQLALchemy basics](https://leportella.com/english/2019/01/10/sqlalchemy-basics-tutorial.html) and [another is a post specifically for the problem I had with creating relationships after the tables were already created](https://leportella.com/english/2019/01/11/sqlalchemy-foreign-key-separate-files.html). 

## Tests

Now that I had implemented a function on the `__init__` method of my authenticator, my tests started to fail. That's because before this I would simple instantiate my authenticator (`NativeAuthenticator()`) and mock the `authenticate` method, like this:

```python
from unittest.mock import Mock
from nativeauthenticator import NativeAuthenticator

async def test_basic():
    auth = NativeAuthenticator()
    info = {'username': 'name', 'password': '123'}
    response = await auth.authenticate(Mock(), info)
    assert response == 'name'                                            
```

I found a library called [alchemy-mock](https://github.com/miki725/alchemy-mock) that could mock a SQLAlchemy database. But talking to [Min](https://twitter.com/minrk) I found that [JupyterHub](https://github.com/jupyterhub/jupyterhub) already has a [MockHub](https://github.com/jupyterhub/jupyterhub/blob/master/jupyterhub/tests/mocking.py#L219) class mocks not only the database on a Python level, but creates the whole JupyterHub structure on a temporary folder. 

With [MockHub](https://github.com/jupyterhub/jupyterhub/blob/master/jupyterhub/tests/mocking.py#L219) and the wonderful of [pytest fixtures](https://docs.pytest.org/en/latest/fixture.html) I created a fixture that would instantiate MockHub and initialize the db for me everytime I run a test:

```python
from jupyterhub.tests.mocking import MockHub
import pytest 

@pytest.fixture
def app():
    hub = MockHub()
    hub.init_db()
    return hub
```

And now I could simple pass the fixture to my test function and use its database without any problem:

```python
```python
from nativeauthenticator import NativeAuthenticator

async def test_basic(app):
    auth = NativeAuthenticator(db=app.db)
    # ...
```

<center><img src="https://media.giphy.com/media/l0MYEqEzwMWFCg8rm/giphy.gif" style="height:200px;"/></center>


## So... what's next?

So far we have:

* A signup page so new users can fill a form
* A way to store information from the signup page (including a hashed password)
* An authenticate method that is checking for user information

Now some features we would like to add:

* Allow admin to allow or block new users that just signed up
* Make authenticate method check if new users have authorization from the admin
* Block users after number of failed logins
* ... much more! Let's not spoil it :)

<center><img src="
https://media.giphy.com/media/3bznFj6OB5381BEjDu/giphy.gif" style="height:250px;"/></center>

See you later for more tales of struggles!


