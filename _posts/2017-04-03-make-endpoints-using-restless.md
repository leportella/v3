---
layout: post
title: "How to make simple endpoints with Django using Restless"
categories:
  - english
tags:
  - en 
  - python
  - Django 
  - restless
featured-img: rest
last_modified_at: 2017-03-09T14:25:52-05:00
---

Most of all Django tutorials teach us how to return HTML as response to a request. Sometimes, it is useful to make it a little more RESTful. 
One option is to use [Django REST Framework](http://www.django-rest-framework.org/) but sometimes you need something a little bit simpler. Then you have [Restless](http://restless.readthedocs.io/). 
Restless is a miniframework made by [Daniel Lindsley](https://github.com/toastdriven) based on what he learned by making [Tastypie](https://django-tastypie.readthedocs.io/en/latest/) and some other REST libraries.

As Lindsley said in the documentation *“While other frameworks attempt to be very complete, include special features or tie deeply to ORMs, Restless is a trip back to the basics”*. 
Here I am going to show how you can create very simple endpoints using Restless.

![](https://cdn-images-1.medium.com/max/800/1*BWYEnAFaPtrWCnpLWJ_gZA.gif)

First, let’s install Restless:

```
$ pip install restless
```

Now, let’s see it in action! I’ll use a simple Django models as example:

```python
class Author(models.Model):
    name = models.CharField(max_length=50)

class Book(models.Model):
    author = models.ForeignKey(Author)
    title = models.CharField(max_length=250)
pages = models.IntegerField()
```

To make an endpoint that lists all of the instances of **Book** you need to create a file to add a class called Resource. Here, we will assume that this file will be **api.py**.

Resources are classes that have methods (implemented by you) for all the main HTTP methods. In our first example, we will write the method named **list**, which represents the method GET.

```python
# On the api.py...
from restless.dj import DjangoResource
from restless.preparers import FieldsPreparer

from .models import Book

class BookResource(DjangoResource):
    preparer = FieldsPreparer(fields={
        'book_author': 'author.name',
        'book_title': 'title',
        'book_pages': 'pages',
    })

    def list(self):
      return Book.objects.all()


# On the urls.py...
from django.conf.urls import include, url
from .api import BookResource

urlpatterns = [
    url(r'^books/', include(BookResource.urls())),
]
```

So, what do we have here? First we define a **preparer**. 
The preparer is where you will define which attributes and properties of your instance will go to our API response. 
The keys on the dictionary you define inside your **preparer** variable can be whatever you want them to be (here, I made them start with a **book_** so you can see they can be whatever). The values in the dictionary, however, must be a **book** instance attribute. If you have a ForeignKey (which is the case of the **author** field here), 
you can access its attributes and properties the same way you would in the related manager.

So, in our preparer we added 3 attributes: one that is related to the author of the book (called inside the preparer as a **author.name**), and two propeties stored on the book instance: **title** and **pages**.

After we define our preparer, we applied a method named **list**. Which simply returns the query in the **Book** model where we get all the instances on my database.

After that, you must include on your **urls.py** the **BookResource.urls** as you would do with any other Django view. For each method on the BookResource class, the Restless try to find a certain kind of url. 
For the **list** method, the typical url will look like **localhost:8000/books/**. 
So, everytime you make a GET on the **localhost:8000/books/** url, your request will be routed to the BookResource and then it will look for the **list** method.

What Restless does under the hood when it gets to the method, is to get every instance that the query returned (the **.objects.all()**), pass it through the **preparer** to create a dictionary of “serializable” information and then serialize it (return it as a JSON).

So, in resume, when you access the **localhost:8000/books/**, the request will be routed to the BookResource class, and since you are doing a GET, 
it will know that it should look for a **list** method. Done. Your API of all books is there, serialized like this (for 2 book instances on your model):

```
{
  "objects":
    [
      {
        "book_author": "First Author",
        "book_title": "First Title",
        "book_pages": 200
      },
      {
        "book_author": "Seoond Author",
        "book_title": "Second Title",
        "book_pages": 124
      },
    ]  
}
```

Piece of cake, hun?

As we have seen before, different methods on Restless leads us to different url patterns. If you try to make a *POST* on the **localhost:8000/books/** url, it will be routed to a **create** method 
on the BookResource class. For a *PUT* request, it will look for a **update** method. However, If you go to **.../books/book_id/** it will stop routing to the list method and start look for a **detail** method, because you are specifying the value of the instance you want to access.


```python
# On the api.py...
from restless.dj import DjangoResource
from restless.preparers import FieldsPreparer

from .models import Author, Book

class BookResource(DjangoResource):
    preparer = FieldsPreparer(fields={
        'book_author': 'author.name',
        'book_title': 'title',
        'book_pages': 'pages',
    })

    def list(self):
      return Book.objects.all()
    
    def detail(self, pk):
       return Book.objects.get(id=pk)
      
    def create(self):
        return Book.objects.create(
            author=Author.objects.get(name=self.data['author']),
            title=self.data['title'],
            pages=self.data['pages']
        )
    
    def update(self, pk):
        book = Book.objects.get(id=pk)
        book.author = Author.objects.get(name=self.data['author'])
        book.title = self.data['title']
        book.pages = self.data['pages']
        book.save
        return book
```

In our case, the *POST* body must have a valid id for an **Author** instance, the title must be a string and the pages must be an integer. 
For more detailed uses, it is recommended to insert verification for the data your user is sending to you :D

![](https://cdn-images-1.medium.com/max/800/1*8PlVNsci0toMN3ZMbQ5exg.gif)

So, here is a cheat list for our example of the BookResource.

* Restless Class method → HTTP method and url
* **list** method → *GET* method on **localhost:8000/books/**
* **create** method → *POST* method on **localhost:8000/books/**
* **detail** method → *GET* method on **localhost:8000/books/book_id/**
* **update** method → *PUT* method on **localhost:8000/books/book_id/**

Ok, so this is very cool, but now anyone can access your data, right? 
To add some restriction to your Resource methods, you must add an method on your resource 
called **is_authenticated** and it should return a boolean. If the method returns True, your user will be able to access that data. 
This will look like:

```python
# On the api.py...
from restless.dj import DjangoResource
from restless.preparers import FieldsPreparer

from .models import Author, Book

class BookResource(DjangoResource):
  def is_authenticated(self):
    return self.request.user.is_authenticated()
```

This is are some simple examples of how you can you Restless. Although simple, it can be quite powerful. Hope it helps you!
