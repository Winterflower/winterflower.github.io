---
layout: post
title: Conversations about Python Dicts
---

1. What is the main conceptual difference between dictionaries and lists?

Lists in Python store objects based on a positional offset and are fetched
based on an index whereas in dictionary objects are fetched by keys. 

```python
#fetching entries in a list based on position
securities=['equities', 'bonds', 'options', 'futures']
print securities[0]

>>> equities
```

```python
#fetching entries in a dictionary based on key

employees={'engineering':['Lisa', 'Ann', 'Bob'],
	   'marketing':['Charlie', 'Mike']}
print employes['engineering']

>>> ['Lisa', 'Ann', 'Bob']
```

2. What does it mean for a dictionary to be mutable?

A variable stores a reference to a dictionary not a copy.
Not understanding this difference fully can sometimes lead
to silly or rather serious runtime errors.

```python
icecream={'strawberry':4, 'blueberry':5, 'banana':6}
new_icecream=icecream
```

The variale new_icecream refers to the exact same dictionary as the variable icecream.
We can verify this by adding another item in `icecream` and then calling
`new_icecream` to retreve that item. 

```python
icecream['raspberry']=9
new_icecream['raspberry']
>>>9
```
Python did not throw a KeyError, which means that there exists a key called `raspberry` in the dictionary
referred to by the variable `new_icecream` even though
we used the `icecream` variable to add it to the dictionary


3. What are some alternatives to literals when constructing dictionaries?

The vanilla way to construct a dictionary in Python is to use the literal expression.

```python
vanilla_dictionary={'rasberry':4, 'vanilla':2}
```

A dictionary can also be constructed by calling `dict()`.

```python
another_dictionary=dict(raspberry=4, vanilla=2)
```

Alternatively, we can use a list of tuples (key, value) pairs.

```python
dictionary_from_tuples=dict([('raspberry',4),('vanilla',2)])
```

Sometimes, your functions will give you separate lists for the key and for the 
values. In this case, it will be useful to employ the `zip` functions to create 
key-value pairs and then pass the key-value pairs to the `dict()` function. 

```python
flavours=['linux_mint', 'ubuntu','debian','fedora','redhat','scientific_linux']
number_of_users=[40,30,90,100,80,10]
print zip(flavours,number_of_users)
>>>[('linux_mint', 40), ('ubuntu', 30), ('debian', 90), ('fedora', 100), ('redhat', 80), ('scientific_linux', 10)]
```
As we can see, the `zip()` functions creates tuples.
We can then pass the `zip()` function the `dict()`
function to create a dictionary.

```python
nix_users=dict(zip(flavours, number_of_users))
print nix_users
>>>{'scientific_linux': 10, 'fedora': 100, 'redhat': 80, 'linux_mint': 40, 'ubuntu': 30, 'debian': 90}
```

4. How do I find out about the other methods available for dictionaries?

Execute `dir(dict)` or `help(dict)`.

```python
dir(dict)
>>>
['__class__',
 '__cmp__',
 '__contains__',
 '__delattr__',
 '__delitem__',
 '__doc__',
 '__eq__',
 '__format__',
 '__ge__',
 '__getattribute__',
 '__getitem__',
 '__gt__',
 '__hash__',
 '__init__',
 '__iter__',
 '__le__',
 '__len__',
 '__lt__',
 '__ne__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__setattr__',
 '__setitem__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 'clear',
 'copy',
 'fromkeys',
 'get',
 'has_key',
 'items',
 'iteritems',
 'iterkeys',
 'itervalues',
 'keys',
 'pop',
 'popitem',
 'setdefault',
 'update',
 'values',
 'viewitems',
 'viewkeys',
 'viewvalues']
```

As you can see, the list is vast!
