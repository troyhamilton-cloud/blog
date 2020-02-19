---
title: "This post is brought to you by the letters NSA"
date: 2020-02-16T10:36:53-05:00
draft: false
toc: false
images:
categories:
  - infosec
tags:
  - python
---

In 1997 I recall my VB6 college professor [Mr. Shroff](https://www.absolutedental.com/doctor/sulabh-shroff/) asked us to name a programming language.  At the time I was studying for my AA in Electronic Engineering but I was also working at CompUSA which is probably the best job I ever had.  It was the catalyst for me into IT.  Anyway, Mr. Shroff looks at me, asks me to name a language, and after a brief moment I remember chatting with Brandon at the upgrades counter at work and his new jump on Python.  'Great language, very powerful' he exclaims and my dumbass should've picked up on that.  But hey VB6 wasn't all that bad right?  Alas, here we are.

If you haven't seen it yet, thanks to a FOIA submitted by [@chris_swenson](https://twitter.com/chris_swenson/status/1225836060938125313), the NSA has unclassified it's COMP3321 course on Python.  Thanks Chris!

The ocr'd pdf is 395 pages of 30+ lessons or sections.  The material is designed to be covered in a 2 week period if the user allots themselves about an hour per lesson.  Barring any interruptions, I hope to complete it by the end of February with week 1 starting today.

This post should be read in the context of me just chatting with someone at the bar about the course and not actually teaching about Python or programming.  However, if you're new to programming and have somehow fumbled your way into my cozy little spot here then please listen to me when I say, learn Python!  Trust me, it will open many doors for you.

Moving on!

Here's the breakdown for Week 1:

- Lesson 01: Intro to Python
- Lesson 02: Variables and Functions
- Lesson 03: Flow Control
- Lesson 04: Container Data Types
- Lesson 05: File Input and Output
- Lesson 06: Development Environment and Tooling
- Lesson 07: Object Orienteering: Using Classes
- Lesson 08: Modules, Namespaces, and Packages
- Lesson 09: Exceptions, Profiling, and Testing
- Lesson 10: Iterators, Generators, and Duck Typing
- Lesson 11: String Formatting

As with most Python tutorials, Week 1 begins with an introduction which calls for an installation of [Anaconda](https://www.anaconda.com/distribution/), specifically version 4.4.0.  Anaconda is one of the largest Python distributions that give you access to all the libraries, dependencies, packages, and environments that you'll need for this lesson and probably any project a beginner would be involved in.

---

# Lesson01

So what's covered in Lesson 01?  It starts off with 'Basic Basics: Data and Operations' and explaining basic data types such as integers, floats, complexes, strings, and booleans.  We perform data operations against data types with operators, functions, and methods.

I like how they make a distinction between functions and methods:

> Functions: operations that take one or more pieces of data as arguments e.g. `type('hello')`, `len('world')`
> Methods: attached to a piece of data and called from it using a . to seperate the data from the method e.g. `'Hello World'.split()` or `'abc'.upper()`.

In C#, it is more common to use methods for both concepts e.g. `PrintResults(result);`, `result.ToString();`.

Using a split terminal in vscode you can navigate through intro stuff rather quickly.

![split terminal](/images/vscode-python-split-terminal.png)

Moving on we read about a few built in functions:

- `help(x)`
- `dir(x)`
- `type(x)`
- `print`
- `hasattr(a,b)`
- `getattr`
- `id`
- `input`

Followed up with other fuctions that only work with number and string types:

- `abs`, `round`, `float`, `max`, `min`, `pow`, `chr`, `divmod`
- `<<`, `>>`, `&`, `|`, `^`, `~`
- `len`, `min`, `max`, `ord`
- `+`, `*`, `in`
- `strip`, `split`, `startswith`, `upper`, `find`, `index`

---

# Lesson02

Here come the variables!  Very useful stuff here like:
- Finding current variables in use with `dir()`
- Type checking with `isinstance()`
- Conversions like `a = "3.14" b = float(a) type (b)`
- A brief example for an array `l = ["one", "two", "three", "four"]`

Where lesson02 shines is talking about making your own functions.

```
def first_func(x):
      return x*2

first_func(10)
first_func('hello')
```

![python idle](/images/python-idle.png)

Gotta love a little bit of enthusiasm here from the NSA:

> Wow... Python REALLY does not care about types.  Here is the simplest function that you can write in Python (no imput, no output, and mot much else!)

```
def simple():
      pass
```  
