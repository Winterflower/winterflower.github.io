---
layout: post
title: Passivate/Reactivate Pattern in SimPy
---

_In this blog post we will explore how to use SimPy events to control when specific SimPy Processes start and resume_

Suppose we have a train that travels for a random number of time units ( let's say between 5 and 10 units) and stops at each station for a random number of time units (2-5 units) to pick up passengers. Those numbers don't necessarily correspond to any real life time span for a train. I just picked them for the purposes of this example. In later blog posts, I will explore integrating the TfL API with the simulation to provide more realistic data of travelling and boarding times for trains. 

To represent this situation, we can have two processes ``travel`` and ``board``. When the train is travelling, the ``travel`` process will be active and the ``board`` process passive and vice versa when the train is boarding passengers. We can implement this type of pattern by yielding a SimPy ``Event``, which will suspend the process until the event is successfully triggered. 

Let's take a look at a simple example first. 


```python
import simpy
import random

class Train(object):
    def __init__(self, env):
        self.env=env
        self.travel_proc = self.env.process(self.travel())
        self.board_proc = self.env.process(self.board())
        
    def travel(self):
        while True:
            print 'Start travelling at time %d' % self.env.now
            yield self.env.timeout(random.randint(5,11))
            print 'Stopping at time %d' % self.env.now
            
    def board(self):
        while True:
            print 'Start boarding at time %d' % self.env.now
            yield self.env.timeout(random.randint(2,5))
            print 'Stop boarding at time %d' % self.env.now
            
env = simpy.Environment()
train = Train(env)
env.run(until=10)
```

```

    Start travelling at time 0
    Start boarding at time 0
    Stop boarding at time 2
    Start boarding at time 2
    Stop boarding at time 7
    Start boarding at time 7
    Stopping at time 9
    Start travelling at time 9
```

In the previous example, we created a ``Train`` class with two ``Processes``: ``travel`` and ``board`` and we ran the simulation for 10 time units. Note, since we are using ``while True`` in the ``Processes``, it is important to include the ``until`` keyword argument in the call to ``env.run`` to make the simulation stop at a certain timeout. 

As we can see from the print statements, both the ``travel`` and the ``board`` processes start at the same time, which of course is not ideal, since the train cannot be both travelling and boarding customers at once. To make sure that the ``board`` processes is passive when the ``travel`` processes is active and vice versa, we can introduce an ``Event`` that will control when each of the processes is active. 
Let's write an example. 


```python
class Train(object):
    def __init__(self, env):
        self.env=env
        self.travel_proc=self.env.process(self.travel())
        self.board_proc=self.env.process(self.board())
        self.boarding_completed=self.env.event()
        self.train_stopping=self.env.event()
        
    def travel(self):
        while True:
            print 'Start travelling at time %d' % self.env.now
            yield self.env.timeout(random.randint(5,11))
            print 'Stopping at time %d' % self.env.now
            self.train_stopping.succeed()
            self.train_stopping=self.env.event()
            yield self.boarding_completed
            
    def board(self):
        while True:
            yield self.train_stopping
            print 'Start boarding at time %d' % self.env.now
            yield self.env.timeout(random.randint(2,5))
            print 'Stop boarding at time %d' % self.env.now
            self.boarding_completed.succeed()
            self.boarding_completed=self.env.event()
            
env = simpy.Environment()
train=Train(env)
env.run(until=30)
            
        
```

```

    Start travelling at time 0
    Stopping at time 5
    Start boarding at time 5
    Stop boarding at time 9
    Start travelling at time 9
    Stopping at time 19
    Start boarding at time 19
    Stop boarding at time 21
    Start travelling at time 21
```

If we look at the print statements printed during execution, we can see that now, the ``board`` process waits for the ``travel`` process to complete and vice versa. Activating and passivating processes is controlled by using two SimPy ``Events``, one to signal that the train has stopped (``self.train_stopping``) and the other to signal that boarding has completed (``self.boarding_complete``). When we ``yield`` either of these events from a process, the process is suspended until the event is executed successfully. We ensure this by triggering ``Event.succeed()`` after boarding has completed and after the train has stopped. 

The reader should also note that events cannot be recycled. That is, once an event has been triggered (as successful or as failed), it cannot be triggered again. Therefore, after ``self.train_stopping.succeed()`` or ``self.boarding_complete.succeed()`` is triggered, we have to assign new instances of ``Event``  to ``self.train_stopping`` or ``self.boarding_complete``. 
