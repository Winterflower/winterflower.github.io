---
layout: post
title: Tutorial: A very quick and simple introduction to Python part 1
---
Hello and welcome to part 1 of the quick Python overview! This
is a very basic tutorial that will quickly allow you to learn enough Python to
attend the [Intro to ML with Scikit-learn workshop](). For the purpose
of this tutorial, you do not have to install Python. You can do all of the exercises
in the [Online Python shell](http://repl.it/languages/Python)).



If you run into any trouble or you find that a concept is wrong or
poorly explained, please do not hesitate to contact me
at camillamon[at]gmail.com. I'll try to get back to you as soon as I can!
There is also a list of alternative Python resources at the end of this
tutorial.

##1. Python and Objects
One of the cornerstones of machine learning is (you guessed it!) manipulating
data. In Python, data is manipulated using objects such as numbers, strings,
lists and dictionaries. Some of these objects are built-in, others come
from external libraries. You can also define your own objects.

###1.0 Examples of built-in Python Objects

```python
#numbers
15
1234.9

#strings
"hello world"

#lists
my_numbers=[1,4,5,6]
nested_list=[1,[1,2]]
empty_list=[]

#tuples
simple_tuple=(9,10)

#dicts
my_empty_dictionary={}
my_nonempty_dictionary={'song':"Etude 9",
                        'duration':10}

```

###1.1 Assigning names to objects
Suppose we have a program that prints "hello world"
several times. We could simply type "hello world"
every time we want to print it.

```python
print "hello world"
#do something else
print "hello world"
#do something else
print "hello world"
```

Instead of typing "hello world" every time, we can assign
a *name* to "hello world". In Python, assigning a name to an
object is done using the "=" symbol. You can assign names
to any object (number, list, string etc.)

```python
#basic examples with strings, numbers and dicts
title="Do Androids Dream of Electric Sheep"
year_of_publication=1968
my_favorite_books={'Donna Tart':'The Goldfinch',
                    'Sylvia Plath':'The Bell Jar'}
```

####A Brief Note on Variable Names
It is good practice to give you variables descriptive names. This
will save you from having a 'WTF is this thing here' moment
when you come back to your code several weeks or months later.
Python is quite relaxed about variable naming rules, but
there are a few NO-NOs:
1. Do not start a variables 

###Exercises:
1. Create a string object with the name of your favorite novel and assign
it to a variable with a descriptive name
2. Create a list with some of your favorite numbers and give it the name
`my_favorite_numbers`
3. Create a dictionary using whatever keys and value you prefer


###1.3 String operations
In the machine learning workshop, we will manipulate textual data.
Thus it is a good idea to go over some very basic string operations, which
are built into Python.

```python
#how do I create a string?
dna="ACGTGTCGTGTGTGTG"

#how do I find out the length of a string?
len(dna)
#12

#how do I obtain the first letter of a string (or nucleotide for the biologists among us!)
dna[0]   #note indexing starts at zero
#'A'

#how do I obtain the last letter of a string?
dna[-1]
#'G'

#or
dna[len(dna)-1]  #QUIZ: would the command dna[len(dna)] work? Why or why not?
#'G'

# how do I obtain a substring? (also known as slicing)
dna[0:3]
#'ACG'
#QUIZ: what does the operation dna[-2] return?
```

###Exercises:
1. Create a string in the IPython notebook provided (or your own interactive
  Python session) and give it a descriptive name (eg. `my_string` for
    those of us lacking imagination :D)
2. Obtain the first 4 characters of `my_string`
3. Find out the length of `my_string`
4. Create a string object with the value "hello world" by concatenating
two string objects, "hello" and "world".


###1.4 Basic list operations and methods
Lists allow us to store a group of related objects (strings, numbers etc)
together.

```python
#how do I create a list?
favorite_ice_cream=["vanilla", "chocolate", "strawberry"]
prices=[12, 56, 78]

#how do I access an element in the list?
favorite_ice_cream[0]
#'vanilla'

#how do I find out the numbers of elements in a list?
len(favorite_ice_cream)
#3

#how do I concatenate two lists?
favorite_ice_cream+prices
#['vanilla', 'chocolate', 'strawberry', 12, 56, 78]

# as we can see from above,
#Python allows you to have lists with objects of different types (ie. numbers and strings)
```
In addition to the basic operations illustrated above, lists come
with several predefined methods.

```python
#how do I add an element to the end of a list?
favorite_ice_cream.append("cherry garcia")

#how do I delete an item at position n (where n is the index of the element you want to delete)?
favorite_ice_cream.pop(0)
favorite_ice_cream
#['chocolate', 'strawberry', 'cherry garcia']
#the object 'vanilla' was deleted from the list
```


###1.5 Basic dictionary operations and methods
Sometimes we want to associate particular keys with values.
For example, a company may want to store some basic information
about its employees.

```python
#how do I create a dict object?
employee1={
          'name':'Jane Doe',
          'department':'engineering',
          'salary':270000,
          }
#how do I access



```
