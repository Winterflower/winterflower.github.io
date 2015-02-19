---
layout: post
title: "Python Functions : def statement"
---

* This is an on-going series of notes on writing functions in Python. Most of these posts are based on Mark Lutz's excellent book
Learning Python (5th Edition) * 

A function is a single group of statements that makes our Python code
more reusable and structured! So there is every reason to learn how
to write functions effectively and to know the various ways in which functions can be created in Python.

## The `def` statement
The most common way to create functions in Python is using `def`.
The `def` statement simply creates the function object and assigns it to
the name written after `def`. 
For example, 


```python
def average(a,b):
    """
    Computes the average of a and b and returns it
    """
    
    return (a+b)/2.0
    
print average(3,4)
>>> 3.5
```

The name `average` points to the block of code that computes the average of the two numbers.
However, we can do all sorts of things to a function's name. 
For example, we can 

* store it in a list 

``` python
python_functions=[]
python_functions.append(average)

#call the function from the list and print the result
print python_functions[0](6,5)
>>> 5.5
```

* assign it to another name and use the name to refer to it in the future

```python
average_of_two_numbers=average
print average_of_two_numbers(9,8)
>>> 8.5
```

## def is executable
... which means that any function you write will not exist until the Python interpreter 
reads the `def` statement that first creates the function. We can easily test this 
by writing two functions and calling the second function from the first function. 

```python

def print_geometric_series():
    """
    Prints a geometric series in a nice way
    :return None:
    """
    geometric_series=compute_geometric_series(2,2,10)
    for integer in geometric_series:
        print integer

print_geometric_series()

def compute_geometric_series(initial, quotient, n):
    """
    Returns a geometric series of length n
    :param initial:
    :param quotient:
    :param n:
    :return:
    """
    return [initial*quotient**x for x in range(n)]
    
>>>NameError: global name 'compute_geometric_series' is not defined
```


As we can see from the NameError that Python throws, the function `compute_geometric_series` does
not exist at the time when `print_geometric_series` calls it. If you are coming from a Java background, 
this may seem bizarre, because of course in Java, the order in which the methods appear in the file
doesnot matter. 