---
layout: post
title: "Programming is Awesome"
tagline: "oh yes it is"
description: "Or, how I made something lame pretty rad"
category: learning
tags: [python, productivity]
---
{% include JB/setup %}

### The Problem

Alright, so I'm teaching Human Geography in the fall, and the textbook I'm 
using has a bunch of online resources for instructors, like **pre-made 
powerpoint lectures and slides of all the pictures in the book**. It's pretty
awesome. I went to go download those things though and I ran into a problem. 
The publisher's web dev guy made the links wrong.

You see, there are 4 categories of things to download: Lectures, Image Lectures,
Book Images, and Unlabeled Book Images. Problem was, only the first and last
two options had the correct links where I could download all of the files 
for all 18 chapters in one go. I absolutely refused to click on 36 links to
download 36 files, that I could only do 2 at a time anyway because their 
system has some restrictions on it apparently. So I decided to program!

### What I Did

What I wanted to do wasn't all that hard. First, I had to see how the links 
were structured. Because they did this thing right, the .zip files were all 
in order, nice and neat. The URL structure looked something like this (with
the actual URL and book info obscured because of reasons):

`http://media.URL.com/digital_assets_table/BOOK_TITLE/image_ppts/01_imageppt.zip`

So I opened up the python IDLE and pulled out this magic (after a little 
googling, of course, to get the `.zfill()` and `urllib.request.urlretrieve()` 
functions and syntax):

{% highlight python %}
>>> import urllib.request
>>> span = range(1,18)
>>> url = 'http://media.URL.com/digital_assets_table/malinowski_1e/image_ppts/'
>>> suffix = '_imageppt.zip'
>>> for sp in span:
	sp = str(sp).zfill(2)
	urllib.request.urlretrieve(url + sp + suffix)
{% endhighlight %}

It paused for a second--which means I knew it was working--and then it burped 
out 18 lines that looked similar to this...

`('c:\\users\\nathan~1\\appdata\\local\\temp\\tmpn14acl.zip', 
<http.client.HTTPMessage object at 0x00000000034D7908>)`

...and I went ahhhhhhhhhh crap. That's what I get though for running this 
in the IDLE and not saving the script in the directory I wanted all these 
zips in anyway. Oh well, it wasn't too much of a bother to go to that folder 
and nab all those files. Certainly much less than clicking on a shit-ton of 
links, that's for sure.

### What I'll Do Next Time

Line 2 there is superfluous, doing something like 

{% highlight python %}
>>> for x in range(1,18):
{% endhighlight %}

works just as well. Except that I just realized that the `range()` function 
is not inclusive, so gotta bump the number up to 19. Now to manually download 
chapter 18's files. Oh, and I'd save the script as a file first so I don't have 
to go hunting down tmp files.

But look at that, only three things! This is why programming is awesome.

Tell me why you think programming is awesome:
[@thunderchao](https://twitter.com/intent/tweet?screen_name=thunderchao).
