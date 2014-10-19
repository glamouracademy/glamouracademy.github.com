---
layout: post
title: "Refactoring: my 5 steps to 90% less code"
date: 2014-10-17 15:24
comments: true
categories:
---

I recently had a lot of fun coding the <a href="http://rosalind.info/problems/hamm/">hamming distance</a> via <a href="http://exercism.io/getting-started">exercism.io</a>.  Firstly, I want to caveat that I totally recognize that my approach to this problem is done in a bubble.  That is, I don’t have business constraints, tech constraints, just newbie brain constraints.  ;p

I got exercism.io up and running and after reading the `README.md`, ran the first test.*  The exercism.io exercises have tests written that need to pass, which is how you end up writing the program - yeah <a href=”http://en.wikipedia.org/wiki/Test-driven_development”>TDD</a>! 

Before I started coding I decided that my overall approach was going to be Kent Beck’s <a href=”http://c2.com/cgi/wiki?MakeItWorkMakeItRightMakeItFast”>Make it Work, Make it Right, Make it Fast</a>.  This meant I wasn’t going to worry about the next test or the previous test.  I just wanted to do the easiest thing working.

<h2> Step 1: Get it working! </h2>

The tests are 42 lines of code.  My first solution was <em>61 lines of code</em>.  And all the tests passed.  It was ugly but it worked, like using a fork to comb your hair* - it sort of hurts but it gets the job done.

{% img left /images/refactor1.png 350 350 'image' 'images' %}

There were three areas of refactoring that I could think of:

* readability
* duplication
* nested conditionals

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

<h2> Step 2: Refactor for readability </h2>

The first thing that jumped out at me was how I was converting a string into an array. Basically I had an empty array for each string, splitting on each letter of the string and then each of those letters ended up into their respective array. Although it was explicit, it was also a bit taxing to read how it was being split, e.g. `.split(//)`. `.split(//)` is powerful as it allows strings to be split in a number of ways but I just needed a basic splitting by each character.)

{% img right /images/refactor2.png 350 350 'image' 'images' %}

<br>
<br>
<br>

Then, I learned about the `.chars` method. 

OMG `.chars`!  (Yes it’s similar to my name but that’s not why I’m excited.)  `.chars` returns an array of characters for a string <a href=”http://ruby-doc.org/core-2.0/String.html#method-i-chars”>(Ruby docs)</a>. Basically `str.each_char.to_a`.  Or in my mind, delicious magic.

{% img left /images/refactor3.png 350 350 'image' 'images' %}

<em>Down to 43 lines of code</em>.


<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

<h3> Step 3: Refactor explicit duplication </h3>

The easiest thing I noticed next was that I declared the same variable 4 times.  That’s at least 3 times too many.

{% img left /images/refactor4.png 350 350 'image' 'images' %}

Since all my conditionals are in the same method they can all access the variable.  So since Ruby reads code from top to bottom and since , I’ll extract it to the top of my method, outside of all the conditionals. (There’s also a pattern to extract variables in <a href=”http://refactoring.com/catalog/extractVariable.html“>The Refactoring Catalog</a>.)

<h8>Note:</h8> it’s at this stage that I realised there’s a bug within my code (lines 26 & 33).  I’m submitting a pull request to add tests to exercism.io and for the purposes of this blog it should be fine.

<em>Alright, 38 lines!</em>

<br>
<br>
<br>

<h3> Step 4: Refactor simple functional duplication </h3> 

The two conditionals on lines 21 and 27 look very similar.  They both: 

* check if one strand is longer than the other (lines 21, 27)
* trim off the extra letters of the longer strand so that it’s the same length as the shorter strand (lines 22, 28)
* creates an array for the new strand (lines 23, 29)
* creates a combined array of arrays that contains the paired elements from the two arrays (lines 24, 30)
* compares to see if the paired elements match and counts if they do not match (lines 25, 31)

{% img right /images/refactor5.png 350 350 'image' 'images' %}

So really, if one strand is longer than the other, regardless if it’s the first strand passed or second, what we’re really saying is that the two strands don’t equal.

I can use `.min` to determine the minimum length of the two strands and then I can trim both strands to that minimum length while zipping them into one array that contains arrays of paired elements from the two strands.

Lastly, I can iterate through those paired elements and check if they are the same value.  I can then count the number of times they do not match, which is the Hamming Distance.

{% img center /images/refactor6.png 350 350 'image' 'images' %}

<em>31 lines!</em> 

<h3>Step 5: Refactor functional duplication</h3>

{% img left /images/refactor7.png 350 350 'image' 'images' %}

I wrote the conditional on line 5 more explicitly.  But I noticed that it’s doing the same function as the conditional on line 21.  (There’s the beauty of Ruby - many ways to do something!) However, at RailsConf this year I went to Sandi Metz’s talk <a href=”https://speakerdeck.com/skmetz/all-the-little-things-railsconf”>“All the Little Things”</a>. She mentions that nested conditionals are good candidates for refactoring.  And let’s be honest, while I do love that it’s very much step-by-step, it is a bit long and there’s mental overhead to remember where I am in the condition.

(As a sidenote, this refactoring made me realize the potential tradeoff (especially as a newbie) between clarity and readability, specifically:

* laying out all the steps seems clear however it take mental effort to remember all the steps and where I am in those steps
* collapsing code makes the code more dense and that also takes effort to read.)

It’s at this point that I suspect (ever so cautiously) that the conditional on line 21 might pass all my tests.  (For the record, this is like waiting in line for a roller coaster or trying out a new recipe and waiting for it to bake in the oven - you are really excited and hopeful but also a bit scared that it might just go all wrong.  And my imposter syndrome rears it’s head too.)

So I move the magic conditional to the top of my method, remove it’s conditional-ness, and comment out all the rest of the code.  (#fearofcodecommitment)

{% img left /images/refactor8.png 350 350 'image' 'images' %}

…and run my tests.
<br>
<br>
<br>
<br>
<br>
<br>
…and listen to: 

<iframe src="https://www.youtube.com/watch?v=_zBwRDEFMRY" frameborder="0" width="480" height="299" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

It worked!  I’m a wizard!  Actually I’m a paladin!  Someone high five me or fist bump me!

{% img left /images/refactor9.png 350 350 'image' 'images' %}

<br>
<br>
<br>
<br>
<em>7 lines of code!</em> 

<strong>5 steps. 90% less code.</strong> 

You can nitpick me <a href=”http://exercism.io/submissions/b8f1be00cf8ec58d2dfd6679”>here</a>. (On line 4, I do think there’s one debatable improvement for readability and one less debatable improvement for removing duplication.)

*Thanks to exercism.io for inadvertently teaching me about `skip` in MiniTest.

*Yes I’m referencing Ariel in The Little Mermaid.





