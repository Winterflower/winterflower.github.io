---
layout: post
title: "Experiment: Communication between two Python scripts"
---

*In the "Experiment" series, I write about weird "I've always
wanted to do this, but I am not sure if this will work" things*

For a while now, I have wanted to conduct a very simple experiment.
Can two Python scripts communicate with each other while
both of them are running?
Since both of the scripts are running in their respective shells,
there is no immediate way I can think of that would allow a piece of code
in one script to pass a variable to a piece of code in another script.
Except via a shared space on the disk, such as a file.

## Communication between two Python scripts via a shared file

### Experiment 1
My first experimental setup involved the following two
script files:

* publisher.py - a class that "publishes" some financial data by writing it
to a file

* subscriber.py - a class that "subscribes" to financial data by reading
it from the shared file

```python
#publisher.py
import random
import time

publication_buffer=open("publication.buf", 'w')
trade_number=0

print "Starting exchange"

while True:
    time.sleep(5)
    probability=random.uniform(0,1)
    if probability>=0.5:
        print "Making trade"
        trade_number=trade_number+1
        publication_buffer.write("Trade number"+str(trade_number)+"\n")
```

```python
#subscriber.py
number_of_lines=0
print "Opening trade channel"
while True:
    trade_buffer=open("publication.buf", 'r')
    lines=trade_buffer.readlines()
    if len(lines)>number_of_lines:
        print "Update received"
        print lines[len(lines)-1]
        number_of_lines=number_of_lines+1

```

The publisher is started first to give it a chance to "warm up" (ie. open
the file and start publishing trades). Then after a few seconds,
the subscriber starts listening to the published trades. In theory, this
seems like something that might work.
This is what happened:

```bash
#shell 1

$ python publisher.py
Starting exchange
Making trade

```

```bash
#shell 2
$ python subscriber.py
Opening trade channel
```

For some reason, nothing is received by the other file.
If `subscriber` would be able to read the values
published by publisher, some values would be printed in the console.
But nothing happens!
Also, immediately after launching this operation, my computer
starts to whir and hangs. Something is going on!

Let's look at the output of `top` to see if something
is consuming a lot of computational power and indeed
`python` is the top process (eating up 80% of the CPU).
Now I have to admit, my hardware could use a bit of upgrading
(it dates back to the stone age, aka 2010), but 80% CPU for
something as simple as the above two scripts seems

Investigation will continue. :)
