---
layout: default
title:  "Counting with Coin Flips"
date:   2024-05-04 15:07:10 -0400
categories: BC
---

## Introduction
Recently, I read [an article](https://www.quantamagazine.org/computer-scientists-invent-an-efficient-new-way-to-count-20240516/) by Steve Nadis about how to count large numbers using limited memory and randomness. He references a [paper](https://arxiv.org/abs/2301.10191) about estimating distinct elements in a sea of data using a basic system of repeated coin flips. I decided to test my knowledge of Python and try to implement it myself.

## Goals
To set a quantifiable outcome, I tried to emulate the numbers given in Steve's original post about unique words found in Hamlet. Out of the 30,557 words in the play, their program estimated a total of 3,955 distinct words, against the true human-counted total of 3,967. Using these numbers as a guide, I began writing a quick program to strip and count unique words in Hamlet, coming up with a total of 4,938 (possibly due to a different source text). 

```
for line in file:
    for word in line.split():
            word = word.strip('.,!:;$%^&-*()[]{}?\'').lower()
```

## Implementation
The basic concept for this method of counting is to use a series of increasingly unlikely coin flips to ensure that each word has an equal chance of being part of the 'chopping block'. To implement this, first I used the Random Python library to select a 1 or 0 at random. The function takes a number 'n', that indicates which round (or session in this code) of cuts is currently being made, and informs the coin_flip function how many times to loop. This number is incremented every time the list of words reaches it's maximum. The higher the maximum the more accurate the estimate, and more memory required.

```
def coin_flip(n):
    coin = random.randint(0, 1)
    if coin == 0:
        return 0
    else:
        if n == 1:
            return 1
        return coin_flip(n-1)
```

As well as making sure that it becomes more unlikely that a distinct word will stay on the chopping block, we must also make sure that all words are 'flipped' once the maximum is reached, regardless of the round it's on. This also ensures that words from the first round, Round 0, will still have a random chance to be removed.

```
def filter_list(word_dict):
    new_word_dict = {}

    for word in word_dict:
        coin = random.randint(0, 1)
        if coin == 1:
            new_word_dict[word] = word_dict[word]
    return new_word_dict
```

## Calculation
Now that my code successfully iterates through as the paper indicated, it's time to calculate. I adjusted the (shockingly simple) formula for Python and began testing the data. The formula takes the number of rounds (listed here as session) as well as the current amount of distinct elements on the chopping block (listed here as len(word_dict)).

```
return len(word_dict)/(1/(pow(2,session)))
```

## Results
```
Estimated distinct words: 4992.0
Estimated distinct words: 4928.0
...
...
Estimated distinct words: 3968.0
Average Estimate: 4953.6
```

