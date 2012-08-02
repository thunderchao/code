---
layout: post
title: "Putting it Together, Part One"
tagline: "from many, one"
description: "Going for something bigger by gluing everything together"
category: learning
tags: [python, roleplaying, exe]
---
{% include JB/setup %}

### The Plan

At the end of my previous post, I had made a dice roller and an initiative 
tracker. That was just small fries, though, compared to my Grand Vision: to
develop a set of tools that DMs could use to run games easily that included
both of these elements, and a few more.

Because I was getting the hang of it, I decided to continue using `tkinter` 
and `tkk`. *Dramatic Foreshadow Music Playing* I ended up cutting that short
though and only going about half way before I joined the chorus of just about 
every other python dev in the world regarding `tkinter`: it kind of blows.

### Uno: Before I Threw In The Towel

I shouldn't give `tkinter` too bad a rap; it can do some pretty nifty things
rather easily. I mean, have been looking at the code? It's super simple!
So it definitely has that going for it. Yeah there are a couple quirks to it
and sometimes trying to plot out your grid can get ridiculous, but for many 
small GUI programs, it works just fine.

Some of the stuff in its expansion modules `tkk` and `tix` are even pretty 
sweet. Look at this code to make some very easy status-message thingies on
a mouse rollover of an element:

{% highlight python %}
status = Label(frame)
status.grid(column=0, row=8, columnspan=6, sticky=W, padx=10) 

...

b = Balloon(frame, statusbar=status, initwait=0)
b.bind_widget(top_l, statusmsg='©2012 Nathaniel Ray Pickett @thunderchao')
b.bind_widget(ql_load, statusmsg='Load the saved questlog')
b.bind_widget(ql_save, statusmsg='Save the questlog')
b.bind_widget(a_load, statusmsg='Load the saved data for this tab')
b.bind_widget(a_save, statusmsg='Save the data in this tab')
b.bind_widget(b_load, statusmsg='Load the saved data for this tab')
b.bind_widget(b_save, statusmsg='Save the data in this tab')
b.bind_widget(c_save, statusmsg='Load the saved data for this tab')
b.bind_widget(c_save, statusmsg='Save the data in this tab')
b.bind_widget(in_b, statusmsg='Launch the initiatives tracker')
b.bind_widget(dr_b, statusmsg='Launch the dice roller')
b.bind_widget(tr_b, statusmsg='Launch the treasure depot')
b.bind_widget(ct_b, statusmsg='Launch the critical hit/miss tables')
b.bind_widget(ma_b, statusmsg='Launch the map')
b.bind_widget(ot_b, statusmsg='Launch the other tool')
{% endhighlight %}

In fact, I went to town with the many cool things the `tkinter` suite can 
do out-of-the-box. Check out the 'final' product:

[![inits4][i4t]][i4]

[i4t]: /assets/img/inits4t.png
[i4]: /assets/img/inits4.png

You can click on it to see it in full. But look at what we have here: text 
formatting, tabbed navigation, popup windows, big text areas that I programmed 
to load and save into `.txt` files in the root directory, label frames, etc.
It's pretty cool looking. So what was the problem?

### The Problem: An Interlude

The whole point of this project was to make something that others can use. 
It was to be lightweight, look neat, and be rather functional. Two things 
impeded my further use of `tkinter` though: 1) it was getting harder and harder 
how to translate what I wanted to do with the program in terms of functionality 
into `tkinter`'s clunkiness (because I'd probably have to include a whole 
bunch of other modules and figure them out in python by itself first anyway),
and 2) py3k's convert-to-windows-executables methods are *terrible*.

Remember the [first Initiative Tracker program](/learning/2012/07/13/tko/) 
I wrote? Just for funzies I tried to get that to turn into a `.exe` file.
I ran hard into a very large wall: as far as I could find, there was simply
no way at all to make a py3k program into a single-file windows executable.
Pretty much the only method to use is with [cx_Freeze](http://cx-freeze.sourceforge.net/).
My 2kb python file exploded into 13Mb and a few dozen files across a handful
of directories.

From a practical standpoint, with my very slow upload speeds (don't get me started),
it would take dropbox 221 minutes to upload it. Abysmal. So what did I do?
I threw in the towel.

### The Appeal: Fork It!

Just like all of the programs I make/feature here, this ["full-suite" version](https://github.com/thunderchao/roleplaying/tree/master/inits/dmtools/)
is up on github. I encourage you to download it and give it a run. Even if
only the initial screen, dice roller, and inits tracker are functional. In 
fact, download it, give it a run, tweak it, and make it better! Because that's
the fun of forking, kids! And then, I want to see what you've done with it!
Science!

But seriously, this is a learning process for all! Dooooo iiiiiit. And then
tell me about it [@thunderchao](https://twitter.com/intent/tweet?screen_name=thunderchao)!

Next time, I'll unleash upon you a sneak peek at my WIP, New-and-Improved 
version of DM Tools!
