---
layout: post
title: "Scraping Worldcat"
tagline: "book lists and citations"
description: "Pulling down book infor and citations from Worldcat"
category: showing
tags: [python, scrape, research]
---
{% include JB/setup %}

### Found It

A while ago (a couple years ago as it turns out), I wrote a small scraper 
to pull some info down from [Worldcat](http://worldcat.org) to get some easy
citations for books I needed for research. Today, while researching another
topic (this time it's architecture, postcolonialism, and ethics) the KU library
catalog was down and that really made me mad. I don't like having to look 
twice for things, ya know? Then I remembered I wrote this scraper thing.

I wrote it back when I was first learning python and toying with Beautiful 
Soup and lxml and when I ran it, I forgot it also popped up browser tabs 
for each item on the list. Ha.

This was, at the time, the most sophisticated thing I had written. And it
still might be. Looking back over it, I realized I'd forgotten how to use
`urllib`, `webbrowser`, and `lxml` completely. Time to relearn them, I suppose.

### Code

{% highlight python %}
def md(lexicon,key, contents):
    """Generic append key, contents to lexicon"""
    lexicon.setdefault(key,[]).append(contents)

user = input('WorldCat username? ')
wcln = input('WorldCat list number? ')

url = "http://www.worldcat.org/profiles/" + user + "/lists/" + wcln
tree = html.parse(url)
root = html.parse(url).getroot()
t = root.find_class('name')
a = root.find_class('author')
{% endhighlight %}

The first thing the program does is ask for a Worldcat username and list. 
If you've never used it before, Worldcat is the ultimate library search tool 
for books. It searches all libraries, all publishers, all over the world.
It also lets you put these books into different lists, such as the one
[I'm using today](https://www.worldcat.org/profiles/nathanielray/lists/3103354).

There's also a little ditty in there that takes the titles and makes them
searchable terms on Google Scholar, which I suppose I thought was handy in
2011. The code's still in there right now, I just commented it out.

The meat though comes in creating a small .txt of the books, urls, and 
citations. It looks like this:

{% highlight python %}
with open(user + wcln + '.txt', encoding='utf-8', mode='w') as url_list:
    url_list.write("Worldcat list URL:\n\t" + url)
    url_list.write("Book Information and Google Scholar Search URLs\n\n")
    for k in scholar_urls:
        v = books[k]
        url_list.write(k + '\n' + str(v).strip("[']") + '\n' +
                       str(scholar_urls[k]) + '\n\n')
    url_list.write("List of Citations (Chicago)\n\n")
    for c in citations:
        url_list.write(c + '\n')
{% endhighlight %}

### Get It

As always, you can find the whole code on its home on github: 
[lx-worldcat.py](https://github.com/thunderchao/various/blob/master/lx-worldcat.py). 
Please tweet me [@thunderchao](https://twitter.com/intent/tweet?screen_name=thunderchao) 
if you have any comments or questions.
