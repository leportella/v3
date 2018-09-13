---
layout: post
title: "10 tips for switching from Matlab to Python" 
categories:
  - english
tags:
  - en
  - python
  - begginers
featured-img: idea
last_modified_at: 2018-07-27T14:25:52-05:00
---

I started to study programming logic when I came across problems that required knowledge in Matlab. 
After a while studying Matlab I was suggested to switch to Python for its ease, simplicity and for 
being able to be applied to numerous areas (besides being free).

However, as I moved from one tool to the other, I had a number of basic issues 
that made the change a bit tricky. Sometimes I had to change my vision about 
some things that were already rooted in me. I decided to share both my 
main difficulties and some tools that helped me in the transition.

Just to make it clear, everything that is presented here is in Python 3 
because Python 2 is [about to retire](https://pythonclock.org/) :(

![](https://media.giphy.com/media/nI3Npa6iPC4b6/giphy.gif)

It is not the purpose of this post to discuss which tool is better, 
but to show how to make easier the transition from Matlab to Python :)


## 1. Numbers, letters and other "little things" ...

With Matlab we can create vectors and arrays of numbers or texts, which are 
intuitive and quite useful. Basically, we can access any value
 in a vector or matrix based upon its position within it (Ex: `list (1,2)`).

With Python, in addition to lists we get to see different and very powerful 
structures, such as dictionaries. 

Dictionaries are structures of data which you can access through the "key" 
of the value and not by its position in the structure. 
Want to know  its utility? In a phone book which would it be easier: 
to find numbers by the position in the list or by the name of the contact? :D

Besides, there is a difference between decimal numbers (**float**) 
and **integers**. That is, first it is better to get acquainted with 
existing Python types of variables and structures! That's a pretty big advantage!


![](https://media.giphy.com/media/6FymBmqKeBrl6/giphy.gif)
*Me when I first saw dictionaries and tuples*

If you are in doubt about your 
[variable type](https://docs.python.org/3/library/stdtypes.html), just type in the 
intepreter: `type(variable)` ;)


## 2. Working with words and numbers

Whether through dictionaries or lists (standard Python frameworks) 
or DataFrames (Pandas structure of matrix/array), Python is quite easy 
to work with numeric data and text in the same context. T
he same can't be said of Matlab, where entering text complicates everything!

Listing of type `list = [1, 'two', [1,2,3], {'key': 1}]` it is totally acceptable!

## 3. Accessing values in lists (and list of lists)

In Matlab, counting lists starts at 1 and it is accessed through parentheses. 
The last value can be accessed through the word "end".

```matlab
% Accessing list items in Matlab

my_list = [1, 2, 3];

% returns 1
disp(my_list(1))
  
% retuns 3
disp(my_list(end))
```

In Python, lists are accessed with brackets (parentheses are reserved for function and 
classes calls) and the counting starts at **0**. The reason why it actually starts at 0, instead 
of 1 is outside of the idea behind this post, but you can check some 
discussions [here](https://www.quora.com/Why-do-array-indexes-start-with-0-zero-in-many-programming-languages?share=1). 
Anyway, this was very hard for me to get used to, as simple as this sounds.

Tipo: to access the last value, we can use the index **-1**. Can you guess what **-2** does? :)

```python
# Accessing list items in Python

my_list = [1, 2, 3];

# retuns 1
print(my_list[0])
  
# retuns 3
print(my_list[-1])

# list of lists
complex_list = [[1, 2, 3], [4, 5, 6]]

# returns 3
print(complex_list[0][2])

# retorna 5
print(complex_list[1][1])
```

## 4. Blocks do not end with "end" and need ":"

In Matlab, the simplest way to make a loop is shown below, where you have a 
list and enumerate them through its items. In this loop below, 
`for` is the beginning of the loop and `end` define its end. 
The interpreter will read everything that is between these two 
statements as part of the loop.

Note that indentation¹ inserted is optional, it is just a good practice.

*¹Identation means when the blocks of your code are markedby a space to
identify what's nested inside the code. In the example below, the four spaces 
before the declaration of line 6 indicate that this statement 
is nested (ie inside) of the loop indicated in line 5 (without indentation).*


```matlab
% Creating the list 
y_list = ['a', 'b' , 'c'];

% Looping by accessing list items
for item=my_list  
    disp(item)
end

disp('Outside of loop')
```

We can see below the same loop written in Python. Now, `:` marks the beginning 
of the loop and there is no `end` to show the end. 
The end is pointed out by the end of indentation on line 7. 
The interpreter sees that line 7 is not nested within the loop 
started in line 5 and, therefore, the interpreter does not consider 
that line within the block. In this way, 
it is obvious that indentation, in this case, is not optional but obligatory!

```python
# Creating the list
my_list = ['a', 'b' , 'c']

# Looping by accessing list items
for item in my_list:  
    print(item)
print('Outside of loop')
```

## 5. Functions and methods available

In Matlab, several functions are loaded by default, for example the basic math 
functions such as sine and cosine. Python has a different philosophy where 
[few functions](https://docs.python.org/3/library/functions.html#built-in-funcs) 
are loaded by default and most of them are separated into modules that you 
import into your code. This allows your code (and Python itself) to be faster and just load what is necessary for it to work.

Some modules already come by default in pure Python, 
for example the mathematical module. 
Others should be downloaded from a repository, 
such as Numpy and Pandas. 
Today, more than 107 thousand projects are registered in the 
[official repository](https://pypi.org/). 
If you don't find what you are looking for, you are strongly encouraged to build a new module and to add it to the repository!


```python
import math

print(math.cos(0.5))

# another way to use the same cosine method

from math import cos

print(cos(0.5))
```

However, besides the functions that come in the modules, 
each variable has its own functions. 
Specific to each type of variable, those functions that can be accessed 
through the variable's name as shown in the example below.

For example, *string* variables are interpreted as text. For text variables 
there are functions to check if all letters are lowercase (`.islower()`) 
or to transform all the letters in lowercase (`.lower ()`) or even to check 
if the contents of the string are numeric (`.isdigit ()`).

For each type of variable 
(string, integer, float, list, dictionary, etc.) 
there are different builtin methods. Best way to get to know them is to open 
the interpreter, create variables and play!


```python
# string variable 
word = 'word'

# check if variable is all lowercase 
# returns True
word.islower()

# check if there is numeric character in the word
# returns False
word.isdigit()

my_list = [1, 2, '3']

# adds item to list
# returns [1, 2, '3', 'four']
my_list.append('four')

# account recurrence of an element in the list
# returns 1
my_list.count('3')
```

## 6. How to know which module I should use?

If you are using Matlab, [Numpy](http://www.numpy.org/), 
[Scipy](https://www.scipy.org/), 
[Matplotlib](http://matplotlib.org/) and 
[Pandas](http://pandas.pydata.org/) will meet your need. Those are the main 
data analysis tools in Python, but there are many others. Look for codes that 
have already done something similar that are available in 
[Github](http://github.com/), [Stackoverflow](http://stackoverflow.com/) or 
check foruns.

## 7. Where do I see the loaded variables?

One of the most recurring problems in Matlab was that we were used to 
visualizing variables. The "terminal" was there, but also the script we were 
using and the loaded variables as well. 
The possibility of running just one line of code with a 
simple F9 without necessarily running the entire code was awkward to me.

In Python tutorials, we see that "terminal" where you type things and 
the results come out. 
In the beginning, this is frightening. Eventually I got used to it, 
but using [Spyder](https://pypi.python.org/pypi/spyder) helped in the transition... A LOT.

Spyder is a tool for Python, made in Python, which has an interface 
very similar to that of Matlab: scripts are on the left side, 
on the upper right side it is possible to see the loaded data and 
how they are composed (even with a colored background that helps a lot 
to quickly find the discrepancies) and a terminal with ipython in 
the lower right part. F9 also works the same way! 
For me, it was love at first sight ❤.

![](https://media.giphy.com/media/l3V0dy1zzyjbYTQQM/giphy.gif)

Look at its face:
![](https://i.imgur.com/RgKovVN.png)

Example of Spyder running with an 11-line script (left), variables loaded (upper right), and ipython terminal (lower right)

## 8. Seeking help

If you don't know what a function does, simply type: `help(function)` and be 
happy :) If you have it in Spyder, you can go on the function and type `Ctrl + I`.

## 9. Orientation to object

Although Matlab constructed something similar to Object Orientation, 
Python is strictly oriented towards object. 
As you progress with language, the concepts of classes 
(with methods, attributes and class inheritance) should be studied further. Stay tuned!

## 10. Code Formatting

There are a number of good code practices guides, such as acceptable line 
sizes, how to name your variables, how many rows between functions, and so on. 
I had never heard of good code practices before studying another 
language ... so that was kind of strange (and unnecessary) at first. 
After a while, you realize how good this is and it makes a huge difference!

Considering Python, this good practice guide is tied to a clean, well written, 
readable pythonic philosophy which is very much rooted in the community. 
To learn more about the suggested style, see [PEP8](https://www.python.org/dev/peps/pep-0008/). 
To learn more about the Python philosophy go to the terminal and type `import this` and welcome!

![](https://media.giphy.com/media/3o6ZsYMuMkxBNiy7pC/giphy.gif)

*Welcome to the Zen of Python*
