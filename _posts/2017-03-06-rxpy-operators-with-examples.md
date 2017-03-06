---
title: RxPy Operators with Examples
layout: post
---

RxPy is a Python programming library that allows us to compose reactive programs. Now, "reactive" is quickly becoming one of those buzzwords that everyone throws around, but no one really knows how to explain, so for the scope of this article, I'll consider "reactive" programming to be a paradigm of programming where programmers think of streams of data instead of individual data values. Libraries that support reactive programming supply operators that can be applied to data streams to achieve the goal of the program. In this set of notes, I will go through some of the common operators available in RxPy. 

## Filtering a data stream

Let's start with simple examples. One of the simplest manipulation one can perform on a stream of data is filtration with a certain criterion.
For example, we may want to filter out all integers above a certain value from our stream of data. 

```python
from rx import Observable 

#Filter data based on certain criteria
Observable.from_([1,2,3,4,5])\
          .filter(lambda s: s>=5)\
          .subscribe(lambda s: print(s))
```

In addition to filtering, we may also want to limit the number of data points that the subscribers of the ``Observable`` see. This can be achieved using the ``take`` operator, which take as an argument the number of 
items that should be 'taken' from the data stream. 

```python
#take 2 items from the data stream
Observable.from_([1,2,3,4,5])\
           .take(2)\
           .subscribe(lambda s: print(s))
```

An interesting thing to note is that ``take`` behaves gracefully if there are fewer data items in the stream than specified in the argument to ``take``. 

```python
#take 2 items from the data stream
Observable.from_([1,2,3])\
           .take(5)\
           .subscribe(lambda s: print(s))
```

A variation of ``take`` is ``take_while`` which feeds data from the streams to the subscribers until a certain condition is met. 
In the example below, we will feed data items to the subscriber as long as they are less than 4.

```python
Observable.from_([1,2,3,4,5,4,3,2])\
          .take_while(lambda s: s<4)\
          .subscribe(lambda s: print(s))

>>>
1
2
3
```
Please note that the data items, which fulfill the filter criterion, but which appear after the first number 4 in the stream are not passed to the subscriber. 

## Reducing data streams

In the previous examples, we were mainly concerned with manipulating an incoming data stream and producing another, filtered data stream as an output. In this section, we'll take a look at operators that aggregate data streams in some way. For example, we may want to ``count`` the number of items in a certain data stream. In the example below, we count how many cities have an 'N'. 

```python
Observable.from_(['Helsinki', 'London', 'Tokyo'])\
          .filter(lambda s: 'N' in s.upper() )\
          .count()\
          .subscribe(lambda x:print(x))
```

Another simple, but commonly used operation is to find the ``sum`` of data items in a stream.

```python
print('Find the sum of items in a data stream')
Observable.from_([1,2,3,4,5])\
           .sum()\
           .subscribe(lambda s: print(s)) 
```

One thing to notice about the ``sum`` operator is the fact that the final result will only be returned when all of the items in the data stream have been processed. While this may be ideal in batch processing of data, in more real-time solution we may want to output a rolling sum after we process each incoming data point. In this case, we should express the sum function as ``lambda x,y: x+y`` and use it in the scan operator. 

```python
Observable.from_([1,2,3,4,5])\
          .scan(lambda subtotal, i: subtotal+i)\
          .subscribe(lambda x: print(x))

```

## Merging two or more data streams

In addition to filtering and aggregating, we may want to combine multiple data streams into one before performing additional analytics. We can interleave data points from one or more streams using ``merge``. 

```python
obs1 = Observable.from_([1,2,3])
obs2 = Observable.from_([10,11,12])

Observable.merge(obs1, obs2)\
           .subscribe(lambda s:print(s))
```

