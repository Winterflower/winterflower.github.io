---
layout: post
title: watchalong
---

It's 2 am in London and for once the neighbourhood is asleep. I should be too, but when the brain is buzzing with all kinds of cool programming and writing ideas, it's hard to lie still in a quiet room and meditate on sheep counts. 

So here I am: in my natural habitat, in front of a screen with a keyboard under my fingers and a braindump ready to be parsed into text format. As with all 'omgsuperawsomeshizzideas' I have in the middle of the night, this text might turn out completely awful, but here goes.  

One of the perks of living in a city with a sizeable tech community is the number of tech meetups that are regularly hosted at various local tech companies. Yesterday, Ana Balica from PyLadies London was facilitating a watchalong - a meetup where attendees gather to watch and discuss a technical talk from a relevant conference. The talk selected for this meetup was Brandon Rhodes' PyCon 2010 talk [The Mighty Dictionary](https://www.youtube.com/watch?v=C4Kc8xzcA68).

Although I was a bit skeptical about going to a watchalong (I usually listen to conf talks as background while cooking), this proved to be one of the best meetups I have been to in London to date. Ana paused the talk after every 10 minutes or so and the group discussed the technical points presented on Bradon's slides, clarified any confusion and tried out some of the concepts in a live coding demo. I can certainly say I learned a lot about hashing, what happens when the last three bits of two hashes collide, how resizing works in Python, why the iteration order of a dict depends on its history and why you cannot add a new (key, value) pair into a dict while you are iterating through its existing elements.  

The best kinds of meetup talks leave you chomping a the bit to learn more about the technology or topic presented. Six hours post-meetup and I have tons of Python dict questions I want to research. In fact, I'm so excited about the humble (or maybe not so) Python dict, that I can't sleep. That thing that most people running on CPython happily take for granted is actually a complex piece of code machinery that makes sure that even in unlucky situations with multiple hash-collisions, the dict lookup performance stays good. How did Guido et team come with this design originally? Who was the first developer to implement dict resizing? What is the exact algorithm that determines what happens in the case of a hash collision in a three bit dict? Can the design be improved? How do other languages handle hash collisions and resizing?

If you are a meetup organizer and are struggling to find suitable speakers or just want to try an awesome new meetup format, I highly recommend trying out the watchalong. 


