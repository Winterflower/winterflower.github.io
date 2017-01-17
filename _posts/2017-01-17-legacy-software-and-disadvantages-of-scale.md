---
layout: post
title: Legacy software and disadvantages of highly specialised teams
---

## when making a small adjustment becomes an un-testable multi-team problem

Today I'd like to talk about some frustrations that arise when working on a legacy system developed by multiple remote software development teams.

A long time ago, I worked on a system that snapped some realtime ticking data, carried out a few computationally expensive calculations ( they had to be carried out on a remote server machine ) and sent the result to a user's front end. I was placed in charge of building out the infrastructure for client-server communication. The data manipulation libraries that calculated values based on data ticks were developed by an independent team and I was not given access to modify this code. Although this system had many shortcomings (most introduced by me), a particular pain point was the system of databases and APIs that had grown around the service that supplied ticking data. At the lowest level of the system was a message queue, which monitored the various tick data sources. The data on the queue was pushed into a database, which exposed a direct query API to any clients. However, the query language was more or less intractable to people without deep experience with the database software and thus yet another team was setup to develop an abstraction on top of the database API. None of the APIs were properly documented and had been designed a few years before I came along.

Thus, the correct functioning of my software depended on a 3rd party calculation library, a database managed by a remote team, a hard to use database API managed by the same remote team and a another abstraction built on top of the database API to 'make things easier' for the ultimate client applications (such as the system I developed ). At first the components of the system cooperated on a fairly regular basis and eventually thousands of different datatypes were onboarded to the same system. Eventually the vendor software that governed the values being placed into the message queue was replaced with another software that did not have the same filtration capabilities. Very soon I started seeing invalid data values propagating into my server side calculation process and the 3rd party calculation library struggled to cope with them. It did not have proper error handling to exclude bad data - since the need had never come up, no one had ever thought of developing it. This library was mission critical and used by hundreds of applications. Adding any kinds of changes would have required a testing process that would span months and potentially tens of different teams. What made matters worse, I did not have any control of the data values directly since the abstraction API on top of the database communicated directly with the 3rd party calculation library without returning any results into my server process. 

My next step then was to talk to the abstraction API team, but the solutions that were offered for filtering offending data could not be implemented, because such a change would have to be carried out for all datatypes and not just the one that was causing issues for me. The API had not been designed to provide granularity based on particular data types. 
In addition, it was hard to convince the abstraction API team that my problem was a legitimate problem. The data value ticking on the message bus was a legitimate value, but in the context of the business it made very little sense, which is why the computational 3rd party API would have never expected it. 

Eventually, the solution had to be moved into the lowest level - the database API - only after multiple discussions with multiple teams. 

I think there are a few important lessons and some questions:

1) Segregating software developers into highly specialised teams produces software quickly, but the APIs delivered by such teams can easily ignore the needs of developers working on client libraries.

2) This is a hard one: but software should be designed so that it can easile be extended at a future time when requirements change. This fact alone makes comparisons between civil engineering and software engineering hard. I'd imagine that once a team of engineers decides to build a pedestrian bridge, they build a pedestrian bridge. No one will come along and say, "Hey, now your pedestrian bridge will also have to accomodate large trucks." This happens very often in software engineering when one tries to scale an application - the infrastructure that was able to support 100 users, simply won't be able to cope with 1 000 000 users.  

3) What are the best practices for designing data delivery layers? What features should be a part of APIs that expose realtime data to application developers?


