---
layout: post
title: "Using MongoDB with Python and MongoEngine <3"
categories:
  - english
tags:
  - en
  - open-source
  - python
  - mongo
  - code
  - mongoengine
featured-img: database
last_modified_at: 2017-03-09T14:25:52-05:00
---

I started working with MongoDB for fun and for some side projects in the last year. The main idea of using MongoDB is its flexibility. The [pymongo](https://api.mongodb.com/python/current/) library is really nice for getting some information, 
but on a project more complex, we may need something a little more intense. A nice alternative is the [**MongoEngine**](https://github.com/MongoEngine/mongoengine) library, which is an Object-Document Mapper (ODM), which treats MongoDB documents as a kind of ORM. 

So, I decided to make this post to teach a little bit about MongoDB, MongoCompass and MongoEngine. On a next post, I will 
 talk a little bit on how can we test this project in a very simple and nice way.

So, shall we?

![](https://media.giphy.com/media/3o6ZsXHLRnkgPtEYVi/giphy.gif)

## Initial Configuration

First thing, you can add MongoDB to your machine or you can use the [MongoDB Atlas](https://www.mongodb.com/cloud/atlas), 
where you can use their infrastructure to have a free 500mb database for free. 

If you want it on your machine, you can also use docker to run a MongoDB:

```
$ docker run -d -p 27017:27017 mongo:3.6.5-jessie
```

My favorite tool to checkout 
the data on the database and manipulate it easily is [Mongo Compass](https://www.mongodb.com/products/compass). The 
interface is really easy going and very intuive, and pretty fast to day-to-day use, even if you are not working 
with a local machine. You can 
connect it directly to your cluster on Atlas or your machine and favorite that connection so you don't have to worry about it 
anymore. 

Via MongoCompass I added a database called `imdb` with a collection called `movie`. I got a series of movie information on 
[this link](https://raw.githubusercontent.com/steveren/docs-assets/charts-tutorial/movieDetails.json) given on 
[this](https://docs.mongodb.com/charts/master/tutorial/movie-details/prereqs-and-import-data/) tutorial. 

Once you created a new database and collection, you can add all this data stores as JSON to your collection in a very simple 
way.
Go to your collection area on the MongoCompass interface

![](https://i.imgur.com/HgZHPht.png)

Access on the upper bar `Collection` > `Import Data`

![](https://i.imgur.com/cSJV9f6.png)

And select the file you want to add to your database

![](https://i.imgur.com/OA8KvZo.png)

There you go :) A lot of movies to check the scores were uploaded to our collection.

![](https://media.giphy.com/media/pUeXcg80cO8I8/giphy.gif)

## MongoDB and Pymongo

First, let's understand a little bit more about MongoDB's documents using *pymongo*. 
To access my collection through *pymongo* I will first connect to my MongoClient, my database and my collection, such as this:

```python
from pymongo import MongoClient

client = MongoClient() # connects to client locally
db = client['imdb'] # get the database
collection = db['movie'] # get the connection
```

However, until now, the database was not accessed. To access it and get one movie you can use the method `find_one`:

```python
collection.find_one()
```

The return will be something similar to this:

```python
{'_id': ObjectId('5b107bec1d2952d0da9046e0'),
 'title': 'Once Upon a Time in the West',
 'year': 1968,
 'rated': 'PG-13',
 'runtime': 175,
 'countries': ['Italy', 'USA', 'Spain'],
 'genres': ['Western'],
 'director': 'Sergio Leone',
 'writers': ['Sergio Donati',
  'Sergio Leone',
  'Dario Argento',
  'Bernardo Bertolucci',
  'Sergio Leone'],
 'actors': ['Claudia Cardinale',
  'Henry Fonda',
  'Jason Robards',
  'Charles Bronson'],
 'plot': 'Epic story of a mysterious stranger with a harmonica who joins forces with a notorious desperado to protect a beautiful widow from a ruthless assassin working for the railroad.',
 'poster': 'http://ia.media-imdb.com/images/M/MV5BMTEyODQzNDkzNjVeQTJeQWpwZ15BbWU4MDgyODk1NDEx._V1_SX300.jpg',
 'imdb': {'id': 'tt0064116', 'rating': 8.6, 'votes': 201283},
 'tomato': {'meter': 98,
  'image': 'certified',
  'rating': 9,
  'reviews': 54,
  'fresh': 53,
  'consensus': 'A landmark Sergio Leone spaghetti western masterpiece featuring a classic Morricone score.',
  'userMeter': 95,
  'userRating': 4.3,
  'userReviews': 64006},
 'metacritic': 80,
 'awards': {'wins': 4, 'nominations': 5, 'text': '4 wins & 5 nominations.'},
 'type': 'movie'}
```

If you check the type of this return, 
you'll see that this is a dictionary instance, and you can change it as you wish and then 
add it again on your database. Pretty cool. So, basically, a MongoDB document works similarly to what we know as a 
dictionary on Python. You can add new *keys* to your document, and these *keys* can have values that could be anything: 
from integers, to list and even new dictionaries. 

The great advantage of using the MongoCompass, is that these documents can be aggregated and expanded for 
better visualization. Here are the view of the same document agreggated and expanded:

<img src="https://i.imgur.com/p16d738.png" height="400" style="max-width: 20%" />

<img src="https://i.imgur.com/cPCTlSq.png" height="550" style="max-width: 20%" />


## Setting up the MongoEngine

Now that we have an idea of what a MongoDB document looks like in Python, we can start to use MongoEngine for our collection.
First we need to have an understanding on how the collection is structured. 

We can observe that our movie document contains these "keys" and "values":

* _id: ObjectId
* title: string
* year: integer
* rated: string
* runtime: integer
* countries: list
* genres: list
* director: string
* writers: list
* actors: list
* plot: string
* poster: document
* imdb: document
* tomato: document
* metacritic: integer
* awards: document
* type: string

So we can use this knowledge on how our data is structured to create a MongoEngine class. Let's ignore for now the fields 
that are documents and let's focus on the others. We create a class called `Movie` in a file called `models.py` 
and this class will inherit from `mongoengine.Document`. If you are familiarized with Django, you can see the similarity. 

The idea is that the class name must have the same name of 
your collection, in this case `movie`. Each attribute of the class will be named as the "keys" that we listed above 
 and the value must be a mongoengine instance. These instances can be `StringField` for string attributes, 
 `IntField` for integers, and `ListField` to lists (but many others are available). 
By doing this, mongoengine will not let you add an integer field to a string field or a list field. It 
maintains the consistence.

```python
# models.py
import mongoengine


class Movie(mongoengine.Document):

    title = mongoengine.StringField()
    year = mongoengine.IntField()
    rated = mongoengine.StringField()
    runtime = mongoengine.IntField()
    countries = mongoengine.ListField()
    genres = mongoengine.ListField()
    director = mongoengine.StringField()
    writers = mongoengine.ListField()
    actors = mongoengine.ListField()
    plot = mongoengine.StringField()
    poster = mongoengine.StringField()
    metacritic = mongoengine.IntField()
    type = mongoengine.StringField()
```

Let's try to use this class of ours. First, we shall connect to the database. With MongoEngine, if you are using a 
local MongoDB instance, just pass the name of your database:

```python
from mongoengine import connect

connect('imdb')
```

Now we are connected. However, when we import our class and try to get a document from our database, we get an error...

```python
from models import Movie

Movie.objects.first()
```

Error message: `FieldDoesNotExist: The fields "{'awards', 'imdb', 'tomato'}" do not exist on the document "Movie"`:

So, MongoEngine checks the database and sees that our class is not corresponding to our database document.

We have two options: we can either change our class to adapt to our instances, or we can inherit from the
`DynamicDocument` class instead of the `Document`class, so it can "ignore" the fields that your class doesn't have declared.

Since it's harder to configure everything (by inheriting from `Document` instead of `DynamicDocument`), let's go this way. You can add fields as Documents, and we will configure it in 
a very similar way we just did. There is only one difference: it should inherit from `mongoengine.EmbeddedDocument` instead 
of just `mongoengine.Document`. 

So, our new `models.py` will look like this:

```python
import mongoengine


class Imdb(mongoengine.EmbeddedDocument):
    id = mongoengine.StringField()
    rating = mongoengine.DecimalField()
    votes = mongoengine.IntField()


class Tomato(mongoengine.EmbeddedDocument):
    meter = mongoengine.IntField()
    image = mongoengine.StringField()
    rating = mongoengine.IntField()
    reviews = mongoengine.IntField()
    fresh = mongoengine.IntField()
    consensus = mongoengine.StringField()
    userMeter = mongoengine.IntField()
    userRating = mongoengine.DecimalField()
    userReviews = mongoengine.IntField()


class Awards(mongoengine.EmbeddedDocument):
    wins = mongoengine.IntField()
    nominations = mongoengine.IntField()
    text = mongoengine.StringField()


class Movie(mongoengine.Document):
    title = mongoengine.StringField()
    year = mongoengine.IntField()
    rated = mongoengine.StringField()
    runtime = mongoengine.IntField()
    countries = mongoengine.ListField()
    genres = mongoengine.ListField()
    director = mongoengine.StringField()
    writers = mongoengine.ListField()
    actors = mongoengine.ListField()
    plot = mongoengine.StringField()
    poster = mongoengine.StringField()
    imdb = mongoengine.EmbeddedDocumentField(Imdb)
    tomato = mongoengine.EmbeddedDocumentField(Tomato)
    metacritic = mongoengine.IntField()
    awards = mongoengine.EmbeddedDocumentField(Awards)
    type = mongoengine.StringField()
```

And... voilá! We have our model prepared and ready to go!

Now, when you can get a document from your database by using:

```python
movie = Movie.objects.first()
```

And you'll see that your `movie` instance have a lot of functions to help you deal with your document. One really nice feature 
is a method to transform your whole data into json:

```python
movie.to_json()
```

## Some tips...

Now that we know how a model works, some things we should know:

* Attributes can be `required`
* Attributes can be set as `unique`
* MongoEngine handle the `_id` attribute automatically, but you can set another field as the default id
* You can add choices to `StringField`s
* You can use your class to create and save new instances: `Movie(title='My new Movie').save()`
* If you don't want your class name to be the same name as the collection, you can add an extra paremeter called `meta` 
with a dictionary with the real collection name like this: `meta = {'collection': 'movie'}` :)

## Querying the dataset

![](https://media.giphy.com/media/3ohhwuzTxy3eQK10Yw/giphy.gif)

MongoEngine allows us to make queries in a very similiar way the Django ORM. For instance, we can filter movies that were 
launched before 1988:

```python
Movie.objects(year__lte=1988)
```

We can filter movies where the Imdb rating is equal or over 9:

```python
Movie.objects(imdb__rating__gte=9)
```

Movies which titles contains a certain word...

```python
Movie.objects(title__contains='Love')
```

Not only that, but we can count the number of movies our database currently have

```python
Movie.objects.count()
```

Movies that have two actors on it:

```python
Movie.objects(actors__size=2)
```

The possibilities are endless. Check 
[the official documentation](http://docs.mongoengine.org/tutorial.html) for more insights on what you can do :)
