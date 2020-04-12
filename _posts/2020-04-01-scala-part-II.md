---
layout: post
title: "My saga learning Scala - Part 2"
categories:
  - english
tags:
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
  - Software engineer
  - software engineering
  - engenheira de software
  - desenvolvedora
  - mulheres que codam
  - mulhers desenvolvedoras
  - programadora
  - events
  - payments
  - development
  - women in tech
  - woman in tech
  - girls who code
  - career
  - computer science
  - self-taugh engineer
  - self-learning
  - programmer
  - ruby
  - scala
  - python
featured-img: scala
last_modified_at: 2020-04-01T18:25:52-05:00
---


Continuation of [My Saga learning Scala - Part 1](https://leportella.com/english/2020/03/08/scala-part-I.html). 

On the last post I talked about somethings I've learned on Scala. Although [my tweet about this post](https://twitter.com/leleportella/status/1237322864514281472) generated some comments about the language (both good and bad) I am still decided to [at least finish the course I started doing](https://www.udemy.com/course/rock-the-jvm-scala-for-beginners/). So far the feeling is that I am starting to understand some things and starting to be more comfortable, although I am very far away from the magic stuff that scares me.

Another thing is that most of the comparisons and things that I find interesting are based on my knowledge from Python and may be super normal in other languages. So remember that while reading this post 🙂 


## Scala object

All classes start with an object and we write things inside it. The object always extends an `App` and things must be done inside it:

```scala
object MyObject extends App {
  // do stuff...
}
```

The course I am doing was never clear (so far) why we should do things inside it or why to use an object instead of a class. [The official tutorial says the following](https://www.scala-lang.org/documentation/your-first-lines-of-scala.html):


> *If that object extends trait `scala.App`, then all statements contained in that object will be executed; otherwise you have to add a method `main` which will act as the entry point of your program.*

So the alternative would be to do something like this:

```scala
object MyObject {
  def main() {
    // do stuff
  }
}
```

But apparently [there are also some advantages](https://stackoverflow.com/a/11667791/3538098) by using the `extends App` instead of the main method.


## Functions

I **loved** the course classes about functions. It was amazing. First because functions are defined very simple:

```scala
object MyObj extends App {
  def aParameterlessFunction(): Int = 42

  def aFunction(x: Int) = x
}
```

If you notice, the first function (`aParameterlessFunction`) indicates that the return type of the function is an Integer. However, the second one (`aFunction`) I didn't! Scala compiler understands that the function returns an Integer, and it works just fine. I don't really like to keep defining things while I am writing my functions, so this seems really useful. However, this is not true in one type of function: recursive functions. Recursive functions are the only types of function that requires you to specify the return type. Your program won't work if you don't. 

Talking about recursion, I really enjoyed the classes about recursion on the course. It was so amazing I would like to study more and do a whole post dedicated to it.

### Some function weirdness

While doing my exercises, I came up to a problem. If I create a function with no parameters, I can call the function with no parentheses:

```scala
def fn = 1
println(fn)
```

However, if I define a function (even with a default value) I need the empty parentheses. 

```scala
def fn(i: Int = 1) = 1
println(fn()) // works
println(fn) // doesnt work
```

It took me quite some time to figure out what I was doing wrong when I tried to call a function with a default parameter without the parentheses. And the answer (that the parentheses was now required) made no sense to me. 

Apparently, Scala has a differentiation between methods and functions, where there function is a value a method isn't. My colleague Graham indicated me [this blog post](https://tpolecat.github.io/2014/06/09/methods-functions.html) for more about it, but Scala magic is already very heavy, so it was not that simple 😅 

### Function within functions

You can also write functions within functions, which is pretty helpful on optimizing recursive functions:


```scala
def aBigFunction(n: Int): Int = {
  def aSmallerFunction(a: Int, b: Int): Int = a + b + n
  aSmallerFunction(n, n-1)
}
```


## Strings!

I had a whole class about string manipulation and for the first time I felt closer to Python! Strings are objects that have several methods just like Python, with the difference that Scala is written with camelcase: `.toLowerCase`, `.replace`, `.startsWith`, `.length`. 

Then you have three types of interpolator. The `s` interpolators are pretty straightforward, it replaces the variable after the symbol `$`:

```scala
val name = "Leticia"
val greeting = s"Hello, my name is $name"
// Hello, my name is Leticia
```

Then you have `f` interpolators for dealing with decimal cases on numbers:


```scala
val height = 1.7f // this is a float
println(f"I am $height%.2f m height")
// I am 1.70 m height
```

Finally you have the raw interpolators, where special characters, like the `\n` that indicates a new line won't be interpreted as a new line, but as the normal characters `\` and `n` in sequence:


```scala
println(raw"This is a \n newline")
// This is a \n newline

println("This is a \n newline")
// This is a
// newline
```

Scala allows both single `'` and double `"`, but the first is a value of type Char while the second is a String. Char is basically a structure that allows only a single character of a String. The bevahiour is very different if you try to apply functions from one to the other:


```scala
println("a" +: 123 :+ "b")
// a123b

println('a' +: 123 :+ 'b')
// Vector(a, 1, 2, 3, b)
```

Oh yeah, the this awesome operators `+:` and `:+` are also useful!

----------

This is it for now 🙂 See you in [Part 3](https://leportella.com/english/2020/04/12/scala-part-III.html) (hopefully)! 

