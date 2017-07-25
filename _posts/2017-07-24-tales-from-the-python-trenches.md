---
layout: post
title: Tales from the Python Trenches
---

A year and a half ago, serious doubts, along with the occasional bug (or ten ) crept into my daily coding life. At the time, I was working for a startup company, which made specialised analytics products for operations and computer security. Most of my days were spent wielding Python against unruly CSV files with missing, incomplete or corrupted data. Although the job was enjoyable and I was absorbing knowledge, I felt that I was failing to develop an ability to understand how large software systems and products are built and maintained. For some reason, at the time, this seemed like a paramount step in the career of an aspiring software engineer ( I hesitated to call myself an engineer back and then and still do, even though it has been my designated job title for a while).

Memories of a recent visit to Google added more fuel to the fire. One of my hosts for the day was the lead developer on Google Chrome and I remember how he beamed with pride when he remarked that the best part about his job was 'riding on the Tube in the morning and seeing people using your product'. I remember thinking how cool this must be: knowing about and working on the internals of a system used by millions of people. Yet, the event also showed how far I was from being able to work as a professional developer on anything remotely as large and complicated as Chrome. My background was in the world of chemistry laboratories and then the chalk and blackboard floors of the mathematics deparment. I had learned a bit Java on my own and then Python, but that was about it. My graduate degree blended machine learning and bioinformatics, not systems design and operating systems. Thus when my fellow attendees discussed binary search trees, stacks, caches and the Linux kernel all I could do was nod politely and add these items to my 'To-Study' list. 

After a few months of interviewing while working in my role as Python data analyst, I was able to secure a role in the tech department of Big Corp, where I spent the next year and a half as part of a global team of a hundred or so developers building a large analytics platform in Python. In spite of the barrage of corporate politics and bureaucracy that I encountered, I learned a few very valuable lessons that I would like to note down. Granted not all of the lessons-learned are directly related to Python. Some are bugs with peopleware or process - but important bugs nonetheless! 

### Invest in Memory Profiling

For junior developers like me, Python's automatic memory management is a blessing and a curse. A blessing, because with a few lines we can build an entire web application or a server, a curse, because our lack of battle scars with pointers and malloc can easily lead to a support call from hell. This is precisely the situation I found myself one unlucky Thursday morning. With a coffee in tow to jolt awake my still-sleeping brain cells, I logged into my work terminal at my desk and opened the developer chat. There were several messages marked as urgent from the team in Asia-Pacific. They were linking to server side jobs that were crashing and restarting with out-of-memory errors. Within half-an hour or so, the same issues started plaguing the instances serving European users. This had not happened previously and nothing in the environment had changed. There was no monitoring on the data volumes passing through the system, so I tried to eyeball the logfiles to size up if there had been a change in volume. Nothing seemed out of the orginary, yet like clockwork, the server side process kept ballooning and crashing every hour or so.

Eventually, the root cause was found: an innocent looking code change that had been released into production on the previous day. Since the CI system had no performance regression tests, there was no automated alert or failing build to use as a starting point for finding the root cause. Instead, our saving grace was luck - the engineer who had made the commit was online and saw the conversation on developer chat. 

Performance regression testing is hard. How often do you run your regression tests? After every commit? Once a day? What data do you use and how do you predict production loads? After you have discovered a memory leak, how do you find which objects are being retained? 


### OOP is Great until it's Not 

At a recent ClojureBridge meeting, I met an engineer whose first programming language was ClojureScript. After spending many years in Java land, it was strange to discuss programming with someone who was 'functional-first'. Unfortunately (or fortunately), my first language was Java and so my brain will now and probably far into the foreseeable future be, at least to a certain extent, influenced by the principles of OOP. 

In any case, OOP is great if you're working on a small codebase, but without discipline, OOP in large (>50 million lines) codebases becomes one large inheritance forest. It probably makes sense to the developers who have been working in the same code space for a while, but for a new person, the levels of indirection in giant inheritance hierarchies are overwhelming. As are object public interfaces that span hundreds of methods. 

### Name Your Variables Well

In 3 weeks time, you won't remember what type of object is numberContainer. Use variable names and use them well. 

### Build a Robust Incident Response and Documentation culture

What happens after a major bug or production issue is solved? Is the issue logged anywhere? Does anyone write down the steps that were used to diagnose the root cause of the issue? How was the root cause fixed and what testing was done to make sure the fix will work? 

This lesson is not Python specific, but important nonetheless. Too often, it's easy to get sucked into the chase for the root cause and then, after the bug has been successfully fixed, forget everything about it. Only until the next time, when something similar happens and no one at hand remembers the details well enough. 
I witnessed this happen many, many times. I was one of the engineers who fixed a bug and then went off to have a midday latte instead of being disciplined and writing down a proper postmortem. 
Setup a documentation system (Jira, Trello, etc ) that is easy to use, allows code snippets, attachments and screenshorts and that can be easily searched. It will be a valuable record of how resilient your system is and how it evolves.

### Coupling Client and Server Code

This one, is once again, not strictly Python specific, but makes ever single server or client release or bug fix into a nightmare. If you don't use some kind of code-agnostic format of messaging between client and server, but rely on object serialisation, at some point there will be some user somewhere, who refuses to update his client program. If your change is not backwards compatible, the server side process will be in trouble when clients running on the old codebases make connections. 

### Concurrency is Hard

CPython makes it even harder. A. Jesse Jiruy Davis does a better job than I ever could explaining concurrency in CPython in this [post](https://opensource.com/article/17/4/grok-gil). 

### Design Your Systems for Monitoring

If monitoring is an afterthought, it will always be an afterthought. In some places, monitoring means a few hastily added debug statements or a few loglines that time the execution of a code block. You might not be motivated during development time to invest in proper monitoring, but come production-issue-time, those hours invested in designing how to monitor a system will pay off. However, pure logging noise (such as logging all messages your program receives) is not helpful to anyone, least of all an on-call engineer who has never committed code into your program. 

There will be situations when monitoring is your only hope for figuring out what's going on. The incidents where a component is crashing are in many ways the easiest, because at least you have a concrete starting point for debug. What about situations where everything seems to be going well, but a user is telling you that a number is incorrect? Or that a number is updating too slowly? If that number is supplied by one service, you have a starting point. What if that number comes out of a pipeline that depends on 5 services interacting together? None of the services has crashed, so there is no obvious point of failure, no obvious point to begin an investigation.

I always strive to think about the metrics that I would find most useful in a incident debug situation. For example, if there is a critical piece of code that needs to executed before an update is sent to the client, I make sure to log this. I also log the number of updates received and sent by the application. In the absence of good metric collecting systems, these give me some ballpark estimates of load. Logging procedures can slow your system down, be mindful of this.

Starting off with what you, the designer of a system, considers useful is not always ideal, because someone else, someone who has never touched your system, may be on-call on the day that it breaks. This is why, 'debug scenarios' is one of my favourite team activities. Write down several real or hypothetical user complaints (for example, 'number X is wrong on my display and it's taking Y minutes to update. I expect the number to be A and the update to be immediate') and then ask your team members to diagnose the issue. Observe how they interact with the logs of the application and make notes of log statements that are confusing, unhelpful or red herrings. 

For a more persuasive argument about the important of good monitoring, see Nathan Marz's excellent post ("How becoming a pilot made me a better programmer")[http://nathanmarz.com/blog/how-becoming-a-pilot-made-me-a-better-programmer.html].

