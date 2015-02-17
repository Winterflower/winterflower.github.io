---
layout: post
title: Random Bezier Curves with Bokeh
---

Recently, I have been exploring (Bokeh)[http://bokeh.pydata.org/en/latest/index.html], a Python library
for creating beautiful data visualizations in the vein of d3.js. Not only does
the library provide a good API for creating anything from histograms to
chloropleths, but it also provides support for interactive graphs with real-time
data updates, which is especially fascinating for anyone working with streaming
data such as stock prices.

One of my current side-projects involves creating a dynamically updating map
of the London Underground and for that I need to create a map-like
visualization of the relative positions of the stations. My initial
experiments with using only circles to indicate the positions were less
than satisfying (see the pseudomap below).

![My helpful screenshot]({{ site.url }}/images/londontubemapinitial.png)

 