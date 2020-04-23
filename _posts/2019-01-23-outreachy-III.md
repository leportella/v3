---
layout: post
title: "Outreachy Report III"
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
permalink: outreachy-III.html
redirect_from: /english/2019/01/23/outreachy-III.html
last_modified_at: 2019-01-22T11:48:52-05:00
---

<center><img src="https://cdn-images-1.medium.com/max/1600/1*OsCmvuJ-lLeC7UtWK8CkNA.png" style="height:300px;"/></center>

Hello there! This is the third report on my work on [Outreachy](https://www.outreachy.org/) internship program. If you want to be fully updated on what I have done so far, I recommend you to read the [first](https://leportella.com/english/2018/12/12/outreachy-I.html) and [second](https://leportella.com/english/2019/01/11/outreachy-II.html) reports. 


## Eyes on the prize

If you saw my previous posts you probably know that the goal of this internships is the following:

> Build better native user management features into JupyterHub. 

[Some of the things we want to get on this authenticator](https://github.com/jupyterhub/outreachy/blob/master/ideas/native-jupyterhub-user-management.rst) are:

* Username / Password based sign-up
* Administrator approval for new users (optional)
* Password can be reset by user or admin.
* Throttling of login attempts
* Temporarily / permanently deactivating a user account
* Password strength meter on signup



I always keep remembering me what tasks I've done so far and what I want to be done by the end of my internship. This always helps me when dealing with developing something.

<center><img src="https://media.giphy.com/media/3oz8xvhl6mTmF4MJI4/giphy.gif" style="height:200px;"/></center>


So far the following things were added to Native Authenticator (details can be seen on the first two reports):

- Created a minimum structure;
- Test the authenticator;
- Add checks on pull request (Circle CI with code style, tests and code coverage);
- Created [an official documentation](https://native-authenticator.readthedocs.io/en/latest/) using [Read the Docs and Sphinx](https://docs.readthedocs.io/en/latest/intro/getting-started-with-sphinx.html);
- Added some [badges](https://shields.io/) to my README, so people can easily see how my project is looking so far;
- Added a sign up page, so users can create a new account and be able to login;
- Improved the `authenticate` method, so it actually checks if the user trying to log in has already made a sign up.

Until the last report, I didn't have a proper cicle going. Any user that did a sign up, automatically could enter the system. On this cicle, I focused on creating a more complete flow.

## Native Authenticator flow so far

What I created until now is a full cicle that I will give you an idea on this topic. The main details of implementation will be on the next topic.

Assume the system is working and there is a new user to access the system. First, the person must make a sign up. To do this, he/she can go to `/signup` and create a new user. So far the sign up is pretty simple: you must create a username and a password.  

<center><img src="https://i.imgur.com/BwsTaL8.png" style="height:250px;"/></center>

The user, by default, won't be able to access the system. The user is created in the system, but it is still unauthorized.

<center><img src="https://i.imgur.com/ZP0BedM.png" style="height:200px;"/></center>

To authorize and manage users, an admin must enter the `/authorize` area. This area shows users that are allowed to log in the system as green rows and unauthorized users as blank rows. The buttons on each row handles the authorization of each user.

<center><img src="https://i.imgur.com/gWzRo8y.png" style="height:200px;"/></center>

Once the admin authorizes the user, the user will be able to access the system by going to the home (`/`) page and logging in normally.


<center><img src="https://i.imgur.com/1R4Uwha.png" style="height:400px;"/></center>

There is one exception to this default behavior of needing authorization for logging in. If the username that made the sign up is listed as an admin on the config file, it will automatically have authorization to the system. You can add admins by adding this line on your config file:

`c.Authenticator.admin_users = {'leportella', 'admin'}`

## Talk code to me

<center><img src="https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif" style="height:300px;"/></center>


The first thing I did after I had the sign up cicle going on was to [create the authorization flow](https://github.com/jupyterhub/nativeauthenticator/pull/29) I just showed you. 

First I had to create a new endpoint for the authorization area. This was made with a new handler called `AuthorizationHandler` that have a `get` method that would return an html template (`autorization-area.html`).

After that I had to make the endpoint available only for admins, since we don't want anyone with access to user management. JupyterHub already has a decorator `@admin_only` that does all the job for you. I had to put the decorator on the methods I wanted to be accessed only by admins.

Then I needed the authorize area to show the list of users and their current status. To do this I had to pass to the template all users listed in my database (`self.db.query(UserInfo).all()`). Once I had all the users on my template, I used [Jinja](http://jinja.pocoo.org/) magics to create a new row in my table for every user in the list.

I passed the list of users to my templaye as a variable called `user`. Thus, I could use a `for` (in Jinja sintax) to create new rows for every user. Notice that inside the double brackets (`{{ variable }}`) I can access and use the `User` object as I would in any Python code:

```python
# autorization-area.html

{% for user in users %}
    <tr>
        <td>{{ user.username }}</td>
    </tr>
{% endfor %}
```

It was also necessary to make all users unauthorized when created, because this should be the default pattern. [On the ORM](https://github.com/jupyterhub/nativeauthenticator/blob/master/nativeauthenticator/orm.py#L14) I created a new attribute called `is_authorized` that is always created with a `False` value. On the method of creating new users, I added a verification to see if the user created is listed as an admin, and set the `is_authorized` variable to `True` if it is.

Then I added a verification on the `authenticate` method, to see if the user not only has a correct password, but if it has the `is_authorized` attribute set to `True`.

And this was it: We had the whole flow described on the images 🙃.

## Let them choose!

<center><img src="https://media.giphy.com/media/3oEhmDF8PZDtSNjFAI/giphy.gif" style="height:250px;"/></center>

A thing that I really wanted was to give choices to our users. We wanted a lot of configurable features to help make **Native Authenticator** useful for a lot of people with different needs. During this period I worked on one option: [password strength](https://github.com/jupyterhub/nativeauthenticator/pull/31). 

The first thing that came in my mind when trying to check for stronger passwords was to check for both upper and lower cases and numbers. First I used [traitlets](https://github.com/ipython/traitlets) to add a new variable that would be defined on the config file:

```python
from traitlets import Bool

class NativeAuthenticator(Authenticator):

    check_password_strength = Bool(
        config=True,
        default=False,
        help=('Creates a verification of password strength '
              'when a new user makes signup')
    )
    
    ...

```

`check_password_strength` would be a traitlet defined on the config file (`config=True`) that by default would be `False`. Then I added the check for uppers, lowers and numbers on my `authenticate` method. 

However, [Yuvi pointed out](https://github.com/jupyterhub/nativeauthenticator/pull/31) that this is not the safest way of making sure we have strong passwords. He then recommended [this post](https://auth0.com/blog/dont-pass-on-the-new-nist-password-guidelines/) on how to assure strong passwords. Basically it suggests 3 things:
   
* Forbid commonly used passwords;
* Don't use password hints or knowledge-based authentication
* Limit the number of password attempts

So, instead of simply adding a boolean field to check for stronger passwords, I created two separate options: 

* `check_common_password`: if this variable is set to `True`, when a user creates a new password, it will check [this list of 10 thousand commonly used credentials](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Common-Credentials/10-million-password-list-top-10000.txt) to see if the password is common. If it is, it won't allow the user to make the sign up.

* `minimim_password_length`: an integer that will block users to set passwords smaller than this size. 

Now we have two additional configuration that can be used by adding one or both of the following lines:

```python
c.Authenticator.check_common_password = True
c.Authenticator.minimum_password_length = 10
```

## Improving tests

While doing thease features, I added several tests. I had to test authentication in some scenarios:

* When user gives a wrong username
* When user gives a wrong password
* When user inserts the right username and password but is not authorized
* When user inserts the right username and password but is authorized

For each scenario I added a test. This is good in terms of system but is a lot of tests **really** similar: I had to instantiate the authenticator, create a user and test the `authenticate` method with several setups.

```python
async def test_authentication_succeed(app):
    auth = NativeAuthenticator(db=app.db)
    user = auth.get_or_create_user('John Snow', 'password')
    UserInfo.change_authorization(app.db, 'John Snow')
    response = await auth.authenticate(app, {'username': 'John Snow',
                                             'password': 'password'})
    assert response == user.username
```

Then I used one of the amazing magics [pytest](https://docs.pytest.org/en/latest/) has which is a fixture called `parametrize`.

Basically you pass a list of inputs your test will receive as a string with names separated by comma. Like this: `"username,password,authorized,expected"`. In this case, our test function will receive 4 additional parameters. This paramaters will be received first by the test function (before any other fixtures), like this:

```python
async def test_authentication(username, password, authorized, 
                              expected, app):
    pass
```

Then you can create tuples containing the values for each variable, for different scenarios. Such as the following which has a username `name`, a password `123`, not authorized (`False`) and expected a `False` result. 

```python
 ("name", '123', False, False)
```

You can create as many scenarios as you want. All this will be put together like this:

```python
@pytest.mark.parametrize("username,password,authorized,expected", [
    ("name", '123', False, False),
    ("John Snow", '123', True, False),
    ("Snow", 'password', True, False),
    ("John Snow", 'password', False, False),
    ("John Snow", 'password', True, True),
])
async def test_authentication(username, password, authorized, 
                              expected, app):
    pass
```

This will run the test `test_authentication` over and over again, replacing each parameter by the values you defined in the list. Thus, instead of adding a single test for each scenario, I wrote 1 test that is repeated several times with different scenarios. Less code, less problem mantaining it :)

<center><img src="https://media.giphy.com/media/12NUbkX6p4xOO4/giphy.gif" style="height:250px;"/></center>

The full result is this:

```python
@pytest.mark.parametrize("username,password,authorized,expected", [
    ("name", '123', False, False),
    ("John Snow", '123', True, False),
    ("Snow", 'password', True, False),
    ("John Snow", 'password', False, False),
    ("John Snow", 'password', True, True),
])
async def test_authentication(username, password, authorized, 
                              expected, app):

    auth = NativeAuthenticator(db=app.db)
    auth.get_or_create_user('John Snow', 'password')

    if authorized:
        UserInfo.change_authorization(app.db, 'John Snow')
    
    response = await auth.authenticate(app, {'username': username,
                                             'password': password})

    assert bool(response) == expected 

```


That's all for today! :)