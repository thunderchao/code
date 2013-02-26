---
layout: post
title: "Quick Library Maker"
tagline: "staving off boredom with organization"
description: "Making a SPLODE for collections of things"
category: showing
tags: [python, productivity]
---
{% include JB/setup %}

### Preface

We've had an unprecedented number of snow days lately, including today, 
and I've been terrible at being productive during them. It's just so easy 
to do absolutely nothing when the city is telling you to stay indoors. 
Last night, when I should have been reading or writing or doing something 
school-related, I instead figured I'd code something, just for funsies.

I have a lot of books. Like, thousands of dollars worth between Katie and 
myself. I've toyed around with making a library-type system before with 
php and mysql, but that's harder than it should be and you have to run it 
on a server, and it's all a little cumbersome. Granted, it comes with extra 
functionality, but I wanted to do something simple.

### The Beginning

I wanted a small program that could add things to a list, sort that list,
and then display the list. This isn't a hard thing, and I barely had to 
look anything up (yay!), but that didn't help me from starting things off 
all wrong.

The first thing I wrote was a simple options list. I did this at first with 
a bunch of `if` statements. This was not the thing to do, as nesting 
ifs together gets cumbersome and trying to link between them is downright 
stupid. And wrong. (Though I suppose if python had a `GOTO` command, 
that could work. But it doesn't. Because GOTO isn't pythonic.)

{% highlight python %}
topmenu = "\t1. View Contents\n\t2. Add an Item\n\t3. Exit"
print("Select an option\n" + topmenu)
select = input("\n\t# ")
if select == "1":
    #do all the view library stuff
    #find a way to go back to the start?
elif select == "2":
    #do all the add an item stuff, which includes choosing between books and movies
    #some more nested ifs
    #again, get back to the start?
elif select == "3":
    quit
{% endhighlight %}

So I revised when I remembered that functions are so the way to go.

### Functions, Not GOTO

Once I started with functions, I decided to go functions all the way, because 
why not, right? Besides, it's always fun to have the entire programmy part 
of the program be one line. So this is what I ended up with:

{% highlight python %}
def libload():
    if os.path.exists(".booklib.txt"):
        with open(".booklib.txt", "r") as bf:
            booklib = [line.strip() for line in bf]
            booklib.sort()
            global booklib
    else:
        booklib = []
        global booklib
    if os.path.exists(".movielib.txt"):
        with open(".movielib.txt", "r") as mf:
            movielib = [line.strip() for line in mf]
            movielib.sort()
            global movielib
    else:
        movielib = []
        global movielib
        
def startup():
    libload()
    os.system("cls")
    topmenu = "\t1. View Contents\n\t2. Add an Item\n\t3. Exit"
    print("Select an option\n" + topmenu)
    select = input("\n\t# ")
    if select == "1":
        viewlib()
    elif select == "2":
        addstart()
    elif select == "3":
        quit
    else:
        input("\nTry again. Hit enter.")
        startup()

def viewlib():
    os.system("cls")
    print("\nView the Library\n")
    date = datetime.datetime.now()
    stamp = date.strftime("%Y%m%d-%H%M")
    fname = "Library-" + stamp + ".txt"
    f = open(fname, 'w')
    f.write("Books:\n")
    for item in booklib:
        f.write("%s\n" % item)
    f.write("\nMovies:\n")
    for item in movielib:
        f.write("%s\n" % item)
    f.close()
    print("File " + fname + " created.")
    webbrowser.open(fname)
    input("Press enter to return.")
    startup()

def addstart():
    os.system("cls")
    print("\nAdd an Item\n\t1. Book\n\t2. Movie\n")
    addsel = input("\t# ")
    if addsel == "1":
        addbook()
    elif addsel == "2":
        addmovie()
    else:
        startup()
    
def addbook():
    os.system("cls")
    print(header + "\nAdd a Book\n")
    bauthor = input("Author's Name (Last, First):\n\t")
    btitle = input("Book Title:\n\t")
    byear = input("Year Published:\n\t")
    addb = [bauthor, btitle, byear]
    booklib.append(addb)
    with open(".booklib.txt", "w") as ab:
        for item in booklib:
            ab.write("%s\n" % item)
    print("\nAdded \"" + btitle + "\" to the library")
    input("Press enter to go back")
    startup()

def addmovie():
    os.system("cls")
    print(header + "\nAdd a Movie\n")
    mtitle = input("Movie Title:\n\t")
    myear = input("Year Released:\n\t")
    addm = [mtitle, myear]
    movielib.append(addm)
    with open(".movielib.txt", "w") as am:
        for item in movielib:
            am.write("%s\n" % item)
    print("\nAdded \"" + mtitle + "\" to the library")
    input("Press enter to go back")
    startup()

startup()
{% endhighlight %}

### Some Notes on the Above

There are a few things up in there that tricked me. First of all was needing
to use the `global` so I could access the variables `booklib` and `movielib` 
in other functions. It wasn't that I wasn't sure I needed `global`, but it
was just how to use it correctly, since I really haven't had to mess with 
it that much.

There was also the issue of sorting these lists. Hot damn, that was messing 
me up. I was trying to sort `booklib` and `movielib` all over the place until 
I realized that if I turned the opening of the libraries into a function that 
fired at the beginning of `startup()` I could do all the sorting in one place.
The only problem I forsee with this is when there are hundreds of entries 
in each file, opening, sorting, and closing it every time I go back to the 
first screen could get reeeeeeeally slow. But for now, it's fine.

### Appearances

With the meat of the code functional, I wanted to make it look alright. I 
googled around to figure out how to resize a console window, which turned 
out to be super easy:

{% highlight python %}
import os
os.system("mode con cols=81 lines=60")
{% endhighlight %}

Then I pulled Colorama in, which I used in the [Roll programs](/showing/2012/06/21/roll/), 
to put some colors in. It's by no means fantastic-looking, but it's better 
than white-on-black I suppose. I also gave it a fancycool name: SPLODE.
Here's a screenshot after an item was added (yes, I know there's a lot of 
unused space in that window...):

[![libadd][libaddt]][la]

[libaddt]: /assets/img/libaddt.png
[la]: /assets/img/libadd.png

### Get It

You can find a copy of the whole of [library.py](https://github.com/thunderchao/various/blob/master/library.py) 
at its home on github. As always, tweet me up [@thunderchao](https://twitter.com/intent/tweet?screen_name=thunderchao) 
if you have any comments or questions.

### PS

Yeah, I know my last post was in August 2012. Whoops. The demands and pressures 
of starting a PhD program I suppose.
