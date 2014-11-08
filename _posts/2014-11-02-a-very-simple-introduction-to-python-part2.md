---
layout: post
title: A very quick and simple introduction to Python (part 2)
---
Version 1.0 (last updated on Nov 8th, 2014)


Welcome back to the second part of the quick Python overview.
In this section, we will cover the basics of if-else
statements, while and for loops and creating methods. Once again, all
comments and questions are encouraged and very welcome.
Please feel free to email me at camillamon[at]gmail.com.

##2.0 Control-flow in Python programs
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
if test:
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
or the [online Python shell](http://repl.it/languages/Python).
Which one of the statements is printed to the terminal?

####2.1.1 A more complicated example
In the little program above, we only tested for one condition (whether
  or not the number we stored in `random_number` is divisible by 5).
Usually in real programming life, testing for one condition
is not enough for what we aim to achieve. So let's take
our little example one step further.

Suppose that we still want to check whether the number
stored in `random_number` is divisible by 5. If no, we
want to check whether it is divisible by 3. This
means that our program has to branch into three different
'logical ' branches.

* `random_number` is divisible by 5
* `random_number` is not divisible by 5 but is divisible by 3
* `random_number` is neither divisible by 5 nor divisible by 3

In Python, this could be achieved in the following manner:

```python
random_number=10 #or assign a number of your choice
if random_number%5==0:
  print "The number is divisible by 5!"
elif random_number%3==0:
  print "The number is divisible by 3!"
else:
  print "The number is divisible neither by 5 nor 3 "
```


##Excercises:
1. Find out if the year 1044 is a leap year.
A year is a leap year, if it is divisible by 4 and 400,
but not divisible by a 100.

###2.2 The while loop
Now that we are familiar with `if`, `else` and `elif`
statements, can take a look at the `while` loop.
The `while` loop executes while some condition is true and
is especially useful if we want to execute a block of code repeatedly.
Let's illustrate this with a simple example.
Suppose we want to print out all of the numbers from 1 to 10.

```python
number=1   #the initial number
while number<11:  
  print number
  number+=1
```
As we can see from the example above, a `while`
statement is declared with the following syntax

```python
while test:
  do something
```

The loop will keep executing until the `test` becomes false.
In the little number printing example, the test in the while
statement evaluates whether the number object referenced
by `number` is less than 11. If yes, the statements
inside the `while` block are executed.


####A Word of Warning: Do not write infinite loops!
When writing your first `while` loops, it's easy to forget
to make sure that the loop terminates.
What would happen if we leave out the statement `number+=1`
from the `while` loop we wrote above?


###Exercises:
1. Write a small program that checks the numbers from 1 to 25 and
prints only those that are divisible by 5


###2.3 For loops
The `for` 'loop is a close cousin of the `while` loop.
It is design to iterate (or step through) items for example
in a list or string. Let's look at the general syntax of the `for` loop.

```python
#general syntax for a for-loop
for element in object:
  execute code here
```

The `for` loop begins with a
header similar to do that of the `while` loop.
There is one key difference, though. The header for the `for`-loop
also includes something called an assignment target which we called
`element` in the script above.

You can think of the assignment target as a box.
When we loop through an iterable object such as a list,
every element takes a turn jumping into the box. While
the element 'lives in the box', we can carry out operations on it.

If all of this seems nebulous right now, do not worry!
We will make all of this concrete by working through
a `for` loop example.

We are given a list of elements (these may be strings
or numbers of a mixture of both) and we have to print out
each element.

```python
random_elements=["apple", "Jack", 12, 1+8, "athlete"]

#let's print out each element using the for-loop
for element in random_elements:
  print element
```
It does not matter what we call the assignment target.
Instead of `element`, we could have called it `word` or
`chocolatebar` or even simply `x`. 
