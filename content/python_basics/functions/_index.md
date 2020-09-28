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
