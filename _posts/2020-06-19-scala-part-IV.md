---
layout: post
title: "Classes, Objects and Traits in Scala"
categories:
  - english
tags:
  - english
  - english
  - tecnologia
  - Scala
  - Programming Language
  - programming language
  - java
  - jvm
  - java virtual machine
  - spark
  - San Francisco
  - Dublin
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
  - self-learning
  - programmer
  - ruby
  - scala
  - python
featured-img: scala
permalink: scala-IV
redirect_from: scala-IV.html
last_modified_at: 2020-06-19T23:25:52-05:00
---

*This post is also known as “My saga learning Scala - Part 4“*

- [Part 1 - Why am I learning Scala and resources](https://leportella.com/english/2020/03/08/scala-part-I.html)
- [Part 2 - Functions and Strings](https://leportella.com/english/2020/04/01/scala-part-II.html)
- [Part 3 - Classes in Scala: method and attribute definition](https://leportella.com/scala-III.html)


**New suggestions!**

Apparently, at least one person is reading these Scala posts! Bram Elfrink [send me a link to a series of Scala classes given at Twitter and it seems pretty cool](https://twitter.github.io/scala_school/)! Make sure you check it out! Thanks, Bram!


## Objects

[In the last post](https://leportella.com/scala-III.html), I presented how to work with class attributes and methods, especially how to limit the usage of both the class, the attributes, and the methods. But classes are only one type of object in Scala. As we discussed in [Part 2](https://leportella.com/english/2020/04/01/scala-part-II.html), when you want to run something in Scala, we create an object and not a class. So what's the difference between one and the other?

When we create a new `class`, we can instantiate it, such as:

```scala
class Person(val name: String) 
val mary = new Person("Mary") 
```


<center>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/img/posts/scala-classes1.png" style="height:150px;"/>
</center>
<br/>


If I create a new person, this will create a new instance:

```scala
val john = new Person("John")
```

The instance  `john` is not the same as the `mary` instance, because each time we call the `new Person` we create a new instance. 


<center>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/img/posts/scala-classes2.png" style="height:250px;"/>
</center>
<br/>



In Scala, an `object`  will always have a single instance, no matter how many times you "instantiate" it. That's why an `object`  is called a *singleton*.


<center>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/img/posts/scala-classes3.png" style="height:150px;"/>
</center>
<br/>




I don't even need the `new` keyword to create an instance, because we actually don't need to instantiate the `object` at all. When you create an `object`, it is already instantiated, so you can simply use it. 

Because of this characteristics, a Scala `object` can't have a constructor, this is, it can't receive any parameters.

In the following example, you will see that both values are actually the same instance.

```scala
object Person
val mary = Person // same as `new Person`
val john = Person // same as `new Person`

println(mary == john) // true
```

Because an `object` is always an instance - and only one instance -  you can always access attributes and methods inside an object directly:

```scala
object House {
  val has_kitchen: Boolean = true  
}

println(House.has_kitchen) // true
```

Another interesting thing is that you can also have a `class` with the same name as an `object`, in the same file! The following example works just fine:

```scala
object Person {
  num_eyes = 2
}

class Person(name: String)

val person = Person // object
val mary = new Person("Mary") // class
```


## Overloading

In the last post that we can create a `class` with multiple constructors. We could do that by simply writing two constructor methods with the same name. This feature is called *overloading* and, as you can see, can be pretty useful:

```scala
class Person(name: String) { 
    def greet(name: String) = println(s"${this.name} says: Hello, $name")
    // overloading
    def greet = println(s"Hi! My name is $name")

val john = new Person("John")

println(john.greet) // Hi! My name is John!
println(john.greet("Mary")) // John says: Hello, Mary
```


## Abstraction!

Now that we've seen how `classes` and `objects` work,  it is time to understand how to use abstract objects that can be inherited by a class. [Inheritance strongly supports the concept of reusability](https://techdifferences.com/difference-between-single-and-multiple-inheritance.html), allowing classes to reuse properties and methods already available in other classes, thus, avoiding code duplication!

### **Abstract class**
An [abstract class](https://docs.scala-lang.org/overviews/scala-book/abstract-classes.html) is pretty much a normal class with the `abstract` reserved word in front of it. It can have a constructor and receive parameters (just like a normal class would) and can have methods and attributes that can be overwritten.


```scala
abstract class Animal {
  val creatureType: String
  def eat: Unit
}

class Dog extends Animal {
  override val creatureType: String = "Dog"
  override def eat: Unit = ???
}

val animal = new Animal  // Error! class Animal is abstract; cannot be instantiated
val dog = new Dog // Works!
```

You can also require parameters on an abstract class by adding a constructor such as:

```scala
abstract class Animal(val identification: String)
class Dog(val name: String) extends Animal(name)
```

However, there is a catch! You can only extend **1** abstract class on a regular, simple class. So our class `Dog` won't extend any other class other than `Animal`.  The model of only allowing a class to extend from just one other single class is called *single inheritance*. I tried to understand why would a language allow only a single inheritance system and [the explanation I found for C# was](http://www.net-informations.com/faq/general/inheritance.htm):


> *C# does not support **multiple inheritance**, because they reasoned that adding multiple inheritance added too much **complexity** to C# while providing too little benefit.*


### **Traits**

If you take a look at what's written about traits in the [Programming in Scala First Edition](https://www.artima.com/pins1ed/index.html#TOC), you'll see that:


> *Traits are a fundamental unit of code reuse in Scala.* [[1]](https://www.artima.com/pins1ed/traits.html)

Similar to abstract classes, traits are a way to gather functions and methods in a single place and re-use it in other classes. You can define a trait as:

```scala
trait Carnivore {
  def chasePrey: Unit
}
```

But, there are two main differences (that I could see) between traits and abstract classes:

- Traits don't have a constructor, so they can't receive parameters
- A single class can inherit multiple traits 

So we can have such as:

```scala
trait Animal {
  val isAlive: Boolean = true
}

trait Carnivore {
  val creatureType: String = "Carnivore"
  def eat(creature: String): String = s"${this.creatureType} eats $creature"
}

trait ColdBlooded {
  val environment: String = "Warm"
}

class Crocodile extends Animal with Carnivore with ColdBlooded {
  override val creatureType: String = "Crocodile"
}
class Dog extends Animal with Carnivore {
  override val creatureType: String = "Dog"
}

val dog = new Dog
val croc = new Crocodile

println(croc.eat(dog.creatureType)) // Crocodile eats Dog
```

### **Choosing between an abstract class and a trait**

Ok, amazing! But now what? Which one do I use?

[There is a really long explanation on which one to use](https://stackoverflow.com/a/15330312/3538098), but the general idea is: when in doubt, **use traits**! From the explanation I just mentioned:


>  *If you still do not know, after considering the above, then start by making it as a trait. You can always change it later, and in general using a trait keeps more options open.*


----------

That was it for today! This took me a while and I am not as active as I was on my Scala course. There was a chapter on Generics that killed my motivation, but I haven't given up! Hopefully, I'll write about it eventually, but who knows!? I never imagined getting to Part 4! 🙂 

