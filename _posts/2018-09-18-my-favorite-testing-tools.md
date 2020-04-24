---
layout: post
title: "My favorite testing tools on Django"
categories:
  - english
tags:
  - en
  - open-source
  - python
  - testing
  - code
  - Django
  - pytest
  - factory-boy
featured-img: board-chalk-chalkboard-459793
permalink: testing-tools-django.html
redirect_from: /english/2018/09/18/my-favorite-testing-tools.html
last_modified_at: 2018-09-18T12:28:52-05:00
---

In today's development, tests are a fundamental tool for keeping things nice and easy and to keep programmer's sanity. 
I've been 
using a set of tools for developing my web applications with Django and it is time for me to share a little bit about them.

![](https://media.giphy.com/media/12WhkSmwGOGIUM/giphy.gif)

*Changing an untested code*

To exemplify what we can do with the tools, we will use a Django project that has the two models below: 
`Student` and `Parent`. `Parent` is a simple model with only two attributes, but `Student` has a lot of attributes to fill in.


```python
from django.db import models


class Parent(models.Model):
    first_name = models.CharField(max_length=200)
    last_name = models.CharField(max_length=200)


class Student(models.Model):
    first_name = models.CharField(max_length=200)
    last_name = models.CharField(max_length=200)
    address = models.CharField(max_length=300)
    resume = models.TextField()
    age = models.PositiveSmallIntegerField()
    email = models.CharField(max_length=250)
    date_started = models.DateTimeField()
    gender = models.CharField(max_length=200)
    parent = models.ForeignKey(Parent, related_name='children',
                               on_delete=models.CASCADE)
```

Remark: The full project code is available [here](https://github.com/leportella/testing-tools) for reference :)


## Lazy shell

![Lazy](https://media.giphy.com/media/UiCSHJrkCr02Y/giphy.gif)

Once we have our model setup and ou database up and running, we can check what's hapenning on our database using Django 
default shell by running (using [pipenv](https://pipenv.readthedocs.io/en/latest/)):

`$ pipenv run school/manage.py shell`

After this, shell prompt is available for us. 
We can them import our model `Parent`, for instance, and check how many instances we 
already have on our database. Since we just started things up, no entries so far.

Now imagine that you are working with a big project 
with lots of model classes to import. 
Importing every single model class becomes kind of a boring and time consuming task. It's time for something better.

<img src="https://i.imgur.com/OygSiVi.png" height="500" style="max-width: 50%" />

I use [django_extensions](https://github.com/django-extensions/django-extensions) to help me deal with this. By default all 
model classes are imported. It has a lot of cool stuff too but, for me, 
just the imports are enough to make it crucial for me to use 
it on day-to-day development.

Once you've installed the lib, just add it to the INSTALLED_APPS:

```python
# settings.py

INSTALLED_APPS = [
    ...

    # 3rd parties
    'django_extensions', 
]
```

To access your new improved shell, just type:

`pipenv run school/manage.py shell_plus`

Done and done... on the first line you can already check how's many instances you already have:

<img src="https://i.imgur.com/uZ4lsSj.png" height="300" style="max-width: 60%" />

## Configuring Pytest

To test your code you should really, really look into [pytest](https://docs.pytest.org/en/latest/), which is a great tool 
specialized in tests. Pytest is a framework, full of extensions and tricks that are way far the scope of this post. 
Bruno Oliveira just launched his [new book](https://www.packtpub.com/web-development/pytest-quick-start-guide) to 
help you learn it :) Check it out if you like what you see here, ok?

To configure `pytest` to work with Django, create a file `pytest.ini` in the same folder of `manage.py`. 
The file must be similar to this:

```python
# pytest.ini
[pytest]
DJANGO_SETTINGS_MODULE = school.settings
python_files = tests.py test_*.py *_tests.py
```

Don't forget to change `school` to your project's name, ok? :) 

Let's try it out?

`$ pipenv run pytest`

The result is pretty but no tests were found...

<img src="https://i.imgur.com/uVwLcSG.png" height="280"/>

Let's check if it's working... create a folder `tests` on our app `student` and add a file `tests.py`. Don't forget to add an 
empty file `__init__.py` on that same folder, so `pytest` is able to found the folder. In the `tests.py` file, we create 
a simple test that will fail for sure, just to get things going...

```python
# student/tests/tests.py

def test_something():
    assert True == False
```

Run again and... voilá! It found the test and a red alert is printed in our screen.

<img src="https://i.imgur.com/bQVyEFA.png" height="230"/>


If we fix the test to assert a true comparisson, then everything is clean and green:

<img src="https://i.imgur.com/ltr7cL1.png" height="250"/>


## Lazy records

To have something to test, let's use a simple endpoint example that shows details of a specific student. We will use 
the lib `restless` to this, the same one I showed how to write endpoints on 
[this post.](https://leportella.com/english/2017/04/03/make-endpoints-using-restless.html) 


You can see on the `preparer` variable that we will only show our student first name on the api response:

```python
from restless.dj import DjangoResource
from restless.preparers import FieldsPreparer

from .models import Student


class StudentResource(DjangoResource):
    preparer = FieldsPreparer(fields={
        'name': 'first_name',
    })

    def detail(self, pk):
        return Student.objects.get(pk=pk)
```

Let's test our brand new endpoint... and we have a problem: there's nothing on our database to test the response:

<img src="https://i.imgur.com/d6LEYcq.png" height="150"  style="max-width: 60%"/>

We can open our brand new `shell_plus` and start adding stuff. Well, `Student` depends on a `Parent` instance, 
so first we add a new parent. We can't forget to save it, otherwise it won't work 
(trust me, I did this while writing this). Now we have a ton of 
information we have to come up with to make it a new database record. And again... don't forget to save it.

<img src="https://i.imgur.com/wDXQU7i.png" height="200"/>

Now we could manually test our api:

<img src="https://i.imgur.com/ODgKrIF.png" height="150" style="max-width: 60%" />

So things are working but things are pretty manual. 
As a project goes bigger, testing things becomes more and more difficult and more instances are requeried 
to test multiple features in this complex world. Thus, writing everything by hand truly doesn't seem like a good approach.

I use a lib called `factory-boy` to come up with this information for me. Let's start with the `Parent` model. We create a 
class called `ParentFactory` that inherits the `factory.DjangoModelFactory`. On the `class Meta` we tell this factory, which 
model will this factory refer to. Now we take all atributes of the model and add it on this factory 
using this `factory.Faker`. Yes, it will come up with names in a coherent manner and you don't have to worry 
about names or remember a cool character from Game of Thrones to add its name here.

```python
# student/tests/factories.py

import factory
from student.models import Parent

class ParentFactory(factory.DjangoModelFactory):
    class Meta:
        model = Parent

    first_name = factory.Faker('first_name')
    last_name = factory.Faker('last_name')
```

Now let's see... we start with no record of `Parent` on our database. After we instantiate our `ParentFactory` we have a new
record saved on our database. Pretty nice! And you can see now that this new parent is called `Karen Palmer`, that is, our 
factory created a new instance on the database with a normal name (not just a bunch of letters together).

<img src="https://i.imgur.com/OJdQkyh.png" height="200" style="max-width: 70%" />

Now we can do the same with the `Student` class. `Factory boy` have a lot of tools that can help you on this task: 
`Fakers` for first name, last name, address and text, random integer, create emails based on the instance first and last name, 
and random choice for the gender. 

We need not only a lot of information, but we also need another instance from the database. 
For this, we add it as a `Subfactory` of the 
`ParentFactory`. This ensures that everytime a new `Student` instance is created, a `Parent` instance is created with it to 
fill this necessity. 

```python
class StudentFactory(factory.DjangoModelFactory):
    class Meta:
        model = Student

    parent = factory.SubFactory(ParentFactory)
    first_name = factory.Faker('first_name')
    last_name = factory.Faker('last_name')
    address = factory.Faker('address')
    resume = factory.Faker('text')
    age = fuzzy.FuzzyInteger(6, 12)
    email = factory.LazyAttribute(
        lambda o: f'{o.first_name.lower()}.{o.last_name.lower()}@mail.org')
    date_started = fuzzy.FuzzyDateTime(datetime.now(tz=utc))
    gender = fuzzy.FuzzyChoice(['male', 'female', 'other'])
```

Now let's test it. We create a new `Student` and although our database already have a `Parent` instance, the factory 
create a new parent to associate it with this new instance of `Student` that was just created. 
And notice that it doesn't get the old `Parent` we created earlier, it is a brand new instance:

<img src="https://i.imgur.com/kiMtm4t.png" height="200" style="max-width: 70%" />

Now, if we want to create a sibling for this previous student, we only need to pass to 
the new `StudentFactory` will will create an already created instance 
of `Parent`. This way, it will not create a new instance but rather add the instance you just passed to it. 
Now we kept the same number of parents we already had on our database but now we have two students with the same 
parent:

<img src="https://i.imgur.com/VhvUavi.png" height="200" style="max-width: 70%" />

So, this already makes our life pretty easy... but there's more! It can also create batches of instances. So you can 
actually populate your database in a single line!

<img src="https://i.imgur.com/ky1zjCC.png" height="300" style="max-width: 70%" />

Lazy as can be, right? :)

![](https://media.giphy.com/media/l0MYtTptyL8h88UHm/giphy.gif)

## Automated test of the api

We tested our endpoint manually, but that's not the right way to do this. We have to create a unitary test to make sure 
it is working now and it will be in the future when we add more stuff to our project. To test the endpoint we will need to
use Django client (or something similar). To use a client with `pytest` is 
super hard. Just kidding :) We actually just have to install `pytest_django` and that's it. 

![](https://media.giphy.com/media/l0K4jrpWppNAAzucU/giphy.gif)

Now, we just create a test and pass a parameter `client` to the function. That's it. Pytest will work his magic for you. 
Now we have our client up and running 
for testing. On this test we create a new instance of `Student`, used the `client.get` method to access the endpoint. The url 
is mounted using the id of the instance we just created.

For now, let's just make sure that our response get's a `200` code:

```python
# student/tests/tests.py

import pytest
from .factories import StudentFactory

@pytest.mark.django_db
def test_endpoint(client):
    student = StudentFactory()
    response = client.get(f'/api/{student.id}', follow=True)
    assert response.status_code == 200
```

Did you notice that above our test function we had a decorator `@pytest.mark.django_db`? This is a helper and it
is necessary to mark a 
that this test function is requiring the database. It will ensure the database is set up correctly for the test. More 
information on helpers can be seen [here](https://pytest-django.readthedocs.io/en/latest/helpers.html).

We can also load the response content using the lib `json` and be sure that the name the api returned is the name we 
actually want. 

```python
import json
import pytest
from .factories import StudentFactory

@pytest.mark.django_db
def test_endpoint(client):
    student = StudentFactory()
    response = client.get(f'/api/{student.id}', follow=True)
    content = json.loads(response.content)
    assert response.status_code == 200
    assert content['name'] == student.first_name
```

Now imagine that we have several tests that need this student record to be created. It is a waste to keep creating it over 
and over again. Pytest allows you to create `fixtures`, which are functions that can be added on you test. 
For instance, in the code bellow you see that we create a fixture that return the instance. To use it on our test, just add 
`user` (the function name) as an input parameter of our test. The only thing is that the client must be the first parameter.
Now, we don't need to create this student anymore in any other line. 
The student instance will be available on this and any other test that add the function `user` to the test function.

```python
import json
import pytest

from .factories import StudentFactory

@pytest.fixture
def user():
    return StudentFactory()


@pytest.mark.django_db
def test_endpoint(client, user):
    response = client.get(f'/api/{user.id}', follow=True)
    content = json.loads(response.content)
    assert response.status_code == 200
    assert content['name'] == student.first_name
```

And if you ever need to use `ipdb` with it, just run `pytest` with an extra parameter: `pipenv run pytest -s`

This is it... with the combo `django_extensions` + `factory boy` + `pytest` testing becomes a really fun thing to do :)

![](https://media.giphy.com/media/aiE3JQU3vLqTK/giphy.gif)
