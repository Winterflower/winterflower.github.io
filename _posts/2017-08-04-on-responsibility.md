---
layout: post
title: on responsibility
---

_A note: Although this post is dated 4th of August, it was completed in early 2018 and thus references some events that took place later in 2017._

I'm writing about software here. 

And perhaps some other things. 

Sometimes, all of the best intentions - the carefully piled Jenga tower of checks and balances, of process and communication - collapse with a thud into - well - an epic fail.

So it is, that I find myself in this moment, looking at the face of the woman, whose work an error in the software program has just wiped out. 

There isn't much to say. But I have to say something. 

"I take responsibility"? 

"We'll improve the process"?

"I'll make sure it won't happen again."?

The junior developer I guided through the disastrous change is next to me. Silent. 
We go back upstairs to engineering, each to our workstations and wait for the fallout to start.

While the email chains are percolating further and further up the many chains that separate those who make decisions and those who suffer their consequences, I get to think about responsibility. 

---

We didn't read the code carefully enough. It was too long, too much copy pasta, too many lines, too many long functions. We tried, we tried, we tried, to guess what type this variable would be and how it would behave. We hoped that no one else was using any of the fields we were going modify. We sent emails and did testing in staging environments.

But here we were. The code was pulled out of prod. Apologies were sent. Data was lost. Someone, someone else, would now have a late night manually unfucking the things that got fucked.

I thought I knew what responsibility was. A ticking of boxes to make sure some sort of process was followed. 

But the real responsbility is what comes after.

---

I did not know about Paul Kalanithi until he had passed away. A few days after his death, I read his obituary in the New York Times and then a review of his book _When Breath Becomes Air_. Then, a year or so later, I found the title in my local bookstore and purchased it. 

When you lose data, you shrug and toss one into the bin of 'Fuckups'.

When you lose a life, however. 

Paul Kalanithi dedicated his life to being responsible for other people's future. 
He did not take this lightly.
 
In a passage from _When Breath Becomes Air_, he wrote

_ "As a chief resident, nearly all responsibility fell on my shoulders, and the opportunities to succeed -- or fail -- were greater than ever. The pain of failure had led me to understand that technical excellence was a _moral_ requirement. Good intentions were not enough, not when so much depended on my skills, when the difference between tragedy and triumph was defined by one or two millimeters."_


---

My failure, unlike Dr. Kalanithi's, rarely carries consequences as dire as death of someone's mother, father, brother, sister, or child, but on that day, on that floor with its endless rows of desks and fluorescent lights, looking into the eyes of this woman who now had to work late to get her work done, just because I hadn't done mine properly, I felt it, the pain of failure.

Then and there, I said the only thing I could.
"I'm sorry".

"No, you're not," she replied. It was loud enough to echo across two rows.

I was, but I knew what she was saying.

"Things won't change" is what I think she wanted to say.
You'll go back upstairs, there will be a few emails, a few words of apology, but next week, next month or maybe next quarter, there will be a bugfix, a feature, a push to prod and this will happen again.

And I knew, she was probably right. 

---

A lot of software is built on good intentions. On being cool and using the latest, hottest, just-off-Github framework. It's about being a techie kid in the candy store, except the shelves are lined with stickers, t-shirts, confs and blog posts, and tech, cool cool tech! Or as Martin Thompson phrased in his GOTO Copenhagen 2017 keynote, "intellectual masturbation". 

But some of it is really not just 'software'. Joel Spolsky's post ["Birdcage liners"](https://www.joelonsoftware.com/2018/01/12/birdcage-liners/) examines the fragmentation of social discourse (courtesy of our favourite platforms) and warns, 

_"What is the lesson? The lesson here is that when you design software, you create the future."_

Designing the future is exciting, being responsible for it - terrifying.

We should not take it lightly.



