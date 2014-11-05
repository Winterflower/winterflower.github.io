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
For example, suppose we have a number and we want
to check whether it is divisible by 5.
One very simple way to do this is to use the if-statement.

The syntax of the if-statement in Python is as follows:

```
if test1:
  do something
else:
  do something else
```

Here is a small example illustrating the concept:


```python
random_number=10
if random_number%5==0:
  print "Divisible by 5"
else:
  print "Not divisible by 5"
```

Let's step through the example above line by line.
In the first line, we create the number object ('10') and
give it the name `random_number`. Next, we want
to find out if, the object that the name `random_number`
points to is divisible by 5 or not. In order to do this,
we employ the modulus operation, which gives us the remainder
of `random_number` divided by 5. If the remainder is 0 (ie. `random_number`
is divisible by 5), then we will print out "Divisible by 5".

If you want, run this program using the interactive Python shell
or the online Python shell (link at the beginning of part 1).
Which one of the statements is printed to the terminal?
