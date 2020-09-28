---
title: "Lists and Loops"
date: 2020-09-28T11:26:46+02:00
draft: false
weight: 1
---

### Defining a list

Let's start by defining a list. Lists in python are created by using square brackets like this `[item1, item2]`. To create a list we can do this:

```python
example_list = ['a', [1,2], 3, 4.5]
```

We know that this is a list by inspecting it using:

```python
>>> type(example_list)
<class 'list'>
```

Note how Python doesn't complain about the mixed types in the list:

- `'a'` is a string
- `[1,2]` is a list
- `3` is an integer
- `4.5` is a float

This is because Python is duck typed, which comes from the phrase: "if it walks like a duck and it quacks like a duck, then it must be a duck". This means that Python does not check the type of a variable, but rather checks for a given method or attribute. For example, we can take the first element of our `example_list` and add a string to it:

```python
>>> example_list[0] + ' snake'
'a snake'
```

Firstly, let's notice how we retrieved the first element of the list. We used `example_list[0]` this is known as indexing, and in Python, the first index starts at position 0.

Now, let's try add 'snake' to the end of the second element:

```python
>>> example_list[1] + ' snake'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate list (not "str") to list
```

Here, an error was raised, this is because Python doesn't know how to add a string to a list. The underlying method which defines how elements can be added is defined in a class as the `__add__()` method (pronounced dunder add, dunder means double underscore).
You might now be wondering, where does this `__add__()` exist? and this exists within the `list` or `str` classes. In fact, we can rewrite our first command as:

```python
>>> example_list[0].__add__(' snake')
'a snake'
```

But remember,

> Beautiful is better than ugly.

And that's why we make use of the `+` sign.

**But we've diverged.**

### For Loops

Let's come back to our example list. What if we wanted to find out what the `type` of each _index_ in our list is? We'd need a **loop**.
For most programmers coming from other languages, they may be tempted to write the following:

```python
>>> for i in range(len(example_list)):
...     print(f'element {i} is {type(example_list[i])}')

element 0 is <class 'str'>
element 1 is <class 'list'>
element 2 is <class 'int'>
element 3 is <class 'float'>
```

Let's break this down:

- The `len()` function, tells us how long the list is (this is defined in the `__len__()` function in a class).
- The `range` function gives us an **iterable** from 0 to the number passed to the function
  - For loops require an **iterable** to iterate over, these are often known as **generators** which we will cover later.
- Inside the `print()` function we have, what we call an **f-string**, which is a formatted string where any variable places inside the curly brackets is evaluated.
- At each stage of the for loop, we are evaluating the type of `example_list` at the i-th position.

This is quite messy though, maybe we can find a better way?

```python
>>> i = 0
>>> for item in example_list:
...     print(f'element {i} is {type(item)}')
...     i += 1

element 0 is <class 'str'>
element 1 is <class 'list'>
element 2 is <class 'int'>
element 3 is <class 'float'>
```

Nice, this looks a bit neater. We have:

- Initialized `i` to be `0` at the start of our loop
- Utilized Python's unpacking properties. `for item in example_list` now uses the **item** in the **list** as the element
- We increment `i` at the end. The syntax `i+=1` is equivalent to `i = i+1` but much better looking and more pythonic.

But wouldn't it be nice if we didn't need to initialize and increment `i`?

```python
>>> for i, item in enumerate(example_list):
...     print(f'element {i} is {type(item)}')

element 0 is <class 'str'>
element 1 is <class 'list'>
element 2 is <class 'int'>
element 3 is <class 'float'>
```

Here, we've made use of the Python `enumerate` function which keeps track of both the **index**, and the **element** that we're on in the for loop.

Amazing. Lets move one step deeper, lets create a list that stores the type of each element in a new list.

```python
>>> type_list = []
>>> for item in example_list:
...     type_list.append(type(item))

>>> type_list
[<class 'str'>, <class 'list'>, <class 'int'>, <class 'float'>]
```

{{%expand "append() function" %}}
Here we've made use of the `append()` method. This method acts in place. This means that you do not need to assign it to a variable, but it will append to the list it is executed on.
{{% /expand%}}

Nice and easy. But again, there must be a better way than first initialising the list.

```python
>>> type_list = [type(item) for item in example_list]
>>> type_list
[<class 'str'>, <class 'list'>, <class 'int'>, <class 'float'>]
```

This method is known as a **list comprehension** and increases the readability of your code. Note the square brackets around the expression.

Lastly, what happens if we want to store a list of tuples, an ordered, unchangeable (immutable), datatype that has the element's value, and it's type stored as `(element, element_type)` using the two lists that we have.

```python
>>> tuple_list = [(element, element_type)
...               for (element, element_type) in zip(example_list, type_list)]
>>> tuple_list
[('a', <class 'str'>), ([1, 2], <class 'list'>), (3, <class 'int'>), (4.5, <class 'float'>)]
```

The Python `zip()` function can be used to create an iterable tuple out of any two lists. This takes away from writing length for loops such as:

```python
>>> for i, element in enumerate(example_list):
...     tuple_list.append((element, type_list[i]))
>>> tuple_list
[('a', <class 'str'>), ([1, 2], <class 'list'>), (3, <class 'int'>), (4.5, <class 'float'>)]
```

which looks quite messy.
