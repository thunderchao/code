---
layout: post
title: "Hermits, man"
tagline: "hiding under rocks since 304 BC"
description: "Writing a simple hermit generator"
category: showing
tags: [python, generator]
---
{% include JB/setup %}

### The Idea

While skimming a collection of roleplaying blogs, I came across a post
by some Rolang character wherein he sets out some criteria for a 
[random hermit](http://www.rolang.com/archives/446). It was alright, but
it got a little weird in the end for me. But, after glancing at it, I 
figured I could make this into a neat little python script.

I only know python, so it's not like I was going to write it in any other
language though.

### The Core

The actual mechanic is a simple step-by-step set of RNGs. See here:

```python
nr = 5
while nr >= 0:
    if nr == 5:
        obs_res = obsession[random.randint(1,10)]
        nr -= 1
```

This process continues until `nr == 0`, which allows the program to run
through each of the 5 randomized elements then `print` out the results. 
All in all, it's a pretty simple program.

I'm sure there's a way to modulize it, make it even more compact, but I 
wouldn't know, since I'm still new to this. The `obs_res` variable is the
value to the random key which gets called later when everything is printed
out.

So you'll notice above that the `if` loop calls on something called
`obsession`, which is a dictionary that I defined/populated/whatevered 
higher up in the code. The `randint` takes a random number between 1 and
10 which then serves as a key value for the `obsession` dict. 

### The Content

I created 5 dictionaries to contain the randomized values. Those dicts 
cover the hermit's obsession, with whom he will converse, the kinds of 
people he tolerates, a defining quirk, and his secret. Here's the `converse`
dict:

```python
converse = { 1 : 'no one (he\'s mute)',
             2 : 'birds',
             3 : 'plants',
             4 : 'animals',
             5 : 'rocks',
             6 : 'invisible friend',
             7 : 'ghosts',
             8 : 'puppets',
             9 : 'dead mother\'s corpse',
             10: 'penis', }
```

So it's not a *total* random hermit: he has bounds and only 5 given areas
of randomization. You'll have to supply the name and clothing and his 
habitat and anything else about him, but to make things a bit easier, the
script does print out a very nice paragraph for you to read aloud to the 
players. Here's an example:

>Your randomly generated hermit is obsessed with supernatural events,
>converses only with no one (he's mute), and only tolerates warriors.
>Curiously, he constantly talks bakwards and in realiity, 
>he is actually related to a party member!

Amusing? Sure. Useful? Maybe. Awesome? Most definitely.

### Give it a spin

Check out the github page for [`hermit-gen.py` here](https://github.com/thunderchao/roleplaying/blob/master/hermit-gen.py).
Oh, and just as a note, all my code is in **python 3** (I know, I know, 
but that's what I learned). For something like this, I think you might 
only have to change one or two things to get it to work in python 2, but
I wanted to forewarn you there.
