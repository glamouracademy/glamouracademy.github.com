---
layout: post
title: "How to Bulk Delete Files in Git in 5 Steps"
date: 2015-02-12 10:20
comments: true
categories: [Github, Git]
---

You might be familiar with `rm -rf` and if you are, you probably also were warned to be _very careful_. So I’m pretty cautious about deletions for fear of accidentally deleting directories. Generally I’m not deleting a lot of files, so a simple `git rm /path/to/file` will suit just fine.  However if you find yourself in a situation where you’re trying to bulk delete files, deleting each file is quite tedious. 

You also might have noticed that when you do a `git add .` only the staged, modified files get added and you get the following message: "(use "git add/rm \<file\>..." to update what will be committed)"

So here’s how you bulk delete files:

1. See what is actually staged via `git status` (btw this is generally a good practice). I’m assuming that your deleted files are “not staged for commit”.

2. Stage your deleted files - `git add -u`

3. Confirm that your deleted files are staged - `git status`

4. Commit your now staged deleted files - `git commit -m "your_message"`

5. Push your commit like normal - `git push`

For more info, <a href="http://stackoverflow.com/questions/1786692/bulk-delete-in-git">this</a> stackoverflow is also helpful! And many thanks to <a href="https://twitter.com/joelbyler">@joelbyler</a> for patiently walking me through it!

Hope everyone is staying warm this winter!
