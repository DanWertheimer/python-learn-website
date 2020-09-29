---
title: "Functions"
date: 2020-09-28T13:53:30+02:00
draft: false
weight: 2
---

Next, we're going to take a look at how we can write functions in Python. A function is a block of code which only runs when it is called. It can take data in the form of parameters and can optionally return a result.

```python
def hello():
    print("Hello there")
```

The above is a function, note how when this is run, there is no output in the terminal, however, when you call the function using `hello()` there is an output.

An example of how to use this with a parameter would be:

```python
>>> def greet(name):
...    print(f'hello {name}')

>>> greet('Daniel')
hello Daniel
```

However, it is preferable to write the code as follows:

```python
def greet(name: str) -> None:
    print(f'hello {name}')
```

The reason for the extra code is to provide readability. We tell users of this function that `name` should be a string and that the function doesn't return anything. It's important to understand that Python doesn't do anything differently if you pass an integer to the `name` parameter. This only aims to increase the readability. This is called [Type Hinting](https://docs.python.org/3/library/typing.html).

#### Returning Values

Functions can return values, lets create a function that scales a number by a fixed amount:

```python
from typing import Union

def scale_number(number: Union[float, int], scaler: float) -> float:
    return number*scaler
```

Woah! We've encountered our first import! In Python, we can import packages built by other people, here we've imported the `Union` function from the `typing` package to help with out type hinting. This means that the `number` we are expecting is either a float or an integer. The function also returns a `float`. You'll notice the `return` method in the function which tells the function what to return.

To use this function:

```python
>>> scale_number(100,1.3)
130.0

# But a better way of typing this is to use the parameter names for better readability
>>> scale_number(number=100, scaler=1.3)
130.0
```

#### Documenting Functions

For people to better adopt our code as well as for us to understand what exactly we've done, it's important to document your functions. This can be done through [docstrings](https://www.python.org/dev/peps/pep-0257/#:~:text=A%20docstring%20is%20a%20string,module%20should%20also%20have%20docstrings).

Lets document the `scale_number` function using the VSCode extension [Python Docstring Generator](https://marketplace.visualstudio.com/items?itemName=njpwerner.autodocstring).

```python
def scale_number(number: Union[float, int], scaler: float) -> float:
    """scales a number by a fixed amount

    Args:
        number (Union[float, int]): the number to be scaled
        scaler (float): the factor to scale the number by

    Returns:
        float: a scaled number

    Example:
        >>> scale_number(number=100, scaler=1.3)
        130.0

    """
    return number*scaler
```

Great, now anyone that wants to use this functions knows exactly how it works, and it has a handy example! This example can also serve multiple purposes. Not only does it tell us how to use it but it also serves to test the function itself. This can be done through the `doctest` library as follows:

```python
def scale_number(number: Union[float, int], scaler: float) -> float:
    """scales a number by a fixed amount

    Args:
        number (Union[float, int]): the number to be scaled
        scaler (float): the factor to scale the number by

    Returns:
        float: a scaled number

    Example:
        >>> scale_number(number=100, scaler=1.3)
        130.0

    """
    return number*scaler

if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

#### Nested functions

Functions can exist within functions. Lets say we want another function that will only scale a number if that number is more than a specific value:

```python
def scale_more_than_x(number: Union[float, int],
                      scaler: float,
                      limit: Union[float, int] = 1000) -> float:
    """scales a number if it's more than a specific number

    Args:
        number (Union[float, int]): the number to be scaled
        scaler (float): the factor to scale the number by
        limit (int, optional): the number at which anything larger gets scaled. Defaults to 1000.

    Returns:
        float: a scaled number if it is larger than the limit, otherwise the number
    """
    if number > limit:
        return scale_number(number=number, scaler=scaler)
    return number
```

What did we do here? Firstly, we defined the `limit` and we set it's default value to 1000. This means if we call this function normally, without the `limit` value, it will default to 1000.

Secondly we've encountered our first `if-statement`. The syntax is simple, if the statement evaluates to `True` then it returns whatever is in the loop, otherwise it will return the number.

Let's try make this a little more robust though. If we try something like this:

```python
>>> scale_more_than_x(number='a', scaler=1.3)
Traceback (most recent call last):
  File "/Users/danwertheimer/Documents/PersonalWork/learn_python_github/python_basics/functions.py", line 59, in <module>
    scale_more_than_x(number='a', scaler=1.3)
  File "/Users/danwertheimer/Documents/PersonalWork/learn_python_github/python_basics/functions.py", line 54, in scale_more_than_x
    if number > limit:
TypeError: '>' not supported between instances of 'str' and 'int'
```

This isn't a very useful error for anyone using this function. Let's update our `scale_number` function:

```python
import numbers
from typing import Any, List, Union

def _check_numbers(check_list: List[Any]) -> None:
    """checks if all values in a list return a number

    Args:
        check_list (List[Any]): List of numbers to check

    Raises:
        TypeError: Input values must be a number
    """
    for value in check_list:
        if not isinstance(value, numbers.Number):
            raise TypeError("Input must be a number")

def scale_more_than_x(number: Union[float, int],
                      scaler: float,
                      limit: Union[float, int] = 1000) -> float:
    """scales a number if it's more than a specific number

    Args:
        number (Union[float, int]): the number to be scaled
        scaler (float): the factor to scale the number by
        limit (int, optional): the number at which anything larger gets scaled. Defaults to 1000.

    Returns:
        float: a scaled number if it is larger than the limit, otherwise the number
    """
    _check_numbers([number, scaler, limit])
    if number > limit:
        return scale_number(number=number, scaler=scaler)
    return number
```

Let's break this down,

```python
import numbers
from typing import Any, List, Union
```

We imported the [numbers](https://docs.python.org/3.6/library/numbers.html) module. This is a part of the Python standard library. We also imported `Any` and `List` from the `typing` module.

```python
def _check_numbers(check_list: List[Any]) -> None:
    """checks if all values in a list are a number

    Args:
        check_list (List[Any]): List of numbers to check

    Raises:
        TypeError: Input values must be a number
    """
    for value in check_list:
        if not isinstance(value, numbers.Number):
            raise TypeError("Input must be a number")
```

Here we've implemented a function called `_check_numbers()` the leading underscore is how you would define private variables or functions in Python. Python doesn't have the concept of private variables or functions. One useful part of this is that if you were to import the package our function is in, like we've imported numbers, the `_check_numbers()` function won't be imported.

Our function takes a list of any type and returns nothing. In the function we iterate through every value in the list and **if the value is not a Number**, we **raise a TypeError** letting the user know the input must be a number.
The `numbers.Number` refers to the Number class inside the `numbers` module. We know this is a class because the first letter is capitalized, as with Python standards.
the `isinstance()` function checks to see if the type of the value is of the type `numbers.Number` this is more robust than saying, for example:

```python
if type(value) != numbers.Number:
    raise TypeError("Input must be a number")
```

Because `isinstance()` handles something called inheritance. So **every time you need to check types, use `isinstance()`**.

Lastly we `raise TypeError()` the `raise` function tells us we've run into an error and the program must exit. The `TypeError()` tells us what kind of error it is we are raising, here the types are incorrect so we raise a `TypeError`. There are many errors that can be raised in the Python Standard Library.

We've then placed this function inside our `scale_more_than_x()` function to catch the errors before we evaluate any of our parameters.

#### Running from the command line

Lastly, lets make this runnable from the command line. Our final script looks like this:

```python
import numbers
from typing import Any, List, Union


def scale_number(number: Union[float, int], scaler: float) -> float:
    """scales a number by a fixed amount

    Args:
        number (Union[float, int]): the number to be scaled
        scaler (float): the factor to scale the number by

    Returns:
        float: a scaled number

    Example:
        >>> scale_number(number=100, scaler=1.3)
        130.0

    """
    return number*scaler


def scale_more_than_x(number: Union[float, int],
                      scaler: float,
                      limit: Union[float, int] = 1000) -> float:
    """scales a number if it's more than a specific number

    Args:
        number (Union[float, int]): the number to be scaled
        scaler (float): the factor to scale the number by
        limit (int, optional): the number at which anything larger gets scaled. Defaults to 1000.

    Returns:
        float: a scaled number if it is larger than the limit, otherwise the number
    """
    _check_numbers([number, scaler, limit])
    if number > limit:
        return scale_number(number=number, scaler=scaler)
    return number


def _check_numbers(check_list: List[Any]) -> None:
    """checks if all values in a list return a number

    Args:
        check_list (List[Any]): List of numbers to check

    Raises:
        TypeError: Input values must be a number
    """
    for value in check_list:
        if not isinstance(value, numbers.Number):
            raise TypeError("Input must be a number")


if __name__ == "__main__":
    number = float(input("What is the number you'd like to scale: "))
    scaler = float(input("how much would you like to scale it by?: "))
    limit_ind = input("would you like to put a limit? (y/n)")
    if limit_ind.lower() == 'y':
        limit = float(input("input your limit: "))
    elif limit_ind.lower() == 'n':
        limit = -999999999.0
    else:
        raise ValueError("Please select either y/n")
    print(scale_more_than_x(number=number, scaler=scaler, limit=limit))
```

What's new is the method at the bottom `if __name__ == "__main__`. This statement means that if you are running the python function from the command line, execute this code block. We've covered most of the code already. The `input()` function allows users to input a value by the command line and `elif` is shorthand for `else if`. the `.lower()` method converts the string to lower case.
