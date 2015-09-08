---
layout: post
title: "Git Gardening with Git Prune"
date: 2015-03-24 14:41
author: steven.murawski@gmail.com
comments: true
categories: []
tags: []
---


I work on a number of projects on GitHub now, with Chef (client) being one of the largest and most active.


Every time I fetch from GitHub, I get all the current branches. &nbsp;Branches tend to be short lived in this project, existing solely for a feature or a bugfix (which allows us to use Pull Requests for code reviews and discussion). &nbsp;However, when a branch gets removed from GitHub's version of the repository, that change doesn't sync down to me when I fetch changes. &nbsp;This is a good thing - as it keeps me in control of my local repository.


However, from time to time, I want to clean up those remote branches that do not exist anymore.


[Git pruning to the rescue!&nbsp;](http://git-scm.com/docs/git-prune)&nbsp;&nbsp;


git remote prune origin</pre>
    
    To show what would be pruned:
    <pre>git remote prune origin--dry-run
<p dir="ltr">There are more options with [git gc](http://git-scm.com/docs/git-gc)&nbsp;(and that is recommended), but git prune did what I was looking for.&nbsp;

