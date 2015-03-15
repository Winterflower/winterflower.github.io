---
layout: post
title: 10th PyData London Meetup recap
---

Yesterday, I attended the 10th Pydata London Meetup at Lyst. 
The venue was packed by the time I arrived: I never realised there
were so many data obsessed Pythonistas in town! 

## The Talks

During the course of the night, the audience was treated to two great talks:
Kim Nilsson talked about building great data analyst teams and Evgeniy Burovskiy
talked about cool things you can do with Numpy. I'd like to take a moment to write
down some very interesting pointers I took away from the talks. 

### Data Geeks Unite! Communication and Team-building 101 for Scientists, Analysts and Engineers.

I hate to say this, but I am usually very prejudiced against 'soft skills' talks at meetups.
They inevitably remind me of some Agile retrospective's that I've attended, where people
promise to do a lot of things to improve how the team works, but never actually get around
to implementing those ideas in practice. I am happy to say Kim's talk proved me 100% wrong. 
Here are some interesting things I took away form the talk:

* if you are transitioning from an academic role into a business role, be prepared for culture shock

My own experience matches pretty well with this. I graduated last fall and immediately accepted a job
at software engineering house. All of my experiences for the first few months were isolating and foreign.

* business deadlines are shorter than the deadlines in academia

In academia, deadlines are often well defined and far away in the future. For example, your thesis might be 
due in 12 months. Many tech business, (at least in agile software engineering teams) operate in sprints and 
one usually releases something every few weeks. 

* collaboration with your co-workers is more 'intense' than in academia

When I was completing my Master's degree, I attended maybe 10 hours worth of lectures and tutorials and 
the rest of the time was devoted for research. An academic job allows you to hack away on your code
pretty much without interference save for the occasional meeting with a supervisor and perhaps some seminars.
Coding at a business is different. There are daily stand-ups and sprint demos. Your co-workers will likely
pair up with you and code review what you do. In other, words you will spend a lot more time with your co-workers. 

* spend some time researching how to build a great team

Kim talked about Belbin's 9 different team roles and how an effective team can be structured
by placing different personalities in different roles at key stages in constructing
the product (or data model). 

* allow time for learning and playing

One of the most amazing things about writing code for a living is that one has the opportunity to learn
something nnew every day. At my current job, there has not been a day that I have not picked up
at least one new amazing thing about a programming language, a software engineering design principle
or computer architecture. In fact, the flow of useful and important information was so high, I felt
compelled to start this blog and a learning journal! I highly recommend it to other
programmers and data scientists. Reflecting on what you learned solidifies the concepts in your mind.
Even better if you can teach it to someone else later on!


## "SciPy Roadmap discussion (with short intro to numpy/scipy)"

Evgeniy's talk was a menagerie of Numpy and Scipy gotchas and a bunch of useful tips 
for Numpy newbies such as myself. Some of the most important points I took away from the talk:

* Numpy runs on arrays. Know your arrays and use them. 

```python
import numpy as np 

array=np.arange(10)
print array
#array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```
Applying an expression on (to?) the variable `array` will apply it
on every element in the array.

```python
array1=array+1
print array1
#array([ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10])
```

* Broadcasting can introduce memory and performance issues 

I have to admit, I am not a big expert on broadcasting, but is certainly
something that would be cool to explore further!

* You achieve in-place operations on Numpy array by piping the output back into the input

```python
random_array=np.random.random_sample(10)
np.exp(random_array, out=random_array)
```


* Practice your Numpy! 

There is a [great series of exercises compiled by Nicolas Rougier](https://github.com/rougier/numpy-100), which will take you
all the way from Neophyte to Numpy master. I'm still working my way through
the Neophyte level :)

* You can write Conway's Game of Life in only a few lines of Numpy. 

This looks like a great weekend project!



