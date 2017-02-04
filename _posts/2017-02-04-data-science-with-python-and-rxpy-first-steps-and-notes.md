---
layout: post
title: Data science with Python and RxPy - First steps and notes
---

The world is full of data that changes or 'ticks' over time - prices of financial instruments on the market, quality of air over a given day, the number
of Twitter followers a particular user has (I'm sure you can come up with many more!). How to best capture and analyse this data? 
One way of engineering data systems around contantly changing streams of data is to design reactive event-driven systems. That is a mouthful that probably has
as many definitions as there are practitioners, so it might be best to examine what components are needed to construct reactive event-driven systems. 

To build the basics of a reactive system we need Observers ( objects that perform some action - 'react'- when a piece of data they are interested in ticks )
and Observables, which represent the streams of data. In essence, we could characterise this system as a publish-subscribe system: the Observables publish streams of data
to all interested Observer classes.

Let's make all of this concrete by implementing a simple example using the RxPy library and Python3. 
Suppose the air quality of a specific area is measured using an index that can take values from 1 to 10. Let's design an Observer that subscribes to this stream of
mocked air quality data and emits warnings based on the value of the index. The Observer that we need to write should inherit from the RxPy library's 
Observer class and implement three functions: 

* ``on_next`` which is triggered when an event from an Observable is emitted

* ``on_completed`` which is called when an Observable has exhausted it's stream of data (there are no more events)

* ``on_error`` which is triggered when something goes wrong


```python
from rx import Observer, Observable
from numpy import random

class DataAnalyser(Observer):
    #a class for analyzing data
    def on_next(self, value):
        if value<=3:
            print('Safe to enjoy the outdoors!')
        else:
            print('Air pollution is high in your area - please take care!')
    def on_completed(self):
        print('Finished analyzing pollution data')
    def on_error(self, error):
        print('Something went wrong in data analysis')
```

To complete this example, we also need a an Observable (our stream of mock air quality data ). We can create one very easily using the RxPy Observable class.
Finally, we call the Observable's ``subscribe`` method to register ``DataAnalyser`` as the objected interested in the stream of data published by the 
Observable. 


```python
def main():
    air_pollution_indices = Observable.from_([random.randint(1,10) for _ in range(20)])
    data_analyser = DataAnalyser()
    air_pollution_indices.subscribe(data_analyser)
```

The full sample script is available [on Github Gist](https://gist.github.com/Winterflower/ea8b96bdc0f5f0f262293c6da6ba8fef).
