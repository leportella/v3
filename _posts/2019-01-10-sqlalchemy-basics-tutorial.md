---
layout: post
title: "SQLAlchemy Basics Tutorial"
categories:
  - english
tags:
  - english
  - python
  - machine learning
  - sqlalchemy
  - orm
  - database
  - SQL
  - Project Jupyter
featured-img: accidents
last_modified_at: 2019-01-10T21:55:52-05:00
---


[I've been working with Project Jupyter since December of last year](https://leportella.com/english/2018/12/12/outreachy-I.html) and it has been a wonderful experience. The last couple of days I struggled with the SQLAlchemy library that [JupyterHub](https://github.com/jupyterhub/jupyterhub) works on its internals. Since I studied this library and had to scratch some [Stack Overflow](http://stackoverflow.com/) questions to find some answers, I created this post to help digesting some of my doubts and findings.

Since this post became surprisingly long, I decided that the main problem I was having with SQLAlchemy should be in a separated post. So, keep tuned :)

All code is available [here](https://github.com/leportella/sqlalchemy-basics-post/).


## Summary

* [Creating and understanding the Engine](#engine)
* [Engine or connection?](#engine-connection)
* [Creating and understanding Sessions](#sessions)
* [Creating tables](#creating-tables)
* [Adding new records](#add-records)
* [Making queries](#queries)
* [Creating new tables tables after initial create_all](#creating-tables-posterior)
* [Creating a Foreign Key relationship](#foreign-key)


<h2 id='engine'>Creating and understanding the Engine</h2>

To start workin with SQLAlchemy, the first thing that they thought in the tutorials is to create an Engine. The Engine is basically how SQLAlchemy communicates with your database, so, when creating the Engine you should add your database (db) URL and that's basically it.

Although we can access the db through Engine commands (we will see how), we usually don't. But you can. But you shouldn't :)

```python
from sqlalchemy import create_engine

engine = create_engine('sqlite:///:memory:', echo=True)
```

So, basically you are just telling where your database currently is located. The attribute `echo=True` will make SQLAlchemy to log all SQL commands it is doing while you apply commands. This should not be activated in production, ok?

<img src="https://i.imgur.com/0gVcCUg.png" style="height:200px;"/>

Once your engine knows your database, it is easy to execute commands on it by using a method called `engine.execute(...)`. You can see how this is done here:

![](https://github.com/leportella/sqlalchemy-basics-post/blob/master/gifs/engine_execute.gif)

So you basically now have a two way street: the Engine that knows where your db is and a method (`engine.execute(...)`) to change the db using the Engine:

<img src="https://i.imgur.com/yjdhaTZ.png" style="height:200px;"/>

<h2 id='engine-connection'>Engine or connection?</h2>

I also saw in some tutorials that you have another way of doing SQL commands through the `engine` by making a `connection` such as:

```
conn = engine.connect()
conn.execute(...)
```

This allows us to create transaction commands, which mean that all commands must be done successfully or all should rollback in case of an error [[1](https://docs.microsoft.com/en-us/sql/t-sql/language-elements/transactions-transact-sql?view=sql-server-2017)]:

```python
trans = conn.begin()
conn.execute('INSERT INTO "EX1" (name) '
             'VALUES ("Hello")')
trans.commit()
```

So, actually, the structure looks more like this now:

<img src="https://i.imgur.com/Bcp1Zku.png" style="height:200px;"/>


However, looking more deeply [some answers](https://stackoverflow.com/a/34364247/3538098) about the differences between `engine.execute(...)` and `connection.execute(...)` I found that they are not different at all:

> "Using Engine.execute() and Connection.execute() is (almost) one the same thing, in formal, Connection object gets created implicitly, and in later case we explicitly instantiate it.''

So, feel free to use each one of those if you would like :)

<h2 id='sessions'>Creating and understanding Sessions</h2>

Until now we connected to our database and were able to execute commands through SQL statements. However, the thing that makes SQLAlchemy so attractive is its ORM, which was not discussed so far.

The ORM must have a `session` to make the middle-ground between the objects we will deal with in Python and the engine that actually communicates with the database. So, we need a function called `sessionmaker` that we'll pass our engine to.

```python
from sqlalchemy.orm import sessionmaker

Session = sessionmaker(bind=engine)
session = Session()
```

So, basically, you will use sessions to talk to your tables and make queries, but is the `engine` that is actually implementing things on your db.

<img src="https://i.imgur.com/iqV59ky.png" style="height:300px;"/>

Although it seems confusing having three entities before even starting with our tables, most of the times after the initial setup you will use the `session` much more than the `engine` and `connection` will be done implicitly by the two firsts, ok?


<h2 id='creating-tables'>Creating tables</h2>

Now we want to create tables in our db to work with them and finally start to take a look at SQLAlchemy's ORM. To create new tables we will create classes that contain attributes. Each class will be a table in our db and each attribute will be a column in this table. To map which table in the db will be related to each class in our files, we will use a SQLAlchemy system called *Declarative*. To use this, the first thing we must do is to instantiate a `Base`:

```python
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()
```

Now we create a class `User` that inherits from our `Base` declarative. We will create only three attributes to our class: `id` (which is a primary key and can't be null), a name and a password. Since we are using *Declaratives*, we must add at least two attributes: 1) a `__tablename__` that how your table will be actually called inside the db and 2) at least one Column which is part of a primary key [[2](https://docs.sqlalchemy.org/en/latest/orm/tutorial.html#declare-a-mapping)].

We will also add an optional method called `__repr__` that will be a string that will be returned when we see our user instance.

```python
from sqlalchemy import Column, Integer, String

class User(Base):
    __tablename__ = 'users'

    id = Column(Integer, primary_key=True)
    name = Column(String)
    password = Column(String)

    def __repr__(self):
        return f'User {self.name}'
```

We now have a class that indicates us how our table must be on our database. However, nothing is changed so far. To actually create the tables on our database, we will need the declarative `Base` we just created and the `engine`:

```python
Base.metadata.create_all(engine)
```

This is when SQLAlchemy will actually do something in our database. Since we have the variable `echo` set to `True`, we can see exactly which SQL statement the `engine` actually did on the database:

<img src="https://i.imgur.com/kU4Snpb.png" style="height:200px;"/>


<h2 id='add-records'>Adding new records</h2>

Now, we can use the class to create a new record on our database. We can user the `User` class to create a new user and `session.add(...)` to add the instance to our database.

```python
user = User(name='John Snow', password='johnspassword')
session.add(user)

print(user.id)  # None
```

Now, even though we said we needed a primary key, I didn't pass one for the model. And if I try to print the id of the user I just created, it will return `None`.

This is because `session.add` just register the transactions we want it to do, but it doesn't actually do it [[3](https://stackoverflow.com/a/4202016/3538098)].

As explained on [this link](https://stackoverflow.com/a/4202016/3538098), we have two operations that can be done here:

> `session.flush()` communicates a series of operations to the database (insert, update, delete). The database maintains them as pending operations in a transaction. The changes aren't persisted permanently to disk, or visible to other transactions until the database receives a COMMIT for the current transaction (which is what `session.commit()` does).

or

> `session.commit()` commits (persists) those changes to the database. `session.commit()` always calls for `session.flush()` as part of it.


<h2 id='queries'>Making queries</h2>

Once we have our records on our database, we need to be able to find them :)

So, to the `query` function of our `session` we will pass the class we want to look for our instance, an then use the method to filter by an attribute called `filter_by`.

```python
query = session.query(User).filter_by(name='John')
```

Finally, we pass a method to indicate what we want to do with this query: count the number of records found (`.count()`), return all records found (`.all()`), return the first record (`.first()`) and so on:

```python
query.count()
```

Another way is to use the `filter` method, instead of the `filter_by`, which has a slightly different syntax:

```python
session.query(User).filter(User.name=='John').first()
```

With `filter` method, you can also look for strings similar to what you have:

```python
session.query(User).filter(User.name.like('%John%')).first()
```

On [Jupyterhub](https://github.com/jupyterhub/jupyterhub) they added to each model a *classmethod* that would simplify this rather complicated syntax. We can add a method that you can pass a `session`, an attribute, and it returns all the elements, for instance:

```python
class User(Base):
  ...

  @classmethod
  def find_by_name(cls, session, name):
    return session.query(cls).filter_by(name=name).all()
```

So, the new way to find all users named only 'John':

```python
Product.find_by_name(session, 'John')
```


<h2 id='creating-tables-posterior'>Creating new tables tables after initial create_all</h2>

On problem I bump into while I was working with [Project Jupyter](https://jupyter.org/) was that I needed to create a new table to a database and `engine` that already had a initial creation (the `Base.metadata.create_all(engine)`).

So, imagine that now I want a table with Products such as the following:

```python
from sqlalchemy import Column, Integer, String

class Product(Base):
    __tablename__ = 'products'

    id = Column(Integer, primary_key=True)
    name = Column(String)
```

The simplest way I found to do this was simply:

```python
Product.__table__.create(engine)
```


<h2 id='foreign-key'>Creating a Foreign Key relationship</h2>

Imagine that you would like to connect each product to a user in your system. So, in each instance of `Product` you would like to store an instance of `User`:


<img src="https://i.imgur.com/BJqWSMj.png" style="height:350px;"/>

If you you are creating all tables now, you should add a Column on your `Product` class that references the Foreign Key of the user and a relationship with the `User` class:

```python
from sqlalchemy import Column, ForeignKey, Integer, String
from sqlalchemy.orm import relationship

class Product(Base):
    __tablename__ = 'product'

    id = Column(Integer, primary_key=True)
    name = Column(String)
    user_id = Column(Integer, ForeignKey('user.id'))
    user = relationship('User')
```

And add a relationship between `User` and `Product` on `User` class:

```python
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import relationship

class User(Base):
    __tablename__ = 'user'  # if you use base it is obligatory

    id = Column(Integer, primary_key=True)  # obligatory
    name = Column(String)
    password = Column(String)
    products = relationship(Product, backref="users")
```

Now you can create all tables by using the `Base.metada.create_all(engine)` we have seen previously.

Now, you can create a user and a product that are related with each other:

```python
user = User(name='John')
product = Product(name='wolf', user=user)

session.add_all([user, product])
session.commit()
```

And that's it :)

![](https://media.giphy.com/media/3o7btQsLqXMJAPu6Na/giphy.gif)
