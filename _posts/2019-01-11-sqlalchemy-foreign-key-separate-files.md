---
layout: post
title: "Adding relationship to tables already created on SQLAlchemy"
categories:
  - english
tags:
  - english
  - python
  - outreachy
  - sqlalchemy
  - orm
  - database
  - SQL
  - Project Jupyter
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
featured-img: alone-anime
permalink: relationship-sqlalchemy.html
redirect_from: /english/2019/01/11/sqlalchemy-foreign-key-separate-files.html
last_modified_at: 2019-01-11T11:21:52-05:00
---

On [this post](https://leportella.com/english/2019/01/10/sqlalchemy-basics-tutorial.html) you could have a small idea how SQLAlchemy works. However, all my study on SQLAlchemy basics was due to a problem I was having that took me a lot of time to figure it out. Since the problem was more complex and didn't actually fit on the last post, I decided to create a new one dedicated to it, so here it is :)

While I was creating the [Native Authenticator](https://github.com/jupyterhub/nativeauthenticator/) I realized that I needed to store some information about my user that wasn't available in the default `User` table. Informations such as email or password, for instance. The [JupuyterHub `User` class](https://github.com/jupyterhub/jupyterhub/blob/master/jupyterhub/orm.py#L111) was like this:

```python
# jupyterhub/orm.py

class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(Unicode(255), unique=True)
```

I decided, then, to create a `UserInfo` table that would store any other information that I wanted. My initial class was like this:

```python
# nativeauthenticator/orm.py 

from sqlalchemy import Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class UserInfo(Base):
    __tablename__ = 'users_info'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String, nullable=False)
    password = Column(String, nullable=False)
```

Once I had the class ready, all I needed was to add this table to my database. So I added this creation on the `__init__` method of my authenticator class. Such as:

```python
# nativeauthenticator/nativeauthenticator.py

class NativeAuthenticator(Authenticator):

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)

        inspector = inspect(self.db.bind)
        if 'users_info' not in inspector.get_table_names():
            UserInfo.__table__.create(self.db.bind)
```

Obs: *self.db.bind* is the SQLAlchemy engine.

I first checked to see if my table isn't there already (otherwise I would get an error) and, if it isn't, than I created the table. All was working so far :)

The problem started when I decided to add a relationship between `UserInfo` and `User`. 

For some reason, I couldn't make it work. The `UserInfo` table would never find the `User` table and I kept getting this error:

>  `NoReferencedTableError: Foreign key associated with column 'product.user_id' could not find table 'user' with which to generate a foreign key to target column 'id'`.

I spent some time going through [Stack Overflow](https://stackoverflow.com/) issues and after getting nowhere I wrote [an issue](https://github.com/jupyterhub/nativeauthenticator/issues/26) asking for help. 

That's when [André](https://github.com/dedeco) asked me if I were using the same `Base` for both models and, guess what? I wasn't. 

So, the first thing you should do when creating two classes for the same `SQLAlchemy engine` is to **use the same `Base` in all models**. Seems trivial, but it isn't. So, I fixed this:

```python
# nativeauthenticator/orm.py 

from jupyterhub.orm import Base
from sqlalchemy import Column, Integer, String

class UserInfo(Base):
    __tablename__ = 'users_info'
    id = Column(Integer, primary_key=True, autoincrement=True)
    username = Column(String, nullable=False)
    password = Column(String, nullable=False)
```

After that, you can add the attributes for the relationship on your class

```python
class UserInfo(Base):
    ...
    user_id = Column(Integer, ForeignKey('users.id'))
    user = relationship(User)
```

Now you can simply add the table and it will work! 

However, this way you won't be able to see which `UserInfo` instances are connected to your `User`, because only `UserInfo` knows `User`. Thus, you must add a relationship to the `User` class. Since we are not adding it on the class creation (aka original file), we add it to the class in the moment that you need it:

```pyhon
User.info = relationship(UserInfo, backref='users')
```

See a full example on how you work with these two classes here:

![](https://raw.githubusercontent.com/leportella/sqlalchemy-basics-post/master/gifs/jupyterhub_example.gif)


Done! Everything worked!

Special thanks to [André](https://github.com/dedeco), [Yuvi](https://github.com/yuvipanda) and [Pastore](https://github.com/apast) for all the help ❤️!
