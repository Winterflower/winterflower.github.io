---
layout: post
title: What is going on in this SimPy Monitoring example?
---

## What is going on in this SimPy Monitoring example?

_This post is long and rambling (not unlike a midsummer Sunday's walk around the Surrey Hills) so you may get frutsrated and angry at the author while reading this. The author wishes to apologize to her readers in advance._

SimPy is a simulation library that you can use to easily create event driven simulations in Python. It's great to play around with if you like simulating queues, traffic systems and monitoring the usage of shared resources ( for example, the number of seats currently occupied on a train or the number of cashiers free at a checkout queue ). In the simplest cases, you can have SimPy print out the usage statistics for your shared SimPy resources to stdout so that you can monitor it, but for longer and more complex situations, it is better to have an automated simulation data collection tool. 

Luckily, the SimPy documentation comes in handy and the authors have [included a helpful example](https://simpy.readthedocs.io/en/latest/topical_guides/monitoring.html), which will allow you to patch a SimPy ``Resource`` object and monitor it during the simulation. I have reproduced it, with a few comments removed for the sake of brevity, below. Decorators and functools are a bit of a deep sea for me, so in this blogpost I am going to clarify what is going on in the ``patch_resource`` function and why it can be used to monitor resource usage in SimPy simulations. 


### Patching resources in SimPy

```python
from functools import partial, wraps
import simpy

def patch_resource(resource, pre=None, post=None):
    def get_wrapper(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            if pre:
                pre(resource) 
            ret = func(*args, **kwargs)
            if post:
                print 'Calling post function'
                post(resource)
            return ret
        return wrapper
    for name in ['put', 'get', 'request', 'release']:
        if hasattr(resource, name):
            setattr(resource, name, get_wrapper(getattr(resource, name)))
```

Whoah, whoah there - functools, decorators, some strange @wraps(func) thing! - let's take it one step at a time!

### Functools
[Functools](https://docs.python.org/2/library/functools.html) is a Python library for higher-order functions - functions that take other functions as arguments, manipulate functions and return new functions. In the ``patch_resource`` method, we use two items from the functools module: ``wraps`` and ``partial``. Let's take a look at ``wraps``.

According to the [official documentation](https://docs.python.org/2/library/functools.html) ``wraps`` is a "convenience function for invoking ``update_wrapper`` ", another method in the ``functools`` module, as a decorator (as the SimPy documentation authors have done in the example above). After reading the documentation for ``update_wrapper``, I'm not really sure what it does - at this point it might be better to poke the function instead of trying to decipher what the documentation means. The ``update_wrapper`` function takes in a ``wrapper`` function and a ``wrapped`` function as arguments, so I am going to setup a simple ``simplefunc`` that prints out 'hello, world' and a ``wrapper`` func, which pretends to do something useful before and after calling ``simplefunc``.


```python
from functools import update_wrapper

def simplefunc():
    print 'hello, world'

def wrapper(func, *args, **kwargs):
    print 'Doing some stuff before calling func'
    value = func(*args, **kwargs)
    print 'Doing some stuff after calling func'
    return value

obj = update_wrapper(wrapper, simplefunc)
```

According to the documentation, the ``wrapper`` function should now look like the ``wrapped`` function. If I understood it correctly, I should now be able to call ``wrapper`` like ``simplefuncs``. 

```python
obj()
``` 
results in 

```
>>> obj()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: wrapper() takes at least 1 argument (0 given)
```
so I have clearly missed something. A quick googling lands me on this [SO answer](http://stackoverflow.com/questions/308999/what-does-functools-wraps-do) and reveals that ``update_wrapper`` has nothing to do with the function signatures. What it does, instead, is carries over important dunder attributes from the original function to the new wrapped function. For example, if we rework the ``simplefunc`` and ``wrapper`` example above to the following:

```python

def simplefunc():
    '''print hello world'''
    print 'hello world'

def decoratorfunc(f):
    def wrapper(*args, **kwargs):
        print 'f.__name__', f.__name__
        value = f(*args, **kwargs)
        return value
    return wrapper

newFunc = decoratorfunc(simplefunc)
newFunc()
```

The output is something like this

```python
>>> newFunc()
f.__name__ simplefunc
f.__doc__ print hello world
hello world
```

However, if we try to print out the name and doc properties of ``newFunc``, which is essentially our ``simplefunc``, just wrapped with some helpful methods, we get the following 

```python
>>> newFunc.__name__
'wrapper'
>>> newFunc.__doc__
>>> 

```
The docstring and name of the original ``simplefunc`` have been lost. This is where ``update_wrapper`` or its decorator equivalent can come in handy. Let's rewrite the example above to user the ``@wraps`` syntax:


```python
from functools import wraps

def decoratorfunc(f):
    @wraps(f)
    def wrapper(*args, **kwargs):
        print 'f.__name__', f.__name__
        value= f(*args, **kwargs)
        return value
    return wrapper

newFunc = decoratorfunc(simplefunc)
newFunc()
print newFunc.__name__
print newFunc.__doc__
```

We get the following output

```python
>>> newFunc()
f.__name__ simplefunc
hello world
>>> newFunc.__name__
'simplefunc'
>>> newFunc.__doc__
'print hello world'
>>> 
```
As the example shows, ``update_wrapper``or its decorator equivalent ``@wraps`` preserves the original function's attributes in the decorated function. 

