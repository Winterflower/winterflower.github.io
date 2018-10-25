---
layout: post
title: failing fast
---

Failing fast is usually a good thing in application code. Much less so when it comes to job applications.
And yet here we are. The perils of checking one's email right before bed only to discover that
the job application one spent hours polishing has been rejected within hours of submission.

It was a damn good opportunity too. A great company and a great set of problems to work on.
As much as I like to pretend I can take a good slap-in-the-face rejection as well as any seasoned 
programmer (who once, many years before, used to regularly submit my badly written thinkpieces and fiction to literary journals),
it hurts. My brain has dug out the familiar 'Compilation of Epic Fails' reel from the Forbidden Archives and all those moments are now gleefully playing in my mind. Not a good recipe for sleep. So perhaps it's time for some more thinkpieces.

Any given application is like a garden of forking paths. Data flows through functions, taking left
or right turns at if-junctures, looping about in loops, before finally exiting the application 
boundaries. When application developers design these convoluted gardens, we often make many
assumptions about the paths that come before the particular segment we are working on and
what the data will look like once it has traversed those and reached our little nook of the garden. We build our own abstractions and instructions based on those assumptions and often, our utopian views of the world upstream from us, is flawed. 
We rely on the gardeneres who designed the paths before us to trim the weeds and lay enough traps so that any rogue bits and bytes
don't reach our pristine constructions. Laying these traps are what we speak about when we speak about failing fast in code.
In Python, my current language of choice, it is a common task to lookup a value in a data structure known as a dictionary (though other programming languages may know it as a hashmap) using a key. Sometimes, we may not know before runtime whether a given key will exist in the dictionary, so to help out programmers in these situations, Python allows for two ways to look up dictionary values.
The first

```
my_dictionary['key']
```
will fail if the key `key` does not exist.
The second

```
my_dictionary.get('key')
```
will not. It will return to the caller the value of `None` and the program will continue on its merry way. If the value of whatever is stored at `key` in this dictionary is critical to the functioning of the rest of the program, it would be best to use the first method and force the program to fail fast. Otherwise, the downstream error that would result from allowing the `None` value to propagate might be cryptic and result in many wasted braincycles of debugging. 

Is there a life equivalent to 'failing fast'? Is there some test one can carry out to determine if it's worth continuing on this path or if one should just throw in the towel? 
A few years ago as an undergraduate I worked in a chemical physics lab for the summer. While I wasn't zapping dye molecules with ultrafast lasers (a story for another time!), I shared an office with a postdoc, who told me about her strategy for failing fast. She told me that she would solicit honest feedback about her work from her supervisors and ask if they thought she could make it as a scientist. I wonder if I should start doing the same and if I could take the answers whatever they might be.

Hearing that you're just not cut out for something you really really like doing has never been easy (even if deep down inside you already know you're not cut out for it).
When I was in high school, I got really into mathematics. By sheer chance, I'd done a few problems from an advanced book on sequences and series and fell in love with the problems. They were addicting to solve! That same month, my mathematics teacher returned our exams and looking at the class told us, 'There are no diamonds here.' I think she meant that none of us were going to be the kind of star students that score high on math olympiad questions. I already knew this. I was never going to be able to do research in math or compete in the olympiad and yet, I felt a bit deflated. Why try if the person teaching me already thinks it's a lost cause? Later, when the teacher tried to encourage some girls from the class (weirdly enough, me too) to sign up for the math olympiad, I flat out declined. There are no diamonds here. 
 
So no I wonder: Can I really make it in technology? I don't come from a traditional background. I don't have a degree from Oxbridge or an Ivy. I don't have the Big 4 on my resume. I was not a diamond in anything back in high school and I sure as hell am not one now. Can I make it? Or am I just deluding myself? Have I gotten this far just because of my gender? Maybe I really don't have what it takes.  
