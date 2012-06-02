---
layout: post
title: "Gitting it Done"
tagline: "Git it? Puns! Git!"
description: "learning git"
category: learning
tags: [git, jekyll, bootstrap]
---
{% include JB/setup %}

### Git for the people

As I started getting into programming and actually looking at people's source 
code, I came across [github](http://www.github.com) links and I thought to 
myself, Hey those look pretty neat. Not only is the source code for programs 
laid bare for all to see, it also looks damn pretty. Like with all new, pretty 
things I see (ok not all) I wanted to give it a swing once I actually started 
writing some code.

Signing up was pretty easy but actually trying to figure out how to use git 
and how to make it work was kinda rough, but thankfully there's all kinds of 
people on the internet who have put up all kinds of stuff to help newcomers 
like me figure git out.

### Getting git

Aside from the online tools, which are nice, there's also the git bash shell 
which, as it turns out, is quite necessary to manage your repos (projects) 
and to make the changes you need to your code. I had messed around with bash 
before having used/played with a few linux distros, so I wasn't starting from 
zero, which was very nice. Again, the help docs came in quite handy and in no 
time I was able to figure it all out. Mostly. But the mostly comes later.

### Webbing with git

This is where I ran into the most trouble (but that said, it wasn't much). 
Github has this nice feature called [pages](http://pages.github.com), where 
you can easily turn a project into a webpage (like this one). Since I already 
owned `nathanielray.com` though I wanted to shift my hosting over from a 
free hosting company that has been disappointing me lately. So I followed the 
instructions github provided and it worked great&mdash;after dumping my website 
files into a new repo, pushing it to github, and changing my DNS settings, 
`thunderchao.github.com` turned into `nathanielray.com`.

Making this blog was a bit harder than that however. First of all, I wanted
`nathanielray.com/code` to resolve at `code.nathanielray.com`. A little bit 
more fiddling with the DNS settings and waiting a bit and that worked, but I 
wanted to make full use of github's features&mdash;namely using [jekyll](https://github.com/mojombo/jekyll) 
and [markdown](http://daringfireball.net/projects/markdown) to create a blog-like 
site. This one.

### Bootstrapping

Some internet guy named Jade created [Jekyll Bootstrap](http://jekyllbootstrap.com/) 
to help people get started using the platform. Basically, you download a zip, 
decompress it, and then you have a site ready to go. All you need to do is edit 
it up and delete the one sample post. It's been working like a dream (even if 
it doesn't look Super Rad yet, but that one's on me).

It also makes use of [Bootstrap](http://twitter.github.com/bootstrap/), a CSS 
and JS platform to help make sites look nice. In its function, it works quite 
a bit like [Blueprint CSS](http://blueprintcss.org), which I had used before 
at work, so the concept was familiar (12-column grid layout, fluid options, 
nice-looking things) even if the syntax was a little different. But because it 
was bundled into the Jekyll Bootstrap, I rolled with it and picked it up (yay 
skills!). While some of the JS functions still stump me as I'm playing with it 
on my computer, it looks and feels very fresh, and I like that.

### Got it

In all, I probably took me about 2 hours to get the hang of working with gits 
and the bash shell, a couple more to actually remember the commands you use most 
frequently, and then one or two more beyond that to figure out the bootstraps 
and come up with this site. In all, I think that's a pretty good time investment 
as now not only do I have a place where I can put my code up and a system to edit 
and update it, but now I've also got a place where I can talk about it all. 
I'll chalk it up as a win.
