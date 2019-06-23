---
layout: post
title: "We need to talk about Julia language"
categories:
  - english
tags:
  - english
  - community
  - comunidade
  - sponsorship
  - open-source
  - events
  - technology
featured-img: julia
last_modified_at: 2019-06-23T18:25:52-05:00
---

<center><img src="https://media.giphy.com/media/VMmRM3EjhjBII/giphy.gif" style="height:300px;"/></center>
<center><i></i></center>
<br/>

Julia is a programming language that I heard a lot for some time now and I knew it deserved my attention. However, a number of libraries and frameworks for machine learning and deep learning keep emerging and I ended up prioritizing them first. We of the [Pizza de Dados](http://pizzadedados.com/) (my brazilian podcast about data science), wanting to give the best to our listeners, decided [to make an episode about a language](https://podcast.pizzadedados.com/e/episodio-021/). That was when I decided I needed to sit in the chair and actually look at this unknown yet intriguing language. And I confess that I fell in love with the little I saw! So we need to talk about this wonderful language that is not being recognized as it should.

Obs: I use a lot of my Python experience to compare what I was finding in Julia. If you are just starting in programming, data science and Python, I strongly recommend you read [this text](https://leportella.com/english/2019/01/25/common-data-science-tools.html) before going further 🙃

Obs 2: My podcast, [Pizza de Dados](http://pizzadedados.com/), [launched an episode in portuguese about the language](https://podcast.pizzadedados.com/e/episodio-021/). If you speak portuguese be sure to listen to the episode as well!


## Starting to dig deeper in the language

Julia was created in 2012 by Alan Edelman, Stefan Karpinski, Jeff Bezanson and Viral Shah ([Bezanson et al., 2012](https://julialang.org/images/julia-dynamic-2012-tr.pdf)). It is a [free and open source language](https://github.com/JuliaLang/julia), just like R and Python.


I had already heard people saying how Julia is a performance language, and this is also described in several places of the official website. What really impressed me was that, despite being high-level like Python, speed tests put the language at the same level of extremely fast compiled languages as Rust or Go. See the run-time comparisons of some algorithms in different languages:

<center><img src="https://i.imgur.com/Ail3AU6.png" style="height:400px;"/></center>
<center><i>Source: <a href="https://julialang.org/benchmarks/">Official website</a></i></center>
<br/>


In fact, this was the main objective when the authors created the language: the performance of a statically compiled language (such as C and Fortran) with the interactive / dynamic behavior and productivity of languages such as Python and Ruby ([Bezanson et al., 2012](https://julialang.org/images/julia-dynamic-2012-tr.pdf)).

Many people point out that Julia's Just-In-Time (JIT) compilation is the main reason for the speed. However, other languages like R and Python also use this type of compilation. [This tutorial](http://ucidatascienceinitiative.github.io/IntroToJulia/Html/WhyJulia) shows that several decisions in code design were contributing factors more than JIT.

It can still be mentioned that the language [is made to allow competition, parallelism and distributed computing](https://en.wikipedia.org/wiki/Julia_%28programming_language%29). It is also possible [to directly call C and Fortran libraries without the need for an intermediate library](https://docs.julialang.org/en/v1/).


## Where to start?

Julia's own website has [a list of materials to study, outside the official documentation](https://julialang.org/learning/). The [ThinkJulia](https://benlauwens.github.io/ThinkJulia.jl/latest/book.html) book, based on the famous [ThinkPython](http://shop.oreilly.com/product/0636920025696.do), was a good place to start. Also, if you already have a base in R, Python, MATLAB or C / C++, [the documentation lists the main differences between Julia and your base language](https://docs.julialang.org/en/v1/manual/noteworthy-differences/). Simply sensational!

## Installation

It was pretty straightforward to install it. [In the Downloads page of the official website](https://julialang.org/downloads/) I downloaded an installer for MacOS and that was it! I opened the program and found that, in fact, what it opened Was a simple terminal that runs the binary that is located in a folder. Like this:

```
exec /Applications/Julia-1.1.app/Contents/Resources/julia/bin/julia
```

So, to make life easier, I added an `alias` in my `.bashrc` so that every time I wrote `julia`, he actually called that file:

```
alias julia="/Applications/Julia-1.1.app/Contents/Resources/julia/bin/julia"
```

Once I did it... everything was ready to run 😜

## Installing packages 

Julia has [an extensive collection of packages](https://juliaobserver.com/), similar to Python's [PyPi](https://pypi.org/). 
To install a package just type `]` inside the interpreter that it "transforms" the interpreter into an installer. Take a look:


<center><img src="https://cdn-images-1.medium.com/max/1600/1*DkyKrnt1spV_oFm9Gkyang.gif" style="height:300px;"/></center>
<center><i>Installing a package in Julia</i></center>
<br/>

In the case above I installed the `ThinkJulia` package, the book I followed to study for this text. After installing, I must declare inside the interpreter so I want to use the `ThinkJulia` package:

```julia
julia> using ThinkJulia
```

Now [all the functions inside that package are available for us](https://benlauwens.github.io/ThinkJulia.jl/latest/book.html#chap04):

```julia
julia> using ThinkJulia
julia> 🐢 = Turtle()
Luxor.Turtle(0.0, 0.0, true, 0.0, (0.0, 0.0, 0.0))
```

Another way to add packages without opening the default installer is to use a package to install other packages. We can declare that we want to use the `Pkg` package in the same way we called `ThinkJulia`, and then use the `.add()` method to do the actual installation:

```julia
julia> using Pkg
julia> Pkg.add("ThinkJulia")
```

## Using Julia in notebooks

You can use [Jupyter Notebooks](https://jupyter-notebook-beginner-guide.readthedocs.io/en/latest/what_is_jupyter.html) with languages other than Python. [A long list of kernels](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels) supported by Jupyter is available, and IJulia (in reference to [IPython](https://ipython.org/) that was the basis of Jupyter) is among them:

```julia
julia> using Pkg
julia> Pkg.add("IJulia")
```

Once the kernel is installed, we can use the `IJulia` package and start the notebook:

```julia
julia> using IJulia
julia> notebook()
```

In my case, the program asks if I want to install Jupyter though [Conda](https://docs.conda.io/en/latest/miniconda.html) and, despite having both in [Python Virtual Environments](https://docs.python.org/3/tutorial/venv.html), a series of packages were installed:

<center><img src="https://i.imgur.com/BOyniWX.png" style="height:200px;"/></center>
<center><i>Installations that are made when we first call the Jupyter Notebook inside the Julia terminal</i></center>
<br/>


Once I had everything installed, the default screen of the Jupyter Notebook opened on my browser and now I had both Python 2, 3 and Julia as options:

<center><img src="https://i.imgur.com/eQQVhQV.png" style="height:300px;"/></center>
<center><i>When I started the notebook, I had the option of Julia</i></center>
<br/>


## Starting the work


### Variable names

The variable definitions are the same as those found in Python, and also Julia has extensive Unicode support. This means you can have variable names with Japanese letters, accent and even emojis 😰.

<center><img src="https://i.imgur.com/xcFTZxr.png" style="height:300px;"/></center>
<center><i>Julia has extensive support to Unicode</i></center>
<br/>

[Not all unicode characters are available](https://benlauwens.github.io/ThinkJulia.jl/latest/book.html#characters) to name variables. The `@` character can not be used. But it is very interesting to think about the possibilities, especially considering countries whose languages do not even use the Latin alphabet.


### Creating a script


Julia files have the `.jl` extension and can be executed using the our `julia` alias:


```
julia my_file.jl
```

I created a file called `julia1.jl` calling the `println` function to print a short text on the screen:

```
# julia1.jl
println("My favorite 🍕 is Pizza de Dados!")
```

My first script in Julia!


Just now I faced my first bug: coming from Python, I never worried in thinking if I was using single quotes (`'`) and double quotes (`"`). When I used single quotes, I got a syntax error, warning that I had used an invalid character.


### Strings

Some things in string manipulations caught my attention. The first is that string concatenation does not use the sum item, but the multiplication item! So `"hello" + "world"` returns an error while the expression `"hello" * "world"` returns the result we want: `hello world`.


Several string manipulation functions come by default. As the `isreverse` that returns if one string is the reverse of the other, `inboth` that returns the common characters in two strings, and the `findfirst` that returns the interval where one particular string is in another string.

### Mathematical symbols are trending!

In the various things I read about Julia, it is always said that it is a language that brings a lot of mathematics with it. Not surprisingly, in order to verify that a letter is inside a word, the mathematical symbol of "belongs to a set" is used:

```
julia> "a" ∈ "banana"
true
```

And to know the pi value, you can just type use the greek symbol:

```
julia> π
π = 3.1415926535897…
```

In the interpreter you can access these and other symbols similar to what you do in LaTex: `\in + Tab` turns to `∈` while `\pi + Tab` generates the `π`.

Similar to MATLAB/Octave, Julia closes blocks with the `end` statement. So we can add our `println` inside a function like this:

```julia
# julia2.jl
function favorite_pizza()
  println("My favorite 🍕 is Pizza de Dados!")
end

favorite_pizza()
```

And we can write a function that receives paramaters like this:

```julia
function mysum(x, y)
  x + y
end

# returns 5
mysum(2, 3)
```

But, remember that Julia is strongly attached to mathematics? [We can actually write the same function the same way we would write it as a mathematical formula](https://docs.julialang.org/en/v1/manual/functions/):

```
julia> soma(x, y) = x + y
```

Very cool and very easy to understand! The more I read, the more my head was exploding... in Julia, the operators (`+`, for example) are only functions with special characteristics. Then the sum of some elements can be done by calling the sum function, or the `+` in this case:


<center><img src="https://i.imgur.com/93wMaPR.png" style="height:200px;"/></center>
<br/>


<center><img src="https://media.giphy.com/media/Ysce790SgjJK0/giphy.gif" style="height:200px;"/></center>
<br/>


## Let's stop here

A language is always a new world to be discovered! This text was only a tale of the delightful (and crazy) experience of beginning to enter into this new and rather interesting world. As we talked about in the Pizza de Dados episode, the language is still new and there is still a lot to be done: many packages still need to be stable, the community should increase, and so on. But of course, this is a language that was born with a lot of potential!
