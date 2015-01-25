---
layout: post
title: The Module Conundrum (part 1)
---
# Module Import Basics

Even if you only recently picked up Python programming, chances
are you have already encountered the `import ` statement.
For example, if you want to generate some numbers from the uniform
distribution on a certain interval, you can fire up Python's standard
`random` module.

```python
import random
random.uniform(0,1)
"""
$ 0.6894232262733903
"""
```

The `import` statement is used to give your script access to methods
and attributes defined in other .py files. These files are often
referred to as **modules**. In fact, modules form one of the cornerstones
of Python's program architecture philosophy. A large Python program usually has multiple
modules and one "main" script module, which controls the execution and
importing of other modules.

Let's set up a small module example, which will help us understand how
module importing works in Python.

## Module Importing Example
We need to create two .py files:

* main.py will play the role of the main script module

* essentialfunctions.py will be the script module we want to import

For now, be sure to place the files in the same directory.
Later on, we will see how to import modules which reside in different
directories from the main module script.

```python
#essentialfunctions.py
#Python 2.X

def average(number1, number2):
  """
  Returns the average of two integers
  """
  return (number1+number2)/2.0

#the line below seems pointless,
#but will be used to illustrate a module import concept
print "Hello, you have just imported essentialfunctions.py module"

some_constant=1.234
```


```python
#main.py
#Python 2.X
import essentialfunctions
print "Starting your main module"
```

For now, our main.py file only imports the essentialfunctions
module without using any of the methods or variables defined
in the essentialfunctions.py module. Let's see what happens.
Open up your favorite terminal and run the main.py file.
You should see something like this

```bash
$ python main.py

Hello, you have just imported essentialfunctions.py module
Starting your main module

```
We printed out two strings to the terminal: the first string comes
from the essentialfunctions module, the second one from the main.py
script file. This is a feature of Python's import statement: when
a module is imported, the code in the module file is executed.

We can now take full advantage of our import and use the methods
and variables defined in the essentialfunctions module.
For example, we could alter our main.py script
to compute the average of two integers using the `average` function
defined in the essentialfunctions module.

```python
import essentialfunctions
print "Starting your main module"
print essentialfunctions.average(9,10)
```
The result should be something like the following

```bash
$ python main.py

Hello, you have just imported essentialfunctions.py module
Starting your main module
9.5
```
## Importing Python Modules in Interactive Sessions
Let's try to import our essentialfunctions module into an interactive
Python session.

```python
Python 2.7.6 (default, Mar 22 2014, 22:59:56)
[GCC 4.8.2] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import essentialfunctions
Hello, you have just imported essentialfunctions.py module
```

Just as before, we see that when Python imports a module, all
of the code in the module file is executed and as a result
"Hello, you have just imported essentialfunctions.py module"
is printed to output.

Let's try rerunning the import statement in the same interactive session.

```python
>>> import essentialfunctions
>>>
```
We can see that "Hello, you have just imported essentialfunctions.py module"
is not printed out this time. This is a feature of Python's import statement.
A module's code is executed only once even if the code changes.
For example, suppose we edit essentialfunctions.py in another window
while our interactive Python session is running.

```python
#essentialfunctions.py MODIFIED
#Python 2.X

def average(number1, number2):
  """
  Returns the average of two integers
  """
  return (number1+number2)/2.0

#the line below seems pointless,
#but will be used to illustrate a module import concept
print "Hello, you have just imported essentialfunctions.py module"

some_constant=1.234

print "The essentialfunctions.py module has been altered"
```

We can try importing essentialfunctions.py in our interactive session
again.

```python
>>> import essentialfunctions
>>>
```
Nothing is printed.
Mark Lutz in *Learning Python*, writes "This is by design; imports are too
expensive an operation to repeat more than once per file, per
program run". If we do want Python to read the modified essentialfunctions.py
module, we have to call the `reload()` function. The reload
function is built-in only in Python 2.X. For Python 3.X, you
will first have to import the reload function from the `imp`
standard library module.

```python
#using reload in Python 3.X
from imp import reload
reload(essentialfunctions)

#or alternatively without using from
import imp
imp.reload(essentialfunctions)
```

```python
>>> reload(essentialfunctions)
Hello, you have just imported essentialfunctions.py module
The essentialfunctions.py module has been alteredt
The string "The essentialfunctions.py module has been altered" is now printed
to the terminal.
```

## Summary
* Modules are a cornerstone of Python's program architecture philosophy
* Modules are essentially .py files, which contain useful functions, objects
and variables (a module's attributes)
* Other files can use a module's attributes by first importing the
module using the `import` statement
* Python executes the code in the module file once when a module is imported
* Subsequent imports in the same Python session will not execute the code.To
force Python to execute the code use `reload()`. What do you have to do to
use `reload()` in Python 3.X?
