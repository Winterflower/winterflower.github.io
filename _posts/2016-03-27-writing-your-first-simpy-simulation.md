---
layout: post
title: Writing your first simulation using SimPy (Part I )
---

![A London Double decker Routemaster bus (though you don't see these too often nowadays ) by Oxyman/Wikipedia CC]({{ site.url }}/images/London_Bus.jpg)

SimPy is a [Python library](https://simpy.readthedocs.org/en/latest/index.html)
that allows you to simulate a variety of discrete-event scenarios. For example,
you may want to investigate how the number of available checkout machines
influences the length of the customer queue at your local supermarket or how the
number of bus stops in a crowded neighbourhood affect your morning commute.
SimPy offers you an easy way to build these kinds of simulations. In this blog
post series, I am going to give you some introductory examples to help you get
started with your own simulations.

## Requirements

1. Knowing a bit about Python generator functions and how to write them (and
sort of what they do, though I don't really dwell into the details here )
2. Python 2.7 with SimPy installed (apologies all Python 3 fans, I will produce
a Python 3 version soon )

## Basic SimPy Terminology

A SimPy simulation is constructed using three simple components:
1. A *Process* is the actor or active agent in your simulation (for example,
customers in a busy store or buses on the road). In SimPy Processes are Python
generator functions.
2. An *Environment* is a like a container for your simulation, which helps to
schedule and coordinate processes
3. An *Event* is a way for the *Processes* in your simulation to commmunicate
with each other.

Hopefully each of these will become clearer once we dive into the practical
examples.

## A Simple Bus Simulation

Let's construct a simple SimPy simulation for a bus on a route around a London
neighbourhood. The route of the bus includes 14 stops and the time it takes to
board and offload commuters at each stop takes a constant 10 minutes (a
simplification, but let's roll with it for now). Also let's suppose, for
simplicity, that the time the bus drives between each stop stays constant (this,
clearly, does not hold in any realworld situation).

The bus will be a SimPy *Process*. Thus to begin our simulation, we will
construct a simple Python generator function to represent our bus. I will show
the whole generator function below and then explain how the individual parts of
the function work in the context of the SimPy simulation.


    import simpy
    import random 
    
    def bus(simpy_environment):
        '''Simple Simpy bus simulation'''
        driving_duration = 5 #time taken to drive between two stops in this neighbourhood
        stopping_duration = 10
        for i in range(15):
            print 'Start driving to bus stop %d at time %d' % (i, simpy_environment.now )
            yield simpy_environment.timeout(driving_duration)
            print 'Stopping to pick up commuters at bus stop %d at time %d' % (i, simpy_environment.now )
            yield simpy_environment.timeout(stopping_duration)

There are a few interesting things going on in the ``bus`` method. First, notice
that we pass an argument ``simpy_environment`` into the method. This is a SimPy
``Environment`` object that is in charge of scheduling, starting and suspending
SimPy processes (the meaning of this will become clear in a moment).
Second, notice that we are yielding two ``timeout`` events. When a SimPy
*Process* yields an event, the *Environment* in which the process is running
suspends the *Process* for the duration of the event. The *Process* (the ``bus``
generator function) resumes executing once the event has been finished.

In our case, we 'suspend' the Bus process while it is driving and when it is
boarding commuters. It might be a bit strange to think about the bus process as
suspended when the bus is actually driving or boarding commuters. The key here,
in my opinion, is not to think of the process as being *idle* (ie not doing
anything) when it is suspended, but to think about it as being in a state where
the process itself need not execute any extra logic to complete the event. For
example, in our simple simulation, from the bus processes perspective boarding
commuters is simply an action that takes 10 minutes. There is no extra logic
that needs to be executed. Thus  but for now in a simple simulation, we
represent 'boarding commuters' and 'driving'.

Let's add some more code to allow the SimPy simulation *Environment* to execute
this process.


    env = simpy.Environment() #create the SimPy environment
    env.process( bus(env) ) # create an instance of the Bus process
    env.run()

    Start driving to bus stop 0 at time 0
    Stopping to pick up commuters at bus stop 0 at time 5
    Start driving to bus stop 1 at time 15
    Stopping to pick up commuters at bus stop 1 at time 20
    Start driving to bus stop 2 at time 30
    Stopping to pick up commuters at bus stop 2 at time 35
    Start driving to bus stop 3 at time 45
    Stopping to pick up commuters at bus stop 3 at time 50
    Start driving to bus stop 4 at time 60
    Stopping to pick up commuters at bus stop 4 at time 65
    Start driving to bus stop 5 at time 75
    Stopping to pick up commuters at bus stop 5 at time 80
    Start driving to bus stop 6 at time 90
    Stopping to pick up commuters at bus stop 6 at time 95
    Start driving to bus stop 7 at time 105
    Stopping to pick up commuters at bus stop 7 at time 110
    Start driving to bus stop 8 at time 120
    Stopping to pick up commuters at bus stop 8 at time 125
    Start driving to bus stop 9 at time 135
    Stopping to pick up commuters at bus stop 9 at time 140
    Start driving to bus stop 10 at time 150
    Stopping to pick up commuters at bus stop 10 at time 155
    Start driving to bus stop 11 at time 165
    Stopping to pick up commuters at bus stop 11 at time 170
    Start driving to bus stop 12 at time 180
    Stopping to pick up commuters at bus stop 12 at time 185
    Start driving to bus stop 13 at time 195
    Stopping to pick up commuters at bus stop 13 at time 200
    Start driving to bus stop 14 at time 210
    Stopping to pick up commuters at bus stop 14 at time 215


There we go! Our first bus simulation completes its 14-stop round in 215
minutes!
Admittedly, the logic is a bit borked and unrealistic, but we will work on that
shortly. Let's start by implementing (trying to implement :D ) the following
things:

1. A different driving time between each of the bus stops that varies according
to a normal distribution to simulate what might happen as traffic varies
according to different areas of the neighbourhood.
2. A random commuter boarding time that varies between 0 minutes (bus did not
stop) and 10 minutes (boarding commuters took a really long time)


    import numpy
    random.seed(10)
    def bus_improved(simpy_environment):
        '''Improved Bus simulation'''
        drive_times = [random.randint(5, 15) for _ in range(15) ] #14 driving times
        for i in range(15):
            print 'Driving to bus stop %d at time %d' % (i, simpy_environment.now)
            yield simpy_environment.timeout(drive_times[i])
            stop_time = random.randint(0,10) + abs(numpy.random.randn())
            if stop_time>0:
                print 'Stopping at bus stop %d at time %d' % (i, simpy_environment.now)
            else:
                print 'No one stopping at bus stop %d.'
            yield simpy_environment.timeout(stop_time)
            
    env = simpy.Environment()
    env.process(bus_improved(env))
    env.run()
        

    Driving to bus stop 0 at time 0
    Stopping at bus stop 0 at time 11
    Driving to bus stop 1 at time 17
    Stopping at bus stop 1 at time 26
    Driving to bus stop 2 at time 31
    Stopping at bus stop 2 at time 42
    Driving to bus stop 3 at time 46
    Stopping at bus stop 3 at time 53
    Driving to bus stop 4 at time 62
    Stopping at bus stop 4 at time 75
    Driving to bus stop 5 at time 81
    Stopping at bus stop 5 at time 95
    Driving to bus stop 6 at time 103
    Stopping at bus stop 6 at time 115
    Driving to bus stop 7 at time 123
    Stopping at bus stop 7 at time 129
    Driving to bus stop 8 at time 132
    Stopping at bus stop 8 at time 142
    Driving to bus stop 9 at time 151
    Stopping at bus stop 9 at time 159
    Driving to bus stop 10 at time 170
    Stopping at bus stop 10 at time 177
    Driving to bus stop 11 at time 187
    Stopping at bus stop 11 at time 202
    Driving to bus stop 12 at time 209
    Stopping at bus stop 12 at time 224
    Driving to bus stop 13 at time 225
    Stopping at bus stop 13 at time 230
    Driving to bus stop 14 at time 230
    Stopping at bus stop 14 at time 244


We can see from the information printed out that the drive times and boarding
times have been randomized. While this is still not entirely realistic, it is a
bit better than the very first simple bus example.
I hope you found this tutorial useful!
In the next parts of the tutorial, we'll take a look at how we can add more
logic to our Bus process to build a *realistic* (more or less) simulation of a
morning commute on the Isle of Dogs ( a geographic region in East London ).


    
