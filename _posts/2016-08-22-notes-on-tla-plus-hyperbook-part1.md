---
layout: post
title: Notes on TLA+ Hyperbook (part 1)
---

_I am currently working through Leslie Lamport's (TLA+ Hyperbook)[http://research.microsoft.com/en-us/um/people/lamport/tla/hyperbook.html] and writing up these notes/summaries/review questions to hopefully help me internalise the material._

# Notes on TLA+ Hyperbook by Leslie Lamport

What follows are my notes on Leslie Lamport's book _TLA+ Hyperbook_. 

## Chapter1: Introduction 

1. What is concurrent computation? What is parallel computation?

In concurrent computation, things occur at the same time. In parallel computation, a single task is executed concurrently (that is in chunks that occur at the same time ). Parallelism is optional (unless prohibited by costly computation time) and is usually easier than concurrency, because the programmer is in control of which chunks of the single task are executed concurrently. 

2. What is a digital system? How to choose a suitable abstraction for a digital system?

A digital system is a system that performs computation as a collection of discrete events. What constitutes a discrete events depends on the abstraction we are using to model the system and who will be using that abstraction. Lamport given an example of a digital calculator. For a user of the digital calculator, pressing the key 5 represents one event, while for the calculator engineer the act of pressing can be two events. The abstraction chosen to model the system must be simple enough to model the system well. 

3. What is the Standard Model?

A system is a collection of behaviours. 
A behaviour is a sequence of states and represents an execution path of the system.
Each state is an assignment of values. 

This is interesting! Can we come up with some "real world" example? Let's go with an example of an apple. ( To everyone that grows apples, I apologise - my knowledge of orchards is very poor!). An apple is a system that has many different execution paths. It starts its life as a flower on the apple tree and from there can follow any number of execution paths

* flower -> frost bite

* flower -> pollination -> ripped out by storm

* flower -> pollination -> unripened apple -> eaten by bird

* flower -> pollination -> unripened apple -> ripe apple 

etc. I'm sure you can come with many more execution paths for an apple. 

4. What is a specification? What is a formal speficiation?

A specification is a description of a model, a formal spec a description that is written in precisely defined language. 


