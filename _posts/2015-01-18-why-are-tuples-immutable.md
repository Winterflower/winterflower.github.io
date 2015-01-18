---
layout: post
title: Why are Python's tuples immutable?
---

I was recently interviewing for a position at a financial
software company. The interviewer and I were discussing
some features of built-in Python objects and we got onto the topic
of mutable and immutable objects.
I was asked to explain why are Python tuples immutable.
"Wow, I've never though about that," I said.

Afterwards, I felt like a huge dolt. Here I was waxing code-o-sophical about
mutable lists and immutable strings and I didn't even know why
a core built-in object is immutable! Cue: embarrassed silence and a lot
of facepalming.

I probably won't be called for a second interview (due to
the aforementioned "stellar" knowledge of Python's built-in types), but I still want to know
why Python's tuples are immutable. There is probably no better way to spend
a cold and wet London Sunday evening than cozying up
with Mark Lutz's *Learning Python* and a bottle of cold Coke Zero.
So here goes: Python's tuples!

# Introducing the Tuple

A Python tuple is the lovechild of a list and a string. Like a list, a tuple
is a sequence of elements. However, once set, we cannot change a tuple. Like
a Python string, a tuple is immutable. Let's see how this works in practice.

## Tuple Syntax

* A tuple is created using parentheses

```python
#tuple with strings and integers
random_tuple=('1', 'hello', 1, 4)

#tuple with only integers
another_random_tuple= (1,2,3,4,5)
```

* a tuple can contain objects of mixed types

```python
#tuple with lists and dicts
dict_list_tuple=({'food':'icecream', 'country':'Finland'}, [1,2,3,4,5])
```

## Implications of Tuple immutability

The tuple object supports many of the same methods that appear
with lists and strings. Because a tuple is immutable, you have
to pay attention to how you handle the object that is returned after a method call.
For example, suppose we have a tuple which represents the food
currently stored in my fridge.

```python
fridge_contents=('sushi', 'coca-cola', 'apples')
```
Suppose I buy some carrots and oranges and want to add them to the tuple
`fridge_contents`. I can try "tuple concatenation" in the interactive
shell to get something like the following.

```
>>> fridge_contents=fridge_contents+('carrots', 'oranges')
('sushi', 'coca-cola', 'apples', 'carrots', 'oranges')
```

Note that I have to reassign the variable `fridge_contents`
to the new concatenated tuple.
Now, suppose I want to substitute 'sushi' with
'tanmen' (a Wasabi dish I am particularly fond of). I could try a trick
that often works with lists

```
>>> fridge_contents[0]='tanmen'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
```

The Python shell doesn't like it. Once you've created the tuple, you can't change it.
You can only create a new tuple and reassign your variable to point to it.

## So why is a tuple immutable?

We already have lists to store collections of objects. Why do we need tuples?
According to M. Lutz, the answer lies in their immutability.
Suppose you write a method that depends on a particular sequence of objects
remaining the same throughout the lifetime of the code. If you store these objects
in a list, there is a chance that some other method using the list will accidentally
alter it and thus break your method. Thus, in a way, a tuple offers a guarantee
that a particular collection of objects will remain fixed. 
