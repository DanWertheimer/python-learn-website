---
title: "Classes"
date: 2020-09-30T09:16:28+02:00
draft: false
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

Lets construct an employee class:

```python
class Employee:
    """This is a foogle employee"""
    def __init__(self, name: str, surname: str, salary: float) -> None:
        self.name = name
        self.surname = surname
        self.salary = salary
        self.email = f'{name.lower()}.{surname.lower()}@foogle.com'
```

The most important part of the class constructor is the `__init__()` method. This is the method that is called when you initialize the class, for example:

```python
>>> employee_1 = Employee('John', 'Smith', 1000)
>>> employee_1.__doc__
'This is a foogle employee'
>>> employee_1
<__main__.Employee at 0x7fbfd40e79b0>
>>> employee_1.email
'john.smith@foogle.com'
```

But what is this `self` variable? `self` refers to the object that is created. this means that by assigning `self.name=name` in our initializer, we can access the `self.name` variable anywhere in our class.

You'll note in the above function, we don't need to define the email and it is automatically constructed based on the `name` and `surname` parameters we passed to it. But you'll notice that when we try to inspect `employee_1` directly, we don't get a very nice representation. We get this `__name__` of the module and the location in memory that it's stored. Not very useful!
To solve this we can use two special dunder methods of classes. These are known as the `repr` and `str` for the class:

```python
class Employee:
    """This is a foogle employee"""

    def __init__(self, name: str, surname: str, salary: float) -> None:
        self.name = name
        self.surname = surname
        self.salary = salary
        self.email = f'{name.lower()}.{surname.lower()}@foogle.com'

    def __str__(self) -> str:
        return f'{self.name} {self.surname}'

    def __repr__(self) -> str:
        return f'Employee(name="{self.name}", surname="{self.surname}", email="{self.email}")'
```

The `__str__` method is a readable **string representation** of the class. In this case when we call `str(employee_1)` we want to return the name and surname of the employee.
The `__repr__` method is also a string representation but it is used for debugging and is unambiguous in that it should return a view of what your data should look like.

```python
>>> employee_1 = Employee('John', 'Smith', 1000)
>>> employee_1 # this will access the __repr__
Employee(name="John", surname="Smith", email="john.smith@foogle.com")
>>> str(employee_1)
'John Smith'
```

Right now, all we've included in our Employee class is some data about an employee. But we can also create functions:

```python
class Employee:
    """This is a foogle employee"""

    def __init__(self, name: str, surname: str, salary: float) -> None:
        self.name = name
        self.surname = surname
        self.salary = salary
        self.email = f'{name.lower()}.{surname.lower()}@foogle.com'

    def __str__(self) -> str:
        return f'{self.name} {self.surname}'

    def __repr__(self) -> str:
        return f'Employee(name="{self.name}", surname="{self.surname}", email="{self.email}")'

    def give_raise(self, increase_percent: float) -> float:
        self.salary *= increase_percent
        return self.salary
```

Here we've created a `give_raise()` function which takes the class instance, as all functions must, and the percentage to increase someone's salary by:

```python
>>> employee_1.salary
1000
>>> employee_1.give_raise(2.0)
2000
>>> employee_1.salary
2000
```

There are some dangers when dealing with classes though. Let's say we hire Daniel Smith, but his salary is going to be the same as John Smith. It might make sense to just clone `employee_1`.

```python
>>> employee_4 = employee_1
>>> employee_4.name = "Daniel"
>>> employee_4 # This is Daniel
Employee(name="Daniel", surname="Smith", email="john.smith@foogle.com")
>>> employee_1 # This is John
Employee(name="Daniel", surname="Smith", email="john.smith@foogle.com")
```

Oh no! What happened here? We wanted to create a new employee using our old one but we've ended up editing them both! This is because `employee_1` and `employee_4` are now the **same object**. This is intended and the way to do this is through the `deepcopy` module.

```python
>>> from copy import deepcopy
>>> employee_5 = deepcopy(employee_1)
>>> employee_5 == employee_1
False
>>> employee_5.name = "Alex"
>>> employee_1.name
"John"
>>> employee_5.name
"Alex"
```

There are other dunder methods that we can implement for classes. Lets say we want to compare employee salaries using the `>` and `<` symbols. If we tried to do this with our current `Employee` class:

```python
>>> employee_1 > employee_2
TypeError: '>' not supported between instances of 'Employee' and 'Employee'
```

lets edit our original employee class:

```python
class Employee:
    """This is a foogle employee"""

    def __init__(self, name: str, surname: str, salary: float) -> None:
        self.name = name
        self.surname = surname
        self.salary = salary
        self.email = f'{name.lower()}.{surname.lower()}@foogle.com'

    def __str__(self) -> str:
        return f'{self.name} {self.surname}'

    def __repr__(self) -> str:
        return f'Employee(name="{self.name}", surname="{self.surname}", email="{self.email}")'

    def __gt__(self, other):
        return True if self.salary > other.salary else False

    def __lt__(self, other):
        return True if self.salary < other.salary else False

    def give_raise(self, increase_percent: float) -> float:
        self.salary *= increase_percent
        return self.salary
```

We've implemented the `__gt__` and `__lt__` methods which tell Python how to deal with greater than and less than operators. So now:

```python
>>> employee_1 > employee_2
False
>>> employee_1.salary
1000
>>> employee_2.salary
2500
```

There are many more dunder methods to help you when working with your own classes and you can view many of them [here](https://dbader.org/blog/python-dunder-methods).
