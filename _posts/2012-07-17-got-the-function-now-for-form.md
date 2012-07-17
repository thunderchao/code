---
layout: post
title: "Got the Function, Now for Form"
tagline: "rotate and enhance!"
description: "Because bad design is just plain bad"
category: learning
tags: [python, roleplaying]
---
{% include JB/setup %}

### Recap

If you recall, I made a little Tcl program using python to track initiatives 
that looked like this:

![inits1](/assets/img/inits1.png)

That's fine and all, but it looks like crap, and it doesn't even do all that 
I had hoped for when I imagined this program (like manually moving entries 
up and down). So I tinkered with `tkinter` a bit.

### Round 2

Even though I make it seem like I had function in the bag from the title of 
this post, the first thing that I wanted to do when messing with this program 
was to pull everything into one screen. In the first version, you first had 
to type in a name, then select it, then click roll for selected, then type in 
a value for the initiative bonus, then hit okay. Way too many steps. So I 
simplified by adding more.

First, I had to redesign the grid for the window. In the original, I had 4 
columns and 5 rows (the scrollbar oddly takes up its own column) but the grid 
wasn't all that hard to figure out. Wanting to add a bunch more buttons and 
fields really wonked that up though, and I ended up with 8 columns and 9 rows 
to produce the following:

![inits2](/assets/img/inits2.png)

Looks much better, right? I think so. I mean, it still looks pretty crappy, 
but now at least the interface is intuitive and stuff. Adding in those extra 
buttons--˄, ˅, and edit--meant extra functions to be defined as well. I also 
had to change the `add_item()` function because now it was going to do more 
then just drop a name into that listbox. This meant that I had to take some 
lines from `roll_it()` and put them into my new adding function. In place 
of the `AskBox` class from the last version, I made it an `EditBox`--here I 
figured an extra popup window would be the right choice since 1) editing 
shouldn't happen that often and 2) editing something in-line like that would 
be a nightmare to code.

All of these changes made my functions look thus (excluding the ones that 
didn't change at all):

{% highlight python %}
def add_item2():
    nums = int(roll_e.get()) + int(bonus_e.get())
    whole = str(nums).rjust(2, '0'), name_e.get()
    listbox.insert(END, whole)
    name_e.delete(0, END)
    roll_e.delete(0, END)
    bonus_e.delete(0, END)
    bonus_e.insert(END, '0')

def move_up():
    try:
        selected = listbox.curselection()[0]
        selected_i = listbox.index(selected)
        selected_v = listbox.get(selected_i)
        
        above_i = selected_i - 1
        
        listbox.delete(selected_i)
        listbox.insert(above_i, selected_v)
        listbox.select_set(above_i)
        listbox.activate(above_i)
    except IndexError:
        pass

def move_down():
    try:
        selected = listbox.curselection()[0]
        selected_i = listbox.index(selected)
        selected_v = listbox.get(selected_i)
        
        below_i = selected_i + 1
        
        listbox.delete(selected_i)
        listbox.insert(below_i, selected_v)
        listbox.select_set(below_i)
        listbox.activate(below_i)
    except IndexError:
        pass

class EditBox:
    def __init__(self, parent):
        top = self.top = tkinter.Toplevel(parent)
        top.title('Edit Entry')

        self.lf = tkinter.LabelFrame(top, text='Edit Entry')
        self.lf.grid(column=0, row=0, columnspan=2, padx=5, pady=5)
        
        self.l = tkinter.Label(self.lf, text='Format: "## name"')
        self.l.grid(column=0, row=1, columnspan=2, padx=5, pady=5)
                
        self.e = tkinter.Entry(self.lf, width=15)
        self.e.insert(0, '')
        self.e.grid(column=0, row=2, padx=5, pady=5)

        b = tkinter.Button(self.lf, text="ok", command=self.ok)
        b.grid(column=1, row=2, padx=5, pady=5)

    def ok(self):
        self.result = self.e.get()
        self.top.destroy()

def edit_item():
    try:
        index = listbox.curselection()[0]
        item_to_change = listbox.get(index)
        
        main_window.update()
        edit = EditBox(main_window)
        main_window.wait_window(edit.top)
        
        item_to_change = edit.result
        listbox.delete(index)
    except IndexError:
        pass
    listbox.insert(index, item_to_change)

def roll_it2():
    try:
        rand = random.randint(1,20)
        roll_e.insert(0, rand)
    except IndexError:
        pass
{% endhighlight %}

You can see and get the code for this version at its github home: 
[initiatives-1.1.py](https://github.com/thunderchao/roleplaying/blob/master/inits/initiatives-1.1.py).

### Round 3

After producing version 1.1, I let it sit for a couple days, not quite happy 
with the way it looked, but satisfied with its functionality. During that fallow 
period, I investigated how to make `tkinter` look, you know, not like shit.

The answer I found was in `Tix` and `TTK`, a couple of addon modules for `tkinter` 
that update its looks beyond its Windows XP style and throw in a couple of 
extra things to play with. This added a couple of lines to my import section, 
and as a result, I was able to come up with something much more compact and 
prettier:

![inits3](/assets/img/inits3.png)

Ooooh, so niiiiiiiiiice.

Functionally, all I changed here was lumping the roll and add functions together. 
Everything else was to make it look good, and damn I think it does. Now, some 
things I wasn't able to figure out--or, more correctly, they worked well enough 
and I didn't feel like investigating how to make it work *perfectly*. That's 
why entries with spaces in the name (like the Goblin Archers) have curly brackets 
around them.

Even though I consolidated the design of the program, I still ended up adding 
a dozen extra lines. Ah! I just found out why! OK, so `tkinter` has this odd
property where hitting enter to act as a button click works differently than 
just clicking it. Not only do you have to set a key binding manually, but then 
you have to create a separate function (at least because of the way I wrote 
this program) with a `self` parameter to not return an args error. This is 
what it looks like in code, using the new adding function for this version:

{% highlight python %}
def add_item3():
    nums = random.randint(1,20) + int(bonus_e.get())
    whole = str(nums).rjust(2, '0'), name_e.get()
    listbox.insert(END, whole)
    name_e.focus()
    name_e.delete(0, END)
    bonus_e.delete(0, END)
    bonus_e.insert(END, '0')

def add_item_b(self):
    nums = random.randint(1,20) + int(bonus_e.get())
    whole = str(nums).rjust(2, '0'), name_e.get()
    listbox.insert(END, whole)
    name_e.focus()
    name_e.delete(0, END)
    bonus_e.delete(0, END)
    bonus_e.insert(END, '0')

...

name_e = Entry(add_f, width=15)
name_e.focus()
name_e.insert(0, '')
name_e.bind("<Key-Return>", add_item_b)
name_e.grid(column=5, row=2, padx=2, pady=2)

add_b = Button(add_f, text="roll and add", command=add_item3, width=16)
add_b.bind("<Key-Return>", add_item_b)
add_b.grid(column=5, row=3, columnspan=2, padx=5, pady=5)
{% endhighlight %}

So on top there are two versions of the same function--the only differences 
are the name and that one takes an argument. On the bottom is the reason why. 
When you click the button, `command=add_item3` fires, but if you hit enter 
either in the `name_e` field or when the roll and add button is in focus,
`add_item_b` fires. It took me far too long to figure it out, but here it 
is and now you all know!

Ha, I also just realized/remembered that I also added some tooltip functionality! 
Man, there were all kinds of neat things going on here!

Like the previous version, you can check this one out in its github page too:
[initiativesTix.py](https://github.com/thunderchao/roleplaying/blob/master/inits/initiativesTix.py)

### You Bet There's More

By this point, I was very satisfied with my little initiative tracker program. 
It works, it looks neat, and it actually has some real-world application. 
The next thing I wanted to do was to incorporate this and my dicerolling 
program(s) into something more robust, a program that would be like a replacement 
for a DM's toolkit. You'll have to wait until next time (which won't be for 
a couple weeks because I'm leaving for a two-week vacation in the next couple 
days) to see how bad I messed that one up.

And, as always, if you have any suggestions/comments tweet me up 
[@thunderchao](https://twitter.com/intent/tweet?screen_name=thunderchao).
