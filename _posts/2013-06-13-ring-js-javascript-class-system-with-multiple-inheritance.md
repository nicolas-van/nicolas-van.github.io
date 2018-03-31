---
title: Ring.js – JavaScript Class System with Multiple Inheritance
image: /assets/img/ring_2.png
---

[Ring.js](http://ringjs.neoname.eu/) is JavaScript library implementing a multiple inheritance model for the JavaScript language.

Recently, I was quite disappointed by the class systems available for JavaScript. You see, I’m a JavaScript programmer but also a Python programmer. When you are used to multiple inheritance, especially to the way it’s implemented in Python (unlike C++ which is evil), you can figure there are a lot of problems caused by single inheritance that you can easily solve using multiple inheritance.

There are a lot of class systems available in JavaScript, but they are all more or less the same. The smarter ones implement interfaces and mixins. I tried to use mixins for a time, thinking it should be a good compromise to implement what could easily be achieved in Python with multiple inheritance but with the single inheritance model of those libraries. I tried, really hard. But in the end I came to think that mixins are just a bad concept. There is no way to really determine what deserves to be a mixin and what should not. What could be reusable and what is not.

With all that frustration, I read the Python’s multiple inheritance algorithm specification. And I saw that… it was not so complicated. Actually it was even quite easy to implement in JavaScript. So I implemented it, packaged a library, tested it, deployed it and made some documentation and web site.

[See Ring.js here](http://ringjs.neoname.eu/).
