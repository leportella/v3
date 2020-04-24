---
layout: post
title: "Outreachy Report IV"
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
permalink: outreachy-IV.html
redirect_from: /english/2019/02/05/outreachy-IV.html
last_modified_at: 2019-02-05T11:48:52-05:00
---

<center><img src="https://cdn-images-1.medium.com/max/1600/1*OsCmvuJ-lLeC7UtWK8CkNA.png" style="height:300px;"/></center>

This is the forth post on my internship on the [Outreachy Program](https://www.outreachy.org/) with [Project Jupyter](https://jupyter.org/). [The first](https://leportella.com/english/2018/12/12/outreachy-I.html), [the second](https://leportella.com/english/2019/01/11/outreachy-II.html) and [third post](https://leportella.com/english/2019/01/23/outreachy-III.html) are already available, if you want to understand the big picture.


On [the last post](https://leportella.com/english/2019/01/23/outreachy-III.html) I talked about how we built the complete user cycle of signing up and getting authorization of the user for actually logging in. Then we talked a bit on adding some optional password check features that admins could configure. This time we continue implementing optional features for admins to use on **Native Authenticator**.



## Block user after failed attempts of login

<center><img src="https://media.giphy.com/media/Qt7ZKXj42izpm/giphy.gif" style="height:300px;"/></center>

On the last time, Yuvi recommended me [this post](https://auth0.com/blog/dont-pass-on-the-new-nist-password-guidelines/) about password security guidelines that helped me implement the two options I talked about on the last report: the option to block common passwords on sign up and the option of checking the password length.

One of the things the article recommended was to add a system that would block a user after a certain number of attempts of failed logins. 

This was something that, as simple as sounds, took me a lot of work. Although I'll explain the big picture that I eventually created, but the construction process was not as simple as I am showing you here 🙃. 

The first thing was to construct a function that would add a count everytime the user failed to make a login. It would also store the moment when the failed attempt happened. The idea was the following:

<center><img src="https://i.imgur.com/wcLJ58d.png" style="height:300px;"/></center>

To do this, I created an empty dictionary attribute called `login_attempts`. When I call the function `add_login attempts` on my `authenticate` method, it will check if the user is already in the dictionary. If it is, then it will increase the count and change the time. Otherwise, it will add the dictionary to the user. 

```python
from datetime import datetime

class NativeAuthenticator(Authenticator):

    def __init__(self):
        self.login_attempts = dict()


    def add_login_attempt(self, username):
        time = datetime.now()
        if not self.login_attempts.get(username):
            self.login_attempts[username] = {'count': 1,
                                             'time': time}
        else:
            self.login_attempts[username]['count'] += 1
            self.login_attempts[username]['time'] = time
```

Once the count was working ([tests](https://github.com/jupyterhub/nativeauthenticator/blob/master/nativeauthenticator/tests/test_authenticator.py#L99) helped me on this one 😅), I added another function to the `authenticate` method, that would check if the user is blocked or not. This would allow me to exit the `authenticate` method if the user is blocked, not even let it try to login:   

<center><img src="https://i.imgur.com/cROySZN.png" style="height:350px;"/></center>

Cool. Now I just needed to actually check if the user is blocked or not 😅. 

I had to check if the user had tried at last 3 times and if they had tried multiple times, if they waited at least 10 minutes before trying again. 

So, the main flux of the system is something like this:


<center><img src="https://i.imgur.com/KHhNCrT.png" style="height:350px;"/></center>

So, my `is_blocked` function will go to my dictionary `login_attemps` and get the user info (stored here in the variable `logins`) if the user has no register or if the number of counts is less then 3, the function returns that the user is not blocked. If the the user is blocked then it has to check the time. Since this part is a little bit more complicated, I separated in another function called `can_try_login_again`. 

The `can_try_again_function`, check if the timedelta between the last datetime stored in the dictionary and now. If the number of seconds is smaller then 600 (10 minutes), the user can try again and the fucntion return `True`.  


```python
from datetime import datetime

class NativeAuthenticator(Authenticator):

    def is_blocked(self, username):
        logins = self.login_attempts.get(username)

        if not logins or logins['count'] < 3:
            return False

        if self.can_try_to_login_again(username):
            return False
        return True

    def can_try_to_login_again(self, username):
        login_attempts = self.login_attempts.get(username)
        if not login_attempts:
            return True

        time_last_attempt = datetime.now() - login_attempts['time']
        if time_last_attempt.seconds > 10 * 60:
            return True

        return False
```


Of course, looking more close you can easily imagine that 3 atempts and a 10 minute wait are things that should be an admin's decision! And that's what I did once everything was working! Now the admin can add both configuration on the `config` file:

```python
c.Authenticator.allowed_failed_logins = 3
c.Authenticator.seconds_before_next_try = 1200
```

It may seem simple now that it is done, but thi [took a lot of work](https://github.com/jupyterhub/nativeauthenticator/pull/36) and several commits, but I am really happy with the result


## Option to see the password when typing

<center><img src="https://media.giphy.com/media/lQeGS2LCeZg7m/giphy.gif" style="height:350px;"/></center>


[The same post](https://auth0.com/blog/dont-pass-on-the-new-nist-password-guidelines/) that suggested to check for common passwords and block after failed attempt logins also recommended to add an option to let the user check the password while typing. 

Looking around the problem I realized I would need Javascript to make it work. I look [the base tamplate](https://github.com/jupyterhub/jupyterhub/blob/master/share/jupyterhub/templates/page.html#L61) and realized that Javascript was already being used in `<script>` tags inside the HTML. 

So, the first thing was to add a button on the password input that you would click to see the password. With this ready, I could get the `id` of the element, and change it through the Javascript function:


```html
<div class="input-group">
<input
    type="password"
    class="form-control"
    name="password"
    id="pwd"
    tabindex="2"
    required
/>
<span class="input-group-addon">
    <button style="border:0;" type="button" id="eye">
    👁
    </button>
</span>
</div>
```

I tried a couple approaches I saw online, but everytime I tried to get the `button` element through the JS code, it would return empty. When I tried again on the console, it would return correctly. After a few researches I discovered that the problem was that my code was trying to get the button before the whole document was ready to go. So, in the moment my code tried to get the button, it was actually not there!

I found [this answer](https://stackoverflow.com/a/800010/3538098) on Stack Overflow that really helped me. I should add a function that could only run after the whole document was ready. Like this:

```js
document.addEventListener("DOMContentLoaded", function(event) { 
  //do work
});
```

Once I added this, my simple code worked! What I did was to get my button element through its id:

```js
var button = document.getElementById('eye');
```

Then I added an Event Listener to listen to clicks on the button, which means, everytime the button was clicked, it would call this function:

```js
button.addEventListener("click", somefunction)
```

What the function did was to get the password input element. Then, it would change the attribute `type`. If it was equals `password` (secret) it would change to `text` and vice versa. It also changed the simbol from an open eye (👁) to see the password to a key (🔑) to hide the password.


```js
button.addEventListener("click", function(e){ 
    var pwd = document.getElementById("pwd"); // get input element

    if(pwd.getAttribute("type")=="password"){
        pwd.setAttribute("type", "text");
        button.textContent = "🔑";
    } else {
        pwd.setAttribute("type", "password");
        button.textContent = "👁";
    }
});
```

And that was it! [I had an option](https://github.com/jupyterhub/nativeauthenticator/pull/37) to see the password while typing :)

## Open SignUp 

<center><img src="https://media.giphy.com/media/aCRe3EFA17kWc/giphy.gif" style="height:350px;"/></center>

One option we decided to add was the possibility of an open Sign Up. If you remember [the last post](https://leportella.com/english/2019/01/23/outreachy-III.html), but default if a user signs up it will be blocked to log in the system until an admin authorizes. We wanted to make an option to add the opposite: the user is always authorized just after the sign up until an admin blocks them.  

To add an open sign up, the admin just had to add to the config file:

```python 
c.Authenticator.open_signup = True
```

The solution was pretty simple. On the creation of a new user, I add the authorization if they are in the admin list of users. I did the same if the variable `open_signup` was `True`:

```python
infos = {'username': username, 'password': encoded_pw}
if username in self.admin_users or self.open_signup:
    infos.update({'is_authorized': True})
```

One thing I had to change here were the result messages after the sign up. Instead of two options: *wait for admin authorization* or *error with your password*, now I needed a new option to let the user know if they can already login. To make things more clean, on my `SignUpHandler` I removed the result message from the `post` method and created a separate function, just to handle this and the type of banner color we need.

If the Sign Up depends on Admin authorization:

<center><img src="https://i.imgur.com/QtTLM95.png" style="height:200px;"/></center>

If the Sign Up had problem with the password:

<center><img src="https://i.imgur.com/FIPrW0t.png" style="height:200px;"/></center>

If there is an Open Sign Up and no problem with the password:

<center><img src="https://i.imgur.com/1gUFXwy.png" style="height:200px;"/></center>


## Change user password

If you remember my first post, I talked about how I added an endpoint so an user could change their password when logged in. I used [that pull request]*https://github.com/jupyterhub/firstuseauthenticator/pull/8) as a base to Native Authenticator and created 
[a similar endopoint](https://github.com/jupyterhub/nativeauthenticator/pull/40). Now, users logged in through Native Authenticator can acces `/hub/change-password` and change their password. 


## Username sanitization

[I also added a prohibition of commas and spaces on usernames](https://github.com/jupyterhub/nativeauthenticator/pull/43/files). This is important to avoid errors of typos and copy-and-paste extra spaces. The default `Authenticator` already have a `validate_username` to check for other problems, I just supercribe the method and added the verification I needed.

```python
from datetime import datetime

class NativeAuthenticator(Authenticator):

    def validate_username(self, username):
        invalid_chars = [',', ' ']
        if any((char in username) for char in invalid_chars):
            return False
        return super().validate_username(username)
```

Then I added the validation check on the method where I create the user, along with the password verification :)


## What's next

We still have some things to do, but things are getting polish now. While writing this I realized some tests were missing, the codecov still not working and [there is an issue](https://github.com/jupyterhub/nativeauthenticator/issues/45) to fix some bugs during installation. 

If you are following this reports and want to test and give feedbacks on what I can improve the authenticator, feel free to leave your issue :) 


<center><img src="https://media.giphy.com/media/100QWMdxQJzQC4/giphy.gif" style="height:250px;"/></center>