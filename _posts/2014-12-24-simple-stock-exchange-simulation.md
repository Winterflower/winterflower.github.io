---
layout: post
title: A Simple Stock Exchange Simulation (part 1)
---
*The full script is available [here](https://github.com/Winterflower/python-finance)*

## A Simple Stock Exchange
Suppose we have a stock exchange that only trades equities
of three companies: Nokia, Apple and Google. The number
of shares of each company that the exchange can trade is listed
as follows:

```python
equities={'Nokia':1000000,
          'Google': 2000000,
          'Apple': 2400000}
```

The exchange has allowed three traders: Laura, John and Mark
to trade on the exchange.

```python
tally={'Laura': {'Nokia':0,
                 'Google':0,
                  'Apple':0},
       'John':{'Nokia':0,
                'Google':0,
                'Apple':0},
       'Mark': {'Nokia':0,
                 'Google':0,
                  'Apple':0}}
```

Every day, the probability of a trader making a purchase is determined by drawing from
the uniform distribution. If the probability is greater or equal
to 0.5, the trade purchases 50 shares of each company.
Else, the trader sells 25 shares of purchased stock.

```python
import random

def purchase_stock(broker):
    """
    Trader buys 50 shares of each company
    """
    for company in equities.keys():
        equities[company]=equities[company]-50
    temp_dict=tally[broker]
    for company in temp_dict.keys():
        temp_dict[company]=temp_dict[company]+50

def sell_stock(broker):
    """
    Trader sells 25 shares of each company back to the exchange
    """
    temp_dict=tally[broker]
    for company in temp_dict.keys():
        if temp_dict[company]!=0:
            """
            Trader can sell stock back only if he/she bought it
            in the first place
            """
            temp_dict[company]=temp_dict[company]-25
            equities[company]=equities[company]+25
```
We will also add a method to reset the numbers of shares owned
by all traders and a method which simulates one
day of trading at the exchange.

```python
def reset_exchange():
    """
    Sets the tally of each trader to 0
    """
    tally={'Laura': {'Nokia':0,
                     'Google':0,
                      'Apple':0},
           'John':{'Nokia':0,
                    'Google':0,
                    'Apple':0},
           'Mark': {'Nokia':0,
                     'Google':0,
                      'Apple':0}}
def trade():
    for broker in tally.keys():
        probability=random.uniform(0,1)
        if probability>0.5:
            purchase_stock(broker)
        else:
            sell_stock(broker)
```

After each day of trading at the exchange, we want to print
the number of shares owned by each broker in a nice fashion.
Therefore, we add a method called `pretty_print`, which will
print our the tally in a nice fashion.

```python
def pretty_print_tally(dictionary):
    """
    Pretty prints a tally of stocks
    """
    for broker in dictionary.keys():
        print broker + ": "
        print "Equities"
        for company in dictionary[broker].keys():
            print company + ": " + str(dictionary[broker][company])
        print "#########################################################"
```

After ten trading days at the exchange, the standing is as follows:

```
#########################################################
After day 10
Laura:
Equities
Nokia: 275
Apple: 275
Google: 275
#########################################################
John:
Equities
Nokia: 50
Apple: 50
Google: 50
#########################################################
Mark:
Equities
Nokia: 100
Apple: 100
Google: 100
#########################################################
```

## Programming Postmortem
* No TDD! I launched straight into writing code without pausing
and thinking about it. Doing TDD requires discipline and skipping on
good TDD practices is not going to help develop my TDD-fu.

* No tests whatsoever! Thus refactoring will most likely be a nightmare.
