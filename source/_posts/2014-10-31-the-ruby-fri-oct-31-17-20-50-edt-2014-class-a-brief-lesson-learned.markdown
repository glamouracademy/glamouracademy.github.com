---
layout: post
title: "The Ruby Date Class: a Brief Lesson Learned"
date: 2014-10-31 17:20
comments: true
categories: 
---
<h2><em>Why can I use the Ruby `Date` class without requiring date?</em></h2>


I was doing an <a href="http://exercism.io">exercism.io</a> exercise, Gigasecond, which required me to address my fear of `date` and `time`.  (You can see <a href="http://exercism.io/submissions/d056c2d12fb6363735f483ca">my submission here and nitpick</a> if you’d like.)

I noticed that I could use `date` without actually requiring `date.`

I thought to myself, “hmmm, that seems awfully inefficient.  Why should I type `require 'date’` if I don’t need to?” So this caused an investigation into: Do I actually need to `require ‘date’`?

My first stop was irb, where I noticed that I would get different results depending on whether or not required date and further if I required date after the fact, the result would change too.

{% codeblock %}

date = Date.new
=> #<Date: -4712-01-01 ((0j,0s,0n),+0s,2299161j)>

date.class
=> Date


require ‘Date’
new_date = Date.new
=> #<Date: -4712-01-01 ((0j,0s,0n),+0s,2299161j)>

new_date.class
=> Date

date.class
=> uninitialized constant error/bug!

{% endcodeblock %}

Suspicious.

I did the same exercise in <a href="www.repl.it">repl.it</a> and got an uninitialized constant error. I also did the same investigation for `Time` but there was no inconsistency in the results. So this led me to believe there was some versioning issue or perhaps there really is an bug in irb.

I <a href="http://stackoverflow.com/questions/26639080/why-can-i-use-the-date-class-without-requiring-date">posted on StackOverflow</a> and got some good information. 

Basically, it’s <a href="https://github.com/rubygems/rubygems/pull/948">a Rubygems bug for Rubygems versions earlier than 2.4.0</a>!

So, if you run `gem environment` in terminal you should see what version of ruby gems and ruby you’re running.

Apparently there is an intention to include this Rubygems fix into Ruby 2.2.  So until then, you can <a href="https://rubygems.org/pages/download">update your version of Rubygems</a> by: 

{% codeblock %}

$ gem install rubygems-update
$ update_rubygems

{% endcodeblock %}

and voila! When you try to create a Date object without requiring date you should now get a uninitialized constant error.  

Happy halloween.

<img src="http://thehydrant.files.wordpress.com/2012/09/h22.jpg?w=490">