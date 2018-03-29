---
title: PyGreen – The Simplest Static Web Site Generator Ever For Python Programmers
---

[Pygreen](http://pygreen.neoname.eu/) is a simple tool for Python programmers to generate static web sites.

I’m using [Github Pages](http://pages.github.com/) since a long time now. I always thought it was a good idea because a lot of small web sites (like open-source project home pages as example) do not need a complete CMS. Static web sites are cheap to host and nothing can be as fast.

The problem is to find a good tool to generate them. Github choosed to propose Jekyll to their users. Other, similar, alternatives are Hyde or nanoc. I tried many of them but could not find anything really useful for me. In my opinion, the problem of those tools is that they try to do everything the “good” way, with configuration files, plugin system, Convention over Configuration, declarative stuff, etc…

It seems a good idea at first, but when using them you realize it’s not. Out of the box, these web site generators have very few features. Those features are rarelly documented correctly. Yeah, there is a plugin system, but the API is not documented correctly either. In the end, creating a simple blog with Jekyll can be a long, painful, session of code reverse-engineering to result in a web site with quite a lot of limitations because there is a log of things it will always be impossible to do with Jekyll.

So I thought “Hey, I’m a damn programmer, if I want to automatize a simple task I write a small piece of code and that’s it”. That’s why I  created [Pygreen](http://pygreen.neoname.eu/). PyGreen is a tool to help you make your own static web site generator in Python. Basically, you drop files in a folder and all html files can contain [Mako Templates](http://www.makotemplates.org/). If you’re a Python programmer and already know Mako, or can take 5 minutes to read the tutorial, that’s all you need to make 80% of the static web sites. Really, there is not much more PyGreen can do, and why would you need more? Of course this results in some sort of PHP-like programming style and PHP is an evil script kiddies programming language. But that’s exactly what you need to generate a static web site: a quick and dirty way to get the job done in less than 5 minutes. Plus Python is still a far better language that PHP and Mako is also a good template language with inheritance and everything you need.

Pygreen was also created to allow people to hack it like crazy. Want markdown? Use ‘import markdown’ in you Mako template and create a macro for that. Want dynamic routes? Insert some code to extend [Bottle](http://bottlepy.org/docs/dev/)‘s routes and that’s it. If, at some point of your project, you realize your web site can not be a static web site anymore and you need to go dynamic you can do it too, because PyGreen has the same conception as a dynamic web site.

So, that’s it. Read the 5 lines quick start guide on [the home page of Pygreen](http://pygreen.neoname.eu/) and try it. You could fall in love with it.
