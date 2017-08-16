---
layout: post
title: the software engineering notebook
---

Fellow software engineers/hackers/devs/code gardeners, do you keep a notebook (digital or plain dead-tree version) to record things you learn?

Since my days assembling glassware and synthesizing various chemicals in the organic chemistry lab, I've found keeping notes to be an indispensable tool at getting better and remembering important lessons learned. One of my professors recommended writing down, after every lab sessions, what had been accomplished and what needed to be done next time. When lab sessions are few and far apart (weekly instead of daily), it is easy to forget the details (for example, the mistakes that were made during weighing of chemicals ). A good quick summary helps with this!

When I first started working for a software company, I was overwhelmed. Academic software development was indeed very different to large scale distributed software development. For example, the academic software I wrote was rarely version controlled and had few tests. I had never heard of a 'build' or DEV/QA/PROD environments, not to mention things like Gradle or Jenkins. The academic software I worked on was distributed in zip files and usually edited by only one person (usually the original author). The systems I started working on were simultaneously developed by tens of developers across the globe. 

To deal with the newbie developer info-flood, I went back to the concept of a 'software engineering lab notebook'. At first, I jotted down commands needed to setup proper compilation flags for the dev environment and how to run the build locally to debug errors. A bit later, I started jotting down diagrams of the internals of the systems I was working on and summaries of code snippets that I had found particularly thorny to understand. Sometimes these notes proved indispensable in under-stress debug scenarios when I needed to quickly revisit what was happening in a particular area of the codebase without the luxury of a long debug. 

In addition to keeping a record of things that can make your development and debug life easier, a software engineering lab notebook can serve as a good way to learn from previous mistakes. When I revisit some of the code I wrote a year ago or even a few months ago, I often cringe. It's the same feeling as when you read a draft of a hastily written essay or work of fiction and then approach it again with fresh eyes. All of the great ideas suddenly seem - well- less than great. For example, recently I was looking at a server side process that I wrote to perform computations on a stream of events (coming via ZeroMQ connection from another server ) and saw that for some reason I had included a logging functionality that looped through every single item in an update (potentially  100s ) and wrote a log statement with the data! Had the rate of events been higher, this could have caused some performance issues, though the exact quantification of the impact still remains an area where I need to improve. Things such as these go into the notebook to the 'avoid-in-the-future-list'.  



