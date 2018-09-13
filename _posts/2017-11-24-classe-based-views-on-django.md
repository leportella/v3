---
layout: post
title: "Class Based Views on Django"
categories:
  - english 
tags:
  - en 
  - python 
  - intermediate
last_modified_at: 2017-03-09T14:25:52-05:00
---

This post could also be called **what comes after the tutorials** :)

In several Django tutorials, we learn how to receive *requests* and return *responses* with html pages having several information.
This is very easy to start understanding the process that Django does: receiving *requests* and returning *templates*. But what happens after that?

When I started developing a system that was a little bit more complex than what the tutorials gave me, I found my self drowned with several *get* and *post*
functions that should have several verifications. This turned out to be much more complex than I expected and not at all as effective as they should be to cover all possibilities I pictured.


That's when I discovered the [Class Based Views](https://docs.djangoproject.com/en/1.11/topics/class-based-views/).

In a simple way, we can aggregate several basic functions that we write on a view as methods in a class. But the big advantage in using Class Based Views is in some
classes that are almost ready for use and that your class can inherit. After that the modification you must do are minimal!

I will show you step by step the evolution of a system from function, to the problems I faced until a system based on pre-existing classes.


![](https://media.giphy.com/media/IYjiXRV622OBO/giphy.gif)
TLDR → See the [project code here](https://github.com/leportella/class-based-views-example) and fly away :)

## You're constructing a system and need a function to return templates

I'll use an example for a small bookstore.

You want to return a welcome page. Simple, you receive a *request* and return a html template.
You add this function on **urls.py** as some url. It looks similar to this:

```python
# views.py
from django. shortcuts import render

def welcome(request):
  return render(request, 'welcome.html')

# urls.py

urlpatterns = [
  url(r'welcome/', views.welcome) 
]
```

## Functions are working well, but I need info from database...

So far, so good. But now you need a function to list all books on your database.

```python
# views.py
from django. shortcuts import render

from .models import Book

def get_books_list(request):
  books = Book.objects.all()
  return render(request, 'my-books.html', {'books': books})

# urls.py

urlpatterns = [
  url(r'my-books/', views.get_books_list) 
]
```

## I want a url to check details on a specific book

Now we want urls for book details. What now? Things start to get a little more complex, right? I need to search for a specific book on my database 
and I need to handle possible errors, in case the book doesn't exist, for instance. Now the complexity is a little bit higher, but we still have a straight forward code:

```python
# views.py
from django.http import Http404
from django. shortcuts import render

from .models import Book

def get_book_by_id(request, pk):
  try:
    books = Book.objects.get(id=pk)
  except Book.DoesNotExist:
    raise Http404("Book does not exist")
  return render(request, 'my-book-detail.html', {'book': book}

# urls.py

urlpatterns = [
  url(r'book-detail-by-id/(?P<pk>[-\w]+)', views.get_book_by_id) 
]
```

## Still too simple. How about an update?

From now on we have a simple visualization system. Cool! Next step? Let's allow our users to edit book information.

To edit book information we need an *update* page. At this moment, I realize I need a function that can handle two different methods:

a) is the user sends a GET at that *endpoint*, I need him to receive a template with the fields that can be edited

b)if the user sends a POST, I want my book to be updated 

Also, I need to guarantee that my user is not messing things up and the data are, in fact, valid. 
Now the complexity is increasing even more...

```python
# views.py
from django.http import Http404
from django. shortcuts import redirect, render

from .forms import BookForm
from .models import Book

def edit_book(request, pk):
  try:
    books = Book.objects.get(id=pk)
  except Book.DoesNotExist:
    raise Http404("Book does not exist")
    
  if request.method == 'GET':
    render(request, 'my-book-edit.html', {'form': BookForm, 'object': book}

  if request.method == 'POST':
    form = BookForm(request.POST)
    if form.is_valid():
      form.save()
    return redirect('/my-books')

# urls.py

urlpatterns = [
   url(r'book-update/(?P<pk>[-\w]+)', views.edit_book) 
]
```

It is easy to realize that the level of complexity is growing very quickly. The function is becoming longer (one *try* and two *ifs*) and things are looking messy.

To be honest, we are not even covering all possibilities, for example if the *form* is invalid (when the user sent wrong info).

What can we do now?

## Everything is looking weird...

All the steps described above are the same steps I did when I started to create my system. In some moment I stopped, looked... and something was clearly not smelling good. Do you know the feeling? 

At this moment I started to look to Class Based Views with calm.

Django, by default, have a lot of classes that have all functionalities needed for a project: render a template, list instances from the database, show details from a specific instance, 
create or edit an instance, and so on.
Error handling and *form* errors, returning a 404... all this is already done by default. We need "just" to give the class some information, and it is done!

The first question will probably be: what if I don't want the standard behaviour? What if I need to change something? In this case, we can re-write some class methods. A little more complex, but still 
easier than treating all possible errors and paths. Shall we see how this work?

## Render templates are still a simple task

To render a template, we will create a class that inherits from TemplateView class. Then, we just need to add the template file as a class attribute, and it is done.
Notice that one the **urls.py** we will need to call the class method "*as_view*", since we are passing a class rather than a function.

In this way, the class already implements how to render the template and response it in the correct way. The function "*as_view*" is responsible for the urls understand that, even if it is a class, 
it should be handled as a normal view.

Still simple, right?

```python
# views.py
from django.views.generic import TemplateView

class WelcomeView(TemplateView):
  template_name = 'welcome.html'
  
# urls.py
urlpatterns = [
  url(r'welcome/', views.WelcomeView.as_view()) 
]
```

## List instances from database in a simpler way

Forget go to the database and pass a context to the template. Let ListView class to do this for you! You define the template and the model you want to search for instances :)

Obs: listing is passed to the template as "*object_list*".

```python
# views.py
from django.views.generic import ListView

from .models import Book

class BooksView(ListView):
  model = Book
  template_name = 'my-books.html'
  
# urls.py
urlpatterns = [
  url(r'my-books/', views.BooksView.as_view()) 
]
```

## ...detail page with just 2 lines? Wut?

Detail url creation are similar to what we have seen so far. You should define a model and a template. By default, the system will look by the instance id. Done! 
Error handling already including. Interested yeat?

Obs: the instance is passed to the template as "*object*".

```python
# views.py
from django.views.generic import DetailView

from .models import Book

class BookDetailView(DetailView):
  model = Book
  template_name = 'my-book-detail.html'
  
# urls.py
urlpatterns = [
  url(r'book-detail/(?P<pk>[-\w]+)', views.BookDetailView.as_view()) 
]
```

## What about update?

The idea behind *updates* are the same as other classes we've seen before. In the *update* case, besides the model, the initial template, we can also send a *form* 
and we need a url to where the user should be redirected in case of success. 

```python
# views.py
from django.views.generic import UpdateView

from .forms import BookForm
from .models import Book

class BookUpdateView(UpdateView):
  model = Book
  template_name = 'my-book-edit.html'
  form_class = BookForm
  success_url = '/my-books'
  
# urls.py
urlpatterns = [
  url(r'book-update/(?P<pk>[-\w]+)', views.BookUpdateView.as_view()) 
]
```

## Cool... but still didn't understand how to change standard behaviour :/

*Class Based Views* have several methods that can be overwritten to behave just like you wanted. Here, we'll see an example on DetailView. 
Assume that you want to list books not by its id, but by an internal code. 

In this case, we want to alter the way the class searches the instance inside my database. The method we need to re-write is the "*get_objects*".
The method return should be an instance searched by any method you want. Done! Now we have a detail *endpoint* with a personalized search :)

```python
# views.py
from django.views.generic import DetailView

from .models import Book

class BookDetailView(DetailView):
  model = Book
  template_name = 'my-book-detail.html'
  
  def get_object(self):
    return Book.objects.get(code=self.kwargs['code'])
  
# urls.py
urlpatterns = [
  url(r'book-detail/(?P<code>[-\w]+)', views.BookDetailView.as_view()) 
]
```

At this moment, you should probably be wondering: how will I know which method should I overwrite?

In my case I use [this website](http://ccbv.co.uk/) that contains all available methods for each of the classes available.
I follow the methods being called until I find the one that do what I want to. From there, we need to understand the method, and re-write it!


## What about authentication?

Easy! The class LoginRequiredMixin is there to help you! Make your class inherit it and define the attribute "*login_url*". This will be the url your 
user will be redirected in case it is not logged on your system and tries to access it.

```python
# views.py
from django.contrib.auth.mixins import LoginRequiredMixin
from django.views.generic import DetailView

from .models import Book

class BookDetailView(LoginRequiredMixin, DetailView):
  model = Book
  template_name = 'my-book-detail.html'
  login_url = '/'
  
# urls.py
urlpatterns = [
  url(r'book-detail/(?P<pk>[-\w]+)', views.BookDetailView.as_view()) 
]
```

## Finally...

Wow! We covered a lot of things! It seems kind of magic, but this magic makes our life really easy! And without all this classes, it would be much more difficult to understand what is going on!

To make things easier, I make a [little project](https://github.com/leportella/class-based-views-example) that have all the code that you saw here.

Hope this is helpful to you as it was for me!

![](https://media.giphy.com/media/6gy2ySS6nXaDe/giphy.gif)
