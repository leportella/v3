---
layout: post
title: "Classes in Scala: method and attribute definition"
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
permalink: scala-III.html
redirect_from: /english/2020/04/12/scala-part-III.html
last_modified_at: 2020-04-01T18:25:52-05:00
---

*This post is also known as “My saga learning Scala - Part 3“ and is a continuation of* [*Part 1*](https://leportella.com/english/2020/03/08/scala-part-I.html) *and* [*Part 2*](https://leportella.com/english/2020/04/01/scala-part-II.html)

As we discussed in the Part 1 of this series, Scala is an object oriented language. [From Wikipedia](https://en.wikipedia.org/wiki/Object-oriented_programming):


> **Object-oriented programming** (**OOP**) *is a programming paradigm based on the concept of "objects", which can contain data, in the form of fields (often known as attributes or properties), and code, in the form of procedures (often known as methods).*


So on this post I will expose what I learned of the Scala classes, which follow this concept of object that I just presented.


## Creating a class

To create a class you only need to pass the keyword class and the name of the class:

```scala
class Point
    
val point = new Point
```

This is called the class constructor and you can add the attributes that your class can receive, such as:


```scala
class Point(x: Int, y: Int) 

val point = new Point(0, 0)
```

The fun part starts now! You can actually rewrite the constructor to receive different attributes, making your class have default attributes or even other types of attribute


```scala
class Person(name: String, age: Int) {
  def this(name: String) = this(name, 0)
  def this() = this("Bob", 0 )
}

val marco = new Person("Marco", 30)
val babyCarl = new Person("Carl")
val bob = new Person
```



## Adding attributes to a class

The attributes I just added to the class Person can't be accessed directly, like `marco.age` on the example above. The thing it that you actually have 3 options to define attributes when defining a class. Not passing anything (like the example above), with a `val` or with a `var`.

Using nothing, the attributes you create while instantiating a class won't work.

```scala
class Point(x: Int)

val point = new Point(1)
point.x // won't work
```

When using `val`, you can access the attribute (you create an accessor) but can't change the value (it won't have a mutator)

```scala
class Point(val x: Int)

val point = new Point(1)
println(point.x) // 1
point.x = 2 // won't work
```

So this is the same thing as:

```scala
class Point(xc: Int) {
    def x = xc
}

// same thing as

class Point(val x: Int)
```


When using a `var` you probably guessed already: you will both have a mutator and an accessor!


```scala
class Point(var x: Int)

val point = new Point(1)
println(point.x) // 1
point.x = 2 
println(point.x) // 2
```

So… you have:

| **Attribute definition** | **Assessor? (Point.x)** | **Mutator? (Point.x = 4)** | **Can override?** |
| ------------------------ | ----------------------- | -------------------------- | ----------------- |
| `val`                    | yes                     | no                         | yes               |
| `var`                    | yes                     | yes                        | no                |
| nothing                  | no                      | no                         | yes               |

## Using classes

When defining a class, you actually can use other extra keywords to define how you will like your class to behavior. So far I learned 3 major keyword:


| **Keywords**         | **Allowed usage**                                    |
| -------------------- | ---------------------------------------------------- |
| `class Point`        | Normal class that allows creating multiple instances |
| `final class Point`  | Doesn't allow class to be extended                   |
| `sealed class Point` | Allow class to be extended but only in the same file |



## Class methods and overrides

Since classes have keywords that allows us to change the way they are used, it is reasonable to suppose that they also have keyword that can change their behavior. I created a small table with the properties of each one and examples to help us understand what changes exactly.


|   |                                  | Extended class can use? | Extended class can overwritte? | Instances of extended class can use the method? |
| - | -------------------------------- | ----------------------- | ------------------------------ | ----------------------------------------------- |
| 1 | `def` / `val`                    | Yes                     | Yes                            | Yes                                             |
| 2 | `private def` / `private val`    | No                      | Don't need to                  | No                                              |
| 3 | `protected def`/ `protected val` | Yes                     | Yes                            | No                                              |
| 4 | `final def` / `final val`        | Yes                     | No                             | Yes                                             |



## Case 1: `def` and `val`


```scala
class Person {
  val normalVal = 1
  def normalMethod = s"case $normalVal"
}

class Leticia extends Person

val leticia = new Leticia
println(leticia.normalVal) // 1
println(leticia.normalMethod) // case 1
```

Case 2: `private def` and `private val`


```scala
class Person {
  private val privateVal = 2
  private def privateMethod = s"case $privateVal"
}

class Leticia extends Person

val leticia = new Leticia
println(leticia.privateVal) // error
println(leticia.privateMethod) // error
```



## Case 3: `protected def` and `protected val`


```scala
 class Person {
     protected val protectedVal = 3
     protected def protectedMethod = s"case $privateVal"
 }
 
 class Gabi extends Person
 val gabi = new Gabi
 println(gabi.protectedMethod) //error
 
 class Leticia extends Person {
    override val protectedVal = super.protectedVal
    override def protectedMethod = super.protectedMethod
 }
 val leticia = new Leticia
 println(leticia.protectedMethod) // case 3
 
```


## Case 4: `final def`/ `final val`

```scala
class Person {
  final val finalVal = 4
  final def finalMethod = s"case $finalVal"
}

class Leticia extends Person {
  override val finalVal = super.finalVal // error
}

class Marco extends Person
val marco = new Marco
println(marco.finalVal) //case 4
```


----------

Since I wanted this post to be more like a cheatsheet than a heavy text post… this is it for now 🙂 See you in Part 4 (maybe?)! 

