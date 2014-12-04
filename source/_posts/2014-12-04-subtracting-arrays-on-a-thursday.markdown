---
layout: post
title: "Subtracting Arrays on a Thursday"
date: 2014-12-04 16:02
comments: true
categories: 
---

Just a little Thursday afternoon note about subtracting arrays that was discovered doing <a href="http://rosalind.info/problems/hamm/">the hamming test</a>, which detects the number of differences between two different DNA strands.

Let’s say you had to two DNA strands:

```
strand1 = [G,G,A,C,G]
strand2 = [G,G,T,C,G]
```

If you subtracted `strand1` from `strand2`, you’d expect the difference to be `1`, right?  In fact, it is `1`.


Let’s take another example of another two DNA strands:

```
strand3 = [A,G,A]
strand4 = [A,G,G]
```

If you subtracted `strand3` from `strand4`, you’d expect the difference to be `1`, right?  There is 1 letter different between the two strands.

However what you get is `0`.

It seems that the `-` operator checks for uniques rather than checking in place.

Happy Thursday!




