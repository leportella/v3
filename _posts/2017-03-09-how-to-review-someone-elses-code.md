---
layout: post
title: "How to review someone else's code"
categories:
  - english
tags:
  - en
  - python
  - tests 
  - code-review 
featured-img: feedback
last_modified_at: 2017-03-09T14:25:52-05:00
---

Code review is a complicated task and can become overwhelming, specially when you have no idea how to do it.
However, code review can be a powerful tool to increase code quality and assure “healthy” deploys.

![](https://cdn-images-1.medium.com/max/800/1*EFsX-ndhmx4CFsI98zSvKA.gif)

In Crave Food Services we developed a methodology to follow when it is time to review a new piece of code. On this article, we will describe a little better how our methodology works.

It is important to notice that this text doesn’t have the goal to discuss static code analysis, but rather to give
a general guideline for someone that have no idea how to start a code review.
The final goal of this text is to just have a high code quality and a practical idea on how to do a good review.


## 1. Does it work?

All code is written with some purpose. It can solve a specific problem, add a new feature or even refactor
 some old code. So, the first question you should ask yourself is: “does it fulfill the proposed goal?”. Don’t 
believe the code works. Try it. If it is a new endpoint, try it locally. If it is a new function, try it with the correct parameters and the incorrect parameters as well. Can it handle all exceptions in a right way? Try to find possible flaws.

## 2. Does it have docs?

Documentation is something that can help you in your first task (to see if it works). So, to verify if there 
is some documentation and to check if the code in working are joined tasks.
 However, in an ideal world, the code must speak for itself. If it will be somehow used by someone else that
 can’t read your code, then the ideal is to document what has been done.

A good documentation makes everything easier since after some months things just get lost on the flood! Even if you believe that is just a simple file with some function, a readme file is always a good idea. Trust me, you will forget what you have done!

However, always use some good sense when using documentation. Too much documentation can be overwhelming and obsolete!


## 3. Is it readable? Can you easily understand it?

Here we have philosophy that the code must explain itself. We use Python and, for those who doesn’t know it,
its philosophy says: “simple is better than complex”.

The code usually should speak for itself. If something is too complex for someone to understand it, usually
there is a better way to do it. There is an awesome phrase that says: “write your code as if the 
maintainer is 
a serial killer that knows where you live”.

## 4. If the code is complex, there are enough comments to help understand it?

Ok, we can’t always do things in a simple way. If what is written is complex — and must be complex, which means 
that it can’t be any simpler than that — the code must contain comments to help a future reader/maintainer 
understand what is written. Again, memories fade away and the code can become undecipherable after a while. 
And, if possible, remember: “complex is better than complicated”.

## 5. The code is according the quality patterns?

In Python, for example, we have PEP8 that suggest some “rules” of good writing. The code must follow these rules 
of code quality both from the language and from the work environment. It is important to notice that environment 
rules where the code is overcomes all general rules. Understand the environment where the code is and review it thinking about its “natural habitat”.

Here in Crave Food, these checks are manual until we add automatized code quality reviewers. For example, no 
new code is added if it is not ok with Flake8 and Isort.

## 6. Does it have tests?

What can we say about tests? Tests can help a lot in the guarantee of code quality and can avoid a lot of trouble (specially when you changed some part of the system that you were not even aware that existed). By itself, tests can assure code quality on the system. If there is a test policy in your company, demand tests on your review. Check if the tests added covered all possible paths (expected and exceptions) and if the coverage is not diminished.

## 7. Give feedback.

The major goal of a code review is to have an improvement (or maintenance) of code quality. A good review can help a lot for this to happen. If this review is combined with a test suite and automatized code checks, even better.

However, it is always good to remember that whoever wrote this code you are reviewing is a human being just as yourself. That said, remember that the person could be in a bad day while doing the code, can misunderstand what you wrote in your review or even didn’t think about another possible solution for it. Be polite. Never command something. Specially if you are dealing with beginners. If there is something truly bad written or if you thought in a different approach, talk with your colleague in person. Codes are made for machines, but who wrote is very human :)
