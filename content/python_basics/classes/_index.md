---
title: "Classes"
date: 2020-09-30T09:16:28+02:00
draft: true
---

Classes as a concept are the basis of many programming languages. Object Oriented Programming is based on the idea that there are these "objects" which contain both data, and code. This is the Class notion in Python. In fact everything in Python is a class/object and these terms are used interchangeably. You'll note that in the [lists and loops]({{< ref "/python_basics/lists_and_loops" >}}) section, we defined a variable which was a string, but this string had a `__add__()` method inside of it. This is because **everything in Python is an object**.

For example:

```python
>>> (5).__add__(10)
15
>>> (5).__doc__
"int(x=0) -> integer int(x, base=10) -> integer
Convert a number or string to an integer, or return 0 if no arguments are given.  If x is a number, return x.__int__().  For floating point numbers, this truncates towards zero.  If x is not a number or if base is given, then x must be a string, bytes, or bytearray instance representing an integer literal in the given base.  The literal can be preceded by '+' or '-' and be surrounded by whitespace.  The base defaults to 10.  Valid bases are 0 and 2-36. Base 0 means to interpret the base from the string as an integer literal. >>> int('0b100', base=0) 4"
```

Kind of strange right? The number `5` has methods on it! This extends beyond numbers and onto all Python objects.

### How to Construct a Class
