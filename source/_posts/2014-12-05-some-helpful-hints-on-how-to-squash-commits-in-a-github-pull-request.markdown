---
layout: post
title: "Some Helpful Hints on How to Squash Commits in a Github Pull Request"
date: 2014-12-05 11:59
comments: true
categories: 
---

Let's say you want to submit a pull request but you've committed more than once and let's say that also you're not satisfied with the commit message you originally used.  Let's say you might be feeling like this:

<img src="http://1.bp.blogspot.com/-AZNV9I1nBF4/UB6SvKtIR6I/AAAAAAAAAWY/fk8_oWh9JCI/s1600/midvale+school+for+the+gifted.jpg"/>

There's a great post by Steve Klabnik: <a href:"http://blog.steveklabnik.com/posts/2012-11-08-how-to-squash-commits-in-a-github-pull-request">How to Squash Commits in a github pull request</a>

There were a couple of things that I needed to know in order to follow Klabnik’s helpful post.

First, find out what remotes are set up: `git remote -v`.	
	
	```
	origin git@github.com:youraccount/repo.git (fetch)
	origin git@github.com:youraccount/repo.git (push)

	```
In this case I didn't have upstream set up.

So, add upstream: `git remote add upstream git@github:projectrepourl.git`  (You can get this url by navigating to the project’s repo and copying the clone URL.) 
Confirm that you’ve added the remote: `git remote -v`.

	```
	origin git@github.com:youraccount/repo.git (fetch)
	origin git@github.com:youraccount/repo.git (push)
	upstream git@github:projectrepourl.git (fetch)
	upstream git@github:projectrepourl.git (push)
	
	```
Note: if you're not super experienced with git, you might be wondering about 'origin', 'master', 'upstream'.  Here's a just little image that hopefully will help:

<img src="images/git.png"/>


Now you can follow Klabnik’s post:
	```
	$ git fetch upstream
	$ git checkout master
	$ git rebase -i upstream/master

	```

At this stage, you’ll get the opportunity to view the commits you had made previously. 

<img src="images/editcommits.png"/>
<small>Note: this image is after I had squashed so you only see one commit; but, you can get an idea of what you'd see in vim</small>

Add an `s` or `squash` to the commits that you want to squash. For me, I added an `s` to the commit after the first commit.  The word `pick` prepends the commit info.  So delete the word `pick` from the second commit and add `s` in it’s place. You now get the option to edit the commit message.

Finally, as Klabnik says: `$ git pull origin master -f`
If you go back to github, you’ll see that your commits have changed to the squashed number of commits. (For me I squashed two commits to one.) 

