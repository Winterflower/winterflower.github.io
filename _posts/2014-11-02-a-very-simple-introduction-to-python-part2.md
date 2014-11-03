---
layout: post
title: A very quick and simple introduction to Python (part 2)
---

(post in progress)
Welcome back to the second part of the quick Python overview.
In this section, we will cover the basics of if-else
statements, while and for loops and creating methods. Once again, all
comments and questions are encouraged and very welcome.
Please feel free to email me at camillamon[at]gmail.com.

###2.0 Control-flow in Python programs
If-else statements and for and while loops
form the core of control-flow in Python programs.
Let's take a brief tour of these structures, starting
with the if-else statement.

###2.1 If and else
Typically in a more complicated program, we have
to take different actions depending on some previous result.
For example, suppose we have a list of three numbers and we want
to check whether any one of these numbers is divisible by 5.
There are many ways to implement this in Python.

One very simple way is to assign each element in a list
to a variable and then use the if-statement to check whether
these number object to which the variable points is divisible by 5.

The syntax of the if-statement in Python is as follows:

```
if test1:
  do something
else:
  do something else
```

Here is a small example illustrating the concept:


```python
random_number=5
if random_number%5==0:
  print "Divisible by 5"
else:
  print "Not divisible by 5"
```

 An example is shown below.
(Note for the experienced Pythonistas: this can be done very efficiently
with a for-loop of list comprehension, but for now we will keep things
simple)

```python
"""
A very simple program to illustrate if-else statement
"""

list_of_numbers=[10,11,26]
#assign each number to a variable
number1=list_of_numbers[0]   #recall, indexing in Python begins with 0
number2=list_of_numbers[1]
number3=list_of_numbers[2]


```
