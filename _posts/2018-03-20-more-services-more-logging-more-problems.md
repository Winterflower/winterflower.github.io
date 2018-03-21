---
layout: post
title: more services, more logging, more problems
---

The better part of the day was spent, in what one can only describe, as end to end JSON message tracing hell.
I am currently working on a platform where several individual components communicate via JSON messages. These messages are produced, ingested, reprocessed and placed back onto messaging streams for downstream consumer. Each components runs as an independent service. 
Early in the week, we detected several problems in one of the end to end flows of data and started trying to find out what things could be done to unb0rk the process.
The resolution of the bug is not terribly interesting (mostly some fixing in a few places in the Python glue code that keeps the whole thing together), but as an experience, debugging an end to end flow in a microservice-y type of environment is definitely something everyone should do at least once (perhaps even if your stack is a monolith). You quickly learn to appreciate the importance of writing good (informative, not spammy) log messages and having a log aggregator that allows you to filter by regex, keyword and date range. I can't even imagine what it's like to trace a single event across 10s or 100s of microservices. 
