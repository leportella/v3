---
layout: post
title: "How we used generic relations to add comments to model instances"
categories:
  - english
tags:
  - en 
  - python
  - Django 
featured-img: comments
last_modified_at: 2017-03-09T14:25:52-05:00
---

As I talked to some people, few new about Django’s Generic Relation and Generic Foreign Key. And when I was studying it to apply on our system, I realised that the documentation can be kind of tricky and sparse. Nevertheless, Generic Relations helped us a lot, and so I decided to write about it in this blog post :)

When we have a foreign key, we are linking an instance of another model in the current model. Right? So, we can access that other instance and other model very easily. So it would work like this:

```python
class Author(models.Model):
    name = models.CharField(max_length=50)

class Book(models.Model):
    author = models.ForeignKey(Author)
    title = models.CharField(max_length=250)
pages = models.IntegerField()
```

Here you will have an instance of Author associated with all the information we have on the Book instance. 
Ok, this is cool because you can store several books, all linked to the same author.

Imagine now that you want the same thing: associate an instance of a model with more information. But instead of having just an instance of Author (for example), you want the same information through several model classes (such as books and cds).

Here we have the GenericRelations to save us! In a simple way we can say that GenericRelation is a foreign key 
that can store any instance of any of your models at any of your apps.

And now you are asking yourself: why is this useful? You can simply add more fields in your original model or something like this. 
Yes, this is true in most cases. However, for some specific cases, Generic Relations can be really handy. 
In our case, we needed the hability to add comments in two apps of our system. We needed the same functionality in different part of the system in a way it would be easy to mantain and without duplicating code: perfect time for Generic Relations.

We started by creating an Comment model such as (Django 1.10):


```python
from django.contrib.contenttypes.fields import GenericForeignKey
from django.contrib.contenttypes.models import ContentType
from django.db import models

class Comment(models.Model):
    content_type = models.ForeignKey(ContentType)
    object_id = models.Charfield(max_length=50)
    content_object = GenericForeignKey('content_type', 'object_id')
text = models.TextField(blank=True)
```

On this model the **text** field is a normal TextField from Django, used to store the comment itself.
The other fields, **content_type**, **object_id** and **content_objects** are part of the Generic Relation we are adding here.

Instances of ContentType represent and store information about the models installed on your project. 
Everytime a new model is created, new instances of ContentTypes are automatically created. 
Here, the **content_type** will be a Foreign Key to the model you want to associate.

The **object_id**, by the other end, is a simple Charfield that will store an id of an object that 
is stored in your model. On the oficial Django documentation, you will find that the suggestion is to use PositiveIntegerField on this field. However, we use uuid as our id fields so we had to change this to Charfield.

You have the model, you have the id of the object you want to access... so the **content_objects** will actually represent the instance of that particular object on that particular model. The GenericForeignKey does the magic for you!

Let’s apply this model for something useful. Imagine that you have models for Books and CDs, and you want to to be able to add comments from your users in each book or cd available on your database.

To create a new comment in a specific book all you need to do is:

```python
from django.contrib.contenttypes.models import ContentType
from .models import Book, Comment

book = Book.objects.first()
text = 'The message goes here'

new_comment = Comment(text=text,
                       content_object=book)
new_comment.save()
```

or this: 
 
 
```python
from django.contrib.contenttypes.models import ContentType
from .models import Book, Comment

book = Book.objects.first()
text = 'The message goes here'
content_type = ContentType.objects.get(app_label='MyShell', model='Books')
 
new_comment = Comment(text=text,
                      content_type=content_type,
                      object_id=book.id)
new_comment.save()
```
 
So you can first recover the instance you want to associate your comment with (**book** in this case) and send it as the **content_object** 
(and Django does the magic for you). 
Or you can get the model you want from the app it is located with the ContentType method and send it to the Comment along with the book id.

You can do this with the Book model, the Cd model or any other model in any other app you have on your system. You won’t need to rewrite this comment to every app or every model you want to add a series of comments.

Now you ask: how can I recover the comments information in my Book or Cd instance? Here comes the easy part!
 
```python
from django.contrib.contenttypes.fields import GenericRelation
from ..models import Comment

class Book(models.Model):
    author = models.ForeignKey(Author)
    title = models.CharField(max_length=250)
    pages = models.IntegerField()
    comments = GenericRelation(Comment)

class Cd(models.Model):
    artist = models.ForeignKey(Artists)
    title = models.CharField(max_length=250)
comments = GenericRelation(Comment)
```

Done! You can add comments to any Book or Cd instance, are retrieve it by simply doing:

```python
>>> book.comments.all()
```

![](https://cdn-images-1.medium.com/max/800/1*mPUc2fU1VPbW6gjbw1DjeQ.gif)

Another good news is that you can use it in **prefetch_related** to optimize queries with no worries.

Hope you liked it and it can be useful for you too :)

